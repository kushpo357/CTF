# Natas6 → Natas7

Natas6 presents a form asking for a "secret" value, checked against PHP source that includes a separate file:

```php
include "includes/secret.inc";

if(array_key_exists("submit", $_POST)) {
    if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
}
```

**Sticking point:** The first instinct was to look for an injection angle, since `$secret` is compared against user input. But `$secret` isn't derived from any request data before that comparison — it's set entirely by the included file, `includes/secret.inc`, which never appears in the rendered page or its "View sourcecode" output (PHP includes are executed server-side and don't show up in the output unless they error or echo something).

**Resolution:** Since `includes/secret.inc` is just a file sitting in the web root, nothing stops it from being requested directly, the same way `index.php` is:

```
http://natas6.natas.labs.overthewire.org/includes/secret.inc
```

Requesting it directly returned the raw PHP source, including the actual value being assigned to `$secret`. Submitting that value back through the form on the main page returned:

```
Access granted. The password for natas7 is <natas7_password> (will differ per player)
```

**Key insight:** A file only stays "hidden" as PHP logic if the web server is configured to execute it as PHP when requested — but if the extension (`.inc`) isn't mapped to the PHP interpreter, the server just serves it as plain text, dumping the raw source instead of running it. Including a file for logic reuse doesn't protect its contents from being requested directly.

**Key technique:** Directly requesting an included PHP file with a non-`.php` extension to read its raw, unexecuted source.
