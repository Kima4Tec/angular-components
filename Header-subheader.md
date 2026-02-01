# Header med subheader

### HTML
```
<header class="header-wrapper">
  <!-- Main header -->
  <div class="header">
    <div class="brand">MyBrand</div>

    <button class="menu-toggle" (click)="toggleMenu()">
      {{ menuOpen ? '✖' : '☰' }}
    </button>

    <nav class="nav" [class.open]="menuOpen">
      <a routerLink="/" (click)="closeMenu()">Home</a>
      <a routerLink="/books" (click)="closeMenu()">Books</a>
      <a routerLink="/contact" (click)="closeMenu()">Contact</a>
    </nav>

    <div class="about">
      <a routerLink="/about">About</a>
    </div>
  </div>

  <!-- Subheader -->
  <div class="subheader">
    <nav class="subnav">
      <a routerLink="/new">New</a>
      <a routerLink="/popular">Popular</a>
      <a routerLink="/favorites">Favorites</a>
    </nav>
  </div>
</header>

```

### CSS
```
.header-wrapper {
  width: 100%;
  border-bottom: 1px solid #e5e7eb;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;

  padding: 0.75rem 1.5rem;
  background-color: #ffffff;
}

.brand {
  font-size: 1.25rem;
  font-weight: 700;
}

.nav {
  display: flex;
  gap: 1.5rem;
}

.nav a,
.subnav a {
  text-decoration: none;
  color: #374151;
  font-weight: 500;
}

.nav a:hover,
.subnav a:hover {
  color: #2563eb;
}


.about a {
  font-weight: 500;
}

/* Hamburger */

.menu-toggle {
  display: none;
  font-size: 1.75rem;
  background: none;
  border: none;
  cursor: pointer;
}

.subheader {
  background-color: #f9fafb;
  border-top: 1px solid #e5e7eb;
}

.subnav {
  display: flex;
  gap: 1.5rem;
  padding: 0.5rem 1.5rem;
  font-size: 0.95rem;
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
    padding: 1rem;

    display: none;
  }

  .nav.open {
    display: flex;
  }

  .about {
    display: none;
  }

  .subnav {
    justify-content: center;
    flex-wrap: wrap;
  }
}

```

**Hvis der er forskellige subheaders på de underliggende sider:**
På app.html:
```
<app-header></app-header>
<router-outlet></router-outlet>

```
og de øvrige sider:
```
<app-subheader></app-subheader>

```
