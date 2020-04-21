# FUN PHP - Higher order functions

- محمد سفيان شراب - قديم
- [FUN PHP - Higher order functions](https://www.youtube.com/watch?v=sUJ8j2UR5kU)
- Jun 2, 2017
- Completed Apr 20, 2020

---

## Higher-order function

- A function that takes one or more functions as arguments or returns a function as it's result.

## Closure

- function in a variable

```
$add = function ($a, $b) {
  return $a + b;
}

var_dump($add);
object(Closure)#3 (1) {
  ["parameter"]=>
  array(2) {
    ["$a"]=>
    string(10) ""
    ["$b"]=>
    string(10) ""
  }
}

var_dump($add(1, 2));
int(3)

```

### filter

```
$numbers = collect([1, 2, 3, 4, 5]);

$filtered = $numbers->filter(function($item) {
  return $item < 3;
});

var_dump($filtered);
object(Illuminate\Support\Collection)#4 (1) {
  ["items":protected]=>
  array(2) {
    [0]=>
    int(1)
    [1]=>
    int(2)
  }
}
```

```
$min = 3;
$filtered = $numbers->filter(function($item) use ($min) {
  return $item < $min;
});

var_dump($filtered);
object(Illuminate\Support\Collection)#4 (1) {
  ["items":protected]=>
  array(3) {
    [0]=>
    int(1)
    [1]=>
    int(2)
    [2]=>
    int(3)
  }
}
```

```
$numbers = collect([1, 2, 3, 4, 5]);

$min_filter = function($min) {
  return function($item) use ($min) {
    return $item < $min;
  };
}

$filtered = $numbers->filter($min_filter(3));

var_dump($filtered);
object(Illuminate\Support\Collection)#5 (1) {
  ["items":protected]=>
  array(2) {
    [0]=>
    int(1)
    [1]=>
    int(2)
  }
}
```

```
$numbers = collect([1, 2, 3, 4, 5]);

function min_filter($min) {
  return function($item) use ($min) {
    return $item < $min;
  };
}

$filtered = $numbers->filter(min_filter(2));

var_dump($filtered);
object(Illuminate\Support\Collection)#4 (1) {
  ["items":protected]=>
  array(1) {
    [0]=>
    int(1)
  }
}
```
