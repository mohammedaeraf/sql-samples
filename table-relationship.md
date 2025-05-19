## 🔄 **Relationships in tables**
---
In this tutorial, we will add a second table called **`categories`**, establishing a **One-to-Many** relationship with `courses`, and then demonstrate **SQL JOINs** to fetch data from both tables.

Each course belongs to one category, but each category can have **many** courses — this is a **One-to-Many** relationship.

### ✅ Create `categories` Table:

```sql
CREATE TABLE fla.categories (
  id INT NOT NULL,
  name VARCHAR(50) NOT NULL,
  PRIMARY KEY (id)
);
```

### ✅ Alter `courses` Table to Add `category_id`:

```sql
ALTER TABLE fla.courses
ADD category_id INT,
ADD CONSTRAINT fk_category
    FOREIGN KEY (category_id) REFERENCES fla.categories(id);
```

---

## 📥 **Insert Sample Data**

### ➕ Insert into `categories`:

```sql
INSERT INTO fla.categories (id, name)
VALUES 
(1, 'Programming'),
(2, 'Data Science'),
(3, 'Cloud Computing'),
(4, 'Design');
```

### ➕ Update `courses` to include `category_id`:

```sql
UPDATE fla.courses SET category_id = 1 WHERE title = 'Java Full Stack';
UPDATE fla.courses SET category_id = 2 WHERE title = 'Python for Data Science';
UPDATE fla.courses SET category_id = 3 WHERE title = 'AWS Cloud Basics';
UPDATE fla.courses SET category_id = 4 WHERE title = 'Canva Graphic Design';
```

---

## 🔗 **JOINs to Fetch Related Data**

### 1. 🔍 **INNER JOIN**

Returns only records with matching values in both tables.

```sql
SELECT c.id AS course_id, c.title, c.duration, cat.name AS category
FROM fla.courses c
INNER JOIN fla.categories cat ON c.category_id = cat.id;
```

### 2. 🔍 **LEFT JOIN**

Returns all records from the left table (`courses`), even if there's no match in `categories`.

```sql
SELECT c.id AS course_id, c.title, c.duration, cat.name AS category
FROM fla.courses c
LEFT JOIN fla.categories cat ON c.category_id = cat.id;
```

### 3. 🔍 **RIGHT JOIN** (if supported by RDBMS)

Returns all records from `categories`, even if no course belongs to that category.

```sql
SELECT c.id AS course_id, c.title, cat.name AS category
FROM fla.courses c
RIGHT JOIN fla.categories cat ON c.category_id = cat.id;
```

---

## 📌 **Filter and Sort Joined Data**

### Courses under "Programming" category:

```sql
SELECT c.title, c.duration
FROM fla.courses c
JOIN fla.categories cat ON c.category_id = cat.id
WHERE cat.name = 'Programming';
```

### Sorted List by Category Name:

```sql
SELECT c.title, cat.name AS category
FROM fla.courses c
JOIN fla.categories cat ON c.category_id = cat.id
ORDER BY cat.name;
```

---

## 🧾 Review of Concepts

With this setup:

* You’ve normalized your schema.
* Used **foreign keys** to enforce relationships.
* Learned how to use **JOINs** to fetch meaningful combined data.
