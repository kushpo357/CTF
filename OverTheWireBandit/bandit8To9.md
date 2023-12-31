log into bandit 8 using 9th password

---

check the contents
Code:``ls``
output: *data.txt*

check the file type
Code: ``file data.txt``
output: *data.txt: ASCII text*

check the number of lines in the file
Code: ``wc -l data.txt``
output: *1001 data.txt*

---

since the lines are too many we will have to use a tool

we have to find the unique line in this file
for that we will sort file and then print the unique one
Code: ``sort data.txt | uniq -u``
``sort data.txt`` , sorts the file and ``uniq -u`` prints the unique text
output: *10th password*
