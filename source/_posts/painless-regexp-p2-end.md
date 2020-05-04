---
title: 無痛學正規表達式 —— 後篇
date: 2020-05-04 11:29:11
tags: [基礎, 正規式]
---
（這是本系列的第二部分。如果你還沒看過前篇，你可以[先閱讀一下](/2020/05/03/painless-regexp-p1/)！）

讀完前篇後，希望你已經開始享受正規式的魔力了！讓我們再來學一些進階點的東西！

<!-- more -->

# 旗標 （Flags）

首先我們要來學的是旗標。如果你已經開始上網找正規式的範例的話，你可能會發現其中的例子中包含了 `g` 或是 `i` 、或 `gi` 等。這個東西叫做 **flags** （旗標），可以用來告訴程式除了正規式的本體外，還要再加上什麼額外的條件。

雖然我只會在這裡介紹三個 Flag，g、i、跟 m，但其實還有其他幾個 flag。讀完這個教學之後你可以再上網找找 ;)

## 「g」Flag — 找出所有符合的！全域搜尋！

下面這個例子可以列出句子中的所有母音：

```jsx
const input = 'All human beings are born free and equal in dignity and rights. They are endowed with reason and conscience and should act towards one another in a spirit of brotherhood.'

console.log(input.match(/[aeiou]/g))
// [
//  'u', 'a', 'e', 'i', 'a', 'e', 'o', 'e',
//  'e', 'a', 'e', 'u', 'a', 'i', 'i', 'i',
//  'a', 'i', 'e', 'a', 'e', 'e', 'o', 'e',
//  'i', 'e', 'a', 'o', 'a', 'o', 'i', 'e',
//  'e', 'a', 'o', 'u', 'a', 'o', 'a', 'o',
//  'e', 'a', 'o', 'e', 'i', 'a', 'i', 'i',
//  'o', 'o', 'e', 'o', 'o'
// ]
```

如果沒有加上 `g` Flag 的話，就只會回傳第一個找到的結果：

```jsx
console.log(input.match(/[aeiou]/))
// ['u']
```

相信如果你有跟著練習前一篇文章中的例子的話，你應該已經發現了 :)

另外值得一提的是如果你同時使用了 `g` 跟群組功能（小括號）的話，**回傳的結果不會包含群組**。

## 「i」Flag — 忽略大小寫！

前面的例子雖然找出了母音，但其實結果上還缺了一個母音 —— 開頭的「A」！因為 `[aeiou]` 只有找出小寫字母，`A` 並沒有包含在裡面。

當然，我們可以這樣寫 `[aeiouAEIOU]`：

```jsx
console.log(input.match(/[aeiouAEIOU]/g))
// [
//   'A', 'u', 'a', 'e', 'i', 'a', 'e', 'o',
//   'e', 'e', 'a', 'e', 'u', 'a', 'i', 'i',
//   'i', 'a', 'i', 'e', 'a', 'e', 'e', 'o',
//   'e', 'i', 'e', 'a', 'o', 'a', 'o', 'i',
//   'e', 'e', 'a', 'o', 'u', 'a', 'o', 'a',
//   'o', 'e', 'a', 'o', 'e', 'i', 'a', 'i',
//   'i', 'o', 'o', 'e', 'o', 'o'
// ]
```

但這樣寫很煩，而且會讓正規式變得更複雜。讓我們來試試看 `i` Flag！

```jsx
console.log(input.match(/[aeiou]/gi))
// [
//   'A', 'u', 'a', 'e', 'i', 'a', 'e', 'o',
//   'e', 'e', 'a', 'e', 'u', 'a', 'i', 'i',
//   'i', 'a', 'i', 'e', 'a', 'e', 'e', 'o',
//   'e', 'i', 'e', 'a', 'o', 'a', 'o', 'i',
//   'e', 'e', 'a', 'o', 'u', 'a', 'o', 'a',
//   'o', 'e', 'a', 'o', 'e', 'i', 'a', 'i',
//   'i', 'o', 'o', 'e', 'o', 'o'
// ]
```

如何？跟前一個例子一樣獲得了相同的結果。你可以組合使用多個 Flag，或是一次只使用一個。如果要同時用多個的話，順序並不影響。你可以寫成 `/[aeiou]/gi` 或是 `/[aeiou]/ig`。

## 「e」Flag — 在多行中尋找！

這個 Flag 我們會在後面錨點的段落中討論。

# 貪婪 (Greedy) 與慵懶 (Lazy)

我們在前篇已經提過正規式的「貪婪」了，但我並沒有明確講出這個名字。來看看貪婪的正規式長什麼樣：

```jsx
const input = "<a href='http://example.com' target='_blank'>連結</a>"

console.log(input.match(/href='.+'/))
// ["href='http://example.com' target='_blank'"]
```

由於「`.+`」會找到所有符合的字元 —— 一路到碰到最後一個 `'` 而不是第一個遇到的。

有方法可以讓他懶惰點，只找到最少的符合的字元。還記得我們在提到 `?` 時有說它還有另一個功能嗎？讓我們在加號後加上 `?` 試試：

```jsx
console.log(input.match(/href='.+?'/))
// ["href='http://example.com'"]
```

懶惰符號（`?`）可以跟加號 `+` 已經 `*` 一起使用。

# 錨點（Anchor）

錨點代表一個特定的位置，例如字串的開始與結束。

- ^ 行起始
- $ 行結尾

要驗證字串時很好用：

```jsx
const pattern = /^https?:\//.+$/i

pattern.exec('https://example.com') // 有結果
pattern.exec('   https://example.com') // 無結果
```

由於 `.+` 是貪婪查詢，會匹配直到字串結束的所有字元，不寫 `$` 也可以。

當要處理多行的字串時，我們可以搭配 `m` Flag 使用：

```jsx
const input = `Article 1

All human beings are born free and equal in dignity and rights. They are endowed with reason and conscience and should act towards one another in a spirit of brotherhood.

Article 2

Everyone is entitled to all the rights and freedoms set forth in this Declaration, without distinction of any kind, such as race, colour, sex, language, religion, political or other opinion, national or social origin, property, birth or other status.

Furthermore, no distinction shall be made on the basis of the political, jurisdictional or international status of the country or territory to which a person belongs, whether it be independent, trust, non-self-governing or under any other limitation of sovereignty.

Article 3

Everyone has the right to life, liberty and the security of person.`

console.log(input.match(/^E.+/gim)) // 會找出所有以 E 開頭的行
```

加上 `m` Flag 後，每一行的開頭都可以被 `^` 配對，而結尾則是 `$`。若沒有 `m` Flag，則 `^` 與 `$` 只會配對到整個字串的開頭與結尾。

# 字元分類 (Character Classes)

還記得我們用了 `[0123456789]` 來找電話號碼嗎？之後我們改用了 `[0-9]`。但其實在正規式裡還有另一個方法可以找到數字：`\d`，代表數字（digits）。他的作用跟 `[0-9]` 一模一樣。還有其他的字元分類，我這裡只列出幾個我認為常用的。

|字元|同等於|說明|
|---|---|---|
|`\d`|`[0-9]`|數字 Digits|
|`\D`|`[^0-9]`|非數字|
|`\w`|`[a-zA-Z]`|字母 Words|
|`\W`|`[^a-zA-Z]`|非字母|
|`\s`| |空白字元（空格、換行、Tab 等）|
|`\S`| |非空白字元|

# 結語

前篇中我們學會了正規式所需要的所有基礎。在大多數的情況下你可以只用前篇中介紹的語法。

而我把所有我認為並不必要的東西放在後篇中，例如 `i` Flag，你可以用 `[aA]` 這樣的方式來處理大小寫。你也可以用其他 JavaScript 的程式碼來處理全域搜尋而不用 `g` Flag。

如果你還有其他問題的話，儘管留言吧！

祝好運！
