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

