# Unicode

![unicode](./imgs/unicode.png)

我们都知道，计算内部所有的信息都可以用二进制来表示，每个二进制位用 0 或 1表示。二进制中的一位称为**比特**（Bit），它是信息的最小单位。比如，二进制数 0101 就是 4 比特。**字节**（Byte）是计算机信息用于计量存储容量的计量单位，它不区分数据类型，一个字节等于 8 比特（即 8 个二进制位）。

### 认识字符集

**字符**是各种文字和符号的统称，它包括各个国家的文字、数字、标点符号、运算符号等。不同的字符组合在一起就形成了**字符集**，比如常见的字符集有：ASCII 字符集、GB18030 字符集、Unicode 字符集等。

同时，由于计算机只认识二进制数 0 和 1，所以计算机要想准确地处理各种字符集所包含的字符，就需要进行**字符编码**。所谓字符编码，就是对字符进行编码，是一种将字符转换为对应二进制数的规则，通过它可以方便文本在计算机中存储和通过通信网络的传递。

**字符在输入时会被转换为二进制数存储在计算机内**。一个字节有 8 个二进制位，因此一共有 256（2 的 8 次方） 种不同的状态。每种状态对应一个字符，那么一个字节可以表示 256 个字符，从 `00000000` 到 `11111111`。这 256 个字符对于以英语为主的 ASCII 字符集（共有 128 个字符）是足够的，但对于使用其它语言的字符集就不够了，比如，中文的汉字就多达 10 万左右。

同时，**字符在输出时也需要将二进制数转换为对应的字符**。由于之前的字符集更多都是针对同一语言进行编码，所以打开一份文件时就必须要知道它的编码方式，否则用不一致的编码方式打开会产生乱码。

### Unicode

为了解决使用不同编码方式而产生的不兼容情况，需要一种编码方式将全世界所有的字符包含在一个集合里——即 **Unicode** 字符集，这样便不会产生乱码了。

> Unicode 中文称为统一码，是计算机科学领域的业界标准。它整理并编码了世界上大部分的文字系统，使得电脑可以用更为简单的方式来呈现和处理文字。Unicode 至今仍在不断增修，每个新版本都加入更多新的字符。目前最新的版本为 2020 年 3 月公布的13.0.0，已经收录超过 13 万个字符。

**Unicode 为每个字符定义一个唯一的码点**，记作 `U+[十六进制数]`。比如，`一` 的码点为 `U+4E00`，`上` 的码点为 `U+4E0A`。对于这么多字符，Unicode 将它们分成了17 组进行编排，每组称为**平面**，每个平面拥有 65536（2 的 16 次方）个码点。所有最常见的字符都放在 0 号平面，称为基本多语言平面（BMP），码点范围从 `U+0000` 到 `U+FFFF`，共有 65536 个字符。剩下的平面称为辅助平面，码点范围从 `U+010000` 到 `U+10FFFF`。

一个字符的 Unicode 码点确定，但在实际传输过程中，由于不同的系统平台设计不一致，以及出于节省空间的目的，对 Unicode 码点的实现方式有所不同。比如 `上` 的码点 `U+4E0A` 转换为二进制数有 15 位（100 1110 0000 1010），可以用三个字节或四个字节来存，这个时候就取决于不同的 Unicode 的实现方式。Unicode 的实现方式称为 **Unicode 转换格式**（Unicode Transformation Format），简称为 UTF，它用于将 Unicode 码点转换为特定的字节序列，常见的 Unicode 实现方式有：UTF-8、UTF-16、UTF-32 等。

### UTF-32

UTF-32 是一种采用固定长度编码 Unicode 的实现方式，它使用四个字节（32 位比特）表示 Unicode 字符，并且字节内容与码点的数值完全相同一致。比如，`一` 的码点是 `U+4E00` ，在码点前面加两个字节的 0 就是对应的 UTF-32 编码 `0x00004E00`。UTF-32 的主要优点是直接使用 Unicode码位来索引，查找效率高；缺点是每个字符都使用四个字节表示，空间浪费极大。比如，同样英语文本，UTF-32 就会比 UTF-8 的编码大 4 倍，因此 UTF-32 很少使用。

### UTF-8

**UTF-8 是一种可变长度编码 Unicode 的实现方式，它使用一到四个字节表示一个 Unicode 字符**，具体编码规则如下：

- 对于单字节的字符，字节的第一位设为 0，后面 7 位为该字符的 Unicode 码的二进制数。
- 对于多字节的字符，首字节的前 n（n 代表是几个字节）位都设为 1，第 n + 1 位设为 0，后面字节的前两位都设为 10，剩下的二进制位用该字符的 Unicode 码的二进制数填充，还有多出的位补 0。

规则总结如下表，其中 x 表示 Unicode 码点的二进制数所占据的位：

| 字节数 | 码点的位数 | Unicode 字符码点范围（十六进制） | 字节 1   | 字节 2   | 字节 3   | 字节 4   |
| :----: | :--------: | -------------------------------- | -------- | -------- | -------- | -------- |
|   1    |     7      | U+0000 ~ U+007F                  | 1xxxxxxx |          |          |          |
|   2    |     11     | U+0080 ~ U+07FF                  | 110xxxxx | 10xxxxxx |          |          |
|   3    |     16     | U+0800 ~ U+FFFF                  | 1110xxxx | 10xxxxxx | 10xxxxxx |          |
|   4    |     21     | U+10000 ~ U+10FFFF               | 11110xxx | 10xxxxxx | 10xxxxxx | 10xxxxxx |

根据上表，UTF-8 判断一个字符是几个字节就很清楚了，它根据首字节的二进制数进行判断：如果首字节中第一位为 0，那么该字符为单字节；如果首字节第一位为 1，则连续有多少个 1，就表示该字符有几个字节。

下面，以 `上` 为例进行说明编码和解码的过程。

`上` 的 Unicode 码点为 `U+4E0A`，在上表中位于第三行的范围，编码过程如下：

```
   4    E    0    A         // 码点 4E0A 的十六进制数
0100 1110 0000 1010         // 码点 4E0A 对应的二进制数
---------------------------
1110xxxx 10xxxxxxx 10xxxxxx // 码点 4E0A 所属第三行对应模板
11100100 101110000 10010100 // 将对应的二进制数代入模板，得到 上 的 UTF-8 编码二进制数
   E   4     7   0    9   4 // 从而得到 上 的 UTF-8 编码十六进制数 E47094
```

`上` 的 UTF-8 编码十六进制数为 `E47094`，解码过程如下：

```
   E   4     7   0    9   4 // UTF-8 编码的十六进制数 E47094
11100100 101110000 10010100 // 对应的二进制数
1110xxxx 10xxxxxxx 10xxxxxx // 在表格中属于第三行，代入相应模板
---------------------------
    0100 1110 0000 1010     // 得到 上 的 Unicode 码位二进制数
       4    E    0    A     // 得到 上 的 Unicode 码位十六进制数，即 U+4E0A
```

由于 UTF-8 不仅具有节省空间的特性，还兼容 ASCII 码，所以它成为互联网上最主要的编码形式。

### UTF-16

UTF-16 也是一种可变长度编码 Unicode 的实现方式，它使用两个字节或四个字节来表示 Unicode 字符。UTF-16 编码规则：在基本多语言平面中的字符用两个字节表示，辅助平面中的字符用四个字节表示。也就是说，UTF-16 编码长度要么是两个字节，要么是四个字节。

由此便涉及到一个问题，计算机遇到两个字节时，是把它当作一个字符，还是把它和另外两个字节组成的四个字节当作一个字符？解决办法是：在基本多语言平面内，从 `U+D800` 到 `U+DFFF` 是一个空段——即这些码点不对应任何字符。因此，可以用这个空段来映射辅助平面的字符。具体来说，辅助平面字符的码点减去 `0x10000` 得到的值用 20 个二进制位表示，将前 10 位映射在 `U+D800` 到 `U+DBFF`，后 10 位映射在 `U+DC00` 到 `U+DFFF`。这意味着位于辅助平面内的字符的码点会被拆成两个基本多语言平面内字符的码点表示。

所以，当遇到两个字节，发现它的码点在 `U+D800` 到 `U+DBFF `之间，就可以断定，紧跟其后的两个字节的码点，也应该在 `U+DC00` 到 `U+DFFF` 之间，并且这四个字节必须放在一起解读。下表是 UTF-16 转换码点范围：

| 字节数 | Unicode 字符码点范围（十六进制） |
| :----: | -------------------------------- |
|   2    | U+0000 ~ U+D7FF                  |
|   2    | U+E000 ~ U+FFFF                  |
|   4    | U+10000 ~ U+10FFFF               |

根据上表，位于基本多语言平面内的字符，UTF-16 的编码就等价于对应的 Unicode 码点。比如，`一` 的 Unicode 码点是 `U+4E00`，那么它对应的 UTF-16 编码就是 `0x4E00`。

位于辅助平面中的字符，转换规则如下：

1. 码点减去 `0x10000`，得到的值用 20 个二进制位表示（不足在前面补 0 ）。
2. 将前 10 个二进制位加上 `0xD800` 得到前导代理。
3. 将后 10 个二进制位加上 `0xDC00` 得到后尾代理。

以古希腊数字 `𐅀` 为例，它的 Unicode 码点是 `U+10140`，转换过程如下：

1. 码点 `0x10140` 减去 `0xD800`，得到的值用二进制表示为 `00000000000101000000`。
2. 前 10 个二进制位加上 `0xD800` 后得到 `0xD800`。
3. 后 10 个二进制位加上 `0xDC00` 后得到 `0xDD40`。

因此得到 `𐅀` 的 UTF-16 编码是 `0xD800 0xDD40`。

### 大小端问题

当一个字符用多字节存储时，会涉及到哪个字节在前哪个字节在后的问题。字节顺序有两种方式：**大端序**和**小端序**。比如，`且` 的码点是 `U+4E14` ，需要用两个字节存储，一个字节是 `4E`，另一个字节是 `14`。在存储时，如果 `4E` 在前，`14` 在后就是大端序；如果 `14` 在前，`4E` 在后就是小端序。

对字节顺序的理解不一致会造成同一字节流可能会被解释为不同内容。比如，对于十六进制编码为 `4E59` 的某字符，会用两个字节存储，分别是 `4E` 和 `59`，在 Mac上读取时字节顺序是小端序，那么在 macOS 会认为此 `4E59` 编码为 `594E`，找到的字符为“奎”，而在 Windows 上是大端序，则编码为 `U+4E59` 的字符为“乙”。

那么计算机是怎么知道文件采用哪种字节顺序呢？Unicode 中规定使用 `U+FEFF` 来标识字节序，它只能出现在字节流的开头。如果文件第一个字符顺序是 `FE FF`，则表示该文件使用大端序；如果第一个字符顺序是 `FF FE`，则表示该文件使用小端序。

### JavaScript 使用哪种编码方式

我们都知道，JavaScript 使用的是 Unicode 字符集，那它使用的是哪种 Unicode 实现方式呢？答案是 JavaScript 诞生的时候选择的是 **UCS-2** 编码方式。这里需要一点历史背景。

历史上存在两个独立的尝试创立单一字符集的组织，一个是国际标准化组织（ISO）于 1984 年创建的 ISO/IEC

JTC1/SC2/WG2 工作组，它们在 1989 年开始构建 UCS；另一个是 Xerox、Apple 等软件制造商于 1988 年组成的统一码联盟。 1991年前后，两个项目的参与者都认识到，世界不需要两个不兼容的字符集。于是，它们开始合并双方的工作成果，并修订此前发布的字符集，将 UCS 的码点与 Unicode 保持完全一致。

UCS 的开发进度快于 Unicode，于 1990 年就公布了第一套编码方法 UCS-2，使用两个字节表示已经有码点的字符（当时只有一个平面——即基本多语言平面，所以两个字节足够了）。而 UTF-16 编码迟至 1996 年 7 月才公布，并明确宣布它是 UCS-2 的超集，即基本多语言平面字符两者一致（两个字节表示），但辅助平面的字符使用四个字节来表示。简而言之，就是 UTF-16 取代了 UCS-2。所以，现在只有 UTF-16，没有 UCS-2。

再来看看 JavaScript 的诞生时间线，1995 年 5 月，Brendan Eich 用了 10天 设计了 JavaScript 语言；同年 10 月，第一个解释引擎问世；次年11月，Netscape 正式向 ECMA 提交语言标准。再比较 UCS-2 和 UTF-16 的诞生时间，Netscape 公司在那个时间点的比较好的选择就是 UCS-2 编码。

### JavaScript 字符的不足

由于 JavaScript 使用的是 UCS-2 编码，导致它对位于辅助平面的字符（即需要用四个字节表示）操作会出现异常。比如，对于一个 Emoji  表情符 😂，它的码点是 `U+1F602`，对应的 UTF-16 编码是 `0xD83D DE02`。但由于该表情符位于辅助平面会导致 JavaScript 不认识，并且会把它看作单独的两个字符 `U+D83D` 和 `U+DE02`，如下代码：

```javascript
let emoji = '😂';
console.log('\u1F602' === '😂'); // false
console.log(emoji.length); // 2，长度出现问题，但其实它是一个表情符，长度应该为 1
console.log(emoji.charAt(0)); // �，位于 U+D800 到 U+DFFF 是一个空段，即这些码点不对应任何字符
                              // �，对应的码点是 U+FFFD，是一个特殊字符，表示乱码
console.log(emoji.charAt(1)); // �
console.log(emoji.charCodeAt(0)); // 55357，换算成十六进制为 0xD83D
console.log(emoji.charCodeAt(1)); // 56834，换算成十六进制为 0xDE02
```

以上 JavaScript 相关的字符操作都不正确，类似的问题也存在于其它字符函数中。

### ES 6 对字符的改进

ES 6 对字符串有了更多更强的支持，有以下几方面：

- 对码点超出 `\u0000` 到 `\uFFFF` 之间的字符的支持，使用 `u{xxxxx}` 方式来表示：

  ```javascript
  console.log('\u{1F602}'); // 😂
  ```

- ES6 为字符串添加了遍历器接口可以识别大于 `0xFFFF` 的码点，而之前的 `for` 循环是无法识别的：

  ```javascript
  let text = '😂是一个哭笑表情符';
  
  for (let character of text ) {
    console.log(character); // 😂
                            // 是
                            // 一
                            // 个
                            // 哭
                            // 笑
                            // 表
                            // 情
                            // 符
  }
  
  for (let i = 0; i < text.length; i++) {
    console.log(text[i]); // �
                          // �
                          // 是
                          // 一
                          // 个
                          // 哭
                          // 笑
                          // 表
                          // 情
                          // 符
  }
  ```

- 通过 `Array.from(string).length` 可以得到包含四个字节字符的字符串的正确长度：

  ```javascript
  let emoji = '😂';
  Array.from(emoji).length; // 1
  ```

- 新增几个可以处理四个字节字符的函数：

  - `String.prototype.codePointAt()` 返回字符串给定位置字符的码点（十进制）：

    ```javascript
    let emoji = '😂'; // JavaScript 将 emoji 视作两个字符编码 U+D83D U+DE02
    console.log(emoji.codePointAt(0)); // 128514，正确识别字符 😂 并返回码点的十进制
    console.log(emoji.codePointAt(1)); // 56834，返回 U+DE02 码点的十进制
    
    console.log(emoji.charCodeAt(0)); // 55357，无法识别成四个字节表示的字符
    console.log(emoji.charCodeAt(1)); // 56834
    ```

  - `String.fromCodePoint()` 从 Unicode 码点返回对应字符：

    ```javascript
    // 对于 😂
    console.log(String.fromCharCode(0x1F602)); // 
    console.log(String.fromCodePoint(0x1F602)); // 😂
    ```

  - `String.prototype.at()` 返回给定位置的字符串（实验特性，目前还不支持）。

- ES6 提供 u 修饰符，对正则表达式表示四个字节字符提供了支持：

  ```javascript
  let emoji = '😂';
  console.log(/^.$/.test(emoji)); // false
  console.log(/^.$/u.test(emoji)); // true
  ```
  
- 提供 `normalize()` 方法将字符串按照指定的一种 Unicode 正规形式进行正规化。有些字符由字母和语调符号组成，比如 `Ǒ`，Unicode 提供了两种方法来表示。一种是直接提供带重音符号的字符，比如`Ǒ`（`\u01D1`）。另一种是提供合成符号，即字母与语调符号的合成，两个字符合成一个字符，比如将 `O`（`\u004F`）和  `ˇ`（`\u030C`）合成为 `Ǒ`（\u004F\u030C）：

  ```javascript
  // 直接字符
  console.log('\u01D1'); // Ǒ
  
  // 合成字符
  console.log('\u004F\u030C'); // Ǒ
  ```
  
  这两种字符表示方法理应等价，但 JavaScript 却不能准确识别：
  
  ```javascript
  console.log('\u01D1'==='\u004F\u030C' ); // false
  ```
  
  在 ES6 中提供 `normalize()` 方法可以解决这个问题：
  
  ```javascript
  console.log('\u01D1'.normalize() === '\u004F\u030C'.normalize()); // true
  ```
  
  
  
    
  
    







