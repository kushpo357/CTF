Log into bandit 10 using 11th password

---

Check the contents
Code: ``ls`` 
Output: *data.txt*

Check the file type
Code: ``file data.txt``
Output: *data.txt: ASCII text*

---

Check the file content
Code: ``cat data.txt``
Output: *VGhlIHBhc3N3b3JkIGlzIDZ6UGV6aUxkUjJSS05kTllGTmI2blZDS3pwaGxYSEJNCg==*

We will decode it using base64 decoder
Code: ``base64 --decode data.txt``
Output: *The password is 12th_Password*

Save the 12th password
