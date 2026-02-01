#Button med css og ngClass

## *ngClass
ngClass er et Angular-direktiv, som bruges til at tilføje, fjerne eller skifte CSS-klasser dynamisk på et HTML-element.

### button.ts
```
export class Button {
  label = input('');
  bgClass = input<string | null>(null);

  btnClicked = output();

  handleClickButton() {
    this.btnClicked.emit();
  }
}
```

### button.html
login-side
```
<button
  class="btn"
  [ngClass]="bgClass()"
  (click)="btnClicked.emit()"
>
  {{ label() }}
</button>

```

### button.css
login-side
```
.btn {
  cursor: pointer;
  color: white;
  padding: 0.5rem 1.25rem;
  border-radius: 0.375rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: 100%;
}

.btn-green {
  background-color: #16a34a;
}

.btn-red {
  background-color: #dc2626;
}

.btn:hover {
  opacity: 0.9;
}

```

### login-side
```
<app-button
  label="Log ud"
  bgClass="btn-red"
  (btnClicked)="logout()"
/>

<app-button
  label="Log ind"
  bgClass="btn-green"
  routerLink="/login"
/>

```

## Uden ngClass – kun boolean input

### button.component.ts
```
isDanger = input(false);
```
### button.component.html
```
<button
  class="btn"
  [class.btn-red]="isDanger()"
  [class.btn-green]="!isDanger()"
  (click)="btnClicked.emit()"
>
  {{ label() }}
</button>
```
### Brug
```
<app-button label="Log ud" [isDanger]="true" />
<app-button label="Log ind" />
```
