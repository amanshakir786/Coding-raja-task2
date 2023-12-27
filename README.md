# Coding-raja-task2 
LIBRARY MANAGEMENT SYSTEM



import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

class Book {
    private String title;
    private String author;
    private String genre;
    private boolean isAvailable;

    public Book(String title, String author, String genre) {
        this.title = title;
        this.author = author;
        this.genre = genre;
        this.isAvailable = true;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public String getGenre() {
        return genre;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }
}

class Patron {
    private String name;
    private String contactInfo;
    private List<Book> borrowedBooks;

    public Patron(String name, String contactInfo) {
        this.name = name;
        this.contactInfo = contactInfo;
        this.borrowedBooks = new ArrayList<>();
    }

    public String getName() {
        return name;
    }

    public String getContactInfo() {
        return contactInfo;
    }

    public List<Book> getBorrowedBooks() {
        return borrowedBooks;
    }

    public void borrowBook(Book book) {
        borrowedBooks.add(book);
        book.setAvailable(false);
    }

    public void returnBook(Book book) {
        borrowedBooks.remove(book);
        book.setAvailable(true);
    }
}

class Library {
    private Map<String, Book> books;
    private Map<String, Patron> patrons;

    public Library() {
        this.books = new HashMap<>();
        this.patrons = new HashMap<>();
    }

    public void addBook(String title, String author, String genre) {
        Book newBook = new Book(title, author, genre);
        books.put(title, newBook);
    }

    public void addPatron(String name, String contactInfo) {
        Patron newPatron = new Patron(name, contactInfo);
        patrons.put(name, newPatron);
    }

    public Book findBook(String title) {
        return books.get(title);
    }

    public Patron findPatron(String name) {
        return patrons.get(name);
    }

    public void generateReport() {
        System.out.println("Books in the Library:");
        for (Book book : books.values()) {
            System.out.println(book.getTitle() + " by " + book.getAuthor() +
                    " (Genre: " + book.getGenre() + ", Available: " + book.isAvailable() + ")");
        }

        System.out.println("\nPatron Records:");
        for (Patron patron : patrons.values()) {
            System.out.println(patron.getName() + " - Contact: " + patron.getContactInfo() +
                    ", Borrowed Books: " + patron.getBorrowedBooks().size());
        }
    }
}

public class LibraryManagementSystem {
    public static void main(String[] args) {
        Library library = new Library();
        library.addBook("The Great Gatsby", "F. Scott Fitzgerald", "Fiction");
        library.addBook("To Kill a Mockingbird", "Harper Lee", "Classics");
        library.addPatron("John Doe", "johndoe@email.com");

        Scanner scanner = new Scanner(System.in);

        System.out.println("Welcome to the Library Management System!");

        // Book Borrowing
        Book book = library.findBook("The Great Gatsby");
        Patron patron = library.findPatron("John Doe");

        if (book != null && patron != null) {
            patron.borrowBook(book);
            System.out.println(patron.getName() + " borrowed " + book.getTitle());

            // Book Returning
            patron.returnBook(book);
            System.out.println(patron.getName() + " returned " + book.getTitle());

            // Generate Report
            library.generateReport();
        } else {
            System.out.println("Book or patron not found.");
        }

        scanner.close();
    }
}
