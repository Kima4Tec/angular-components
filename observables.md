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


### Officiel Angular dokumentation (Angular.io)

🔗 **Observables in Angular (guide)**
Angular beskriver her, hvordan frameworket bruger observables til at håndtere asynkrone operationer som HTTP‑requests, router‑events og forms. ([angular.io][1])

📌 **Observables for streams of values**
Denne side går mere i dybden med, hvordan observables fungerer som datastrømme over tid og bruges i Angular. ([angular.io][2])

🔗 **RxJS‑biblioteket**
Angular har også en guide til RxJS‑biblioteket, som er den implementering af Observables, Angular bruger. Her kan du lære om operators og hvordan du bygger reaktive streams. ([v17.angular.io][3])

---

### Dokumentation

Det officielle Angular site er:

➡️ **[https://angular.io/guide/observables-in-angular](https://angular.io/guide/observables-in-angular)** – Angular’s guide til Observables og hvordan de bruges i apps (HTTP, events, formularer). ([angular.io][1])

➡️ **[https://angular.io/guide/observables](https://angular.io/guide/observables)** – En mere detaljeret forklaring af Observable‑konceptet i Angular sammenhæng. ([angular.io][2])

➡️ **[https://angular.io/guide/rx-library](https://angular.io/guide/rx-library)** – Baggrund om RxJS‑biblioteket, naming conventions og brug af observables i Angular. ([v17.angular.io][3])

---

### 📌 Ekstra nyttige officielle ressourcer

Ud over Angular‑guides:

✔️ **Angular Reactive Forms guide** – fordi reaktive formularer er bygget på observables. ([angular.dev][4])

✔️ **Angular Signals & RxJS interoperabilitet** – nyere emner, men viser hvordan Angular udvikler sig i forhold til reaktiv programmering. ([angular.dev][5])

---

### Mere om RxJS

Angular bruger **RxJS** til alle observable‑streams, så dokumentationen på:

➡️ **[https://rxjs.dev/guide/observable](https://rxjs.dev/guide/observable)** – er den officielle RxJS guide til *hvad en Observable egentlig er*, hvordan den fungerer, og hvilke muligheder du har. ([rxjs.dev][6])

---

💡 **Tip:** Søg efter “Angular Observables” og “RxJS” på angular.io – de fleste Angular guides linker direkte til brugen af observables i kontekst (HTTP, formularer, routing osv.). ([angular.io][7])

---

[1]: https://angular.io/guide/observables-in-angular?utm_source=chatgpt.com "Observables in Angular"
[2]: https://angular.io/guide/observables?utm_source=chatgpt.com "Using observables for streams of values"
[3]: https://v17.angular.io/guide/rx-library?utm_source=chatgpt.com "The RxJS library"
[4]: https://angular.dev/guide/forms/reactive-forms?utm_source=chatgpt.com "Reactive forms"
[5]: https://angular.dev/ecosystem/rxjs-interop?utm_source=chatgpt.com "Signals interop"
[6]: https://rxjs.dev/guide/observable?utm_source=chatgpt.com "Observable"
[7]: https://angular.io/docs?utm_source=chatgpt.com "Introduction to the Angular docs"

