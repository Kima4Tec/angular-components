# Header med css

### ts
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css'],
})
export class HeaderComponent {
  menuOpen = false;

  toggleMenu() {
    this.menuOpen = !this.menuOpen;
  }

  closeMenu() {
    this.menuOpen = false;
  }
}

```


### HTML
```
<header class="header">
  <!-- Brand-navnet -->
  <div class="brand">
    MyBrand
  </div>

  <!-- Hamburger (mobil) -->
  <button class="menu-toggle" (click)="toggleMenu()">
    {{ menuOpen ? '✖' : '☰' }}
  </button>

  <!-- Navigationsdelen -->
  <nav class="nav" [class.open]="menuOpen">
    <a routerLink="/" (click)="closeMenu()">Home</a>
    <a routerLink="/books" (click)="closeMenu()">Books</a>
    <a routerLink="/contact" (click)="closeMenu()">Contact</a>
  </nav>

  <!-- Om, Kontakt eller andet i højre side (desktop) -->
  <div class="about">
    <a routerLink="/about">About</a>
  </div>
</header>

```

### css
```
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;

  padding: 0.75rem 1.5rem;
  border-bottom: 1px solid #e5e7eb;
  background-color: #ffffff;
  position: relative;
}

.brand {
  font-size: 1.25rem;
  font-weight: 700;
}

/* Navigation */

.nav {
  display: flex;
  gap: 1.5rem;
}

.nav a {
  text-decoration: none;
  color: #374151;
  font-weight: 500;
}

.nav a:hover {
  color: #2563eb;
}

/* Om eller kontakt (desktop) */

.about a {
  text-decoration: none;
  color: #374151;
  font-weight: 500;
}

.about a:hover {
  color: #2563eb;
}

/* Hamburger */

.menu-toggle {
  display: none;
  font-size: 1.75rem;
  background: none;
  border: none;
  cursor: pointer;
  line-height: 1;
}


@media (max-width: 768px) {
  .menu-toggle {
    display: block;
  }

  .nav {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;

    flex-direction: column;
    gap: 1rem;

    background-color: #ffffff;
    border-bottom: 1px solid #e5e7eb;
    padding: 1rem;

    display: none;
  }

  .nav.open {
    display: flex;
  }

  .about {
    display: none;
  }
}


```
