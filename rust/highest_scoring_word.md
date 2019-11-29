# 求最高分数的单词

[原题](https://www.codewars.com/kata/57eb8fcdf670e99d9b000272/train/rust)

给定一串字符，字符中的每个单词得分计算方式为：单词中每个字母在字母表中的位置（a = 1,b = 2, c = 3）之和. 求这串字符中得分最高的单词。若两单词分数相同，则返回字符串中最先出现的单词。

普通解法

```rust
fn high(input: &str) -> &str {
    let words = input.split(" ").collect::<Vec<&str>>();
    let scores = words
        .iter()
        .map(|w| Vec::from(*w).iter().map(|i| i - 96).sum::<u8>())
        .collect::<Vec<u8>>();
    let highest = scores
        .iter()
        .fold(0_u8, |max, &i| if i > max { i } else { max });
    let max_index = scores.iter().position(|&i| i == highest).unwrap();
    words[max_index]
}
```

一行解决

```rust
fn high(input: &str) -> &str {
    input.split_ascii_whitespace().rev().max_by_key(|s| s.chars().map(|c| c as u16 - 96).sum::<u16>()).unwrap_or("")
}
```

知识点

- [split_ascii_whitespace](https://doc.rust-lang.org/std/primitive.str.html#method.split_ascii_whitespace)
- [max_by_key](https://doc.rust-lang.org/core/iter/trait.Iterator.html#method.max_by_key)
