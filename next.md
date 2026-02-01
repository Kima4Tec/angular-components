# Next
fx i denne kode:

```ts
addBook() {
  this.bookService.createBook(this.book).subscribe({
    next: () => this.toast.show('Book added!', 'success'),
    error: err => this.toast.show('Failed to add book: ' + err.message, 'error')
  });
}
```

---

##  `subscribe()`?

* Når du laver en HTTP-request med **HttpClient**, får du en **Observable** tilbage.
  Fx: `this.bookService.createBook(this.book)` → `Observable<Book>`.

* For at **modtage data** fra Observablen, skal du **subscribe** til den:

```ts
observable.subscribe(observer);
```

* `subscribe()` tager et **observer-objekt**, som kan have følgende callbacks:

```ts
observable.subscribe({
  next: (value) => {},    // Kaldt når der kommer data
  error: (err) => {},     // Kaldt hvis der sker fejl
  complete: () => {}      // Kaldt når stream er færdig
});
```

---

## Hvad gør `next`?

* `next` kaldes **hver gang Observablen sender en værdi**.
* I dit eksempel:

```ts
next: () => this.toast.show('Book added!', 'success')
```

* Når `createBook()` sender en succesfuld response fra backend (fx den oprettede bog), så bliver `next` callbacken **eksekveret**.
* Her bruges den til at vise en **toast-meddelelse** om at bogen blev tilføjet.

---

## Hvad gør `error`?

* `error` kaldes hvis Observablen **fejler** (fx netværksfejl, 400/500 fra backend).
* I dit eksempel:

```ts
error: err => this.toast.show('Failed to add book: ' + err.message, 'error')
```

* Den viser en toast med fejlbesked.

---

## Optional: `complete`

* `complete` kaldes **når Observablen er færdig**.
* For HTTP-request er `complete` valgfri, fordi requesten **afsluttes automatisk efter ét svar**.

```ts
complete: () => console.log('Request finished')
```

---

## Visualisering

```
createBook() --> Observable<Book>
        │
        ├─ next()    <- backend svarer succesfuldt → Book added toast
        ├─ error()   <- backend fejl → error toast
        └─ complete() <- afsluttes (valgfri)
```

---

**Opsummering:**

* **`next`** = håndter succesfuld data fra Observable
* **`error`** = håndter fejl
* **`complete`** = håndter afslutning af stream (sjældent brugt for HttpClient)

---


Vil du have, jeg laver det?
