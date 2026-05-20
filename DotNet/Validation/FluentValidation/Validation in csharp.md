### The Golden Blueprint for Chaining

When building an extension method or a standalone rule, copy this layout to prevent edge-case bugs:

$$\text{RuleFor} \rightarrow \text{CascadeMode} \rightarrow \text{Existence Checks (NotEmpty)} \rightarrow \text{Formatting (Length/Regex)} \rightarrow \text{Business/DB Rules} \rightarrow \text{Conditions (When)}$$
```csharp
RuleFor(x => x.Field)
    .Cascade(CascadeMode.Stop)             // 1. Stop behavior
    .NotEmpty()                            // 2. Existence
    .Matches("^[0-9]+$")                  // 3. Format/Type
    .MustAsync(CheckDatabase)              // 4. Heavy/External Logic
    .When(x => x.ShouldValidate);          // 5. Overall Trigger Condition
```

---

### Rule: Where to put `.When()` conditions
- The position of `.When()` changes what it applies to. You can use it as a 
	- **Global line condition**
	- **Targeted rule condition**.
#### Scenario A: Put `.When()` at the very end (Applies to the whole line)

If you place `.When()` at the end of a chain, it wraps around **all** the rules on that line.
```csharp
RuleFor(x => x.TaxId)
    .NotEmpty()
    .Length(9)
    .When(x => x.IsCorporateCustomer); 
// BOTH NotEmpty and Length are skipped if IsCorporateCustomer is false.
```

#### Scenario B: Internal `.When()` (Applies only to previous rules)

If you place a `.When()` in the middle of a chain, it only protects the rules written _before_ it.
```csharp
RuleFor(x => x.PromoCode)
    .NotEmpty()
    .Must(BeValidPromo).When(x => x.HasPromoCode) // Only BeValidPromo is conditional
    .MaximumLength(10); // MaximumLength runs NO MATTER WHAT
```


> [!NOTE]
> Becarful while using `when()` it break scalar documentation. when scalar sees its conditional validation it just neglect it. Use when in optional properties only.

---
### Rule: The `Cascade` Rule (How to stop on first error)

By default, FluentValidation evaluates **every single rule** in the chain, even if the first one fails. If you send an empty string to a chain with 5 formatting rules, it might return 5 different validation errors for that single field.

To stop the chain as soon as one rule fails, use `.Cascade(CascadeMode.Stop)`:
```csharp
RuleLevelCascadeMode = CascadeMode.Stop;

// or inside each property.
RuleFor(x => x.Username)
    .Cascade(CascadeMode.Stop) // Stop immediately if any rule below fails!
    .NotEmpty()
    .MinimumLength(5)
    .MustAsync(BeUniqueInDatabase); 
```


---
### Run validations from other classes
```csharp
RuleForEach(x => x.ImagesVaraintList) 
	.SetValidator(new UpdateProductImagesOrderValidator());
```