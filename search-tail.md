# search med tailwind
```
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-search-field',
  imports: [FormsModule],
  template: `
    <div
      class="flex flex-col sm:flex-row gap-2 sm:items-center md:px-3 md:py-1 md:gap-1"
    >
      <input
        type="search"
        [(ngModel)]="query"
        (keyup.enter)="searchBooks()"
        class="w-full sm:w-64 px-3 py-2 rounded-md border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500"
        placeholder="Søg på titel eller forfatter"
      />
      <button
        (click)="searchBooks()"
        class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700 transition"
      >
        Søg
      </button>
    </div>
  `,
  styles: ``,
})
export class SearchField {
  query: any;
  constructor() {}

  searchBooks() {
    console.log('Searching for:', this.query);
  }
}
```

### brug:
```
    <app-search-field />
```
og i component:
```
searchBooks() {
    console.log('Searching for:', this.query);
  }
```
