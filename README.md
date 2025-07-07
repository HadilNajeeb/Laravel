# Laravel Middleware

## 📌 What is Middleware?

Middleware in Laravel is a filtering mechanism that intercepts HTTP requests entering your application. It acts like a layer that requests must pass through before reaching routes or controllers.

It is commonly used for:
- Authentication & Authorization
- Logging
- CORS and headers management
- Maintenance checks
- CSRF protection

## 🛠 How Middleware Works

Each request in Laravel passes through a stack of middleware before reaching the application logic. Middleware can either allow the request to proceed or stop it (e.g., redirect or return a response). They can also modify the request or response.

Middleware can:
- Run logic **before** the request reaches the controller.
- Run logic **after** the controller handles the request.
- Be global (applied to all requests).
- Be route-specific (applied to certain routes only).
- Be grouped for organization.

## 🧱 Structure

A middleware class has a `handle` method which is called when a request is received. This method receives the request and a `$next` closure. If the middleware allows the request, it calls `$next($request)` to pass it to the next layer.

## 🧭 Types of Middleware

- **Global Middleware:** Registered to run on every request. Defined in `bootstrap/app.php`.
- **Route Middleware:** Assigned to specific routes.
- **Grouped Middleware:** Logical groups of multiple middleware (e.g., `web`, `api`).
- **Terminable Middleware:** Middleware with a `terminate` method that runs *after* the response is sent.

## 📋 Registration

- Middleware are stored in `app/Http/Middleware/`.
- You register middleware in `bootstrap/app.php` using:
  - `append()` or `prepend()` for global middleware.
  - `appendToGroup()` or `prependToGroup()` for grouped middleware.
  - `alias()` for creating short names for middleware classes.

## 🧩 Middleware Groups

Laravel includes two default middleware groups:
- **web**: For browser-based routes (sessions, CSRF, cookies, etc.)
- **api**: For stateless API routes (bindings, throttling, etc.)

You can customize these or create your own groups.

## ⚙️ Middleware Aliases

Aliases simplify usage of middleware with long class names. For example, `auth` is an alias for the authentication middleware. You can define your own aliases in `bootstrap/app.php`.

## ➕ Parameters in Middleware

You can pass additional parameters to middleware when assigning them to routes, such as user roles or access levels.

## 📐 Sorting and Priority

When middleware are added dynamically, Laravel allows you to define execution order using the `priority()` method. This ensures middleware run in a predictable sequence.

## ⏳ Terminable Middleware

Some middleware need to run logic *after* the response has been sent (like logging). Laravel supports this by allowing middleware to implement a `terminate()` method. This is useful for:
- Writing logs
- Cleaning up resources
- Background notifications

## ✅ Summary

- Middleware filters requests entering your Laravel app.
- You can define, register, and apply middleware globally or per route.
- Laravel includes built-in middleware like `auth`, `csrf`, and `throttle`.
- Middleware can be customized, grouped, aliased, and prioritized.
- Middleware supports post-response operations via the `terminate()` method.

## 📚 Laravel Middleware Is Essential

Middleware is essential for building secure, maintainable, and scalable Laravel applications. Mastering it empowers developers to control request flows with ease and clarity.
