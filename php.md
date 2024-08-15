## 筆記
```
array

array_chunk

array_column 

array_diff 差集

array_diff_key

array_filter 

array_flip 交換key value

array_intersect 交集

array_intersect_key

array_key_exists

array_keys 回傳所有key

array_values 回傳所有value 

array_map

array_merge

array_shift 拋除頭部

array_unshift 加入頭部

array_pop 拋出末尾

array_push 加入末尾

array_rand 陣列中隨機抽出一個或多個key

array_reverse 回傳順序相反陣列

array_search 搜尋回傳key

array_sum

array_unique 移除重複key

compact

extract

count

in_array

get_class

get_class_vars

get_class_methods

get_object_vars

method_exists

property_exists

__NAMESPACE__

__CLASS__

__METHOD__

__FUNCTION__

```

# PHP 8.0

## JIT
```
```

## Union Types
```php
#int float string bool array object null callable iterable
function foo(int|float $number): int|float {
    return $number;
}
```

## Named Arguments
```php
function foo($a, $b, $c) {
  echo "{$a}-{$b}-{$c}";
}

foo(a: 1, c: 3, b: 2);
```

## Constructor Property Promotion
```php
class Point {
    public function __construct(
        public float $x = 0.0,
        public float $y = 10.0,
    ) {}
}
```

## Match
```php
$result = match ($status) {
    1 => 'A',
    2 => 'B',
    3 => date("Ymd"),
    4, 5 => "D",
    default => 'Unknown',
};

$age = 23;
$result = match (true) {
    $age >= 65 => 'senior',
    $age >= 25 => 'adult',
    $age >= 18 => 'young adult',
    default => 'kid',
};
```

## Nullsafe
```php
$country = $session?->user?->getAddress()?->country;
```

## Throw Expression
```php
$value = $foo ?? throw new Exception('Invalid value');
```

# PHP 8.1
## Enumerations
```php
enum Status {
    case Pending;
    case Approved;
    case Rejected;
}

// 使用枚舉
function handleStatus(Status $status): void {
    switch ($status) {
        case Status::Pending:
            // 處理 Pending 狀態
            break;
        case Status::Approved:
            // 處理 Approved 狀態
            break;
        case Status::Rejected:
            // 處理 Rejected 狀態
            break;
    }
}

enum UserName: string {
    case ADMIN = 'admin';
    case USER = 'user';
    case GUEST = 'guest';
}

var_dump(UserName::ADMIN->name); // ADMIN
var_dump(UserName::ADMIN->value);// admin
```

## Readonly Properties
```php
class User {
    public readonly string $name;

    public function __construct(string $name) {
        $this->name = $name;
    }
}

$user = new User('John');
echo $user->name; // John
// $user->name = 'Jane'; // 將會拋出錯誤
```

## First-Class Callable Syntax
```php
class MyClass {
    public function sayHello(): void {
        echo 'Hello';
    }
}

$obj = new MyClass();
$callback = $obj->sayHello(...);
$callback(); // 輸出 'Hello'
```

## Fibers
```php
#實現協程（coroutines）和更高效的異步編程
$fiber = new Fiber(function (): void {
    echo "Fiber started\n";
    Fiber::suspend();
    echo "Fiber resumed\n";
});

$fiber->start(); // 輸出 "Fiber started"
$fiber->resume(); // 輸出 "Fiber resumed"
```

## Fibers
```php
#實現協程（coroutines）和更高效的異步編程
$fiber = new Fiber(function (): void {
    echo "Fiber started\n";
    Fiber::suspend();
    echo "Fiber resumed\n";
});

$fiber->start(); // 輸出 "Fiber started"
$fiber->resume(); // 輸出 "Fiber resumed"
```

## Array Unpacking with String Keys
```php
$array1 = ['foo' => 'bar'];
$array2 = ['baz' => 'qux'];

$result = [...$array1, ...$array2];
print_r($result); // 輸出 Array ( [foo] => bar [baz] => qux )
```

## Null Coalescing Assignment Operator
```php
$array = ['name' => 'John'];
$array['name'] ??= 'Doe'; // 會保留 'John'
$array['age'] ??= 30; // 會設置 'age' 為 30
print_r($array); // 輸出 Array ( [name] => John [age] => 30 )
```

## Intersection Types
```php
interface A {}
interface B {}

function process(A&B $value): void {
    // 處理同時實現 A 和 B 的對象
}
```

## Final 
```php
#防止繼承或覆寫
final class FinalClass {
    public function doSomething(): void {}
}

// 這樣會報錯
// class ExtendedClass extends FinalClass {}
```

# PHP 8.2
## Read-Only Classes
```php
readonly class User {
    public string $name;
    public function __construct(string $name) {
        $this->name = $name;
    }
}
```

## Disjunctive Normal Form (DNF) Types
```php
function example((A&B)|C $input): void {
    // 这里 $input 可以是 (A&B) 或 C 类型
}
```

## Traits 
```php
trait MyTrait {
    public const MY_CONSTANT = 'value';
}

class MyClass {
    use MyTrait;
}

echo MyClass::MY_CONSTANT; // 输出 'value'
```

## Traits 
```php
trait MyTrait {
    public const MY_CONSTANT = 'value';
}

class MyClass {
    use MyTrait;
}

echo MyClass::MY_CONSTANT; // 输出 'value'
```

## Dynamic Properties Deprecated
```php
class User {
    public string $name;
}

$user = new User();
$user->name = 'John'; // 正常
$user->age = 30; // 在 PHP 8.2 中将触发一个弃用警告
```

# PHP 8.3
## 
```php

```

