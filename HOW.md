## Main stages of development

#### 1. Installation Project Template. Create Database

`composer create-project --prefer-dist  laravel/laravel .`   
`npm install`  
`npm run dev`  

`git init`  
`git add .`  
`git commit –m "Comment"`  
**`git remote add origin https://github.com/SergeyHub/roles-permissions.git`**  
`git push -u origin master`  

##### 1.1 Postgersql
```
Let's start SQL Shell (psql). The program will prompt you to enter the name    
of the server, database, port and user. You can click/skip these items as they  
will use the default values   
(for server - localhost, for database - postgres, for port - 5432,  
as user - postres superuser). 
Next, you will need to enter a password for the user   
(by default, the postgres user): 123456 (in my case)  
```

![Screenshot](readme/psql.JPG)   

`postgres=# create database db_name;`  
  **database list**  
`select datname from pg_database;`   
pg_dump dbname > outfile 

**`Edit  env. file`**    
```
DB_CONNECTION=pgsql
DB_HOST=localhost
DB_PORT=5432
DB_DATABASE=cargo
DB_USERNAME=postgres
DB_PASSWORD=123456
```
##### 1.2 MySQL

`mysql -u root -p`  
`create database roles_permissions; db_name;`  
`drop database db_name;`   
`show databases;`  
`use db_name;`  
`show tables;`   
`drop table table_name;`  
`exit`  

**`Edit  env. file`**   
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=roles_permissions
DB_USERNAME=root
DB_PASSWORD=123456
```
##### 1.3 Migration

`php artisan migrate`  

##### 1.4 Version
`npm -v`  
`php -v`
#### 2. Install Auth Migration
```
composer require laravel/ui
php artisan ui bootstrap   
php artisan ui bootstrap --auth
npm install
npm run dev
php artisan migrate
```
#### 3. Install Spatie package for Access Control List (ACL)
[spatie/Laravel Premission https://github.com/spatie/laravel-permission](https://github.com/spatie/laravel-permission)   

`composer require spatie/laravel-permission`  
`composer require laravelcollective/html`  

**config/app.php**

```
'providers' => [
	....
	Spatie\Permission\PermissionServiceProvider::class,
],
```
`php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"`  
`php artisan migrate`

#### 4. Product Model Controller Table
`php artisan make:model Product -m`  
`php artisan make:controller ProductController --resource`  
`php artisan migrate`

**product table**
```
$table->string('name');
$table->text('detail');
```
`php artisan migrate:refresh`  

#### 5. Product Model Add Middleware

**app/Models/Product.php**
```
protected $fillable = [
    'name', 'detail'
];
```
**app/Http/Kernel.php**
```
'role' => \Spatie\Permission\Middlewares\RoleMiddleware::class,
'permission' => \Spatie\Permission\Middlewares\PermissionMiddleware::class,
'role_or_permission' => \Spatie\Permission\Middlewares\RoleOrPermissionMiddleware::class,
```

#### 6. Home User Role Controller
#### 7. Views 
