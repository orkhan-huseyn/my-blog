---
title: Java's try with resources
date: 2020-03-26 10:01:54
tags:
---

## Introduction

If you've written some Java, you must be familiar with Java's streams. These streams include `InputStream` and `OutputStream` for reading and writing binary data and `Reader`, `Writer` classes for reading and writing character streams. We will look at the main points of Java streams, which are handling exceptions and closing them.

## Managing streams

The streams mentioned above are likely to fail and when they to they throw an `IOException`. So, a sample code for reading from readable stream would be like this:

```java
        InputStream inputStream = // some input stream
        try {
            int intVal;
            while ((intVal = inputStream.read()) >= 0) {
                byte byteVal = (byte) intVal;
                // do something with the byte data
            }
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }

```

All readable streams include `int read()` and `int read(byte[] buff)` methods. As the name suggests they help us to read from the stream and return bytes that are read from. If returned values is less than 0 (zero) then the stream is ended. Since the `InputStream` is for reading binary data, we also cast the integer value to `byte`.

Now, the above code is somehow complete, but ignores an important point: the stream is not closed after reading from it. We can close it in try-catch's `finally` block. However, since the closing the stream also contains some checking and may throw an `IOException`, the code will look something like this:

```java
        InputStream inputStream = // some input stream
        try {
            int intVal;
            while ((intVal = inputStream.read()) >= 0) {
                byte byteVal = (byte) intVal;
                // do something with the byte data
            }
        } catch (IOException e) {
            System.out.println(e.getMessage());
        } finally {
            // closing stream may throw an IOException
            try {
                // we need to null check
                // before closing the stream
                if (inputStream != null)
                    inputStream.close();
            } catch (IOException e) {
                System.out.println(e.getMessage());
            }
        }
```

This is a lot of code, isn't it? :) To simplify such scenarios we use Java's try with resources.

## The AutoClosable interface

The reason why streams expose a `void close() throws IOException` method, because they implement an interface called `Closable` interface. `Closable` interface extends an interface named `AutoClosable` interface which gives them `void close() throws Exception` method and makes them close automatically when used inside a try-with resorces block. Now, what's try-with resources? I believe, you may have seen them. We can modify the above code like so:

```java
        // resource is automatically closed because
        // it implements AutoClosable interface
        try (InputStream inputStream = null) {
            int intVal;
            while ((intVal = inputStream.read()) >= 0) {
                byte byteVal = (byte) intVal;
                // do something with the byte data
            }
        } catch (IOException e) {
            System.out.println(e.getMessage());
        }
```

The parentheses after `try` contains a resource and the resource is being closed automatically after the code block finishes executing. The good news is you don't have to write that messy code we write earlier.

To prove that the resource is always being closed, let's do some little experiment. Let's create out own class that implements `AutoClosable` interface and use it inside a try-with resources block.

```java
class ExampleStream implements AutoCloseable {

    public void doSomething() throws Exception {
        System.out.println("I am doing something...");
    }

    @Override
    public void close() throws Exception {
        System.out.println("I am being closed.");
    }
}
```

This is our AutoClosable, let's use it:

```java
        try (ExampleStream exampleStream = new ExampleStream()) {
            exampleStream.doSomething();
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
```

If you run this example on your own, you will see it printing `I am doing something...` and then `I am being closed.` like so. This proves that try-with resources calls `void close() throws Exception` method of the resource. There are lots of real life use cases but to show one, let's read a file using `BufferedReader` class:

```java
        try (BufferedReader reader = Files.newBufferedReader(Paths.get("data.txt"))) {
            int intVal;
            while ((intVal = reader.read()) >= 0) {
                char ch = (char) intVal;
                System.out.print(ch);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
```

`BufferedReader` extends abstract `Reader` class, which implements `Closable` (remember: `Closable` interface extends `AutoClosable` interface). So, the read stream is being closed after the code finishes executing :)

## Conclusion

Thanks for reading!

If you find any mistakes in any of my articles, please do not hesitate to fix them and send me a PR.

https://github.com/orkhan-huseyn/my-blog

This article contains some concepts discussed in [Java Fundamentals: The Core Platform](https://app.pluralsight.com/library/courses/java-fundamentals-core-platform/table-of-contents) course in Pluralsight.
