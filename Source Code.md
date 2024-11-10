#include <stdlib.h>
#include <string.h>
#define MAX_BOOKS 100
#define FILE_NAME "library.txt"
typedef struct {
 char title[100];
 char author[100];
 char isbn[20];
} Book;
void addBook();
void viewBooks();
void searchBook();
void deleteBook();
void saveBooks(Book *books, int count);
int main() {
 int choice;
 do {
 printf("\nLibrary Management System\n");
 printf("1. Add Book\n");
 printf("2. View Books\n"
 printf("3. Search Book\n");
 printf("4. Delete Book\n");
 printf("5. Exit\n");
 printf("Enter your choice: ");
 scanf("%d", &choice);
 switch (choice) {
 case 1: addBook(); break;
 case 2: viewBooks(); break;
 case 3: searchBook(); break;
 case 4: deleteBook(); break;
 case 5: printf("Exiting...\n"); break;
 default: printf("Invalid choice! Please try again.\n");
 }
 } while (choice != 5);
 return 0;
}
void addBook() {
 Book book;
 FILE *file = fopen(FILE_NAME, "a");
 if (!file) {
 printf("Could not open file for writing.\n");
 return;
 }
 printf("Enter book title: ");
 scanf(" %[^\n]", book.title);
 printf("Enter author: ");
 scanf(" %[^\n]", book.author);
 printf("Enter ISBN: ");
 scanf(" %[^\n]", book.isbn);
 fwrite(&book, sizeof(Book), 1, file);
 fclose(file);
 printf("Book added successfully!\n");
}
void viewBooks() {
 Book book;
 FILE *file = fopen(FILE_NAME, "r");
 if (!file) {
 printf("Could not open file for reading.\n");
 return;
 }
 printf("\nBooks in Library:\n");
 while (fread(&book, sizeof(Book), 1, file)) {
 printf("Title: %s, Author: %s, ISBN: %s\n", book.title, book.author,
book.isbn);
 }
 fclose(file);
}
void searchBook() {
char search[100];
 Book book;
 FILE *file = fopen(FILE_NAME, "r");
 if (!file) {
 printf("Could not open file for reading.\n");
 return;
 }
 printf("Enter title or author to search: ");
 scanf(" %[^\n]", search);
 int found = 0;
 while (fread(&book, sizeof(Book), 1, file)) {
 if (strstr(book.title, search) || strstr(book.author, search)) {
 printf("Found: Title: %s, Author: %s, ISBN: %s\n", book.title, book.author,
book.isbn);
 found = 1;
 }
 }
 if (!found) {
 printf("No books found matching the search.\n");
 }
 fclose(file);
}
void deleteBook() {
 char isbn[20];
 Book book;
 FILE *file = fopen(FILE_NAME, "r");
 FILE *tempFile = fopen("temp.txt", "w");
 if (!file || !tempFile) {
 printf("Could not open files for reading/writing.\n");
 return;
 }
 printf("Enter ISBN of the book to delete: ");
 scanf(" %[^\n]", isbn);
 int found = 0;
 while (fread(&book, sizeof(Book), 1, file)) {
 if (strcmp(book.isbn, isbn) != 0) {
 fwrite(&book, sizeof(Book), 1, tempFile);
 } else {
 found = 1;
 }
 }
 fclose(file);
 fclose(tempFile);
 remove(FILE_NAME);
 rename("temp.txt", FILE_NAME);
 if (found) {
 printf("Book deleted successfully!\n");
 } else {
 printf("No book found with the given ISBN.\n");
 }
}
