Title: Policies
Description: Policies give you expressive control over user action authorization. 

---

## Getting Started

Policies give you expressive control over user action authorization. For this reason, policies go hand-in-hand with WordPress roles and capabilities.

For example, locate your `app/Auth/PostPilicy`.  

```php
class PostPolicy extends Policy  
{  
    public function update(AuthUser $auth, WPPost $post)  
    {  
        if( $auth->isCapable('edit_posts') || $auth->getID() == $post->getUserID()) {  
            return true;  
        }  
  
        return false;  
   }

  // ...
  
}
```

As you can see with the `PostPolicy`  you can contain the logic for authenticating a logged-in user can `update` a specific model. Note, the `AuthUser` class loads the currently logged-in user.

## Make A Policy

To make a policy:

1. Add a class to your `app/Auth` folder.
2. Update your `AuthServices` with what class(es) should map to the policy.

### 1. Creating the Class

To create a policy class file, you can use the galaxy CLI or do so manually using the template code to follow. 

```
php galaxy make:policy PersonPolicy
```

Creates,

```php
<?php  
namespace App\Auth;  
  
use \App\Models\User;
use \TypeRocket\Models\AuthUser;
use \TypeRocket\Auth\Policy;
  
class PersonPolicy extends Policy  
{  
    public function delete(AuthUser $user, $object) {}  
    public function update(AuthUser $user, $object) {}  
    public function create(AuthUser $user, $object) {}  
    public function read(AuthUser $user, $object) {}  
}
```

### 2. Map Policy

Supposing you had a `Person` model, you might map the `PersonPolicy`  to that `Person` model. However, you can map any class you like, not just a model, to a policy.

```php
<?php  
namespace App\Services;  
  
use TypeRocket\Services\Authorizer;  
  
class AuthService extends Authorizer  
{  
    protected $policies = [
       // Your Policies
       '\App\Models\Person' => '\App\Auth\PersonPolicy',
   
       // Models  
	  '\App\Models\Post' => '\App\Auth\PostPolicy',  
	  '\App\Models\Page' => '\App\Auth\PagePolicy',  
	  '\App\Models\Attachment' => '\App\Auth\PostPolicy',  
	  '\App\Models\Tag' => '\App\Auth\TermPolicy',  
	  // ...
	 
  ];  
}
```

## Using A Policy

Using our example policy and model, you might also have a controller `PersonController`. From that controller, you could access the mapped model's `Policy`  using the `can()` method.

```php
<?php
namespace TypeRocket\Controllers;  

use TypeRocket\Models\AuthUser;
  
class PersonController extends Controller  
{  
  function show(AuthUser $user, $id = null )  
  {
	$model = (new \App\Models\Person)->find($id);  

	if(!$model->can('read', $user)) {  
		tr_abort(401);  
	}
	
	return $model;  
  }
}
```

While model classes have the `can()` method for quickly load a policy, other classes do not have a `can()` method. For all other classes you can use the `tr_auth()` function to locate its mapped policy.

```php
// We use a model here but it 
// can be any mapped class
$person = (new \App\Models\Person)->find($id);

// Check if the current user
// can read the model
if(!tr_auth('read', $person)) { 
   throw new \Exception('Access denied.');
}
```

Or, check a specific user's access.

```php
// We use a model here but it 
// can be any mapped class
$person = (new \App\Models\Person)->find($id);
$user = (new \App\Models\User)->find($id);

// Check mapped policy
if(!tr_auth('read', $person, $user)) {
   throw new \Exception('Access denied.');
}
```

Or, override the mapped policy looked up.

```php
// We use a model here but it 
// can be any mapped class
$person = (new \App\Models\Person)->find($id);
$user = (new \App\Models\User)->find($id);
$policy = new \App\Auth\PersonPolicy;

// Override mapped policy
if(!tr_auth('read', $person, $user, $policy)) {
   throw new \Exception('Access denied.');
}
```