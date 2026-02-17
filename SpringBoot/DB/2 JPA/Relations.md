- Relationships between classes can be Unidirectional, Bidirectional. So if there is relation one-to-many between user and address
	- **Unidirectional**: fetching address means we can know which user, but fetching .
	- **Bidirectional**: fetching User mean we can access its addresses, and fetching addresses mean we can access the corresponding user.

| Feature                      | Unidirectional                                                                                                                                                                                                               | Bidirectional                                                                                                                                     |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Navigation**               | Navigation is possible from only one entity to the other (e.g., from `Bank` to `Account`, but not vice versa in the object model).                                                                                           | Navigation is possible from both entities to each other (`Bank` knows its `Accounts`, and each `Account` knows its `Bank`).                       |
| **Implementation**           | Only one side of the relationship has a reference (e.g., an object property or collection) to the other.                                                                                                                     | Both sides have references to each other. The "owning side" of the relationship manages how updates are made to the database.                     |
| **Database Schema**          | In many cases, both unidirectional and bidirectional relationships result in the same underlying database schema (e.g., a single foreign key column in one table).                                                           | The difference lies in the application layer's ability to traverse the relationship without writing explicit queries.                             |
| **Complexity & Performance** | Simpler to implement and manage in code, with less risk of data inconsistency in the object graph.                                                                                                                           | More complex to manage, as the application is responsible for keeping both sides of the association synchronized in memory.                       |
| **Use Cases**                | Useful when you rarely or never need to access the "other side" of the relationship (e.g., a Comment might need to know its Post, but the Post doesn't necessarily need a collection of all Comments in a simple list view). | Ideal for scenarios requiring frequent access and manipulation of both sides of the relationship, supporting real-time updates and collaboration. |

---

## Bidirectional vs Unidirectional

> [!Question]
> choosing if it bidirectional or unidirectional isn't set by backend dev, its database creator responsability to do so... isn't?

This is a great point of confusion because, in many companies, the "Database Administrator" (DBA) and the "Backend Developer" are different people. However, here is the secret:

**In the Database (SQL), there is no such thing as "Bidirectional."**

If you look at your MariaDB tables on Ubuntu, you will only see a **Foreign Key** in the `addresses` table pointing to the `users` table. That is it. A database relationship is always "unidirectional" at the physical level because the data only lives in one column.

---
### 1. Who decides the "Direction"?

The **Backend Developer** decides the direction, not the database creator.

When you use Spring Boot (JPA/Hibernate), you are creating a "Virtual Model" of your database in Java. The "Direction" is a **Java-only feature** that determines how you can navigate your objects.
- **The Database Creator's Job:** Ensures the `user_id` column exists in the `addresses` table so the data is linked correctly.
- **The Backend Developer's Job (You):** Decides if the `User` class should have a `Set<Address>` field.

---
### 2. Why the Backend Dev makes this choice

As a Backend Dev, you ask yourself: _"In my Java code, will I ever need to say `myUser.getAddresses()`?"_
- **If Yes:** You make it **Bidirectional** in Java. You add the `Set` to the `User` class.
- **If No:** You keep it **Unidirectional**. You only put the `User` field in the `Address` class.

The database schema in MariaDB stays **exactly the same** either way.

---
### 3. The "Ownership" confusion
You mentioned earlier that if A belongs to B, B doesn't necessarily have A. This is a **Business Rule**.
- **Database Level:** The Foreign Key defines who "belongs" to whom. `Address` has the `user_id`, so `Address` belongs to `User`.
- **Java Level:** You decide if that relationship is a "Secret" (Unidirectional) or "Public" (Bidirectional).

---
### 4. Why would you ever choose Unidirectional?

If the Backend Dev decides everything, why not just make everything Bidirectional?
1. **Memory:** Every time you load a User, you might accidentally load 500 Addresses into your Linux server's RAM if you aren't careful.
2. **Complexity:** Bidirectional relationships are harder to maintain. You have to make sure that if you add an address to a user, the address also "knows" about that user.
3. **Circular Logic:** As we discussed, it can cause huge issues with JSON conversion and `hashCode` methods.
---
### Summary
The **Database** is just the "Storage." It only knows that a link exists.

The **Backend Code** is the "Logic." It decides if that link should be reachable from both ends or just one.

In your `store` project, if you want to show a "Profile Page" that lists all a user's addresses, **you (the Backend Dev)** will choose to make it **Bidirectional** so you can easily call `user.getAddresses()`.



---

## Direction of Unidirectional

> [!Question]
> cool now is there rule for specifying the direction? 
> 
> so if we have one to many relationship between user and address. 
> does that mean the Unidirectional should always be from user to address so fetching address mean we can know the user but fetching user can't know the addresses. 
> 
> can't we have the opposite making the direction the opposite direction which fetching the user make us know the addresses but fetching the address won't tell us the user


> [!Summary]
> If you want to use Unidirectional it is best practice to put direction from Child -> Parent "Based on foreign key". So knowing address means knowing user.
> 
> But if you want also to know the opposite which is fetching user and know his addresses, its best to use **Bidirectional**.

This is the exact point where many developers get confused, but you’ve actually described the **two different types of Unidirectional relationships**.

There is no "rule" that says it _must_ go one way, but there are massive **performance and design consequences** depending on which way you choose.

---

### 1. The Two "Unidirectional" Options

You can absolutely have it both ways in Java, even if the database stays the same.

#### Option A: Unidirectional `@ManyToOne` (Child → Parent)
- **The Code:** `Address` has a `private User user;`, but `User` has **nothing**.
- **The Result:** You fetch an `Address` and know the `User`. But if you fetch a `User`, you cannot see their `Addresses` (unless you write a custom query).
- **Performance:** **Excellent.** This is the most efficient way to map a relationship because it maps directly to the Foreign Key column in your MariaDB table.
#### Option B: Unidirectional `@OneToMany` (Parent → Child)
- **The Code:** `User` has a `private List<Address> addresses;`, but `Address` has **nothing**.
- **The Result:** This is what you suggested—fetching a `User` tells you the `Addresses`, but an `Address` doesn't know its `User`.
- **Performance:** **Terrible.** By default, Hibernate will try to create a **Join Table** (a third table just for the link). If you force it to use a Foreign Key with `@JoinColumn`, it performs "extra" update statements that slow down your Linux server.

---
### 2. The "Hidden Trap" of Unidirectional `@OneToMany`

If you choose the "Opposite" direction (User -> Address) without a back-reference, Hibernate does something very inefficient under the hood:
1. It inserts the `Address` into the database with a `NULL` user_id.
2. It then runs a **second SQL UPDATE** to set the `user_id` once it knows which User the address belongs to.

In a high-performance backend, those extra updates are "expensive." This is why most senior devs avoid unidirectional `@OneToMany`.

---
### 3. Comparison of Directions

| **Direction**      | **Annotation**    | **Navigator**           | **Database Impact**                                     |
| ------------------ | ----------------- | ----------------------- | ------------------------------------------------------- |
| **Child → Parent** | `@ManyToOne`      | Address knows User      | **Best.** Simple Foreign Key.                           |
| **Parent → Child** | `@OneToMany`      | User knows Addresses    | **Risky.** Usually needs a Join Table or extra UPDATES. |
| **Both Ways**      | **Bidirectional** | Everyone knows everyone | **Standard.** Uses `mappedBy` to stay efficient.        |

---

### 4. So, is there a rule?
There isn't a strict rule, but there is a **Best Practice**:
1. **Start with Unidirectional `@ManyToOne` (Address → User).** It is the most "honest" representation of how the database works.
2. **Upgrade to Bidirectional ONLY if you need it.** If you find yourself constantly needing to display a list of addresses for a user, add the `Set<Address>` to the `User` class and use `mappedBy`.

> **The "Reverse" Rule:** Almost never use a **Unidirectional `@OneToMany`**. If you want the Parent to know the Children, just go all the way and make it **Bidirectional**. It’s more efficient and easier to manage.

---
### Summary for your project:

Since you are building a `store`, you will almost certainly want to see all addresses for a user.
- Don't try to be "clever" with a one-way `@OneToMany`. 
- Make it **Bidirectional**. Use `@ManyToOne` in `Address` (the owner) and `@OneToMany(mappedBy = "user")` in `User`.