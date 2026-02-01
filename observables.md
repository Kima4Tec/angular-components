# Observables

---

## Hvad er en Observable?

En **Observable** er en datastrøm, som kan **udgive data over tid**, og som man kan **subscribe til** for at modtage disse data.

* Ligner lidt **Promises**, men kan sende **flere værdier over tid**.
* Typisk i Angular bruges Observables til:

  * HTTP requests (`HttpClient.get/post/put/delete`)
  * Event streams (fx `fromEvent`)
  * Timer/intervals (`interval()`, `timer()`)

---

### Grundlæggende diagram

```
Observable ---> Subscriber
  (sender data over tid)
```

* **Observable** = producent
* **Subscriber** = forbruger, som håndterer `next`, `error`, `complete`

---

## Simpelt eksempel med `HttpClient`

```ts
this.http.get<Book[]>('http://localhost:5000/api/books')
  .subscribe({
    next: books => console.log('Books:', books),
    error: err => console.error('Error:', err),
    complete: () => console.log('Request completed')
  });
```

* `this.http.get<Book[]>()` → **Observable<Book[]>**
* `.subscribe()` → vi modtager data
* `next` = succesfuld data
* `error` = fejl
* `complete` = stream færdig

---

## Observable vs Promise

| Feature        | Observable                             | Promise                      |
| -------------- | -------------------------------------- | ---------------------------- |
| Flere værdier  | ✅                                      | ❌ (kun 1)                    |
| Cancelering    | ✅                                      | ❌                            |
| Lazy execution | ✅ (starter først når man subscribes)   | ✅ (starter med det samme)    |
| Operators      | ✅ (map, filter, catchError, tap, etc.) | ❌ (man kan kun `then/catch`) |

---

## Operators (`pipe`)

* Observables kan **transformeres** med **operators**, fx `map`, `filter`, `tap`, `catchError`
* Eksempel fra dit BookService:

```ts
return this.http.get<Book[]>(this.apiUrl)
  .pipe(
    tap(books => console.log('Fetched books', books)),
    catchError(this.handleError)
  );
```

* `tap` → side-effect (fx log)
* `catchError` → håndterer fejl
* `pipe` kæder operators før data når subscriber

---

## Flow i CRUD-eksempel

1. Component kalder `bookService.createBook(this.book)`
2. Service returnerer **Observable**
3. Component `subscribe()` til Observable:

   * `next` → success toast
   * `error` → error toast
   * `complete` → optional, f.eks. log

```
BookService.createBook()
        │
        ├─ Observable<Book>
        │     ├─ next → data modtages
        │     ├─ error → fejl håndteres
        │     └─ complete → stream færdig
        │
  Component subscribes → reagerer på events
```

---

## Tips til Angular + Observables

* **HTTP request er cold Observable** → starter først når man subscribes
* Brug **pipe + operators** til at transformere eller håndtere fejl
* **Unsubscribe** hvis stream ikke afsluttes automatisk (fx `interval()` eller events) for at undgå memory leaks
* Angulars `AsyncPipe` (`| async`) kan bruges i template for automatisk subscription og unsubscription

```html
<ul>
  <li *ngFor="let book of books$ | async">
    {{ book.title }}
  </li>
</ul>
```

---

**Opsummering:**

* Observable = datastrøm, kan sende mange værdier over tid
* Subscribe = modtag data, fejl, afslutning
* Pipe = transformér og håndter data med RxJS operators
* Perfekt til HTTP, events, timers og reaktive UI-elementer

---

Vil du have, jeg laver det?
