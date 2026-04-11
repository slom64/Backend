
## Core MVC Action Results

| Action Result            | Description                        | Typical Use Case                     |
| ------------------------ | ---------------------------------- | ------------------------------------ |
| `ViewResult`             | Renders a Razor view (`.cshtml`)   | Returning UI pages                   |
| `PartialViewResult`      | Renders a partial view (no layout) | AJAX updates, reusable UI components |
| `ContentResult`          | Returns raw string content         | Plain text / custom responses        |
| `JsonResult`             | Returns JSON-formatted data        | APIs, AJAX calls                     |
| `FileResult`             | Returns a file (various subtypes)  | Downloads (PDF, images, etc.)        |
| `RedirectResult`         | Redirects to a URL                 | External or internal redirect        |
| `RedirectToActionResult` | Redirects to another action        | Post-Redirect-Get pattern            |
| `RedirectToRouteResult`  | Redirects using route values       | Advanced routing scenarios           |
| `StatusCodeResult`       | Returns HTTP status code only      | API responses (e.g. 404, 500)        |

---

## Common FileResult Subtypes

| Type                 | Description                     |
| -------------------- | ------------------------------- |
| `FileContentResult`  | Returns file from byte[]        |
| `FileStreamResult`   | Returns file from stream        |
| `VirtualFileResult`  | Returns file from virtual path  |
| `PhysicalFileResult` | Returns file from physical disk |

---

## Modern ASP.NET Core Convenience Methods (Controller Helpers)

These are **helper methods** that return the above results:

| Method               | Returns                  | Description        |
| -------------------- | ------------------------ | ------------------ |
| `View()`             | `ViewResult`             | Render a view      |
| `PartialView()`      | `PartialViewResult`      | Render partial     |
| `Json()`             | `JsonResult`             | Return JSON        |
| `Content()`          | `ContentResult`          | Return string      |
| `File()`             | `FileResult`             | Return file        |
| `Redirect()`         | `RedirectResult`         | Redirect to URL    |
| `RedirectToAction()` | `RedirectToActionResult` | Redirect to action |
| `NotFound()`         | `NotFoundResult`         | 404 response       |
| `BadRequest()`       | `BadRequestResult`       | 400 response       |
| `Ok()`               | `OkObjectResult`         | 200 with data      |
| `Unauthorized()`     | `UnauthorizedResult`     | 401                |
| `Forbid()`           | `ForbidResult`           | 403                |

---

## API-Oriented Results (Important in modern ASP.NET Core)

| Result                   | Description           |
| ------------------------ | --------------------- |
| `OkObjectResult`         | 200 + data            |
| `CreatedResult`          | 201 + location header |
| `NoContentResult`        | 204 no body           |
| `BadRequestObjectResult` | 400 + error details   |
| `NotFoundResult`         | 404                   |
| `UnauthorizedResult`     | 401                   |

---

```csharp
public IActionResult Index()
// or
public ActionResult<MyModel> Get()
```