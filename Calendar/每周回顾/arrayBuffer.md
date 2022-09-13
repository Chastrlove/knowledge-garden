---
date created: 2022-09-05
date modified: 2022-09-05
title: f64位数
---
**基本的二进制对象是 `ArrayBuffer` —— 对固定长度的连续内存空间的引用。**

```js
let buffer = new ArrayBuffer(16); 
alert(buffer.byteLength); // 16
```

它会分配一个 16 字节的连续内存空间，并用 0 进行预填充。

`ArrayBuffer` 不是某种东西的数组

让我们先澄清一个可能的误区。`ArrayBuffer` 与 `Array` 没有任何共同之处：

-   它的长度是固定的，我们无法增加或减少它的长度。
-   它正好占用了内存中的那么多空间。
-   要访问单个字节，需要另一个“视图”对象，而不是 `buffer[index]`。

`ArrayBuffer` 是一个内存区域。它里面存储了什么？无从判断。只是一个原始的字节序列。

**如要操作 `ArrayBuffer`，我们需要使用“视图”对象。**

视图对象本身并不存储任何东西。它是一副“眼镜”，透过它来解释存储在 `ArrayBuffer` 中的字节。

例如：

-   **`Uint8Array`** —— 将 `ArrayBuffer` 中的每个字节视为 0 到 255 之间的单个数字（每个字节是 8 位，因此只能容纳那么多）。这称为 “8 位无符号整数”。
-   **`Uint16Array`** —— 将每 2 个字节视为一个 0 到 65535 之间的整数。这称为 “16 位无符号整数”。
-   **`Uint32Array`** —— 将每 4 个字节视为一个 0 到 4294967295 之间的整数。这称为 “32 位无符号整数”。
-   **`Float64Array`** —— 将每 8 个字节视为一个 `5.0x10-324` 到 `1.8x10308` 之间的浮点数。

因此，一个 16 字节 `ArrayBuffer` 中的二进制数据可以解释为 16 个“小数字”，或 8 个更大的数字（每个数字 2 个字节），或 4 个更大的数字（每个数字 4 个字节），或 2 个高精度的浮点数（每个数字 8 个字节）。


`ArrayBuffer` 是核心对象，是所有的基础，是原始的二进制数据。
![](https://raw.githubusercontent.com/Chastrlove/picgo/main/contacts-book-2-fill.svg)
但是，如果我们要写入值或遍历它，基本上几乎所有操作 —— 我们必须使用视图（view），例如：
```js
let buffer = new ArrayBuffer(16); // 创建一个长度为 16 的 buffer
let view = new Uint32Array(buffer); // 将 buffer 视为一个 32 位整数的序列
alert(Uint32Array.BYTES_PER_ELEMENT); // 每个整数 4 个字节
alert(view.length); // 4，它存储了 4 个整数 
alert(view.byteLength); // 16，字节中的大小 // 让我们写入一个值 
view[0] = 123456; // 遍历值 
for(let num of view) { 
	alert(num); // 123456，然后 0，0，0（一共 4 个值） 
}
```