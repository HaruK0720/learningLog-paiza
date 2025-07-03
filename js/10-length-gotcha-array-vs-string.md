# .length の罠！配列と文字列の違いに気をつけよう

<br>

Paizaの問題で「整数の文字数を出力せよ」と言われ、`.length` を使ったら、思った結果と違う値が出た…。
実は `.length` は「配列」と「文字列」で意味が変わる！`length` の使い方を間違えるとこうなる！

<br><br>

## NGコード（間違えた例）
```js
const n = lines[0];  
for (let i = 1; i <= n; i++) {  
    console.log(lines[i].length);  
}
```

### ❌ なぜ間違い？

`lines[i]` は 「配列の要素」 なので、`.length` を使うと「文字数」ではなく「配列の長さ」を返してしまう！

<br><br>

## 正しい .length の使い方

### OKコード（修正後）
```js
const n = Number(lines[0]);  
for (let i = 1; i <= n; i++) {  
    console.log(String(lines[i]).length);  
}
```

### ✅ 改善点
- `String(lines[i])` で確実に文字列化！
- `Number(n)` で `n` を数値にして正しくループ！

<br><br><br>

## .length まとめ！

💡 配列の `.length` → 要素数 を返す！
💡 文字列の `.length` → 文字数 を返す！
💡 確実に文字列化するなら `String(x)` を使う！

<br>

`.length` を使うときは、データの型を確認するのが大切ですね！
