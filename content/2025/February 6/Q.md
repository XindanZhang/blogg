## Q1

If I use the database scheme shown in the handout, I can see that
```javascript
CREATE TABLE papers (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    authors TEXT NOT NULL,
    published_in TEXT NOT NULL,
    year INTEGER NOT NULL CHECK (year > 1900),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

The time is created with `CURRENT_TIMESTAMP`.

```javascript
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
```

In this case, the format of time that returns would be "YYYY-MM-DD HH:MM:SS". This value only has seconds-level precision, and doesn't include fractional seconds(milliseconds). Even if I convert the timestamps to ISO 8601 format using toISOString(), the underlying value stored by CURRENT_TIMESTAMP only has second-level precision.

So can i change this with:
```javascript
created_at DATETIME DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ','now')),
updated_at DATETIME DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ','now'))
```
