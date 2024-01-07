log into bandit 9 using the 10th password

---

Check the contents
Code: ``ls``
Output: *data.txt*

check the no. of lines int the file
Code: ``wc -l data.txt``
Output: *67*
There are a lot of lines

---

Get human readable lines from the file
Code: ``strings data.txt``
There will be a lot of output

we will feed this output into grep to find lines preceeded by "=="
using the pipe operator
Code: ``strings data.txt | grep "=="``
Output:
*x]T========== theG)"
========== passwordk^
========== is
========== 11th Password*

save the 11th password
