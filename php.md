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
```
```

##JIT
```
```

##Union Types
```
function foo(int|float $number): int|float {
    return $number;
}
```

##Named Arguments
```
function foo($a, $b, $c) {
  return "{$a}-{$b}-{$c}";
}

echo foo(a: 1, c: 3, b: 2);
```
