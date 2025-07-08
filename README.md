
# 📡 Laravel HTTP Requests - Summary

Laravel provides a powerful, object-oriented approach to handle HTTP requests using the `Illuminate\Http\Request` class. This README summarizes the core features and usage patterns.

---

## 📥 Accessing the Request

You can inject the request via **dependency injection** in controllers or route closures:

```php
use Illuminate\Http\Request;

public function store(Request $request) {
    $name = $request->input('name');
}
```

With routes:
```php
Route::get('/', function (Request $request) {
    // ...
});
```

### Route Parameters with Requests
```php
public function update(Request $request, string $id) {
    // Use $request and $id
}
```

---

## 🛣 Request Information

### Path and URL
- `path()`: returns path part of the URL (`foo/bar`)
- `is()`: checks if path matches a pattern
- `routeIs()`: checks if matched route name matches

### Full URLs
- `url()`: base URL (without query string)
- `fullUrl()`: full URL (with query string)
- `fullUrlWithQuery([...])`
- `fullUrlWithoutQuery([...])`

### Host and Scheme
```php
$request->host();
$request->httpHost();
$request->schemeAndHttpHost();
```

### HTTP Method
```php
$request->method();
$request->isMethod('post');
```

---

## 🧾 Headers & IP

- `header('X-Header')`, `hasHeader()`
- `bearerToken()` – for Authorization header
- `ip()`, `ips()` – client IP(s)

---

## 📄 Content Negotiation

```php
$request->getAcceptableContentTypes();
$request->accepts(['application/json']);
$request->prefers(['html', 'json']);
$request->expectsJson();
```

---

## 🔁 PSR-7 Requests

Laravel can convert to/from **PSR-7** requests with:
```bash
composer require symfony/psr-http-message-bridge nyholm/psr7
```

---

## 📥 Retrieving Input

### Get All Input
```php
$request->all();
$request->collect(); // As Collection
```

### Single Inputs
```php
$request->input('name', 'default');
$request->query('name', 'default');
$request->input('products.0.name');
```

### Typed Inputs
- `string('field')->trim()`
- `integer('field')`
- `boolean('field')`
- `array('field')`
- `date('field')`
- `enum('status', Status::class)`

### Dynamic Access
```php
$request->name;
```

---

## ✂️ Filtering Input

- `only('name', 'email')`
- `except('token')`

---

## ✅ Checking Input Presence

- `has('field')`, `hasAny([...])`
- `filled('field')`, `isNotFilled('field')`
- `anyFilled([...])`
- `missing('field')`

Conditional execution:
```php
$request->whenFilled('name', fn ($name) => ...);
$request->whenMissing('name', fn () => ...);
```

---

## 🔄 Merging Input

```php
$request->merge(['key' => 'value']);
$request->mergeIfMissing(['key' => 'value']);
```

---

## ♻️ Old Input (Session)

- Flash input:  
  ```php
  $request->flash();
  $request->flashOnly(['username']);
  $request->flashExcept('password');
  ```

- With redirect:
  ```php
  return redirect()->back()->withInput();
  ```

- Get old input:
  ```php
  old('username');
  ```

---

## 🍪 Cookies

```php
$request->cookie('name');
```

Cookies are encrypted and signed by default.

---

## ✂️ Input Normalization

By default:
- **Trims strings**
- **Converts empty strings to null**

To disable globally (in `bootstrap/app.php`):
```php
$middleware->remove([TrimStrings::class, ConvertEmptyStringsToNull::class]);
```

Or selectively:
```php
$middleware->trimStrings(except: [fn (Request $r) => $r->is('admin/*')]);
```

---

## 📁 File Uploads

### Get Uploaded File
```php
$request->file('photo');
$request->photo;
```

### Check Presence & Validity
```php
$request->hasFile('photo');
$request->file('photo')->isValid();
```

### File Info
```php
$path = $request->photo->path();
$extension = $request->photo->extension();
```

### Store File
```php
$request->photo->store('images');
$request->photo->store('images', 's3');
$request->photo->storeAs('images', 'file.jpg');
```

---

## 🛡 Trusted Proxies & Hosts

Configure **trusted proxies** for HTTPS and headers:

```php
$middleware->trustProxies(at: ['192.168.1.1']);
$middleware->trustProxies(headers: Request::HEADER_X_FORWARDED_ALL);
```

To trust all proxies:
```php
$middleware->trustProxies(at: '*');
```

Restrict Laravel to specific hosts:
```php
$middleware->trustHosts(at: ['example.com'], subdomains: false);
```

---

## ✅ Final Tip

Always validate your request input before processing! Use Laravel's validation system to ensure clean, secure data.
