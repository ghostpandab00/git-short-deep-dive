## Introduction
Git is actually a tool to build copy of files. You may be using 'cp' command in Linux to copy files, right ? So you will be asking what is the difference between using 'cp' command and 'git' command! Before addressing that, we need to know why we need copy of files or what's the requirement of that.

Suppose you're writing a giant code for an app. You're an expert and you can complete the code in one stretch sit. But what if you want to test each section of the code.By using staging method we can achive that and it helps us to roll back to the previous version that we want. 

By using a normal copying we can't achive that because it will overwrite our content with new changes. Where comes our 'git' command. Git copying is more efficient than copying using cp command.

## CheckSum value/Hash Value
We need to understand checksum value in order understand the correct working of git. The hash function comes in play when we need to confirm the source file is came into destination without any modification.

File Input ---> Hash function ---> CheckSum value

```sh
$ echo "I'm Linux" > linux.txt
$ md5sum linux.txt
5047ee8de8abe2e555b8a42b26a85cc3  linux.txt
```

Hash function takes the contents of the file as input. It can be any data and files. Hash function creates a string from the data of the file. It can include alphabets and numerics. The string should be in fixed length. ie, the length of string generated from 1Kb of file and 50Mb file should be same. Hash function created the string from the contents and not from the filename of the file so if the contents are same then checksome value should be same.

```sh
~ ❯ echo "linux" > linux.txt                                                                                                                                06:46:57 PM
~ ❯ echo "linux" > unix.txt                                                                                                                                 06:47:39 PM
~ ❯ echo "Linux" > arch.txt 

~ ❯ md5sum linux.txt                                                                                                                                        06:48:00 PM
5bb062356cddb5d2c0ef41eb2660cb06  linux.txt
~ ❯ md5sum unix.txt                                                                                                                                         06:48:14 PM
5bb062356cddb5d2c0ef41eb2660cb06  unix.txt
~ ❯ md5sum arch.txt                                                                                                                                         06:48:23 PM
1b61f2a016f7478478fcb13130fcec7b  arch.txt
```

Here I've created three files. linux.txt and unix.txt contais the same data but arch.txt contains different data since we user cpaital 'L' in "Linux". Then we've checked the checksum value using a command of md5 function. From that, we can see the checksum value of linux.txt and unix.txt same where arch.txt having a different checksumvalue.

## Working Directory
The directory we using to copy files with the use of Git is called Working Directory. In order to initialze a working directory we need to run below command in the direcrory that we want.

```sh
$ git init
```

By initializing the git, git will create a folder ".git" which is called as "Local git repository". Git stores the files we copying to the local git repo.

## git add
we can use "git add" command to save a copy of file in the working directory to the git local repository. By executing the git add command, an object is creating by git. So what's an Object ? Git first read the contents of file and create checksum value. The object contain the contents of the file. In order to identify the objects, the object should have object id and object id will be the checksum value of content of the file. 

## Index/Staging file
The file name and object id mappig is done using staging file or index file. The index file contains the list of objects of the files. Let's create two files.

```sh
~ ❯ echo "linux" > linux.txt                                                                                                                                06:46:57 PM
~ ❯ echo "linux" > unix.txt 

~ ❯ git add linux.txt
~ ❯ git add unix.txt
```

When you add unix.txt, git first check the checksum value of the contents of unix.txt and it will be same as of linux.txt since the contents are same for both files. Git then check if a file exists with the same object id. The index file contains file with the same object id which means we have a file with the same contents of unix.txt. Git will not create another object however change the index file to include uinx.txt with the same object id of linux.txt.

Now create another file. 

``sh
~ ❯ echo "Linux" > arch.txt
~ ❯ git add arch.txt
```

Git first create checksum value of arch.txt with the contents inside it. Check if an object with the same contents exists by using index page. So Git create a new object and keep arch.txt inside it and keep a refernce of it in the index page.

## git init
A local git repository is created when we initialuze git.
```sh
~/git-sample master ❯ tree .git                                                                                                                             08:11:55 PM
.git ---> Local git repository
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

9 directories, 16 files
```

```sh
~/git-sample master ❯ git config user.name "Vyshnavlal"                                                                                                     08:12:02 PM
~/git-sample master ❯ git config user.email "vyshnavlal6367@gmail.com"  
```

The given credentials are stored in the ".git/config' file
```sh
~/git-sample master ❯ cat .git/config                                                                                                                       08:13:57 PM
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[user]
	name = Vyshnavlal
	email = vyshnavlal6367@gmail.com
```

Now I've created a file.
```sh
~/git-sample master ❯ echo "Linux is awsome" > linux.txt                                                                                                    08:14:49 PM
~/git-sample master ?1 ❯ git status                                                                                                                         08:15:59 PM
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	linux.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Untracked file means there is no copy for the file in git local repository or there is no object created for the file.

I've added the file to git. Now check the directory strcture before and after of git add
```sh
Before

~/git-sample master ?1 ❯ tree .git                                                                                                                          08:18:08 PM
.git
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

After

~/git-sample master +1 ❯ tree .git                                                                                                                          08:18:19 PM
.git
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── objects
│   ├── 9c
│   │   └── cc60f784c480237e7111df592197ada5f5ba2d
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```
An object is created under the "objects" directory and within "9c" folder and having a file inside it called "cc60f784c480237e7111df592197ada5f5ba2d"

Linux is awsome ---> 9ccc60f784c480237e7111df592197ada5f5ba2d (CheckSum value)

So why the filename doesn't contain full checksum value? There are limitations for files to include in a directory. To overcome this, git first create a folder with first two letter of checksum value and create a folder inside it with using remaining letters in checksum.

## How to view the contents of index/staging file ?
index is not a regular text file. You need to use the command "git ls-files -s"

```sh
~/git-sample master +1 ❯ git ls-files -s                                                                                                                    08:26:42 PM
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	linux.txt
```
The index file contains the object id as well as the file name.

## How to view the contents inside of an object?
```sh
~/git-sample master +1 ❯ git cat-file -p 9ccc60f784c480237e7111df592197ada5f5ba2d                                                                           08:26:52 PM
Linux is awsome
```

Use "git cat-file" command followed by the object id to view the contents inside the object.

Now we have created a called unix.txt with the same contents of linux.txt. There will be no object created for it because both contais same data inside. However there will mapping of file name with object id inside the index file.
```sh
~/git-sample master +2 ❯ git ls-files -s                                                                                                                    08:31:30 PM
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	linux.txt
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	unix.txt
```

Now we have created another file called arch.txt with the some diferrent contents. This time a new object will be created. See below.
```sh
~/git-sample master +2 ❯ echo "Arch is cool" > arch.txt                                                                                                     08:31:36 PM
~/git-sample master +2 ?1 ❯ git add arch.txt                                                                                                                08:35:10 PM

~/git-sample master +3 ❯ tree .git                                                                                                                          08:35:16 PM
.git
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── objects
│   ├── 9c
│   │   └── cc60f784c480237e7111df592197ada5f5ba2d
│   ├── d6
│   │   └── 1b84f8daa3bb91b3a478ab061223410dc3b681
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

11 directories, 19 files

~/git-sample master +3 ❯ git ls-files -s                                                                                                                    08:35:19 PM
100644 d61b84f8daa3bb91b3a478ab061223410dc3b681 0	arch.txt
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	linux.txt
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	unix.txt
```
