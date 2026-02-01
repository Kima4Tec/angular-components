# Generisk CRUD service for Angular

```
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, tap } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class CrudService<T extends { id?: number }> {

  constructor(private http: HttpClient) {}

  protected handleError(error: HttpErrorResponse) {
    let message = 'Unknown error';
    if (error.error instanceof ErrorEvent) {
      message = `Network error: ${error.error.message}`;
    } else {
      message = `Error ${error.status}: ${error.error?.message || error.message}`;
    }
    console.error(message);
    return throwError(() => new Error(message));
  }

  // generisk get all
  getAll(url: string): Observable<T[]> {
    return this.http.get<T[]>(url).pipe(catchError(this.handleError));
  }

  // generisk get by id
  getById(url: string, id: number): Observable<T> {
    return this.http.get<T>(`${url}/${id}`).pipe(catchError(this.handleError));
  }

  // generisk create
  create(url: string, entity: T): Observable<T> {
    return this.http.post<T>(url, entity).pipe(
      tap(() => console.log(`Created entity`)),
      catchError(this.handleError)
    );
  }

  // generisk update
  update(url: string, id: number, entity: T): Observable<T> {
    return this.http.put<T>(`${url}/${id}`, entity).pipe(catchError(this.handleError));
  }

  // generisk delete
  delete(url: string, id: number): Observable<void> {
    return this.http.delete<void>(`${url}/${id}`).pipe(catchError(this.handleError));
  }
}

```
## Specifik service med fx book
```
import { Injectable } from '@angular/core';
import { CrudService } from './crud.service';

export interface Book {
  id?: number;
  title: string;
  author: string;
}

@Injectable({
  providedIn: 'root'
})
export class BookService extends CrudService<Book> {
  private apiUrl = 'http://localhost:5000/api/books';

  constructor(http: HttpClient) {
    super(http);
  }

  getAllBooks() {
    return this.getAll(this.apiUrl);
  }

  getBookById(id: number) {
    return this.getById(this.apiUrl, id);
  }

  createBook(book: Book) {
    return this.create(this.apiUrl, book);
  }

  updateBook(id: number, book: Book) {
    return this.update(this.apiUrl, id, book);
  }

  deleteBook(id: number) {
    return this.delete(this.apiUrl, id);
  }
}

```
## Brug i component
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
  styles: [`.error { color: red; margin-top: 1rem; }`]
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
    this.bookService.getAllBooks().subscribe({
      next: data => { this.books = data; this.loading = false; },
      error: err => { this.error = err.message; this.loading = false; }
    });
  }

  deleteBook(id: number) {
    if (!confirm('Are you sure?')) return;
    this.bookService.deleteBook(id).subscribe({
      next: () => { alert('Deleted!'); this.loadBooks(); },
      error: err => alert('Delete failed: ' + err.message)
    });
  }
}

```
