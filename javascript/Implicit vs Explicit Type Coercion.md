# Implicit vs Explicit Type Coercion

## Implicit (automatic)

* Happens in certain operations
* Examples:

  * To string:

```js
10 + " apples"; // "10 apples"
```

	* To number:

```js
"5" * "2"; // 10
+ "10";    // 10
```

	* To boolean:

```js
if ("hello") {
  console.log("truthy");
}
```

---

## Explicit (manual)

* Done deliberately by the developer
* Examples:

  * To string:

```js
String(123);           // "123"
(123).toString();      // "123"
```

	* To number:

```js
Number("123");         // 123
parseInt("123px");     // 123
parseFloat("3.14");    // 3.14
```

	* To boolean:

```js
Boolean(0);           // false
!!"hello";            // true
```

---

#javascript