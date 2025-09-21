
## Retrieval Methods

- **all()**
    
    Retrieve _all_ records from the model’s table.
    

```php
$users = User::all();
```

- **find($id)**
    
    Retrieve a record by its primary key. Returns null if not found.
    

```php
$user = User::find(1);
```

- **findOrFail($id)**
    
    Same as find(), but throws ModelNotFoundException if not found.
    

```php
$user = User::findOrFail(1);
```

- **first()**
    
    Retrieve the _first_ record that matches the query. Returns null if none found.
    

```php
$user = User::where('email', 'test@test.com')->first();
```

- **firstOrFail()**
    
    Same as first(), but throws an exception if no result.
    

```php
$user = User::where('email', 'test@test.com')->firstOrFail();
```

- **pluck($column)**
    
    Retrieve a single column’s values from the result set.
    

```php
$emails = User::pluck('email'); // Collection of emails
```
 
- **value($column)**
    
    Retrieve a single column value from the first record.
    

```php
$email = User::where('id', 1)->value('email');
```

---
## Filtering & Conditions

- **where($column, $operator, $value)**
    
    Add a basic `where` clause.
    

```php
$users = User::where('status', 'active')->get();
```

- **orWhere()**
    
    Chain an OR condition.
    

```php
$users = User::where('status', 'active')
	->orWhere('role', 'admin')
	->get();
```

- **whereIn($column, $array)**
    
    Filter results where column matches any in the array.
    

```php
$users = User::whereIn('id', [1,2,3])->get();
```

- **whereNotIn()**
    
    Opposite of `whereIn`.
    

```php
$users = User::whereNotIn('id', [1,2,3])->get();
```

- **whereNull() / whereNotNull()**
    
    Check for `NULL` values.
    

```php
$users = User::whereNull('deleted_at')->get();
```

- **whereBetween() / whereNotBetween()**
    
    Range condition.
    

```php
$users = User::whereBetween('age', [18,30])->get();
```

- **when($condition, $callback)**
    
    Conditionally apply query clauses.
    

```php
$users = User
	::when($isAdmin, fn($q) => $q->where('role', 'admin'))
	->get();
```

---
## Ordering & Limiting

- **orderBy($column, $direction)**
    
    Sort results.
    

```php
$users = User::orderBy('created_at', 'desc')->get();
```

- **latest() / oldest()**
    
    Shortcut for ordering by date.
    

```php
$users = User::latest()->get();
```

- **inRandomOrder()**
    
    Randomize record order.
    

```php
$users = User::inRandomOrder()->first();
```

- **limit($n) / take($n)**
    
    Restrict result count.
    

```php
$users = User::take(10)->get();
```

- **skip($n) / offset($n)**
    
    Skip first n results.
    

```php
$users = User::skip(5)->take(10)->get();
```

---

## Aggregates

- **count()**
    
    Count number of records.
    

```php
$total = User::count();
```

- **sum($column)**
    
    Sum values of a column.
    

```php
$salary = Employee::sum('salary');
```

- **avg($column)** / **average()**
    
    Average of column.
    

```php
$avgAge = User::avg('age');
```

- **min($column) / max($column)**
    
    Minimum or maximum value.
    

```php
$minPrice = Product::min('price');
```

---
## Relationships

- **with($relation)**
    
    Eager-load relations.
    

```php
$users = User::with('posts')->get();
```

- **load($relation)**
    
    Lazy-load relation after query.
    

```php
$user->load('posts');
```

- **withCount($relation)**
    
    Load relation count.
    

```php
$users = User::withCount('posts')->get();
```

- **withWhereHas($relation, $constraint)**
    
    Check for existence and eager-load in one step.
    

```php
$users = User
	::withWhereHas('posts', fn($q) => $q->where('published', true))
	->get();
```

- **has($relation)**
    
    Filter where relation exists.
    

```php
$users = User::has('posts')->get();
```

- **whereHas($relation, $constraint)**
    
    Filter where relation matches condition.
    

```php
$users = User
	::whereHas('posts', fn($q) => $q->where('published', true))
	->get();
```

  

---
## Create / Update

- **create($data)**
    
    Create and save in one step. (Requires fillable or guarded in model.)
    

```php
User::create(['name' => 'Ahmed', 'email' => 'ahmed@test.com']);
```

- **firstOrCreate($attributes, $values = [])**
    
    Find first record, or create if not found.
    

```php
User::firstOrCreate(
	['email' => 'ahmed@test.com'], 
	['name' => 'Ahmed']
);
```

- **updateOrCreate($attributes, $values)**
    
    Update if exists, else create.
    

```php
User::updateOrCreate(
	['email' => 'ahmed@test.com'], 
	['name' => 'Updated Ahmed']
);
```

- **update($data)**
    
    Update one or more records.
    

```php
User::where('active', false)->update(['active' => true]);
```

- **save()**
    
    Save model instance (create or update depending on state).
    

```php
$user = new User;
$user->name = 'Ahmed';
$user->save();
```

- **touch()**
    
    Update model’s `updated_at` timestamp.
    

```php
$user->touch();
```

  

---
## Deletion

- **delete()**
    
    Delete model or records.
    

```php
User::find(1)->delete();
```

- **destroy($ids)**
    
    Delete by primary keys (multiple allowed).
    

```php
User::destroy([1,2,3]);
```

- **truncate()**
    
    Empty entire table.
    

```php
User::truncate();
```

- **softDeletes (trait)**
    
    Instead of deleting, sets `deleted_at`.
    

```php
User::withTrashed()->get();
User::onlyTrashed()->restore();
```

---
## Pagination

- **paginate($perPage)**
    
    Paginate with links.
    

```php
$users = User::paginate(15);
```

- **simplePaginate($perPage)**
    
    Faster, no total count.
    

```php
$users = User::simplePaginate(15);
```
 
---
## Chunking & Streaming

- **chunk($count, $callback)**
    
    Process results in small groups (avoids loading all in memory).
    

```php
User::chunk(100, function ($users) {
    foreach ($users as $user) {
        // Process each user
    }
});
```

- **chunkById($count, $callback, $column = 'id')**
    
    Like chunk(), but uses ID column for pagination (safer with updates).
    

```php
User::chunkById(100, fn($users) => ...);
```

- **cursor()**
    
    Iterate over results using a generator (lazy-loading, minimal memory).
    

```php
foreach (User::where('active', true)->cursor() as $user) {
    // Each user loaded one at a time
}
```

- **lazy($chunkSize = 1000)**
    
    Similar to cursor(), but fetches in chunks internally.
    

```php
foreach (User::lazy(500) as $user) {
    // Lazy-loaded batch processing
}
```

---
## Subqueries & Raw Expressions

- **selectRaw($expression)**
    
    Add raw SQL to select clause.
    

```php
$users = User::selectRaw('count(*) as total, status')
	->groupBy('status')
	->get();
```

- **whereRaw($expression, $bindings = [])**
    
    Use raw SQL in where clause.
    

```php
$users = User::whereRaw('age > ? AND votes > ?', [25, 100])
	->get();
```

- **orderByRaw($expression)**
    
    Custom sorting.
    

```php
$users = User::orderByRaw('LENGTH(name) desc')->get();
```

- **havingRaw($expression)**
    
    Raw having clause.
    

```php
$sales = Order::select('user_id', DB::raw('SUM(amount) as total'))
              ->groupBy('user_id')
              ->havingRaw('SUM(amount) > 1000')
              ->get();
```

- **selectSub($query, $as)**
    
    Add subquery as column.
    

```php
$latestPosts = User::addSelect([
    'latest_post' => Post::select('title')
                         ->whereColumn('user_id', 'users.id')
                         ->latest()
                         ->limit(1)
])->get();
```

---
## Inserting / Bulk Operations

- **insert($records)**
    
    Insert multiple records at once (no timestamps).
    

```php
User::insert([
    ['name' => 'A', 'email' => 'a@test.com'],
    ['name' => 'B', 'email' => 'b@test.com']
]);
```

- **upsert($values, $uniqueBy, $updateColumns)**
    
    Insert or update in bulk.
    

```php
User::upsert([
    ['email' => 'a@test.com', 'name' => 'New Name']
], ['email'], ['name']);
```

---
## Advanced Relationship Methods

- **morphTo() / morphMany() / morphOne() / morphToMany()**
    
    Polymorphic relations.
    

```php
// Comment model
public function commentable() {
    return $this->morphTo();
}
```

- **morphWithCount()**
    
    Eager load polymorphic relation with counts.
    

```php
$comments = Comment::with(
	['commentable' => fn($q) => $q->withCount('likes')]
)->get();
```

- **without($relation)**
    
    Prevent eager-loading of relation.
    

```php
$users = User::with('posts')->without('posts')->get();
```

- **withoutGlobalScopes() / withoutGlobalScope($scope)**
    
    Ignore global query scopes.
    

```php
User::withoutGlobalScopes()->get();
```

---
## Scopes & Macros

- **scopeName($query, ...)** (Local Scope)
    
    Define reusable query filters inside model.
    

```php
public function scopeActive($query) {
    return $query->where('active', true);
}

$users = User::active()->get();
```

- **Global Scopes**
    
    Define rules applied to all queries of a model.
    

```php
protected static function booted() {
    static::addGlobalScope('active', fn($q) => $q->where('active', true));
}
```

---
## Locking

- **lockForUpdate()**
    
    Pessimistic locking (for transactions).
    

```php
DB::transaction(function () {
    $user = User::where('id', 1)->lockForUpdate()->first();
    // Safe update
});
```

- **sharedLock()**
    
    Prevent updates but allow reads.
    

```php
$user = User::where('id', 1)->sharedLock()->first();
```

---
## Advanced Collection & Casting

- **cast (Model property)**
    
    Automatically cast attributes.
    

```php
protected $casts = [
    'is_admin' => 'boolean',
    'settings' => 'array',
    'published_at' => 'datetime',
];
```

- **append($attribute)**
    
    Add custom accessor attributes to serialization.
    

```php
protected $appends = ['full_name'];
```

- **toArray() / toJson()**
    
    Convert model or collection to array/JSON.
    

```php
return $user->toJson();
```

---
## Events & Observers

- **Model Events** (creating, created, updating, updated, deleting, deleted, etc.)
    

```php
protected static function booted() {
    static::creating(fn($user) => Log::info('Creating user', [$user]));
}
```

- **Observers**
    
    External class to listen to model events.
    

```php
class UserObserver {
    public function created(User $user) {
        // Handle created event
    }
}
```

---
## Miscellaneous

- **replicate()**
    
    Clone a model instance (except primary key).
    

```php
$newPost = $post->replicate();
```

- **refresh()**
    
    Reload model from database.
    

```php
$user->refresh();
```
   
- **exists / doesntExist()**
    
    Check query existence.
    

```php
$exists = User::where('email', 'a@test.com')->exists();
```

- **tap($callback)**
    
    Apply callback to query without breaking chain.
    

```php
User::tap(fn($q) => $q->where('active', true))->get();
```

- **withCasts()**
    
    Apply casts at runtime.
    

```php
$user = User::withCasts(['options' => 'array'])->first();
```

---
## Relationship Types Reference

### One to One

- **Definition:** One model is related to exactly one instance of another.
    
- **Example:** Each User has one Profile.

```php
// User.php
public function profile() {
    return $this->hasOne(Profile::class);
}

// Profile.php
public function user() {
    return $this->belongsTo(User::class);
}
```

**Usage:**

```php
$user = User::with('profile')->find(1);
$profile = $user->profile;
```

---
### One to Many

- **Definition:** One model has many related models.
    
- **Example:** A Post has many Comments.

```php
// Post.php
public function comments() {
    return $this->hasMany(Comment::class);
}

// Comment.php
public function post() {
    return $this->belongsTo(Post::class);
}
```

**Usage:**

```php
$comments = Post::find(1)->comments;
```

---
### Many to Many

- **Definition:** Models are related via a pivot table.
    
- **Example:** A User belongs to many Roles, and a Role belongs to many Users.

```php
// User.php
public function roles() {
    return $this->belongsToMany(Role::class);
}

// Role.php
public function users() {
    return $this->belongsToMany(User::class);
}
```

**Usage:**

```php
$user->roles()->attach($roleId);
$user->roles()->detach($roleId);
$user->roles()->sync([1, 2, 3]); // Replace with IDs
```

---
### Has Many Through

- **Definition:** Access a distant relationship through an intermediate model.
    
- **Example:** A Country has many Posts through Users.

```php
// Country.php
public function posts() {
    return $this->hasManyThrough(Post::class, User::class);
}
```

**Usage:**

```php
$posts = Country::find(1)->posts;
```

---
### One to One (Polymorphic)

- **Definition:** A model can belong to more than one type of model on a single association.
    
- **Example:** Image can belong to either a User or a Post.

```php
// Image.php
public function imageable() {
    return $this->morphTo();
}

// User.php
public function image() {
    return $this->morphOne(Image::class, 'imageable');
}

// Post.php
public function image() {
    return $this->morphOne(Image::class, 'imageable');
}
```

**Usage:**

```php
$image = $user->image;
$postImage = $post->image;
```

---
### One to Many (Polymorphic)

- **Definition:** A model can have many related models of multiple types.
    
- **Example:** Comment can belong to either Post or Video.

```php
// Comment.php
public function commentable() {
    return $this->morphTo();
}

// Post.php
public function comments() {
    return $this->morphMany(Comment::class, 'commentable');
}

// Video.php
public function comments() {
    return $this->morphMany(Comment::class, 'commentable');
}
```

**Usage:**

```php
$post->comments;
$video->comments;
```

---
### Many to Many (Polymorphic)

- **Definition:** A model can belong to many models on multiple types.
    
- **Example:** A Tag can belong to Posts and Videos.

```php
// Tag.php
public function posts() {
    return $this->morphedByMany(Post::class, 'taggable');
}

public function videos() {
    return $this->morphedByMany(Video::class, 'taggable');
}

// Post.php
public function tags() {
    return $this->morphToMany(Tag::class, 'taggable');
}

// Video.php
public function tags() {
    return $this->morphToMany(Tag::class, 'taggable');
}
```

**Usage:**

```php
$post->tags;  
$video->tags;
```

---
### Relationship Utilities

- **associate() / dissociate()**
    
    For belongsTo relationships.
    

```php
$comment->post()->associate($post);
$comment->post()->dissociate();
```

- **attach() / detach() / sync()**
    
    For belongsToMany relationships.
    

```php
$user->roles()->attach(1);
$user->roles()->detach(2);
$user->roles()->sync([1, 2]);
```

- **toggle()**
    
    Attach if not attached, else detach.
    

```php
$user->roles()->toggle([1, 2]);
```

- **withPivot()**
    
    Access pivot table columns.
    

```php
return $this->belongsToMany(Role::class)->withPivot('expires_at');
```

- **pivot property**
    
    Access pivot model instance.
    

```php
$role = $user->roles()->first();
$expires = $role->pivot->expires_at;
```

---
