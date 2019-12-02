# Persistent Bugger

[原题](https://www.codewars.com/kata/persistent-bugger/train/rust)

已知函数 `fn persistence(num: u64) -> u64`, 该函数会求 `num` 各位数乘积，再求该
乘积各位数的乘积，直到乘积为只剩1位，返回计算的次数。

如 `persistence(39) == 3` 因为 `3 * 9 = 27`, `2 * 7 = 14`, `1 * 4 = 4`，一共
计算了三次，所以返回值为3. 注意 `persistence(4) == 0` 因为 4 不用计算。

```rust
fn persistence(num: u64) -> u64 {
    let mut count: u64 = 0;
    let mut num_str = num.to_string();
    loop {
        if num_str.len() == 1 {
            break;
        }
        count += 1;
        num_str = format!(
            "{}",
            num_str
                .chars()
                .map(|i| i.to_digit(10).unwrap() as u64)
                .fold(1, |acc, i| acc * i)
        );
    }
    count
}
```

知识点:

- `to_string` 将某个类型转换为 `String`，当然也可用比较奇怪的方法 `format!("{}", x)`
