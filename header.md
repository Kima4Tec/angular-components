# Header med css

### HTML
```
<header class="header">
  <!-- Venstre side : Brand-name -->
  <div class="brand">
    MyBrand
  </div>

  <!-- Midten: Navigation til de forskellige sider -->
  <nav class="nav">
    <a routerLink="/" routerLinkActive="active">Home</a>
    <a routerLink="/books" routerLinkActive="active">Books</a>
    <a routerLink="/contact" routerLinkActive="active">Contact</a>
  </nav>

  <!-- Højre side: om eller sociale medier eller kontakt fx -->
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
  border-bottom: 1px solid #e5e7eb; /* light gray */
  background-color: #ffffff;
}

.brand {
  font-size: 1.25rem;
  font-weight: 700;
  color: #111827;
}

.nav {
  display: flex;
  gap: 1.5rem;
}

.nav a {
  text-decoration: none;
  color: #374151;
  font-weight: 500;
  position: relative;
}

.nav a:hover {
  color: #2563eb;
}

.nav a.active {
  color: #2563eb;
}

.about a {
  text-decoration: none;
  color: #374151;
  font-weight: 500;
}

.about a:hover {
  color: #2563eb;
}

```
