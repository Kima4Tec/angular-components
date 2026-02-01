# Search felt
### search.html
```
<div class="search-container">
  <input
    type="search"
    [(ngModel)]="query"
    (keyup.enter)="searchBooks()"
    class="search-input"
    placeholder="Søg på titel eller forfatter"
  />

  <button
    (click)="searchBooks()"
    class="search-button"
  >
    Søg
  </button>
</div>
```

search.css
```
.search-container {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

/* Small screens (sm:) */
@media (min-width: 640px) {
  .search-container {
    flex-direction: row;
    align-items: center;
    gap: 0.5rem;
  }
}

/* Medium screens (md:) */
@media (min-width: 768px) {
  .search-container {
    padding: 0.25rem 0.75rem;
    gap: 0.25rem;
  }
}

.search-input {
  width: 100%;
  padding: 0.5rem 0.75rem;
  border-radius: 0.375rem;
  border: 1px solid #d1d5db; /* gray-300 */
  font-size: 1rem;
}

@media (min-width: 640px) {
  .search-input {
    width: 16rem; /* sm:w-64 */
  }
}

.search-input:focus {
  outline: none;
  border-color: #3b82f6; /* blue-500 */
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.4);
}

.search-button {
  padding: 0.5rem 1rem;
  background-color: #2563eb; /* blue-600 */
  color: white;
  border: none;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.search-button:hover {
  background-color: #1d4ed8; /* blue-700 */
}

```
