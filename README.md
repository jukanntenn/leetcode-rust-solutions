# leetcode-rust-solutions

LeetCode Rust 题解。包含对官方解题思路的进一步说明和算法正确性的证明。

## 为什么又建一个刷题仓库？

尽管 LeetCode 题解相关的 GitHub 仓库、书籍、小册、博客等已多如牛毛，每道题也都有权威的官方题解，但在刷题的过程中，我仍然时不时地被这些痛点刺激：

1. 官方解题思路有时候对关键点仅点到为止，天资愚钝的我需要参考很多下方的评论、第三方的解析等才能茅塞顿开。
2. 无论是官方还是第三方的题解，有些正确性没那么显然的算法却没有证明，所以尽管通过了全部测试用例，但仍感觉心虚。
3. 官方没有 Rust 语言的题解，需要满互联网找参考。我找到的一些参考中，有些 Rust 代码优雅而地道，也有不那么 idiomatic 的。

所以这个仓库主要功能是记录自己的刷题过程，提高自己 Rust 编程语言的熟练度，同时也希望这些记录能帮助到同样被上述痛点刺激的后来者。此仓库所有的题解中都会包含以下内容：

1. 从官方复制过来的题目描述。
2. 使用尽可能优雅而地道的 Rust 语言实现的参考解答。
3. 对解题思路关键点的详细阐述；对正确性没那么显然的算法的详细证明。
4. 使参考解答中 Rust 代码更加优雅而地道的小技巧（如果有的话）。

## 搭建刷题环境

可以直接在 LeetCode 的在线编辑器里撸代码，但所谓工欲善其事必先利其器，如果使用 VS Code 并配合 [vscode-leetcode](https://github.com/LeetCode-OpenSource/vscode-leetcode) 插件，将获得更加舒适的刷题体验。

在 VS Code 中安装好 LeetCode 插件后，需要调整一下这几个配置项：

- **LeetCode: Endpoint**。默认是美国站，将其切换为 `leetcode-cn` 选项，才能登录中国站的账号。
- **LeetCode: Default Language**。将其切换为 `rust` 选项。
- **LeetCode: Use Endpoint Translation**。如果想以中文显示题目描述，就勾选此项；想要英文显示，就取消勾选。建议以英文显示题目描述，刷题的同时练习英语。

修改完配置后，记得**重启**一下 VS Code，否则可能配置不生效，无法登录中国区的账户。

在最左侧的 **Activity Bar** 点击 **LeetCode** 图标，登录国区账户就可以开启刷题之旅了。

## 让刷题更加丝滑的小提示

1. **不要修改函数参数和返回值的类型**

   LeetCode 生成的 Rust 模板代码里有些参数或者返回值类型明显不合理，例如 slice 下标索引、长度等最好用 `usize`，但 LeetCode 给你或者请你返回的一般都是 `i32`。碰到情况下最好的办法是使用 Rust 的类型转换去满足 LeetCode 的要求，而不要去改模板代码里的类型，否则可能会遇到各种报错。

2. **字符串的处理**

   Rust 处理字符串没有其他语言方便，例如无法直接对字符串使用下标索引得到单个字符。不过 LeetCode 的题目中，字符串一般仅含 ASCII 字符，所以可以直接使用 `u8` 类型来处理单个字符，这样就可以使用下标索引了。

   ```rust
   let s = String::from("test string");

   // 这样就可以使用下标索引得到单个字符（类型是 u8）
   let b: u8 = s.as_bytes()[1];

   // 也可以转换成 char 类型
   let c = s.as_bytes()[1] as char;

   // 将 bytes 转回字符串。
   let s = unsafe { std::str::from_utf8_unchecked(&bytes[start..end]) }.to_string();

   // String 类型还有一个 chars 方法，可以返回一个元素类型为 char 的迭代器。
   // 可以在只需要按字符迭代 String 的场合使用，但不支持索引访问。
   for c in s.chars() {
      if c == 'a' {}
   }

   // 可以把 chars 迭代器转成 `Vec`，这样可以索引访问，且每个元素都是 char 类型，操作起来比较方便，但缺点是需要复制一遍字符串。
   let chars = s.chars().collect::<Vec<char>>();
   ```

3. **创建矩阵（二维数组）**

   很多题目都需要一个矩阵来存储结果，在 Rust 中一般使用 `Vec<Vec<T>>` 来创建矩阵，使用 `vec!` 宏是最方便的：

   ```rust
   // 创建一个 m*n 的矩阵，每个元素的初始值设置为 false。
   let mut matrix = vec![vec![false; n]; m];
   ```

4. **使用 vscode-leetcode 插件时注意代码起始位置**

   这是 vscode-leetcode 插件生成的模板代码：

   ```rust
   /*
    * @lc app=leetcode.cn id=14 lang=rust
    *
    * [14] Longest Common Prefix
    */

   // @lc code=start
   impl Solution {
       pub fn longest_common_prefix(strs: Vec<String>) -> String {

       }
   }
   // @lc code=end
   ```

   `// @lc code=start` 和 `// @lc code=end` 包裹的代码会被提交至 LeetCode。所以当使用 `use` 时，一定要放在这两个注释之间，否则提交给 LeetCode 的代码是没有 `use` 声明的，当然也就无法运行。

   如果编译或者执行报错，LeetCode 返回的错误信息中的行号需要加上 7 才是本地文件对应的行号。

## 题解索引

| #       | 题目                      | 难度 |
| ------- | ------------------------- | ---- |
| [1][]   | 两数之和                  | 简单 |
| [2][]   | 两数相加                  | 中等 |
| [3][]   | 无重复字符的最长子串      | 中等 |
| [4][]   | 寻找两个正序数组的中位数  | 困难 |
| [5][]   | 最长回文子串              | 中等 |
| [6][]   | N 字形变换                | 中等 |
| [7][]   | 整数反转                  | 中等 |
| [8][]   | 字符串转换整数 (atoi)     | 中等 |
| [9][]   | 回文数                    | 简单 |
| [19][]  | 删除链表的倒数第 N 个结点 | 中等 |
| [21][]  | 合并两个有序链表          | 简单 |
| [35][]  | 搜索插入位置              | 简单 |
| [45][]  | 跳跃游戏 II               | 中等 |
| [134][] | 加油站                    | 中等 |

[1]: ./solutions/1.%20%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.md
[2]: ./solutions/2.%20%E4%B8%A4%E6%95%B0%E7%9B%B8%E5%8A%A0.md
[3]: ./solutions/3.%20%E6%97%A0%E9%87%8D%E5%A4%8D%E5%AD%97%E7%AC%A6%E7%9A%84%E6%9C%80%E9%95%BF%E5%AD%90%E4%B8%B2.md
[4]: ./solutions/4.%20%E5%AF%BB%E6%89%BE%E4%B8%A4%E4%B8%AA%E6%AD%A3%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E4%B8%AD%E4%BD%8D%E6%95%B0.md
[5]: ./solutions/5.%20%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.md
[6]: ./solutions/6.%20N%20%E5%AD%97%E5%BD%A2%E5%8F%98%E6%8D%A2.md
[7]: ./solutions/7.%20%E6%95%B4%E6%95%B0%E5%8F%8D%E8%BD%AC.md
[8]: ./solutions/8.%20%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BD%AC%E6%8D%A2%E6%95%B4%E6%95%B0%20(atoi).md
[9]: ./solutions/9.%20%E5%9B%9E%E6%96%87%E6%95%B0.md
[19]: ./solutions/19.%20删除链表的倒数第%20N%20个结点.md
[21]: ./solutions/21.%20合并两个有序链表.md
[35]: ./solutions/35.%20搜索插入位置.md
[45]: ./solutions/45.%20跳跃游戏%20II.md
[134]: ./solutions/134.%20加油站.md

## 参考资料

大部分题解都是站在巨人的肩膀上写成的，感谢他们的无私分享。

待补充...
