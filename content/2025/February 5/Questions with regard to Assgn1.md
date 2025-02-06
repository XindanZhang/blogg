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

So can I change this with:
```javascript
created_at DATETIME DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ','now')),
updated_at DATETIME DEFAULT (strftime('%Y-%m-%dT%H:%M:%fZ','now'))
```

And also update the updatepaper function:
```javascript
"UPDATE papers SET title = ?, authors = ?, published_in = ?, year = ?, updated_at = (strftime('%Y-%m-%dT%H:%M:%fZ', 'now')) WHERE id = ?",
```

But here is the problem, I need to delete the old database table everytime before I test.
Add the code below for test.

```javascript
// TODO: Create a table named papers with the schema specified in the handout
db.serialize(() => {
  // db.run("DROP TABLE IF EXISTS papers", (err) => {
  //   if (err) {
  //     console.error("Error dropping table papers:", err);
  //   } else {
  //     console.log("Dropped old table 'papers' (if existed)");
  //   }
  // });
  ...
});
```

But the note in handout:

>[!Note:]
> - The table creation should be implemented in database.js
> - Timestamps are **automatically managed by SQLite**

**So should I CHANGE the database schema?**


To actually see millisecond changes, you need to generate a high-precision timestamp in JavaScript (or use another SQLite method, like using strftime to get millisecond precision) when updating the record, rather than relying solely on CURRENT_TIMESTAMP.
```javascript
updatePaper: async (id, paper) => {
  try {
    // 用 JavaScript 生成一个包含毫秒的高精度时间戳
    const timestamp = new Date()
      .toISOString()
      .replace("T", " ")
      .replace("Z", ""); // 得到 "YYYY-MM-DD HH:mm:ss.sss" 格式

    await new Promise((resolve, reject) => {
      db.run(
        "UPDATE papers SET title = ?, authors = ?, published_in = ?, year = ?, updated_at = ? WHERE id = ?",
        [
          paper.title,
          paper.authors,
          paper.published_in,
          paper.year,
          timestamp,
          id,
        ],
        function (err) {
          if (err) reject(err);
          else resolve();
        },
      );
    });
    return await dbOperations.getPaperById(id);
  } catch (error) {
    throw error;
  }
},
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


After communicating with Chen, I **SOLVED** my two questions above.

First, we don't need to be concerned about millisecond precision in the time format.

Second, we can freely add middleware to improve code readability.
