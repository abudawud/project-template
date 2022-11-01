# Project Template

All my project starting point for all tech stack

## Laravel - AdminLTE

### 1. Init project
```bash
composer create-project laravel/laravel .
# install adminlte template
composer require jeroennoten/laravel-adminlte
# install fority for login flow etc
composer require laravel/fortify 
# install permission manager
composer require spatie/laravel-permission 
# install debugbar
composer require barryvdh/laravel-debugbar
# install datatables server side library for laravel
composer require yajra/laravel-datatables-oracle

# -----------------------
# PUBLISH REQUIRED CONFIG
# -----------------------
# install and publish adminlte config
php artisan adminlte:install --with basic_views --with main_views
# publish spatie permission
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
# publish fortify
php artisan vendor:publish --provider="Laravel\Fortify\FortifyServiceProvider"
# publish debugbar
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```

### 2. Other dependencies
```bash
# install excel and downgrade psr/simple-cache
composer require psr/simple-cache:^2.0
composer require maatwebsite/excel
# install dompdf
composer require barryvdh/laravel-dompdf

# -----------------------
# PUBLISH REQUIRED CONFIG
# -----------------------
# publish excel
php artisan vendor:publish --provider="Maatwebsite\Excel\ExcelServiceProvider" --tag=config
# publish dompdf
php artisan vendor:publish --provider="Barryvdh\DomPDF\ServiceProvider"
```

**NOTE:** *make sure database configured correctly and all migration done before step up to the the next part*

### 3. Configure Fortify

1. Register foritify service provider to `config/app.php`
2. Specify view in `FortifyServiceProvider` boot
```php
        Fortify::loginView(function () {
            return view('vendor.adminlte.auth.login');
        });
```

### 4. Finally

Costomize all think that we need

#### 4.1 Custom `users` table and username

1. Add DB connection to `config/database.php` if the table on the other database host.
2. Overide `$connection` and `$table` in User model
3. Change login view to accept non email username
4. Configure fortify auth to meet our requirements
```php
        Fortify::authenticateUsing(function (Request $request) {
            $user = User::where('username', $request->email)->first();

            if ($user && (md5($request->password) == $user->password)) {
                return $user;
            }
        });
```
