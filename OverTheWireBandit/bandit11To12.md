log into bandit 11 using 12th password

---

Check the contents
Code: ``ls``
Output: *data.txt*

---

Decipher the text
Code: ``cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'``
Output: *The password is 13th_Password*

The tr command substitutes the first string elements
to the second string elements. The first string has
uppercase A to Z and lowercase a to z. The second
string has N to Z then A to M and n to z then a to m

Save the 13th password
