---
layout: default
title:  "Shell Tips"
date:   2024-12-08 18:55:51 +0800
tags: shell
---
## Common commands in Linux

### Getting the system information

```shell
# Get the current shell
echo $SHELL
# or
echo $0

# Get the system version.
uname -a
cat /proc/version
cat /proc/version_signature

# Get the system architecture
arch

# Get the CPU info
cat /proc/cpuinfo

# Get the memory info
free -th
cat /proc/meminfo

# Get the disk info
df -hT

# Get the size of current folder
du -sh

# display the path where can find the execution files
echo $PATH
alias path='sed "s/:/\n/g" <<< "$PATH"'
path

# Get who is logged on.
w
who

# Print the user name
whoami

# Print the groups a user is in
groups

# Print the user ids.
id
# only user id
id -u

# show the current date time
date

# show how long the system has been running
uptime

# show the hostname
hostname
# show the IP
hostname -I

# show the network interface
ifconfig

# display the route table
route

# IP command
ip link show
ip route show

# list all ports
netstat -a
# list all TCP ports
netstat -at
# list all UDP ports
netstat -au

# list all listening ports
netstat -l
netstat -lt
netstat -lu

# show the environmentable viriable
env

# show the alias
alias

# list all running processes including the full command string
ps auxww

# list all processes of the current user
# -F extra full format
ps --user $(id -u) -F
# --sort=size sorting by memory size in kilobytes
ps --user $(id -u) -F --sort=size
# reverse sorting
ps --user $(id -u) -F --sort=-size 
# sorting by cpu utilization
ps aux --sort=pcpu

# display a tree of processes
pstree
# display all process trees rooted at processes owned by specific user(e.g. zt)
# -p show the PIDs
pstree -p zt

# list all service
systemctl list-units
# operations about a service
systemctl start|stop|restart|reload|status unit_name

# log out
logout

# reoot
sudo reboot

# power off(halt) immediately
sudo shutdown -h now
# shutdown at 1:00 PM
sudo shutdown -h 13:00
# cancel a pending shutdown
shutdown -c 
```

### Adding/Removing a user

```shell
# add user
# -s login shell
# -G also groups
# -m make home folder
sudo useradd -s /usr/bin/zsh -G adm,sudo,vbosf -m zt
# set password
sudo passwd zt


# remove user
sudo userdel zt
```

### Packages

```shell
# if yum,
# list packages
yum list
# install a package
sudo yum update
sudo yum install pstree

# if apt
# list packages
apt list
# install a package
sudo apt update
sudo apt install pstree

# if a local .deb file
# install a package
dpkg -i package.deb
# list all 
dpkg -l

# if a local .rpm file
# install a package
rpm -ivh package.rpm
```

### Playing with files

```shell
# enter the home directory
cd
# return to the directory where you were last time
cd -

# list files
alias ls='ls -Fh --color=auto'
ls -l
ls -la
# sort by the time, newest first
ls -t
# sort by time, oldest first
ls -tr
# show only the dot files
ls -d .*

# create an empty file
touch 1.txt
# print the output of `ls` to file 2.txt
ls > 2.txt
# append the output to the file
ls -l >> 2.txt

# print the stdout to the file
ls > l1.txt
# print the stderr to the file
ls 2> l2.txt
# print the stdout and stderr to the file
ls &> l3.txt

# create folder
mkdir -p ./d1/d2/d3

# copy file
cp 1.txt 11.txt
# move file
mv 11.txt 22.txt
# delete a file
rm 22.txt

# link a file
ln -s file1 file2

# count newline, word, char and bytes for a file
wc -l -w -m -c 1.txt

# print the file to the stdout
cat file1.txt
# print the file to the stdout in reverse
tac file1.txt

# `less` also print the file
less file1.txt
# -i ignore case
less -i file1.txt

# output the first 10 lines
head -n 10 file1.txt
# output the last 5 lines
tail -n 5 file1.txt
# output appended data as the file grows
# watching the logs
tail -f application.log

# sort lines of text files
sort log1.txt log2.txt
# -k sort via a key
# sort by the second column
sort -k 2 data.txt

# omit repeated lines
uniq file1.txt
# only print duplicate lines
uniq -d file1.txt

echo -e "a\na\nb\nc" | uniq

# split file into pieces
# -l 1000 put 10000 lines per output file
# -d use numeric suffix starting at 0
# a.txt_ prefix of output files
split -l 10000 -d ./a.txt a.txt_

# merge files
cat file1.txt file2.txt file3.txt > merged.txt
(echo "=== File 1 ==="; cat file1.txt; echo "=== File 2 ==="; cat file2.txt; echo "=== File 3 ==="; cat file3.txt) > merged.txt

# select only fields 2, 3 from file
# by default the field delimiter is TAB
cut -f 2,3 file1.txt
# use : instead of TAB for field delimiter
cut -d : -f 3 file2.txt

# merge files by columns, seperate by TAB
paste file1 file2 file3
# use : instead of TAB to merge files by columns
past -d : file1 file2 file3

# change file owner to zt, group to zt
chown -R zt:zt ./ws

# change file mode bits
# user: 7 r+w+x (4+2+1)
# group: 5 r+x (4+1)
# others: 5 r+x (4+1)
# owner can read/write/execute the file
# the users in the group can read/execute the file
# others can read/execute the file
chmod 755 ./test1.sh

# user 6 r+w
# group 4 r
# others 4 r
chmod 644 ./test07

# add execution for all
chmod +x ./test.sh

# add execution to the owner
chmod u+x ./test.sh

# MD5 message digest
# compute MD5
md5sum ./1.txt > 1.txt.md5
# check MD5
md5sum -c 1.txt.md5

# SHA1 message digest
# compute SHA1
sha1sum ./1.txt > 1.txt.sha1
# check SHA1
sha1sum -c 1.txt.sha1

# create .tar file
tar -cvf archive.tar file1 file2 file3
# create .tar.gz file
tar -cvfz t1.tar.gz dir1
# list the contents of an archive
tar -tf t1.tar

# `find` command, search for files
# find all the .bak files in a directory hierarchy
# -iname match is case insensitive
find . -iname '*.bak'

# search all non .bak files
find . ! -iname '*.bak'

# find files which size is greate than 2K
find . -type f -size +2k

# find the files that have been accessed within 7 days
find . -type f -atime -7

# find the files that have updated within the past 10 minutes
find . -type f -mmin 10

# find all the files belonging to user `zt`
find . -user zt

# delete all the .bak files
# -type f file is a regular file
# -print0 print the full file name on the stdout, followed by a null character instead of the newline
find . -iname '*.bak' -type f -print0 | xargs -0 rm -f

# count all the lines of code
find . -type f -iname "*.py" -print0 | xargs -0 wc -l

# `grep` command, search lines that match patterns
# search error in the log file
# -i ignore-case
grep -i error ./application.log
# -B 3 show the 3 lines leading context before matching lines.
# -A 2 show the 2 lines trailing context after matching lines.
grep -B 3 -A 2 -i exception ./application.log

# `sed` command, stream editor for filtering and transforming text
# search all the 'AA', replace with 'BB', output to new file
sed 's/AA/BB/g' test.txt > new.txt
# edit file in place
sed -i 's/AA/BB/g' test.txt

# replace all the TAB with comma(,)
sed 's/\t/,/g' input_file > out_file

# trim all the trailing spaces
sed -i 's/ *$//' t1.py

# 统计每个字母出现的次数
# sed 's/[^\n]/&\n/g' 把找到的非回车字符后面加一个回车字符 意味着在每个字母的后面加了一个回车
#   s 替换命令
#   [\n] 回车字符 
#   [^\n] 非回车字符
#   & 前面找到的字符
# sed '/^$/d' 删除空行
#   ^$ 行开头 和 行结尾，表示空行
#   d 删除命令
# sort 按照字母顺序排序
# uniq -c 统计相同行出现的次数
echo "ahebhaaa" | sed 's/[^\n]/&\n/g' | sed '/^$/d' | sort | uniq -c`

# replace the first `false` in all JSOn files under the current directory with `true` in place,
# also makes backup
find . -type f -iname "*.json" -exec sed -i ".bak" 's/false/true/' {} \;

# `awk` pattern scanning and processing language
# print the 2nd field
ls -l | awk '{print $2}'
# or print 2nd and 3rd fields from a file
awk '{print $2,$3}' file1.txt
# print the last field using a comma as a field sepertor
#   -F field seperator
#   NF the number of fields in the current input record.
awk -F ',' '{print $NF}' ./test2.log
# print the columns, connecting with commas
# output should be like 
#   2025/01/30 12:30:45Z,buyer1,sku1   
awk '{print "2025/01/" $2 " " $3 "Z" "," $7 "," $12 }' ./test1.log
```

## Bash scripting

```shell
#!/usr/bin/env bash
# Program:
#   this is a shell script.
# History:
# 2024-12-20    Peter Song  First release


# This is comment. 

# set value to a variable
myname=zt
# print the value of a variable
echo $myname
# or
echo ${myname}

# get the length
length=${#myname}
echo $length

# in "", $HOME will show as /home/zt
myhome1="my home folder is $HOME"
echo $myhome1

# in '', no change on the $HOME
myhome2='my home folder is $HOME'
echo $myhome2

# get my id
myid=$(id -u)
echo $myid

# remove variables
unset myid myohme myhom2 myname

# print the PID of the current shell
echo $$

# print the exit code of last command
echo $?

read -p "Your name: " yourname
echo $yourname

# set variable values and attributes
# by default, it's `string` attribute
s1=10+9+8
# output is 10+9+8
echo $s1

# make it to have the `integer` attribute
declare -i s2=10+9+8
# outpu is 27
echo $s2
# or use `let` for arithmetic
let s3=10+9+8
echo $s3
# or use `(())` for arithmetic
(( s4=10+9+8 ))
# output is 27
echo $s4
# increase 1
let s4++
# decrease 1
let s4--

n1=2
n2=5
# use `let` to add
let n3=n1+n2
# output is 7
echo $n3

# make it indexed arrays
declare -a ss
ss[1]=small
ss[2]=big
ss[3]="extra big"
echo ${ss[1]} ${ss[2]} ${ss[3]}
# or
aa=(small0 big1 "extra large2")
echo ${aa[0]} ${aa[1]} ${aa[2]}
# display all the elements
echo ${aa[*]}
# or
echo ${aa[@]}
# display the length
echo ${#aa[*]}


# test
# arithmetic test 
# `(( ... ))` and `let` are same.
# for exit code, 0 means true, 1 means false
# output is 1, means false
(( 0 )); echo $?
# output is 0, means true
((1)); echo $?
((2)); echo $?
((5>4)); echo $?
# output is true
# && logical and
# || logical or
((n1=2, n2=5))
((n2>4)) && echo true || echo false
((n2>=5)) && echo true || echo false
((n2==5)) && echo true || echo false
((n1 > 0 && n1 < 3)) && echo true || echo false
((0 < n1 < 3)) && echo true || echo false
# output is false
((n2 < 4)) && echo true || echo false
(( n2 <= 4 )) && echo true || echo false
((n2==4)) && echo true || echo false

# extended test `[[ ... ]]
str1=abc
str2=def
# output is false
[[ $str1 == $str2 ]] && echo true || echo false
[[ $str1 > $str2 ]] && echo true || echo false
# output is true
[[ $str1 != $str2 ]] && echo true || echo false
[[ $str1 < $str2 ]] && echo true || echo false
# -z is empty
# output is false
[[ -z $str1 ]] && echo true || echo false 
# -n is not empty
# output is true
[[ -n $str1 ]] && echo true || echo false

# file checking
var=~/s2.py
# if file is existing
[[ -e $var ]]
# if it is a directory
[[ -d $var ]]
# if it is a regular file
[[ -f $var ]]
# if it is a link file
[[ -L $var ]]
# if it is executable
[[ -x $var ]]
# if it is readable
[[ -r $var ]]
# if it is writable
[[ -w $var ]]
# if it is character device 
[[ -c $var ]]
# if it is block device
[[ -b $var ]]

# old style
# `test` command and `[]` are same
((n1=2, n2=5))
[ $n1 -eq 0 ] && echo 0
[ $n2 -gt 0 ] && echo 1
[ $n2 -ge 0 ] && echo 2
[ $n2 -le 0 ] && echo 3
[ $n2 -lt 0 ] && echo 4

# if
read -p "Continue to delete the file? y/N " ans
if [[ $ans == 'y' || $ans == 'Y' ]]
then
    echo you choosed Y
elif [[ $ans == 'n' || $ans == 'N' ]]
then
    echo you choosed N
else
    echo you typed ${ans}
fi

# while
read -p "Enter your password: " pw
while [[ $pw  != "no" ]]
do
    echo "wrong password."
    read -p "Enter your password: " pw
done
echo "Right password."

# until
read -p "Enter your password: " pw
until [[ $pw  == "no" ]]
do
    echo "wrong password."
    read -p "Enter your password: " pw
done
echo "Right password."

# for
# loop every word
a="hello world why am I so sad"
for word in $a:
do
    echo $word
done

# loop every char
b="hello"
for((i=0;i<${#b};i++))
do
    echo ${b:i:1}
done

# loop every word, using , as seperator
data="name,sex,rollno,location"
oldIFS=$IFS
# use , as seperator
IFS=,
for item in $data;
do
    echo Item: $item
done
IFS=$oldIFS


# define a function
ff1()
{
    # print the number of parameters
    echo $#
    # print the function name
    echo $0
    # print the parameter 1, paraeter 2
    echo $1, $2
    # print all the parameters
    echo "$@"
}
ff1 p1 p2 p3 p4
```


## Homebrew
```shell
# List the packages that the user requests to install.
brew leaves

# List the packages that `pyenv` depends on.
brew deps --tree pyenv

# List the packages that depend on `tree-sitter`.
brew uses --recursive --install tree-sitter
```

## MacPorts
```shell
# List the packages that the user requests to install.
port installed requested

# List the packages that `trash` depends on.
port rdeps trash

# List the packages that depend on `bzip2`.
port rdependents bzip2
```
