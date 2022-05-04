## Introduction
Git is a tool for building copies of files. You're probably copying files with the 'cp' command on Linux, right? So, what's the difference between using the 'cp' command and the 'git' command? Before we can solve it, we must first understand why we desire a copy of the files or what the demand is.

Assume you're building a large piece of code for an app. You're a pro and can crack the code in a single stretch sit. But what if you want to test each section of code separately? We can achieve this by using the staging mechanism, which also allows us to roll back to a previous version.

We won't be able to achieve this with conventional copying because it will overwrite our existing file with new changes. Here comes our 'git'. Git copying is more efficient than copying using cp command.

## CheckSum value/Hash Value
In order to grasp how git works correctly, we must first understand checksum values. When we need to ensure that the source file arrived at its destination properly, we use the hash function.

>File Input ---> Hash function ---> CheckSum value

```sh
$ echo "I'm Linux" > linux.txt
$ md5sum linux.txt
5047ee8de8abe2e555b8a42b26a85cc3  linux.txt
```
The contents of the file are passed to the hash function as input. It could be any type of data or file. The hash function generates a string from the file's data. It can contain both alphabets and numbers. The string should have a predetermined length. i.e., the string length created from a 1KB file and a 50MB file should be the same. The string was formed by the hash function from the contents of the file rather than the filename, thus if the contents are the same, the checksome value should be the same.

```sh
~ ❯ echo "linux" > linux.txt                                                                                                                             
~ ❯ echo "linux" > unix.txt                                                                                                                               
~ ❯ echo "Linux" > arch.txt 

~ ❯ md5sum linux.txt                                                                                                                                     
5bb062356cddb5d2c0ef41eb2660cb06  linux.txt
~ ❯ md5sum unix.txt                                                                                                                                       
5bb062356cddb5d2c0ef41eb2660cb06  unix.txt
~ ❯ md5sum arch.txt                                                                                                                                       
1b61f2a016f7478478fcb13130fcec7b  arch.txt
```
I've created three files in this folder. The data in linux.txt and unix.txt is the same, but arch.txt is different since we use the capital 'L' in "Linux." The checksum value was then validated using a md5 function. We can observe that the checksum values of linux.txt and unix.txt are the same, however arch.txt has a different checksum value.

## Working Directory
Working Directory refers to the directory in which we copy files when using Git. To create a working directory, run the following command in the desired directory.

```sh
$ git init
```
When you run git for the first time, it will create a folder named ".git," which is also known as the "Local git repository." Git saves the files we copy to the local git repository.

## git add
To store a copy of a file in the working directory to the git local repository, use the "git add" command. Git creates an object when you run the git add command. So, what exactly is an Object? Git first reads the file's contents and generates a checksum value using hash function. The file's contents are contained in the object. The object should have an object id in order to be identified, and the object id will be the checksum value of the file's content.

## Index/Staging file
The staging file or index file is used to map the file name and object id. The index file contains a list of the files' objects. Let's start by making two files.

```sh
~ ❯ echo "linux" > linux.txt                                                                                                                             
~ ❯ echo "linux" > unix.txt 

~ ❯ git add linux.txt
~ ❯ git add unix.txt
```
When you add unix.txt, git first examines the checksum value of the contents of unix.txt, which will be the same as the checksum value of linux.txt because the contents of both files are the same. Git then checks to see if a file with the same object id already exists. The index file has a file with the same object id as unix.txt, indicating that we have a file with the same contents as linux.txt. Git will not create a new object, but it will update the index file to include uinx.txt with the same object id as linux.txt. Now create another file. 

```sh
~ ❯ echo "Linux" > arch.txt
~ ❯ git add arch.txt
```
When you execute the git add command. Git initially generates a checksum value for the contents of arch.txt. Using the index page, see if an item with the same contents exists. So, in Git, build a new object and place arch.txt within it, with a reference to it on the index page.

## git init
When we run git for the first time, it creates a local git repository.
```sh
~/git-sample master ❯ tree .git                                                                                                                           
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
Configuring git
```sh
~/git-sample master ❯ git config user.name "Vyshnavlal"                                                                                                   
~/git-sample master ❯ git config user.email "vyshnavlal6367@gmail.com"  
```
The credentials provided are saved in the ".git/config" file.
```sh
~/git-sample master ❯ cat .git/config                                                                                                                     
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[user]
	name = Vyshnavlal
	email = vyshnavlal6367@gmail.com
```
I've now created a file called linux.txt.
```sh
~/git-sample master ❯ echo "Linux is awsome" > linux.txt                                                                                                 
~/git-sample master ?1 ❯ git status                                                                                                                       
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	linux.txt

nothing added to commit but untracked files present (use "git add" to track)
```
Here "Untracked file" indicates that there is no copy of the file in the git local repository or that no object has been generated for the file. I've added the file to git. Now compare the directory structure before and after git add.
```sh
Before

~/git-sample master ?1 ❯ tree .git                                                                                                                       
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

~/git-sample master +1 ❯ tree .git                                                                                                                       
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
An object is generated in the "objects" directory, within the "9c" folder, with the file "cc60f784c480237e7111df592197ada5f5ba2d" inside it.

>Linux is awsome ---> 9ccc60f784c480237e7111df592197ada5f5ba2d (CheckSum value)

So, why isn't the complete checksum value included in the filename? There are restrictions on number of files can be included in a directory. To overcome this, git creates a folder with the first two letters of the checksum value and then creates a folder within it with the remaining letters in the checksum.

## How to view the contents of index/staging file ?
The index file is not a typical text file. You must run the command "git ls-files -s". The index file contains both the object id and the file name.

```sh
~/git-sample master +1 ❯ git ls-files -s                                                                                                                 
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	linux.txt
```
## How to view the contents inside of an object?
```sh
~/git-sample master +1 ❯ git cat-file -p 9ccc60f784c480237e7111df592197ada5f5ba2d                                                                         
Linux is awsome
```
To view the contents of an object, use the "git cat-file" command followed by the object id.

We've now created a file called unix.txt with the identical contents as linux.txt. Because both have the same data, no object will be generated for it. However, inside the index file, there will be a mapping of file name to object id.

```sh
~/git-sample master +2 ❯ git ls-files -s                                                                                                                 
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	linux.txt
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	unix.txt
```
We've now created a new file called arch.txt with somewhat different contents. A new object will be created this time. See the below.
```sh
~/git-sample master +2 ❯ echo "Arch is cool" > arch.txt                                                                                                   
~/git-sample master +2 ?1 ❯ git add arch.txt                                                                                                             

~/git-sample master +3 ❯ tree .git                                                                                                                       
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

~/git-sample master +3 ❯ git ls-files -s                                                                                                                 
100644 d61b84f8daa3bb91b3a478ab061223410dc3b681 0	arch.txt
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	linux.txt
100644 9ccc60f784c480237e7111df592197ada5f5ba2d 0	unix.txt
```
