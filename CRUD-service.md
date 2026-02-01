# Angular service (CRUD) fx med Book

```
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

export interface Book {
  id?: number;
  title: string;
  author: string;
}

@Injectable({
  providedIn: 'root'
})
export class BookService {
  private apiUrl = 'http://localhost:5000/api/books';

  constructor(private http: HttpClient) {}

  getAll(): Observable<Book[]> {
    return this.http.get<Book[]>(this.apiUrl);
  }

  getById(id: number): Observable<Book> {
    return this.http.get<Book>(`${this.apiUrl}/${id}`);
  }

  create(book: Book): Observable<Book> {
    return this.http.post<Book>(this.apiUrl, book);
  }

  update(id: number, book: Book): Observable<Book> {
    return this.http.put<Book>(`${this.apiUrl}/${id}`, book);
  }

  delete(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`);
  }
}

```

## Liste af bøger:
```
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { BookService, Book } from './book.service';

@Component({
  selector: 'app-book-list',
  standalone: true,
  imports: [CommonModule],
  template: `
    <h2>Books</h2>
    <ul>
      <li *ngFor="let book of books">
        {{ book.title }} by {{ book.author }}
        <button (click)="deleteBook(book.id!)">Delete</button>
      </li>
    </ul>
  `
})
export class BookListComponent implements OnInit {
  books: Book[] = [];

  constructor(private bookService: BookService) {}

  ngOnInit() {
    this.loadBooks();
  }

  loadBooks() {
    this.bookService.getAll().subscribe(data => this.books = data);
  }

  deleteBook(id: number) {
    this.bookService.delete(id).subscribe(() => this.loadBooks());
  }
}


```

##Oprettelse af bøger
```
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { BookService, Book } from './book.service';

@Component({
  selector: 'app-book-create',
  standalone: true,
  imports: [CommonModule, FormsModule],
  template: `
    <h2>Add Book</h2>
    <form (ngSubmit)="addBook()">
      <input [(ngModel)]="book.title" name="title" placeholder="Title" required />
      <input [(ngModel)]="book.author" name="author" placeholder="Author" required />
      <button type="submit">Add</button>
    </form>
  `
})
export class BookCreateComponent {
  book: Book = { title: '', author: '' };

  constructor(private bookService: BookService) {}

  addBook() {
    this.bookService.create(this.book).subscribe(() => {
      this.book = { title: '', author: '' }; // reset form
      alert('Book added!');
    });
  }
}

```


