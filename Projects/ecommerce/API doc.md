The goal is a **RESTful API-first backend** (API-only) that a frontend (React/Vue/Thymeleaf admin panel) can consume. It will support:
- **Admin panel** features (product/inventory/order/discount management)
- **Customer-facing storefront** features (catalog, cart, checkout)
- Most core Shopify capabilities: products + variants, collections, inventory, orders + fulfillment, customers, discounts, basic shipping/taxes, payments (mock or real gateway), and simple analytics.

---
### Recommended Tech Stack (Spring Boot 3.x)
- **Core**: Spring Web, Spring Data JPA, Spring Security + JWT (or OAuth2)
- **DB**: PostgreSQL (or MySQL) + Flyway/Liquibase for migrations
- **Validation**: Jakarta Validation + custom DTOs
- **File storage**: Local + AWS S3 (or Cloudinary) for product images
- **Docs**: OpenAPI/Swagger (springdoc-openapi)
- **Testing**: JUnit + Testcontainers + RestAssured
- **Extras**: Lombok, MapStruct, Spring Boot Actuator, Micrometer (for basic metrics)

**Main JPA Entities**:
- `User` (with roles: `ROLE_ADMIN`, `ROLE_CUSTOMER`)
- `Product` + `ProductVariant` (size/color/etc.) + `ProductImage`
- `Inventory` (or embedded in Variant with quantity + low-stock alerts)
- `Category` / `Collection`
- `Cart` + `CartItem` (one cart per user or guest cart via token)
- `Order` + `OrderItem` + `Address` (billing/shipping)
- `Discount` / `Coupon` (price rules)
- `Payment` (transaction record)

Use **DTOs** heavily for requests/responses to avoid exposing entities.

---
### API Structure & Endpoints
All endpoints under `/api/v1`.  
Use **Bearer JWT** auth.  
Admin routes protected with `@PreAuthorize("hasRole('ADMIN')")`.  
Public routes open (or require auth for cart/orders).  
Add pagination (`?page=0&size=20&sort=createdAt,desc`), filtering (`?categoryId=5&minPrice=10&maxPrice=100&search=keyword`), and sorting everywhere possible.

---

#### 1. Authentication & Users
| Method | Endpoint            | What it does                                           | Auth   |
| ------ | ------------------- | ------------------------------------------------------ | ------ |
| POST   | `/auth/register`    | Customer registration (email/password + basic profile) | Public |
| POST   | `/auth/login`       | Login → returns JWT + user info                        | Public |
| POST   | `/auth/refresh`     | Refresh token                                          | Public |
| GET    | `/me`               | Get current user profile + addresses                   | User   |
| PUT    | `/me`               | Update profile/addresses                               | User   |
| POST   | `/me/addresses`     | Add shipping/billing address                           | User   |
| GET    | `/admin/users`      | List/filter customers (pagination)                     | Admin  |
| GET    | `/admin/users/{id}` | View single customer + orders                          | Admin  |
| PATCH  | `/admin/users/{id}` | Ban/suspend or update role                             | Admin  |

#### 2. Products & Catalog (Admin + Public)
**Admin** (full CRUD + Shopify-like features):

| Method | Endpoint                         | What it does                                                                 |
| ------ | -------------------------------- | ---------------------------------------------------------------------------- |
| POST   | `/admin/products`                | Create product (title, description, price, variants array, images multipart) |
| GET    | `/admin/products`                | List all products (with variants, inventory)                                 |
| GET    | `/admin/products/{id}`           | Full product details                                                         |
| PUT    | `/admin/products/{id}`           | Update product + variants                                                    |
| DELETE | `/admin/products/{id}`           | Delete product                                                               |
| POST   | `/admin/products/{id}/variants`  | Add variant                                                                  |
| PATCH  | `/admin/products/{id}/inventory` | Bulk adjust inventory levels                                                 |
| POST   | `/admin/products/{id}/images`    | Upload multiple images                                                       |

**Public** (storefront):

| Method | Endpoint             | What it does                                             |
| ------ | -------------------- | -------------------------------------------------------- |
| GET    | `/products`          | Paginated list + filters (category, price, search, tags) |
| GET    | `/products/{id}`     | Detailed view (variants, images, inventory status)       |
| GET    | `/products/featured` | Featured/new products                                    |

#### 3. Collections / Categories
| Method     | Endpoint                  | What it does                        | Auth   |
| ---------- | ------------------------- | ----------------------------------- | ------ |
| POST       | `/admin/collections`      | Create collection (smart or manual) | Admin  |
| GET        | `/admin/collections`      | List collections                    | Admin  |
| PUT/DELETE | `/admin/collections/{id}` | Update/delete                       | Admin  |
| GET        | `/collections`            | Public list of collections          | Public |
| GET        | `/collections/{handle}`   | Products in a collection            | Public |

#### 4. Cart Management
Support both **logged-in** (persistent) and **guest** (cart token in header/cookie).

| Method | Endpoint               | What it does                                          |
| ------ | ---------------------- | ----------------------------------------------------- |
| GET    | `/cart`                | Get current cart (items + totals + applied discounts) |
| POST   | `/cart/items`          | Add item `{productId, variantId, quantity}`           |
| PUT    | `/cart/items/{itemId}` | Update quantity                                       |
| DELETE | `/cart/items/{itemId}` | Remove item                                           |

#### 5. Orders & Checkout (Core Shopify flow)
| Method | Endpoint                     | What it does                                                                                | Auth  |
| ------ | ---------------------------- | ------------------------------------------------------------------------------------------- | ----- |
| POST   | `/orders/checkout`           | Create order from cart + shipping address + payment method (returns order + payment intent) | User  |
| GET    | `/orders`                    | My orders (paginated)                                                                       | User  |
| GET    | `/orders/{id}`               | Single order details                                                                        | User  |
| GET    | `/admin/orders`              | All orders (filter by status, date, customer)                                               | Admin |
| GET    | `/admin/orders/{id}`         | Order details + line items                                                                  | Admin |
| PATCH  | `/admin/orders/{id}/status`  | Update status (pending → confirmed → shipped → delivered)                                   | Admin |
| POST   | `/admin/orders/{id}/fulfill` | Mark as fulfilled + tracking number                                                         | Admin |
| POST   | `/admin/orders/{id}/cancel`  | Cancel + refund logic                                                                       | Admin |

#### 6. Payments (Mock first, then real)

| Method | Endpoint                                | What it does                                               |
| ------ | --------------------------------------- | ---------------------------------------------------------- |
| GET    | `/user/{userId}/checkout`               | Shows cart and address options before actual payment       |
| POST   | `/user/{userId}/order/`                 | Create order and intention in paymob.                      |
| POST   | `/user/{userId}/order/{orderId}/repay`  | Repay failed payment flow of order                         |
| POST   | `/user/{userId}/order/{orderId}/refund` |                                                            |
| POST   | `/paymob-payment-hook`                  | Called by paymob to tell us what happend in order payment. |

#### 7. Shipping & Taxes (Basic version)
- GET `/shipping/rates` → calculate rates based on address + cart weight/items (mock or use external API later)
- Taxes calculated automatically in checkout (use simple rules or TaxJar later)

#### 8. Analytics / Reports (Admin)
| Method | Endpoint                   | What it does                       |
| ------ | -------------------------- | ---------------------------------- |
| GET    | `/admin/reports/sales`     | Total sales, by period, by product |
| GET    | `/admin/reports/orders`    | Order stats + top products         |
| GET    | `/admin/reports/inventory` | Low-stock alerts                   |

---
### Implementation Tips (Step-by-Step for Practice)
1. **Start small** (MVP in 1-2 weeks):
   - Auth + User
   - Product CRUD + images (admin + public list)
   - Basic catalog search/filter

2. **Next**:
   - Cart + Order placement (with transactions!)

3. **Then**:
   - Variants + inventory
   - Discounts + checkout flow

4. **Advanced** (to reach "most Shopify features"):
   - Fulfillment & status updates
   - Reports
   - Webhooks (order created, payment success)
   - Soft deletes, audit logs, rate limiting

5. **Best practices**:
   - Use **Service + Repository** layers
   - Global exception handler + custom error responses
   - `@Transactional` on order/checkout
   - Event listeners (e.g., order placed → send email via Spring Mail or RabbitMQ later)
   - Version your API (`/api/v1`)

---
### Next Steps
- Generate the project on start.spring.io
- Add Swagger and test the first `/admin/products` endpoint
- Use Postman collection to document everything

---

| Field               | Value               |
| ------------------- | ------------------- |
| **Card Number**     | 5123 4567 8901 2346 |
| **Cardholder Name** | Test Account        |
| **Expiry Month**    | 01                  |
| **Expiry Year**     | 39                  |
| **CVV**             | 123                 |

```
4000000000000002

```

---

> [!NOTE]
> Don't forget to remove the wrong Uri paths of verify-email and forget-password, and frontend domain.


```
i have product management apis. admin can add,update,delete products and their images "each product can have multiple images" and products size and weights and products colors and thier colors image varaint "each color has 1 image". i have tried to do some of the frontend, so firstly the admin will enter basic product info like name, description, price "i have did this", but then after the product is created, he should be redirected to "update product page" which inside of it we can do more things in the product. there should be section for adding and deleting product images, the images order matters so try to find a way to represent the images order "there is api for update the order of product images" then there should be section to add size, so he press on + or add size which shows Modal/Modal Window we should enter the size that we want to add "this can be list of valid sizes or any proper solution you see" and the weight of this size. and there should be section for adding color variants when admin try to add new color a Modal/Modal Window shows up, admin should enter color name, and color hex "Color Picker, find nice way to do it". then after press add there should be table that show the color name and hex and color image, inside this table we will be able to change the color image we can add or delete it, for both color and size variant there should be  Toggle Switch, which update the variant status between enable and disable. read ProductCommandService.cs and ProductCommandController.cs for full understanding
```

```
For collections. they can have the same displayOrder, when that happen we start to display based on name of collection.
I have added also maintenance mode, which block users from checkout, or proceed in payment flow, so when we this mode is enabled i should show to users that the website is currently under maintenance.
```

```
While user is putting his shipping address and billing address, i want to put switch that user will toggle if the billing address is not as shipping address when that happen there will be the default address of the user and "Choose another address" which user can choose from his address catalog and there should be button to add another address. This structure of having default address then address list should be also in shipping address.
```


```
I have added search for user by email function which will be used by admin to search for guessing the user email, after he find the right user, we will display the orders of this user only. don't trigger this API call on _every single keystroke_. Implement a **debounce** of about 300ms. This waits until the admin pauses typing before hammering your database with requests.
```

```
I have added reorder function, PutOrderInCart in ~/api/user/{userId}/order/{OrderId}/reorder which add order products in cart and the frontend should redirect user to cart. Admin may have made changes in database so for some products users won't be able to reorder, so if that happen i want to show for users after redirection a small notification message that says some order aren't avaible.
```