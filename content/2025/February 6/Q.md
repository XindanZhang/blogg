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

In this case, the format of time that returns would be "YYYY-MM-DD HH:MM:SS". This value only has seconds-level precision, and doesn't include fractional seconds(milliseconds). Even if I convert the timestamps to ISO 8601 format using toISOString(), the underlying value stored by CURRENT_TIMESTAMP only has second-level precision. So when we test the update paper function of the code, if the test time is millisecond-level change, it fails.

So can i change this with:
```javascript
created_at DATETIME DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ','now')),
updated_at DATETIME DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ','now'))
```

![[images/validateID.png]]_This image shows we can use
`router get("/papers/:id", validateId, async (req, res, next) →> {})` validateId as the middleware, but in the code given in Github, only this is provided:

```javascript
router.get("/papers/:id", async (req, res, next) => {
```

So can i add validateId as a middleware to the code?
```javascript
const { validatePaper } = require("./middleware");
// const { validatePaper, validateId } = require("./middleware");
```

But if we choose to embed the ID validation logic directly inside each route handler instead of using the separate validateId middleware, then the standalone validateId function becomes redundant.
