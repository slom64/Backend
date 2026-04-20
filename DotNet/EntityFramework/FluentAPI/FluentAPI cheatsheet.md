# Entity & Table Configuration

| Method                  | Example                              | What it does              | When to use                       |
| ----------------------- | ------------------------------------ | ------------------------- | --------------------------------- |
| `ToTable()`             | `builder.ToTable("courses")`         | Maps entity to table name | Naming conventions, schemas       |
| `HasKey()`              | `builder.HasKey(c => c.Id)`          | Defines primary key       | Composite keys or explicit config |
| `HasIndex()`            | `builder.HasIndex(c => c.Title)`     | Creates index             | Performance optimization          |
| `HasIndex().IsUnique()` | `.HasIndex(c => c.Email).IsUnique()` | Unique constraint         | Enforce uniqueness                |

---

# Property (Column) Configuration

| Method                  | Example                           | What it does                      | When to use                  |
| ----------------------- | --------------------------------- | --------------------------------- | ---------------------------- |
| `Property()`            | `builder.Property(c => c.Title)`  | Selects property to configure     | Entry point for config       |
| `HasColumnName()`       | `.HasColumnName("title")`         | Rename column                     | Naming conventions           |
| `HasColumnType()`       | `.HasColumnType("decimal(10,2)")` | Set SQL type                      | Precision control            |
| `IsRequired()`          | `.IsRequired()`                   | NOT NULL                          | Required fields              |
| `HasMaxLength()`        | `.HasMaxLength(255)`              | Limit string length               | Validation + DB optimization |
| `HasDefaultValue()`     | `.HasDefaultValue(0)`             | Default value in DB               | Default columns              |
| `ValueGeneratedOnAdd()` | `.ValueGeneratedOnAdd()`          | Auto-generated value              | Identity columns             |
| `HasConversion()`       | `.HasConversion<string>()`        | Convert type (e.g. enum → string) | Enum storage control         |

---

# Relationships

## One-to-Many

| Method                | Example                                          | What it does             | When to use          |
| --------------------- | ------------------------------------------------ | ------------------------ | -------------------- |
| `HasOne().WithMany()` | `HasOne(c => c.Author).WithMany(a => a.Courses)` | Defines 1→N relationship | Most common relation |
| `HasForeignKey()`     | `.HasForeignKey(c => c.AuthorId)`                | Sets FK column           | Explicit FK control  |
| `OnDelete()`          | `.OnDelete(DeleteBehavior.Cascade)`              | Delete behavior          | Control cascading    |

---

## One-to-One

| Method               | Example                                       | What it does         | When to use        |
| -------------------- | --------------------------------------------- | -------------------- | ------------------ |
| `HasOne().WithOne()` | `HasOne(u => u.Profile).WithOne(p => p.User)` | Defines 1→1 relation | Rare but important |
| `HasForeignKey<T>()` | `.HasForeignKey<Profile>(p => p.UserId)`      | Specifies FK side    | Required in 1–1    |

---

## Many-to-Many

| Method                 | Example                                         | What it does         | When to use              |
| ---------------------- | ----------------------------------------------- | -------------------- | ------------------------ |
| `HasMany().WithMany()` | `HasMany(c => c.Tags).WithMany(t => t.Courses)` | Defines N↔N          | Tagging systems          |
| `UsingEntity()`        | `.UsingEntity(j => j.ToTable("CourseTags"))`    | Customize join table | Naming / advanced config |

---

# Advanced / Global Configuration

| Method                              | Example                                               | What it does              | When to use           |
| ----------------------------------- | ----------------------------------------------------- | ------------------------- | --------------------- |
| `Ignore()`                          | `builder.Ignore(c => c.TempField)`                    | Exclude property from DB  | Non-persistent fields |
| `OwnsOne()`                         | `builder.OwnsOne(c => c.Address)`                     | Value object (owned type) | DDD patterns          |
| `ApplyConfiguration()`              | `modelBuilder.ApplyConfiguration(new CourseConfig())` | Apply config class        | Clean architecture    |
| `ApplyConfigurationsFromAssembly()` | `modelBuilder.ApplyConfigurationsFromAssembly(...)`   | Auto-load configs         | Scalable projects     |
