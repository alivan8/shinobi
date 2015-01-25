Caffeinated Shinobi
===================
[![Laravel](https://img.shields.io/badge/Laravel-5.0-orange.svg?style=flat-square)](http://laravel.com)
[![Source](http://img.shields.io/badge/source-caffeinated/shinobi-blue.svg?style=flat-square)](https://github.com/caffeinated/shinobi)
[![License](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](https://tldrlegal.com/license/mit-license)

Overview
--------
Shinobi brings a simple and light-weight role-based permissions system to Laravel's built in Auth system. Shinobi brings support for the following ACL structure:

- Every user can have zero or more roles.
- Every role can have zero or more permissions.

Permissions are then inherited to the user through the user's assigned roles.

Installation
------------
Begin by installing the package through Composer. The best way to do this is through your terminal via Composer itself:

```
composer require caffeinated/shinobi
```

Migrations
----------
You'll need to run the provided migrations against your database. At the moment there is no publish method, so you'll need to manually copy the migrations to the correct location within your app.

You'll find the migration files within `vendor/caffeinated/shinobi/src/migrations`. Simply copy these to your application's migration directory `app/database/migrations`.

Usage
-----
The Caffeinated Shinobi package comes bundled with a ShinobiTrait file to be used within your User's Model file. This trait file provides all the necessary functions to tie your users in with roles and permissions.

**Example:**

```php
<?php

use Illuminate\Database\Eloquent\Model;
use Caffeinated\Shinobi\Traits\ShinobiTrait;

class User extends Model
{
	use ShinobiTrait;

	...
}
```

### Shinobi Trait Methods
The following methods will become available from your User model.

#### is($roleSlug)
Checks if the user is under the given role.

```php
Auth::user()->is('administrator');
```

You may also use magic methods:

```php
Auth::user()->isAdministrator();
```

#### can($permission)
Checks if the user has the given permission(s). You may pass either a string, or an array of permissions to check for. In the case of an array, ALL permissions must be accountable in order for this to return true.

```php
Auth::user()->can('access.admin');
```

or

```php
Auth::user()->can(['access.admin', 'view.users']);
```

#### assignRole($roleId)
Assign the given role to the user.

```php
Auth::user()->assignRole(1);
```

#### revokeRole($roleId)
Revokes the given role from the user.

```php
Auth::user()->revokeRole(1);
```

#### revokeAllRoles()
Revokes all roles from the user.

```php
Auth::user()->revokeAllRoles();
```

#### syncRoles([$roleIds])
Syncs the given roles with the user. This will revoke any roles not supplied.

```php
Auth::user()->syncRoles([1, 2, 3]);
```
