- [ ] Counting Sort, O(N+K) (where K is the range of numbers)
- [ ] Radix Sort, O(N⋅w) (where w is the number of bits in the integer).


Problem
- [ ] https://www.geeksforgeeks.org/dsa/length-of-longest-subarray-in-which-elements-greater-than-k-are-more-than-elements-not-greater-than-k/
- [ ] https://www.geeksforgeeks.org/dsa/find-subarray-with-given-sum-in-array-of-integers/
- [ ] https://www.geeksforgeeks.org/dsa/given-a-sorted-and-rotated-array-find-if-there-is-a-pair-with-a-given-sum/


```mermaid
erDiagram
  "dbo.Address" {
    Id uniqueidentifier PK
    Country nvarchar(max) 
    City nvarchar(100) 
    ZipCode nvarchar(20) 
    Description nvarchar(max) 
  }
  "dbo.AspNetUsers" {
    Id nvarchar(450) PK
    FirstName nvarchar(max) 
    LastName nvarchar(max) 
    CreatedAt datetime2 
    UpdatedAt datetime2 
    UserName nvarchar(256)(NULL) 
    NormalizedUserName nvarchar(256)(NULL) 
    Email nvarchar(256)(NULL) 
    NormalizedEmail nvarchar(256)(NULL) 
    EmailConfirmed bit 
    PasswordHash nvarchar(max)(NULL) 
    SecurityStamp nvarchar(max)(NULL) 
    ConcurrencyStamp nvarchar(max)(NULL) 
    PhoneNumber nvarchar(max)(NULL) 
    PhoneNumberConfirmed bit 
    TwoFactorEnabled bit 
    LockoutEnd datetimeoffset(NULL) 
    LockoutEnabled bit 
    AccessFailedCount int 
    AddressId uniqueidentifier(NULL) FK
  }
  "dbo.AspNetUsers" }o--|| "dbo.Address" : FK_AspNetUsers_Address_AddressId
  "dbo.Order" {
    Id uniqueidentifier PK
    CreatedAt datetime2 
    AddressId uniqueidentifier FK
    UserId nvarchar(450) FK
  }
  "dbo.Order" }o--|| "dbo.Address" : FK_Order_Address_AddressId
  "dbo.Order" }o--|| "dbo.AspNetUsers" : FK_Order_AspNetUsers_UserId
  "dbo.Product" {
    Id uniqueidentifier PK
    Name nvarchar(max) 
    Description nvarchar(max) 
    BasePrice decimal(18-2) 
  }
  "dbo.ProductVariant" {
    Id uniqueidentifier PK
    Color nvarchar(max) 
    Size nvarchar(max) 
    ColorHex nvarchar(max)(NULL) 
    ColorSwatchUrl nvarchar(max)(NULL) 
    Price decimal(18-2) 
    Stock int 
    SKU nvarchar(max) 
    IsActive bit 
    ProductId uniqueidentifier FK
  }
  "dbo.ProductVariant" }o--|| "dbo.Product" : FK_ProductVariant_Product_ProductId
```
