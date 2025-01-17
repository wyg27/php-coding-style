## 创建 model 对象

```php
<?php

// 可读性低
$order = new Order();
$order->cid = 111;
$order->subtotal = 10;
$order->save();

// 可读性高
$order = Order::create([
    'cid'      => 111,
    'subtotal' => 10,
]);
```

## 更新 model 对象

```php
<?php

// 可读性低
$order = Order::where('id', 111)->first();
$order->subtotal = 10;
$order->save();

// 可读性高
// 注意：这种写法仅适合不需要将更新后的 Order 对象赋值给其它变量的情况。因为这种写法的返回值是一个 int，代表”受影响的记录条数“。
Order::where('id', 111)->update([
    'subtotal' => 10,
]);
```

## 如果 model 对象存在就更新它，否则新建对象

```php
<?php

// 可读性低
$order = Order::where('id', 111)->first();
if (!$order) {
    $order = new Order();
}
$order->subtotal = 10;
$order->save();

// 可读性高
$order = Order::firstOrNew([
    'id' => 111,
]);
$order->subtotal = 10;
$order->save();
```

## 基于 model 的复杂查询/常用查询

```php
<?php
// 假设场景：需要查询“成年男性用户”。

// 可读性低
$target_customers = Customer::where('gender', 'male')->where('age', '>', 16)->get();

// 可读性高（借助 model 的 scope ）
$target_customers = Customer::adult()->get();

// 借助可以传递参数的 scope，你可以写出更灵活的语句。
$target_customers = Customer::male()     /* 男性 */
                            ->ageGTE(16) /* 大于等于16岁 */
                            ->get();
```
