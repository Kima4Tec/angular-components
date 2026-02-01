# Button med tailwind
### ts og html
```
import { Component, input, output } from '@angular/core';
import { NgClass } from '@angular/common';

@Component({
  selector: 'app-button',
  imports: [NgClass],
  template: `
    <button
      class="cursor-pointer text-white px-5 py-2 rounded-md shadow-md w-full"
      [ngClass]="bgClass()"
      (click)="btnClicked.emit()"
    >
      {{ label() }}
    </button>
  `,
  styles: ``,
})
export class Button {
  label = input('');
  bgClass = input<string | null>(null);

  btnClicked = output();

  handleClickButton() {
    this.btnClicked.emit();
  }
}

```

### Anvendelse af komponenten button
```
      <div class="gap-2 ">
        @if (authService.isLoggedIn()) {
        <app-button
          label="Log ud"
          [bgClass]="'bg-red-600 hover:opacity-90'"
          (btnClicked)="logout()"
        />
        } @else {
        <app-button
          label="log ind"
          [bgClass]="'bg-green-600 hover:opacity-90'"
          routerLink="/login"
        />
        }
      </div>
```
