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
I have added also maintenance mode, which block users from checkout, or proceed in payment flow, so when this mode is enabled i should show to users that the website is currently under maintenance.
```

```
While user is putting his shipping address and billing address, i want to put switch that user will toggle if the billing address is not as shipping address when that happen there will be the default address of the user and "Choose another address" which user can choose from his address catalog and there should be button to add another address. This structure of having default address then address list should be also in shipping address.
```

---

## TODO
- [ ] Show the rest order informations like shipping fees, total fees. etc.
- [ ] Redirect user after verify email
- [ ] password weak
- [ ] When remove item from cart, the cart counter doesn't get updated unless we refresh the page.
- [ ] Interactions after deleting product, size variant, color variant.
- [x] Edit product show page, its looking bad.
- [x] Add Collection Editing to overview in admin panel.
- [x] Trim whitespaces. Let add, update functions set the first character as capital letter.
- [x] working on dark mode (front end )
- [x] gradinat of the main image.
- [x] fix add sizing.
- [x] Search by order ID.
- [x] Edit "sort by" in the shop for products.
- [x] Stock.
- [x] Cascade deleting size or color to stock.
- [x] **Code** in product.
- [x] Edit order details to accept **Code** information.
- [x] Refund, Cancel order interaction.
- [x] Add validation limit number of cart items.
- [x] Check Re-cart items.
	- Make redirection to the user for the product page, or shop.
- [x] Add product thumbnail image property to normal users. `Ask abdo`
- [x] Test when 2 users try to buy the same thing and it gets out of stock.
- [x] Add `Order.Status == RequireReview` in `ChangeOrderStatus` method
- [x] Order a product then do successfull payment while the paymob can't call our system backwards, and wait until the system makes the order expired.
- [x] Add logs
- [x] Add SKU in payment order intention.
- [x] remove header in product detail page.
- [x] first name and last name of address should be removed.
- [x] Make the Product images smaller "less zoom".
- [x] Any order stay authorized more than 2 weeks should be cancelled.
- [x] Add losses in order operations service.

### Shipment
- [x] Partial return. What if the user want to return some of the order, not the whole of it.
	- We can make workaround by implementing delete order item. and make this process as manual. 
- [ ] Remove Cancel order button from orders table. and make all the uncommon operations in order details page.
	- **Remove** `Cancel Order` from `completed` orders, so it should call `partial return` function.
- [ ] Add user cancel order if its not shipped yet.
- [ ] When we do return/exchange order, reveal panel to choose the pickupAddress. "Because the current shipping address maynot have pickup availability."
- [ ] Bosta isolates the **Extra COD Fee (80 EGP)** because, legally, cash-handling services are subject to a 14% VAT in Egypt ($80 \times 1.14 = 91.2\text{ EGP}$). Depending on your specific corporate contract with Bosta, the final `shippingFee` (564 EGP) might **not** include that extra $11.2\text{ EGP}$ VAT difference, as they often accumulate VAT and bill it to you on your weekly or monthly invoice.
- [ ] add rule for not exceding 30000 EGP.
- [ ] Packaging
	- Make it hybrid, some items are Flexible and some are Rigid. if its rigid you should specify the L X W X H otherwise you can use the volume to determine the left space.



---

## Issues
- [x] Pressing "All Collections" after specifing a collection doesn't work. "In the drop down menu in navbar"
- [x] Adding address is corrupted.
- [x] Wrong product status when update or delete variants.
- [ ] when deleting thumbnail image, we need to refresh the page to see the new thumbnail image.
- [x] Failing to connect to claudeFlare make data unconsistante. which doesn't create `transaction` for `order`.
- [x] Delete Color id and size id, issue relation with orderItem.
- [x] Change product images order doesn't get updated in UI.
- [x] Error in changing collection image.
- [ ] Strict validaiton on orderimages of product.
- [ ] Wrong passwrod
- [ ] grades-leather-saw-epson.trycloudflare.com, right backend faces.`frontend/build/static/js/main.04..., frontend/src/config.js`
- [ ] When do exchange, if we want to add another item with size and color variant the already exists in the order before. "Currently we create new order item. We should reuse the created one."
- [ ] Don't forget pickupAvailability, dropOffAvailability of bosta.

---
## Questions

1. What shall we do with `Require Check` state?
	- For now we will create endpoint to accept or refund the order.
2. Does paymob send that the user failed to pay or they only communicate with us when we do successfull transaction?
	- Yes it does, it sends back a feedback about the transaction payment status in both cases of failing and successful payment. 
	- If we didn't create intention then we try to query paymob about transaction using `intention_order_id` it will say `Order Not Found` but if we have created intention it will say `Transaction Not Found"`.
3. bosta
	- How can i predict the order size, and cost. 
		- For packaging do we use size or weight.
		- For cost predication is there any endpoint can i reach to compute the cost, or should i calculate it internally.
			- City or distinct
		- the size of the product can be varaint and it doesn't have fixed size because its a clothes
		-  What should i do if there is one big order which i can't fit in single box.
	- How to create CRP. And what is the flow?
		- `Pickup -> Delivered -> returned`. what actually those mean.
	- How to create Exchagne Request
		- And is there any packaging for the returned items ?
		- `exchange forward -> customer -> return to business`.
	- How to know the status of a shipping
	- is there 3 trials for each shippment? and if so, does the same shipping id will be rto or they create new rto.
		- how to know the rto fees, how much should pay.
	- How to terminate an order.
	- Flex shipping
	- what bosta do if the package specs doesn't meet with what we claim like package type
		- do they send message that this order will get more fees in shipping because of that
		- Or do they just take the money from the wallet without notificationl

---
## Concerncs
- Limit number of orders for each user per day. 

```
my current paymob integration is bad, and i want to integrate my application with bosta for shipping. the full flow of order is not sufficient. "currently the system doesn't use Authorize payments from customers which is bad because now if customer try to cancel order while its confirmed 'not shipped' he will pay fees".

rewrite paymob integration. 
and create bosta shipping integration

we support only cash on delivary or credit card using paymob and user pays paymob fees.
for COD, order will be confirmed and the user can cancel it while its not shipped. then we can make order shipped. if user didn't recieve the order "after the 3 trials from bosta" we will make it cancelled and put Losses in order the RTO "we will pay the RTO"
if user saw the order but didn't accept taken it. he will pay the shipping fee and we will make order returned. if he accept it we make order completed, now if user want to return the order. if user didn't answer the shipping courier after rto request, we will pay the rto "in losses" and keep the order completed. if he answered, he will pay RTO and we will make the order returned. 
"always keep admin able to put losses in orders"

but for credit card. order will be confirmed if we are able to authroize the payment amount from user. user can cancel order while its confiremd "without paying paymob fees" then make it cancelled. otherwise when we want to start ship we need to capture money then start shipping. if user didn't recieve order from bosta. the order is cancelled and we refund the user but we take from him RTO fees. if he recieved then its completed. then if he want to return and user didn't answer the shipping courier we will pay RTO "put in losses" and keep order as completed. if he answered, then we will refund him but we will take the rto amount and make order returned
```

---

```
Select Sum(Quantity)
```