---
date created: 2022-09-07
date modified: 2022-09-13
title: Rust 整形范围
---

每个有符号整形规定的数字范围是 -($2^n$ - 1) ~ $2^{n-1}$  - 1，其中 `n` 是该定义形式的位长度。因此 `i8` 可存储数字范围是 -($2^7$) ~ $2^7$ - 1，即 -128 ~ 127。无符号整形可以存储的数字范围是 0 ~ $2^n$ - 1，所以 `u8` 能够存储的数字为 0 ~ $2^8$ - 1，即 0 ~ 255。
