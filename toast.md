# Toast
*En toast er en lille, midlertidig meddelelse, der popper op på skærmen for at informere brugeren om noget.*

#### Typisk bruges de til feedback som:
  - Succesfuld handling (fx “Book added!”)
  - Fejl (fx “Failed to delete book”)
  - Advarsler (fx “Are you sure?”)

### Eksempel
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-toast',
  template: `
    <div *ngIf="message" class="toast" [ngClass]="type">
      {{ message }}
    </div>
  `,
  styles: [`
    .toast {
      position: fixed;
      bottom: 20px;
      right: 20px;
      padding: 1rem 1.5rem;
      border-radius: 5px;
      color: white;
      animation: fadeOut 4s forwards;
    }
    .success { background-color: green; }
    .error { background-color: red; }
    .info { background-color: blue; }
    .warning { background-color: orange; }

    @keyframes fadeOut {
      0% { opacity: 1; }
      80% { opacity: 1; }
      100% { opacity: 0; }
    }
  `]
})
export class ToastComponent {
  message: string | null = null;
  type: 'success' | 'error' | 'info' | 'warning' = 'info';

  show(msg: string, type: 'success' | 'error' | 'info' | 'warning' = 'info') {
    this.message = msg;
    this.type = type;
    setTimeout(() => this.message = null, 4000); // forsvinder efter 4 sek
  }
}

```

### Brug af eksemplet
```
constructor(private bookService: BookService, private toast: ToastComponent) {}

addBook() {
  this.bookService.createBook(this.book).subscribe({
    next: () => this.toast.show('Book added!', 'success'),
    error: err => this.toast.show('Failed to add book: ' + err.message, 'error')
  });
}

```


## Man kan også bruge eksterne biblioteker - gerne brugt i produktion
  - ngx-toastr (link)
  - Angular Material Snackbar

Eksempel med Angular Material:
```
import { MatSnackBar } from '@angular/material/snack-bar';

constructor(private snackBar: MatSnackBar) {}

showToast(msg: string) {
  this.snackBar.open(msg, 'Close', { duration: 3000 });
}

```

- Meget nemt at bruge
- Automatisk styling og animation
- Kan bruges til succes, fejl, advarsler osv.
