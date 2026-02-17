#### 1. The "Extra Columns" Rule (Many-to-Many)

- **Rule:** If your join table needs any data other than the two foreign keys (e.g., `created_at`, `status`, `quantity`), you **cannot** use `@ManyToMany`.
- **Solution:** Convert the join table into a **full Entity**. Break the relationship into two `@OneToMany` relationships.
- **Best Practice:** Use a composite key (`@Embeddable`) if the relationship identity depends on both parents.

#### 2. The Identity vs. Relationship Rule
- **Rule:** Do not place relationship annotations directly on the Primary Key field (`@Id`) if you can avoid it. Keep them as separate class members.
- **Solution:** Use the `@MapsId` annotation. This tells JPA to use the ID of the parent entity as the ID of the child entity.
- **Benefit:** This ensures a Shared Primary Key, which is the most efficient way to map One-to-One relationships.

#### 3. The "Central Entity" Rule
- **Rule:** One entity can be the "Source of Truth" for many different relationships simultaneously.
- **Solution:** The Parent entity uses `mappedBy`, and the Child entities use `@JoinColumn`.
- **Example Mapping:**
    - **User** (The Parent)
    - **Profile** (One-to-One with User using `@MapsId`)
    - **Post** (Many-to-One with User)
    - **Comment** (Many-to-One with User)
#### 4. The Linux Case-Sensitivity Rule
- **Rule:** Linux filesystems are **case-sensitive**, whereas Windows usually is not. Hibernate's naming strategy can vary between OS environments.
- **Solution:** Always explicitly name your tables and columns in lowercase to ensure your code is portable across all environments.
- **Example:** `@Table(name = "user_profile")` instead of relying on the class name `UserProfile`.

---
### 🚀 Practical Code Example

```java
// 1. THE PARENT
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToOne(mappedBy = "user")
    private Profile profile;

    @OneToMany(mappedBy = "user")
    private List<Post> posts;
}

// 2. THE ONE-TO-ONE CHILD (Shares User ID)
@Entity
@Table(name = "profiles")
public class Profile {
    @Id
    private Long id; // No @GeneratedValue here

    @OneToOne
    @MapsId          // Points to the ID of the User entity
    @JoinColumn(name = "user_id")
    private User user;

    private String bio;
}

// 3. THE MANY-TO-ONE CHILD
@Entity
@Table(name = "posts")
public class Post {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "user_id") // Standard Foreign Key
    private User user;

    private String content;
}
```

---
### Summary Checklist for Mapping
- [ ] **Join table has extra fields?** Use a Middle Entity.
- [ ] **One-to-One?** Use `@MapsId` on the child.
- [ ] **One-to-Many?** Use `mappedBy` on the Parent to avoid an unnecessary third join table.
- [ ] **Cross-Platform?** Use explicit lowercase `@Table` names for Linux compatibility.