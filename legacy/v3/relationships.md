Title: Relationships
Description: Define the relationships of models.

---

Model relationships define how records are related in the database. The main relationship types include: "one to one", "one to many", and "many to many".

## One to One

In a [one to one](https://en.wikipedia.org/wiki/One-to-many_(data_model)) relationship, one record can have only one reference to another record and vice-versa. For example, one `Seat` can have one `Person` if you are booking a plane flight.

### Implementation

Take the plane flight example where one `Person` and have one `Seat`. For the `Person` model lets assume there is a table named `wp_people` with the columns `id`, `name`, and `email`. For the `Seat` model lets assume there is a table named `wp_seats` with the columns `id`, `person_id`, `seat_number`, and `price`. How would you represent the database relationship in your TypeRocket models?

Take a look at the `Person` model.

```php
<?php
namespace App\Models;

use \TypeRocket\Models\Model;

class Person extends Model
{
    protected $resource = 'people';

    /**
     * Get the seat record associated with the person.
     */
    public function seat()
    {
        return $this->hasOne('\App\Models\Seat', 'person_id');
    }
}
```

And, look at the `Seat` model.

```php
<?php
namespace App\Models;

use \TypeRocket\Models\Model;

class Seat extends Model
{
    protected $resource = 'seats';

    /**
     * Get the person record associated with the seat.
     */
    public function person()
    {
        return $this->belongsTo('\App\Models\Person', 'person_id');
    }
}
```

Now, you can access the `Seat` from the `Person` using the `seat()` method. Take a look at the example where `$person` is an instance of `Person`.

```php
$seat = $person->findById(1)->seat()->get();
```

You can also access the `Person` in a `Seat`. Take a look at the example where `$seat` is an instance of `Seat`.

```php
$seat = $seat->findById(1)->person()->get();
```

## One To Many

In [one to many](https://en.wikipedia.org/wiki/One-to-many_(data_model)) relationships, one record can have many other related records. For example, one `Game` can have many a `Player` in a video game.

### Implementation

Take the video game example where one `Game` and have many a `Player`. For the `Game` model lets assume there is a table named `wp_games` with the columns `id` and `name`. For the `Player` model lets assume there is a table named `wp_players` with the columns `id`, `games_id` and `name`. How would you represent the database relationship in your TypeRocket models?

Take a look at the `Game` model.

```php
<?php
namespace App\Models;

use \TypeRocket\Models\Model;

class Game extends Model
{
    protected $resource = 'games';

    /**
     * Get the player records associated with the game.
     */
    public function players()
    {
        return $this->hasMany('\App\Models\Player', 'games_id');
    }
}
```

And, look at the `Player` model.

```php
<?php
namespace App\Models;

use \TypeRocket\Models\Model;

class Player extends Model
{
    protected $resource = 'players';

    /**
     * Get the game record associated with the player.
     */
    public function game()
    {
        return $this->belongsTo('\App\Models\Game', 'games_id');
    }
}
```

Now, you can access the `Player` records from the `Game` using the `players()` method. Take a look at the example where `$game` is an instance of `Game`.

```php
$players = $game->findById(1)->players()->get();
```

You can also access the `Game` from a `Player`. Take a look at the example where `$player` is an instance of `Player`.

```php
$game = $player->findById(1)->game()->get();
```

## Many to Many

In [many to many](https://en.wikipedia.org/wiki/Many-to-many_(data_model)) relationships, many records can have many other related records. For example, many an `Author` can have many a `Book` and vice-versa in a bookstore.

### Implementation

Many to many relationships are a more complicated topic. For brevity, just remember that many to many relationships require a "junction table". A junction table simply stores the relationship associations as records.

Take the bookstore example. For the junction table lets assume it is named `wp_authors_books` with the columns `authors_id` and `books_id`. For the `Author` model lets assume there is a table named `wp_authors` with the columns `id` and `name`. For the `Book` model lets assume there is a table named `wp_books` with the columns `id`, `title`, `price` and `pages`. How would you represent the database relationship in your TypeRocket models?

Take a look at the `Author` model.

```php
<?php
namespace App\Models;

use \TypeRocket\Models\Model;

class Author extends Model
{
    protected $resource = 'authors';

    /**
     * Get the player records associated with the game.
     */
    public function books()
    {
      $model = '\App\Models\Book';
      $table = 'wp_authors_books';
      return $this->belongsToMany($model, $table, 'books_id', 'authors_id');
    }
}
```

And, look at the `Book` model.

```php
<?php
namespace App\Models;

use \TypeRocket\Models\Model;

class Book extends Model
{
    protected $resource = 'books';

    /**
     * Get the player records associated with the game.
     */
    public function authors()
    {
      $model = '\App\Models\Author';
      $table = 'wp_authors_books';
      return $this->belongsToMany($model, $table, 'authors_id', 'books_id');
    }
}
```

Now, you can access the `Book` records of the `Author` using the `books()` method. Take a look at the example where `$author` is an instance of `Author`.

```php
$authors_books = $author->findById(1)->books()->get();
```

Now, you can access the `Author` records of the `Book` using the `authors()` method. Take a look at the example where `$book` is an instance of `Book`.

```php
$books_authors = $book->findById(1)->authors()->get();
```

### Saving Many to Many Relationships

To save the relationship of a `Book` to an `Author` use the `attach()` method. For example, if you have author records with the IDs `1`, `2`, and `3` you can attach them to a book as follows.

```php
$book->findById(1)->authors()->attach([1,2,3]);
``` 

You can also detach author records with the `detach()` method.

```php
$book->findById(1)->authors()->detach([4,5,6]);
```

If you want to have only a specific set or records applied then you would need to detach all records and then attach the records you want.

```php
$books_authors = $book->findById(1)->authors();
$books_authors->detach();
$books_authors->attach([1,2,3]);
```

Or, you can use the shortcut method `sync()` to detach all authors and then attach the desired authors.

```php
$book->findById(1)->authors()->sync([1,2,3]);
```