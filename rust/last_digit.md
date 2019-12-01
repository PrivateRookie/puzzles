# 求某个极大数的最后一位

[原题](https://www.codewars.com/kata/last-digit-of-a-large-number/rust)

已知函数 `last_digit(str1: &str, str2: &str) -> i32`，因 Rust 无原生极大值表示，用字符代替输入的数字，求 `str1 ^ str2` 这个数的最后一位。注： `str1` 和 `str2` 均为非负整数，`0 ^ 0 = 1`。

简洁解法

```rust
fn last_digit(str1: &str, str2: &str) -> i32 {
    if str2 == "0" {
        1
    } else {
        let x = str1.chars().last().unwrap().to_digit(10).unwrap();
        let m = str2.chars().last().unwrap().to_digit(10).unwrap();
        let exp = if m == 0 { 4 } else { m };
        (x.pow(exp) % 10) as i32
    }
}
```

思路清晰些的解法

```rust
fn last_digit(str1: &str, str2: &str) -> i32 {
    const DIGITS: [[i32; 4]; 10] = [
        [0, 0, 0, 0],
        [1, 1, 1, 1],
        [6, 2, 4, 8],
        [1, 3, 9, 7],
        [6, 4, 6, 4],
        [5, 5, 5, 5],
        [6, 6, 6, 6],
        [1, 7, 9, 3],
        [6, 8, 4, 2],
        [1, 9, 1, 9],
    ];
    if str2 == "0" {
        1
    } else {
        let i = str1.chars().last().unwrap().to_digit(10).unwrap();
        let j = str2.chars().last().unwrap().to_digit(10).unwrap() % 4;
        DIGITS[i as usize][j as usize]
    }
}
```

思路
注意到每个数字最后一位都是有周期性的，即

> 0 -> [0]
> 
> 1 -> [1]
> 
> 2 -> [6, 2, 4, 8]
> 
> 3 -> [1, 3, 9, 7]
> 
> 4 -> [6, 4]
> 
> 5 -> [5]
> 
> 6 -> [6],
> 
> 7 -> [1, 7, 9, 3]
> 
> 8 -> [6, 8, 4, 2]
> 
> 9 -> [1, 9]

为了方便处理统一取最长的长度 4 作为周期，即

```rust
const DIGITS: [[i32; 4]; 10] = [
        [0, 0, 0, 0],
        [1, 1, 1, 1],
        [6, 2, 4, 8],
        [1, 3, 9, 7],
        [6, 4, 6, 4],
        [5, 5, 5, 5],
        [6, 6, 6, 6],
        [1, 7, 9, 3],
        [6, 8, 4, 2],
        [1, 9, 1, 9],
    ];
```

又注意到对于任何非负整数 `x`，`x % 4` 结果仅与 `x` 最后一位相关（在此省略证明过程），因此仅需求 `str2` 最后一位除以 4 的余数即可。

知识点：

- [`fn last(self) -> Option<Self::Item>`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.last) 求迭代器最后一项
- [`pub fn to_digit(self, radix: u32) -> Option<u32>`](https://doc.rust-lang.org/std/primitive.char.html#method.to_digit)将字符转换为数字

**注意**

对于一个字符 `c: char` 如果使用 `as` 进行类型转化会得到其对应的 ascii 码值。如

```rust
fn main() {
    let c: char = '9';
    println!("use `as`: {}", c as usize);
    println!("use `to_digit`: {}", c.to_digit(10).unwrap());
}
```

输出

```bash
use `as`: 57
use `to_digit`: 9
```