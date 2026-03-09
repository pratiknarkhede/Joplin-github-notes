# Implementing Custom Iterators in Java

I'll guide you through implementing custom iterators in Java with a simple, interview-friendly example that's easy to understand and remember.

## What is an Iterator?

An iterator is a design pattern that provides a way to access elements of a collection sequentially without exposing the underlying representation. In Java, it's formalized through the `Iterator` interface in the `java.util` package.

## Why Use Custom Iterators?

1.  To provide controlled access to collection elements
2.  To implement specialized traversal behaviors
3.  To hide the internal structure of your collection
4.  To support the enhanced for-loop (`for-each`) syntax

## Step-by-Step Implementation Guide

Let's create a simple `BookShelf` collection that stores books and allows iteration through them:

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

// 1. Create your collection class
public class BookShelf implements Iterable<Book> {
    private Book[] books;
    private int numberOfBooks;
    
    public BookShelf(int capacity) {
        books = new Book[capacity];
        numberOfBooks = 0;
    }
    
    public void addBook(Book book) {
        if (numberOfBooks < books.length) {
            books[numberOfBooks] = book;
            numberOfBooks++;
        }
    }
    
    // 2. Implement the Iterable interface
    @Override
    public Iterator<Book> iterator() {
        // 3. Return a new instance of your custom iterator
        return new BookShelfIterator();
    }
    
    // 4. Create your custom iterator as an inner class
    private class BookShelfIterator implements Iterator<Book> {
        private int currentIndex = 0;
        
        // 5. Implement hasNext() method
        @Override
        public boolean hasNext() {
            return currentIndex < numberOfBooks;
        }
        
        // 6. Implement next() method
        @Override
        public Book next() {
            if (!hasNext()) {
                throw new NoSuchElementException("No more books on the shelf");
            }
            return books[currentIndex++];
        }
    }
}

// Simple Book class
class Book {
    private String title;
    private String author;
    
    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }
    
    @Override
    public String toString() {
        return "Book{title='" + title + "', author='" + author + "'}";
    }
}
```

## Using Your Custom Iterator

```java
public class Main {
    public static void main(String[] args) {
        // Create a bookshelf with capacity 3
        BookShelf bookShelf = new BookShelf(3);
        
        // Add books to the shelf
        bookShelf.addBook(new Book("Clean Code", "Robert Martin"));
        bookShelf.addBook(new Book("Effective Java", "Joshua Bloch"));
        bookShelf.addBook(new Book("Design Patterns", "Gang of Four"));
        
        // Method 1: Using iterator explicitly
        System.out.println("Using explicit iterator:");
        Iterator<Book> iterator = bookShelf.iterator();
        while (iterator.hasNext()) {
            Book book = iterator.next();
            System.out.println(book);
        }
        
        // Method 2: Using enhanced for-loop (uses iterator behind the scenes)
        System.out.println("\nUsing enhanced for-loop:");
        for (Book book : bookShelf) {
            System.out.println(book);
        }
    }
}
```

## Key Components Explained

1.  **Iterable Interface**: Your collection class must implement `Iterable<T>` to support iteration
    
    - This requires implementing the `iterator()` method that returns an `Iterator<T>`
2.  **Iterator Interface**: Your custom iterator class must implement `Iterator<T>` with two methods:
    
    - `boolean hasNext()`: Returns `true` if there are more elements
    - `T next()`: Returns the next element and advances the cursor position
3.  **Inner vs. Standalone Class**: The iterator is typically implemented as an inner class because:
    
    - It needs access to the collection's private fields
    - It's conceptually tied to the collection it iterates over

## Common Implementation Patterns

1.  **Inner Class Iterator** (as shown above)
    
    - Simplest approach, with direct access to private fields
2.  **Anonymous Inner Class Iterator**
    
    ```java
    @Override
    public Iterator<Book> iterator() {
        return new Iterator<Book>() {
            private int currentIndex = 0;
            
            @Override
            public boolean hasNext() {
                return currentIndex < numberOfBooks;
            }
            
            @Override
            public Book next() {
                if (!hasNext()) {
                    throw new NoSuchElementException();
                }
                return books[currentIndex++];
            }
        };
    }
    ```
    
3.  **External Iterator Class** (when iterator logic is complex)
    
    - Create a separate class that takes the collection in its constructor

## How Java Uses Iterators

1.  **Collections Framework**: All Java collections implement `Iterable<T>`
    
    - ArrayList, LinkedList, HashSet, TreeSet, etc.
2.  **Enhanced For Loop**: When you use a for-each loop:
    
    ```java
    for (Book book : bookShelf) { ... }
    ```
    
    Java translates this to:
    
    ```java
    for (Iterator<Book> it = bookShelf.iterator(); it.hasNext(); ) {
        Book book = it.next();
        ...
    }
    ```
    
3.  **Stream API**: Modern Java provides streams that use iterators behind the scenes:
    
    ```java
    bookShelf.stream().filter(book -> book.getAuthor().equals("Joshua Bloch")).forEach(System.out::println);
    ```
    

## Interview Tips

1.  **Remember the Key Methods**:
    
    - `iterator()` in your collection class (implements `Iterable`)
    - `hasNext()` and `next()` in your iterator class (implements `Iterator`)
2.  **Know When to Use Custom Iterators**:
    
    - When you need special traversal logic
    - When implementing your own collection types
    - When you want to provide multiple ways to iterate
3.  **Be Aware of Thread-Safety Issues**:
    
    - Standard iterators are not thread-safe
    - They may throw `ConcurrentModificationException` if collection changes during iteration
4.  **Optional remove() Method**:
    
    - Iterator also has a `remove()` method that removes the last element returned by `next()`
    - By default, it throws `UnsupportedOperationException`
    - You can override it if you want to support removal during iteration

## Complete Template for Custom Iterator

1.  Make your collection implement `Iterable<T>`
2.  Implement the `iterator()` method to return a custom iterator
3.  Create an iterator class that implements `Iterator<T>`
4.  Implement `hasNext()` and `next()` methods
5.  Optionally implement `remove()` if needed

This example with a bookshelf is intuitive, easy to remember, and demonstrates all the key components of custom iterators in Java.