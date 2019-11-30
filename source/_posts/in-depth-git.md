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

## Blobs and trees

### Blobs

Git stores compressed data in a **blob** along with metadata in header:

- The identifier: **blob**
- The size of the content
- \0 delimiter (string terminator in c)
- The content

An example of blob data is:

{% asset_img blob.png git-blob-object %}

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

{% asset_img tree.png git-tree-object %}

Because given content the hash is always going to be the same, in Git, identical content will be stored only once. Example of that can be demonstrated by storing copy of a file in other directory.

{% asset_img unique-content.png git-unique-content-hashes %}
