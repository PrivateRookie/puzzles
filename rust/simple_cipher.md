# 简单字符替换加密

[原题](https://www.codewars.com/kata/simple-substitution-cipher-helper/train/rust)

给出一个字符替换映射表，写出一个可以加密和解密一串字符的加密器。若输入的字符在映射表不存咋，则不转换。加密器定义如下：

```rust
struct Cipher {
  //your code here
}

impl Cipher {
  fn new(map1: &str, map2: &str) -> Cipher {
    //your code here
  }
  
  fn encode(&self, string: &str) -> String {
    //your code here
  }
  
  fn decode(&self, string: &str) -> String {
    //your code here
  }
}
```

最先想到的解法是使用 `HashMap`

```rust
use std::collections::HashMap;

struct Cipher {
    encoder: HashMap<char, char>,
    decoder: HashMap<char, char>,
}

impl Cipher {
    fn new(map1: &str, map2: &str) -> Cipher {
        Cipher {
            encoder: map1.chars().zip(map2.chars()).collect(),
            decoder: map2.chars().zip(map1.chars()).collect(),
        }
    }

    fn encode(&self, string: &str) -> String {
        string
            .chars()
            .map(|ch| self.encoder.get(&ch).unwrap_or(&ch).clone())
            .collect()
    }

    fn decode(&self, string: &str) -> String {
        string
            .chars()
            .map(|ch| self.decoder.get(&ch).unwrap_or(&ch).clone())
            .collect()
    }
}
```

当然也可以不使用 `HashMap`

```rust
struct Cipher {
    map: Vec<(char, char)>,
}

impl Cipher {
    fn new(map1: &str, map2: &str) -> Cipher {
        Cipher {
            map: map1.chars().zip(map2.chars()).collect(),
        }
    }

    fn encode(&self, string: &str) -> String {
        string
            .chars()
            .map(|ch| self.map.iter().find(|x| x.0 == ch).map_or(ch, |y| y.1))
            .collect()
    }

    fn decode(&self, string: &str) -> String {
        string
            .chars()
            .map(|ch| self.map.iter().find(|x| x.1 == ch).map_or(ch, |y| y.0))
            .collect()
    }
}
```

第二种的解法使用 `find` 和 `map_or` 函数，对于每个要加密或解密的字符先查找其在 `Cipher.map` 中的位置，接着使用 `map_or` 可以完美应对字符缺省的情况。即

`find -> Option<T>` 

`map_or (U, (T) -> U) -> U`。

更多类型转换可以参考 [Rust CheapSheet](https://upsuper.github.io/rust-cheatsheet/)

注意两种解法的 `collect` 都没有使用 `::` 标志符标出返回值，这是因为函数签名中已经
声明了返回值，Rust 可以通过自动类型推导转换返回值。

知识点：

- [`map_or`](https://doc.rust-lang.org/std/option/enum.Option.html#method.map_or)
- [类型推导](https://www.bookstack.cn/read/rust-by-example-cn/src-cast-inference.md)
