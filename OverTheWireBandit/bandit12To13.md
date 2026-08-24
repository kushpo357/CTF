Log into bandit12 using 13th password

---

The file for this level consits of hex dump of a file in a text file
to make more files we will have to navigate to tmp

Code: ``cd ../../tmp``
Output: *bandit12@bandit:/tmp$*
Now were are in the tmp folder

make a temp dir 
Code: ``mkdir tempdir``

**Note: here ls won't work but the directory is made**

Come back to the initial dir
Code: ``cd``
Output: *bandit12@bandit:~$*

---

Convert the hexdump into a file inside tempdir
Code: ``xxd -r data.txt > ../../tmp/tempdir/data``

xxd is a command which deals with hexdumps, -r is for revert
as we are converting hexdump into a file. '>' is used to write
the content into the desired file, ../../tmp/tempdir/data
is the file name and its location.

---

Go into the tempdir and check the file type of data
Code: ``cd ../../tmp/tempdir && file data``
Output: *data: gzip compressed data, was "data2.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 573*

It is a gzip commpressed file so we have to decmopress it after converting it to .gz file
Code: ``mv data data.gz && ls``
Output: *data.gz*

Decompress it and check the contents
Code: ``gzip -d data.gz && ls``
Output: *data*

Check the file type
Code: ``file data``
Output: *data: bzip2 compressed data, block size = 900k*

It is a bzip2 compressed file so we will decompress it after converting it to .bz2
Code: ``mv data data.bz2 && ls``
Output: *data.bz2*

Code: ``bzip2 -d data.bz2 && ls``
Output: *data*

Check the file type
Code: ``file data``
Output: *data: gzip compressed data, was "data4.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 20480*

Decompress it after changing it's extension
Code: ``mv data data.gz && gzip -d data.gz && ls``
Output: *data*

Check the file type
Code: ``file data``
Output: *data: POSIX tar archive (GNU)*

Convert to tar file and check the contents
Code: ``mv data data.tar && ls``
Output: *data.tar*

Extract
Code: ``tar -xvf data.tar``
Output: *data5.bin*

Checking the file shows its a tar archive again, tar extraction part && check the contents
Code: ``tar -xvf data5.bin``
Output: *data6.bin*

Code: ``file data6.bin``
Output: *data6.bin: bzip2 compressed data, block size = 900k*

Decompress and check file type
Code: ``mv data6.bin data6 && bzip2 -d data6.bin && file data6``
Output: *data6: POSIX tar archive (GNU)*

Convert and extract and check the file type
Code: ``mv data data6.tar && tar -xvf data6.tar``
Output: *data8.bin*

Code: ``file data8.bin``
Output: *data8.bin: gzip compressed data, was "data9.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32*

Rename and extract and check the file contents
Code: ``mv data8.bin data8.gz2 && gzip -d data8.giz && file data8``
Output: *data8: ASCII text*

Open the text
Code: ``cat data8``
Output: *The password is 14thPassword*

SAVE THE 14Th password 

