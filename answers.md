# Lab 2: Answers

Name: Gülcan Yamer
Student ID: 20211701071

## Part A: Creating and Organizing

**A1.**

```
$ pwd
/home/gulcan/lab2/techcorp
```

**A2.**

```
$ mkdir -p projects/2026/{alpha,beta,gamma}
$ ls projects/2026
alpha
beta
gamma
```

**A3.**

```
$ touch projects/2026/alpha/README.md
$ ls -l projects/2026/alpha/README.md
-rw-r--r-- 1 gulcan gulcan 0 Oct 20 12:12 projects/2026/alpha/README.md
```

The file exists and its size is 0, so it is empty.

**A4.**

```
$ cp -r hr/policies projects/2026/beta/
$ ls -l projects/2026/beta/policies
total 8
-rw-r--r-- 1 gulcan gulcan 13 Oct 20 12:12 leave.txt
-rw-r--r-- 1 gulcan gulcan 14 Oct 20 12:12 remote.txt
```

**A5.**

```
$ mv projects/2026/gamma projects/2026/delta
$ ls projects/2026
alpha
beta
delta
```

**A6.**

```
$ ln -s ../engineering/docs/api.md shared/api_link
$ ls -l shared/api_link
lrwxrwxrwx 1 gulcan gulcan 26 Oct 20 12:12 shared/api_link -> ../engineering/docs/api.md
```

**A7.**

```
$ cat shared/api_link
api reference
```

The link shows the content of engineering/docs/api.md. A symbolic link only points to the real file, so cat follows it and prints the original file's content.

## Part B: Users and Groups

**B1.**

```
$ id
uid=1000(gulcan) gid=1000(gulcan) groups=1000(gulcan),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),117(netdev)
```

**B2.**

```
$ sudo groupadd engineering
$ sudo groupadd finance
$ getent group engineering finance
engineering:x:1013:alice
finance:x:1014:bob,alice
```

**B3.**

```
$ sudo useradd -m alice
$ sudo useradd -m bob
$ ls -ld /home/alice /home/bob
drwxr-x--- 2 alice alice 4096 Oct 20 12:12 /home/alice
drwxr-x--- 2 bob   bob   4096 Oct 20 12:12 /home/bob
```

**B4.**

```
$ sudo usermod -aG engineering alice
$ sudo usermod -aG finance bob
$ sudo usermod -aG finance alice
```

The -a (append) option adds alice to finance without removing her from engineering.

**B5.**

```
$ groups alice bob
alice : alice engineering finance
bob : bob finance
```

## Part C: Permissions

**C1.**

```
$ ./engineering/src/build.sh
bash: ./engineering/src/build.sh: Permission denied
$ ls -l engineering/src/build.sh
-rw-r--r-- 1 gulcan gulcan 26 Oct 20 12:05 engineering/src/build.sh
$ chmod u+x engineering/src/build.sh
$ ls -l engineering/src/build.sh
-rwxr--r-- 1 gulcan gulcan 26 Oct 20 12:05 engineering/src/build.sh
$ ./engineering/src/build.sh
building
```

Cause: the file had no execute permission (-rw-r--r--). chmod u+x gives the owner execute permission, so the script runs.

**C2.**

```
$ find . -type f -perm -o=w
./hr/records/anna.txt
./finance/invoices/inv_103.csv
find: './engineering/locked/flag.txt': Permission denied
./engineering/src/mod_7.c
```

**C3.**

```
$ chmod o-w hr/records/anna.txt finance/invoices/inv_103.csv engineering/src/mod_7.c
$ ls -l hr/records/anna.txt finance/invoices/inv_103.csv engineering/src/mod_7.c
-rwxrwxr-x 1 gulcan gulcan 12 Oct 20 12:05 engineering/src/mod_7.c
-rw-rw-r-- 1 gulcan gulcan 12 Oct 20 12:05 finance/invoices/inv_103.csv
-rwxrwxr-x 1 gulcan gulcan 16 Oct 20 12:05 hr/records/anna.txt
$ find . -type f -perm -o=w
find: './engineering/locked/flag.txt': Permission denied
```

No files are listed anymore, so the world-writable problem is fixed.

**C4.**

```
$ touch shared/team.txt
$ sudo chgrp engineering shared/team.txt
$ ls -l shared/team.txt
-rw-r--r-- 1 gulcan engineering 0 Oct 20 12:12 shared/team.txt
```
