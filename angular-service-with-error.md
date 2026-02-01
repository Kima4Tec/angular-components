# Anuglar service med error handling
```
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, tap } from 'rxjs/operators';

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

  private handleError(error: HttpErrorResponse) {
    let message = 'Unknown error';
    if (error.error instanceof ErrorEvent) {
      // Client-side / network error
      message = `Network error: ${error.error.message}`;
    } else {
      // Backend error
      message = `Error ${error.status}: ${error.error?.message || error.message}`;
    }
    console.error(message);
    return throwError(() => new Error(message));
  }

  getAll(): Observable<Book[]> {
    return this.http.get<Book[]>(this.apiUrl)
      .pipe(catchError(this.handleError));
  }

  getById(id: number): Observable<Book> {
    return this.http.get<Book>(`${this.apiUrl}/${id}`)
      .pipe(catchError(this.handleError));
  }

  create(book: Book): Observable<Book> {
    return this.http.post<Book>(this.apiUrl, book)
      .pipe(
        tap(() => console.log(`Book created: ${book.title}`)),
        catchError(this.handleError)
      );
  }

  update(id: number, book: Book): Observable<Book> {
    return this.http.put<Book>(`${this.apiUrl}/${id}`, book)
      .pipe(catchError(this.handleError));
  }

  delete(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`)
      .pipe(catchError(this.handleError));
  }
}

```

## Liste af bøger
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
    
    <div *ngIf="loading">Loading...</div>
    
    <ul *ngIf="!loading">
      <li *ngFor="let book of books">
        {{ book.title }} by {{ book.author }}
        <button (click)="deleteBook(book.id!)">Delete</button>
      </li>
    </ul>

    <div *ngIf="error" class="error">{{ error }}</div>
  `,
  styles: [`
    .error { color: red; margin-top: 1rem; }
  `]
})
export class BookListComponent implements OnInit {
  books: Book[] = [];
  loading = false;
  error: string | null = null;

  constructor(private bookService: BookService) {}

  ngOnInit() {
    this.loadBooks();
  }

  loadBooks() {
    this.loading = true;
    this.error = null;

    this.bookService.getAll().subscribe({
      next: data => {
        this.books = data;
        this.loading = false;
      },
      error: err => {
        this.error = err.message;
        this.loading = false;
      }
    });
  }

  deleteBook(id: number) {
    if (!confirm('Are you sure?')) return;

    this.bookService.delete(id).subscribe({
      next: () => {
        alert('Book deleted!');
        this.loadBooks();
      },
      error: err => alert('Delete failed: ' + err.message)
    });
  }
}

```
## Opret bøger
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
    
    <form (ngSubmit)="addBook()" *ngIf="!loading">
      <input [(ngModel)]="book.title" name="title" placeholder="Title" required />
      <input [(ngModel)]="book.author" name="author" placeholder="Author" required />
      <button type="submit">Add</button>
    </form>

    <div *ngIf="loading">Saving...</div>
    <div *ngIf="error" class="error">{{ error }}</div>
  `,
  styles: [`.error { color: red; margin-top: 1rem; }`]
})
export class BookCreateComponent {
  book: Book = { title: '', author: '' };
  loading = false;
  error: string | null = null;

  constructor(private bookService: BookService) {}

  addBook() {
    this.loading = true;
    this.error = null;

    this.bookService.create(this.book).subscribe({
      next: () => {
        alert('Book added!');
        this.book = { title: '', author: '' };
        this.loading = false;
      },
      error: err => {
        this.error = err.message;
        this.loading = false;
      }
    });
  }
}

```
