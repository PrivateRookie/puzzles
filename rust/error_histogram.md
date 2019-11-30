# 求字符串中的特殊字符次数

[原题](https://www.codewars.com/kata/59f44c7bd4b36946fd000052/train/rust)

给定一个字符串，求字符串中 `uwxz` 这四个字符出现的次数，并打印出直方图，注意：顺序要保持字符顺序，不要打印未出现的字符。

例子：

```
s="uuaaaxbbbbyyhwawiwjjjwwxymzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
输出
hist(s) => "u..2.....**\rw..5.....*****\rx..2.....**\rz..31....*******************************"
```

使用 `HashMap`

```rust
fn hist(s: &str) -> String {
    use std::collections::HashMap;
    let errors = "uwxz";
    let mut counter: HashMap<char, u8> = HashMap::new();
    errors.chars().for_each(|c| {
        counter.insert(c, 0);
    });

    s.chars().for_each(|i| match counter.get_mut(&i) {
        Some(x) => *x += 1,
        None => (),
    });
    let mut result = String::new();
    errors.chars().for_each(|c| {
        let count = counter.get(&c).unwrap();
        if *count > 0 {
            result.push(format!(
                "{:2}{:<6}{}",
                c,
                count,
                "*".repeat(*count as usize)
            ));
            result.push('\r');
        }
    });
    result.pop();
    result
}
```

简洁解法

```rust
fn hist(s: &str) -> String {
    ["u", "w", "x", "z"]
        .iter()
        .fold(vec![], |mut acc, c| {
            let count = s.matches(c).count();
            if count > 0 {
                acc.push(format!("{:2} {:<6}{}", c, count, "*".repeat(count)))
            }
            acc
        })
        .join("\r")
}
```

知识点

- [HashMap](https://doc.rust-lang.org/std/collections/struct.HashMap.html)
- [str::matches](https://doc.rust-lang.org/std/primitive.str.html#method.matches) 查找字符串中某个特定模式
