- **PUT** is used for a **full replacement** of a resource.
- **PATCH** is used for **partial modifications** to a resource

Imagine a simple DTO for storing a phone book record:

```json
{
	"name": "John Smith",
	"phone": "5555555555",
	"address": "123 Fake Street",
	"city": "Nowhere",
	"zip": "90210"
}
```

Now lets say this is the data currently in the API's database.

---

If you send a PATCH request where `phone=1235554444` the data should now look like this:

```json
{
    "name": "John Smith",
    "phone": "1235554444",
    "address": "123 Fake Street",
    "city": "Nowhere",
    "zip": "90210"
}
```

---

If you send a PUT request where `phone=123555444` the data should now look like this:

```json
{
    "phone": "1235554444",
}
```

---
### Conclusion
- `PUT` tells the API that the values you're sending are replacing the existing value for the key.
- `PATCH` tells the API that you are only _amending_ the values you send for the key.