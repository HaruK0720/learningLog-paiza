
# split(" ") の初心者らしいミスと正しい使い方

<br>


今日は改行出力の問題を解いた！「配列を `split(" ") で分割すればいいっしょ？`」って思った僕、無事撃沈しました。
実は `split()` は文字列専用 なので、配列に使うとエラーになります。

<br>

## NG例
```js
const numbers = lines.split(" "); 
// ❌ エラー！（linesは配列）
```
<br>

## OK例
```js
const numbers = lines[1].split(" "); // 2行目の文字列を分割
numbers.forEach(num => console.log(num)); // 各要素を改行出力
```

☆ポイント
- `lines[1]` を指定して 文字列 を取得
- それに対して `split(" ")` を適用

<br>

## 💡 まとめ
- `split(" ")` は 文字列にしか使えない
- 改行出力は `forEach()` か `join("\n")` でOK
