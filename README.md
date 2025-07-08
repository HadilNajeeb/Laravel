# Laravel Controllers

## 📘 Introduction
Controllers in Laravel help organize request handling logic into classes instead of writing everything in route files.  
For example, `UserController` can handle logic related to showing, creating, editing, or deleting users.

🗂️ Default location: `app/Http/Controllers`

---

## 🛠 Writing Controllers

### 🔹 Create a Controller
```bash
php artisan make:controller UserController
public function show(string $id): View {
    return view('user.profile', ['user' => User::findOrFail($id)]);
}
Route::get('/user/{id}', [UserController::class, 'show']);

class ProvisionServer extends Controller {
    public function __invoke() {
        // Action logic
    }
}

Route::post('/server', ProvisionServer::class);
Route::get('/profile', [UserController::class, 'show'])->middleware('auth');
use Illuminate\Routing\Controllers\HasMiddleware;
use Illuminate\Routing\Controllers\Middleware;

class UserController extends Controller implements HasMiddleware {
    public static function middleware(): array {
        return [
            'auth',
            new Middleware('log', only: ['index']),
            new Middleware('subscribed', except: ['store']),
        ];
    }
}

use Closure;
use Illuminate\Http\Request;

public static function middleware(): array {
    return [
        function (Request $request, Closure $next) {
            return $next($request);
        },
    ];
}

php artisan make:controller PhotoController --resource

Route::resource('photos', PhotoController::class);

| Verb      | URI                  | Action  | Route Name     |
| --------- | -------------------- | ------- | -------------- |
| GET       | /photos              | index   | photos.index   |
| GET       | /photos/create       | create  | photos.create  |
| POST      | /photos              | store   | photos.store   |
| GET       | /photos/{photo}      | show    | photos.show    |
| GET       | /photos/{photo}/edit | edit    | photos.edit    |
| PUT/PATCH | /photos/{photo}      | update  | photos.update  |
| DELETE    | /photos/{photo}      | destroy | photos.destroy |

Route::resource('photos', PhotoController::class)->only(['index', 'show']);
Route::resource('photos', PhotoController::class)->except(['create', 'edit']);

Route::apiResource('photos', PhotoController::class);
php artisan make:controller PhotoController --api

Route::resources([
    'photos' => PhotoController::class,
    'posts' => PostController::class,
]);

Route::apiResources([
    'photos' => PhotoController::class,
    'posts' => PostController::class,
]);

Route::resource('photos.comments', PhotoCommentController::class);

Route::resource('photos.comments', PhotoCommentController::class)->scoped([
    'comment' => 'slug',
]);

Route::resource('users', AdminUserController::class)->parameters([
    'users' => 'admin_user'
]);
Route::resource('users', AdminUserController::class)->parameters([
    'users' => 'admin_user'
]);

Route::resourceVerbs([
    'create' => 'crear',
    'edit' => 'editar',
]);

Route::get('/photos/popular', [PhotoController::class, 'popular']);
Route::resource('photos', PhotoController::class);

Route::apiSingleton('profile', ProfileController::class);
Route::apiSingleton('photos.thumbnail', ProfileController::class)->creatable();

Route::resource('users', UserController::class)
    ->middleware(['auth', 'verified']);

Route::middleware(['auth', 'verified', 'subscribed'])->group(function () {
    Route::resource('users', UserController::class)
        ->withoutMiddlewareFor('index', ['auth', 'verified'])
        ->withoutMiddlewareFor(['create', 'store'], 'verified')
        ->withoutMiddlewareFor('destroy', 'subscribed');
});

public function store(Request $request) {
    $name = $request->name;
}

public function update(Request $request, string $id) {
    // Update logic
}

