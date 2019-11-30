---
title: In-depth Git
date: 2019-11-30 20:45:41
tags:
---

## What is git?

Git is a distributed version control system. However, in very basic terms, it is just a key value storage.

## How git stores data?

At its core, Git is a simple key-value store.

- The Value is data
- The Key is hash of that data

Git uses the key to retrieve the content

The key is SHA1 which is a cryptographic hash function that produces 40 digit hexadecimal number. Given a peace of data, the output is always the same (typical of any hash function).

## Git objects

### Blobs

Git stores compressed data in a **blob** along with metadata in header:

- The identifier: **blob**
- The size of the content
- \0 delimiter (string terminator in c)
- The content

An example of blob data is:

<img src="blob.png" alt="git blob object" width="250px"/>

Git generates the hash using `hash-object` command, if we generate a hash of string `hello` and also use `openssl sha1` to generate the hash of the same data, the hashes will be the same:

```bash
$ echo -n hello | git hash-object --stdin
b6fc4c620b67d95f953a5c1c1230aaab5db5a1b0

$ printf 'blob 5\0hello' > test.txt
$ openssl sha1 test.txt
SHA1(test.txt)= b6fc4c620b67d95f953a5c1c1230aaab5db5a1b0
```

As you see the hashes are the same. Blobs are generally unique in a git project. Git stores blobs in `.git/objects` directory. If you want to see it in action then add `-w` flag to previous command, and look at `.git/objects` directory:

```bash
$ echo -n hello | git hash-object -w --stdin
```

### Trees

Blobs are just for storing contents of file. But we need to store directory structure, filename etc. as well.
The other kind of data structure that Git uses is called **Tree**.

_A tree contains pointers to blobs and to other trees along with the metadata._

Metadata contains:

- Type of the pointer (**blob** or **tree**)
- **fielname** or directory name
- **mode** (executable file, symbolic link, ...)

A typical tree would look something like this:

<img src="tree.png" alt="git tree object" width="500px"/>

Because given content the hash is always going to be the same, in Git, identical content will be stored only once. Example of that can be demonstrated by storing copy of a file in other directory.

<img src="unique-content.png" alt="git unique hash of content" width="500px"/>

When we work with git, out files mostly stays similar and we change some lines of files (e.g. add methods, write comments etc.). Because of that, storing same with small changes file over and over would be inefficient. Git knows that and stores such objects in something called a "packfile". Packfile contains an object (compressed) and deltas (difference between versions). Those packfiles are generated when you have too many objects, when you push into a remote or manually run Git's garbage collector. Learn more about packfiles [here](https://git-scm.com/book/en/v2/Git-Internals-Packfiles).

### Commit object

A commit is an object that points to a **tree** and contains some metadata:

- **author and committer**
- **date**
- **message**
- **parent commit** (can have multiple parents)

And commit id is SHA1 hash of all of this information. So, a typical commit would look something like this:

<img src="commit.png" alt="a git commit object" width="250px">

Commits are just snaphosts of the repository at given times.

<img src="commits.png" alt="a git commit and pointers to trees" width="500px">

Since commits are just another kind of objects, they're also stored in `.git/objects` directory. But if you want to take a look at their content you won't find anything there. Git has a command, called `cat-file` to inspect type and contents of an object. The command looks like this.

```bash
$ git cat-file (-t | -p) <sha1>
```

Here, `-t` flat will give you type of the object, and `-p` will print contents of the object. And as you thought, `<sha1>` is SHA1 hash of the object.

For checking this, go ahead and look at your objects and try to see what's inside those objects. I took an arbitrary object from my `.git/objects` directory and inspected some of them. And I should also mention that you don't need to write the whole 40 digits of the hash, just first 6 characters are enough.

<img src="cat-file-results.png" alt="git cat-file results">
