# 求抛物曲线长度

[原题](https://www.codewars.com/kata/parabolic-arc-length/rust)

已知抛物曲线 `y = x^2`, 现用近似法求曲线在区间 [0, 1] 上的长度。`len_curve` 中参数 n 为平均分割大小。

```rust
fn len_curve(n: i32) -> f64 {
    let mut v = vec![];
    for i in 0..n + 1 {
        let x = i as f64 / n as f64;
        let y = x.powi(2);
        v.push((x, y));
    }
    (1..n as usize + 1).fold(0., |acc, i| {
        let (x1, y1) = v[i - 1];
        let (x2, y2) = v[i];
        acc + ((x2 - x1).powi(2) + (y2 - y1).powi(2)).sqrt()
    })
}
```

知识点

- 类型转换 `as`
- 指数函数 `powi` 或 `powf`
