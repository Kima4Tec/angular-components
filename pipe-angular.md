# Angular Pipes - Forklaring

---

## Angular Template Pipes

I Angular templates bruges **pipes** til at **transformere data til visning**.

### Syntax
```html
{{ value | pipeName:arg1:arg2 }}
```
- `value` → data du vil transformere
- `pipeName` → navnet på pipen (fx `date`, `uppercase`, `currency`)
- `arg1, arg2…` → valgfrie argumenter

### Eksempler

#### Uppercase / Lowercase
```html
<p>{{ 'hello world' | uppercase }}</p> <!-- HELLO WORLD -->
<p>{{ 'HELLO WORLD' | lowercase }}</p> <!-- hello world -->
```

#### Date Pipe
```html
<p>{{ today | date:'fullDate' }}</p> <!-- Monday, February 1, 2026 -->
```

#### Currency Pipe
```html
<p>{{ price | currency:'DKK' }}</p> <!-- kr 1.234,56 -->
```

#### Custom Pipe
```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'exclaim' })
export class ExclaimPipe implements PipeTransform {
  transform(value: string): string {
    return value + '!!!';
  }
}
```
```html
<p>{{ 'Hello' | exclaim }}</p> <!-- Hello!!! -->
```

---

## RxJS Pipe (i TypeScript / services)

Når man arbejder med **Observables**, bruges **pipe()** til at transformere eller håndtere data.

### Syntax
```ts
observable.pipe(
  operator1(),
  operator2(),
  operator3()
)
```
- `observable` → fx HttpClient.get<Book[]>()
- Operators → fx `map`, `catchError`, `tap`, `filter`

### Eksempel med HttpClient
```ts
import { catchError, tap, map } from 'rxjs/operators';
import { of } from 'rxjs';

this.http.get<Book[]>('http://localhost:5000/api/books')
  .pipe(
    tap(books => console.log('Fetched books', books)),
    map(books => books.filter(b => b.title.includes('Angular'))),
    catchError(err => {
      console.error(err);
      return of([]); // fallback tom liste
    })
  )
  .subscribe(filteredBooks => this.books = filteredBooks);
```

- `tap` → side-effect, fx logge data
- `map` → transformer data
- `catchError` → håndter fejl

---

## Sammenligning

| Type | Hvor | Hvad den gør |
|------|-----|-------------|
| Template pipe | HTML | Formaterer data til visning (uppercase, date, currency…) |
| RxJS pipe | TypeScript / services | Transformerer eller håndterer Observable streams (map, filter, catchError…) |

**Husk**:
- Template: `{{ value | uppercase }}` → **visning**
- RxJS: `observable.pipe(map(...))` → **data-stream manipulation**

---

## Tips

1. Du kan lave **egne custom pipes** for gentagne formateringer.
2. RxJS `pipe()` kan kæde flere operators sammen.
3. Template pipes kører hver gang Angular detekterer ændringer, så tunge beregninger bør håndteres i TypeScript i stedet.

---

### Eksempel i service
I et service-eksempel er pipe en RxJS-funktion, som bruges på Observables (det, HttpClient altid returnerer).

Formålet er at kæde operators, som kan transformere, logge eller håndtere fejl, før komponenten modtager data.

### Syntax:
```
this.http.get<Book[]>(this.apiUrl)
  .pipe(
    catchError(this.handleError)
  );
```

- this.http.get<Book[]>(...) returnerer en Observable<Book[]>
- .pipe(...) siger: “før dataen sendes til subscriber, anvend disse operators”

### Operators kan fx være:
- map() → transformer data
- tap() → udfør side-effects som logning
- catchError() → håndter fejl

🔹 Eksempler fra service
#### getAll()
```
return this.http.get<Book[]>(this.apiUrl)
  .pipe(catchError(this.handleError));
```

Her bruges kun catchError
- Hvis request fejler (fx server nede, 500 error), vil handleError blive kaldt
- pipe sørger for, at error håndteres inden data når subscriber

#### create()
```
return this.http.post<Book>(this.apiUrl, book)
  .pipe(
    tap(() => console.log(`Book created: ${book.title}`)),
    catchError(this.handleError)
  );
```
- tap() → udfører side-effect uden at ændre data (console.log her)
- catchError() → håndterer errors

#### pipe kombinerer begge operators, rækkefølgen betyder:
- Log data med tap
- Fang fejl med catchError

### Sådan fungerer det visuelt
```
HTTP GET → Observable<Book[]> 
        │
        ├─ pipe()
        │    ├─ tap()       <- (side-effect, fx log)
        │    └─ catchError() <- (håndter fejl)
        │
      Subscriber receives data (eller error)
```
Data flyder gennem pipe før subscriber

**Du kan tilføje så mange operators du vil, fx map, filter, retry, osv.**

#### Kort opsummering
| Element	| Funktion | 
| pipe() | Kæder RxJS operators til en Observable | 
| tap() | Side-effects uden at ændre data (fx console.log) | 
| catchError() | Håndterer fejl og returnerer en ny Observable eller throwError | 
| Subscriber | Modtager endeligt data (eller error) | 

#### Tip:
pipe bruges ALDRIG i template – det er kun i TypeScript / services til observables.

