# JavaScriptでじゃんけん判定、ミスった…でも解決した話 set と .has()

<br>

「じゃんけん？ 余裕でしょ！」と思って書いたコードが、全然動かない…。よくある問題だからこそ、うっかりミスしがち。僕も lines.length の使い方を間違えてハマったのと、もっとスッキリしたコード書けるのに始めは知らなかった。

↓
NGコードでは、`lines.length` をそのままループに使ったけど、実際には `lines[0]` に試合数が入っていた…。
さらに、勝ちパターンの判定を `if` 文でゴリゴリ書いていて、もっとスッキリ書ける方法があった。

<br>

## 改善前（ゴリゴリ if 文）
```js
if (lines[count] == "G C" ) {
    win++;
} else if (lines[count] == "C P") {
    win++;
} else if (lines[count] == "P G") {
    win++;
}
```

このままだと、勝ちパターンが増えたときに `else if` の嵐になってしまう…。

<br>

## 改善後（Set でスッキリ！）
```js
const winPatterns = new Set(["G C", "C P", "P G"]);  
for (let count = 1; count <= Number(lines[0]); count++) {  
    if (winPatterns.has(lines[count])) win++;  
}
```
`Set` を使えば、`if` 文の連続が消えて、勝ちパターンの管理も楽になる！

<br><br>

## Set とは？

`Set` は、重複を許さないデータを管理する便利なオブジェクト。配列と違って `.has()` メソッドで値の存在チェックが簡単にできる！

```js
const winPatterns = new Set(["G C", "C P", "P G"]);  
console.log(winPatterns.has("G C")); // true  
console.log(winPatterns.has("P P")); // false
```
