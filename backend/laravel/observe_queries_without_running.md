
The `DB::pretend()` function allows us to observe the database queries that would be executed without them actually running.

```php
\DB::pretend(\Artisan::call('comments:scan-for-updates'));
```

---