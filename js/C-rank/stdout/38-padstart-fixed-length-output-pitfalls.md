# 文字数指定の出力でミス！padStart()の罠！

<br>

Paizaの「指定幅での出力」問題に挑戦したら、`padStart()`でエラー発生。まさかの 「数値には使えません」 で撃沈…。でも、解決策はシンプルだった！

## <br>📌 問題: 数値を `M` 桁の幅で出力する

与えられた `N` を `M` 桁 になるようにスペースを埋めて出力する。

<br>入力例
```
42 5
```
出力例
```
   42
```
（”42” の前に3つの半角スペース）

https://paiza.jp/works/mondai/stdout_primer/stdout_primer__print_width_step4


## <br>💥 NGコード: 数値のまま `padStart()` を使ってエラー

最初、シンプルに書いてみたけど、エラーになった…。
```js
const [N, M] = input.split(" ").map(Number);  
console.log(N.padStart(M, ' '));  
// ❌ TypeError: N.padStart is not a function
```
😱 なぜエラー？

→ `padStart()` は 文字列専用！ `map(Number)` で `N` を数値に変換してしまったせい。


## <br>✅ OKコード: 文字列のまま `padStart()` を使う！

数値変換せず、`N` を 文字列のまま 使えば解決。
```js
const rl = require('readline').createInterface({input: process.stdin});

rl.on('line', (input) => {
    const [N, M] = input.split(" ");  // Nは文字列のまま
    console.log(N.padStart(Number(M), ' '));  // ✅ スペース埋めOK
    rl.close();
});
```
🔍 修正ポイント
`- .map(Number)` を 削除 → `N` を 文字列のまま 扱う
- `padStart(Number(M), ' ')` で `M` 桁 までスペース埋め

## <br>💡 メモ

✅ `padStart()` は 文字列専用！
✅ `.map(Number)` は便利だけど、型変換に要注意
✅ 文字列として扱う場面なら、余計な変換は不要

## <br>🎯 まとめ: 型の意識、大事！

✔ `padStart()` を使うなら、文字列かどうか確認！
✔ `map(Number)` を使うと、意図せず型が変わることがある
✔ `JavaScript`の型変換は 無意識にやるとハマる

こういう 一見簡単な問題 ほど、意外と落とし穴があるんだよね…。


<br>[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/30/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e6%96%87%e5%ad%97%e6%95%b0%e6%8c%87%e5%ae%9a%e3%81%ae%e5%87%ba%e5%8a%9b%e3%81%a7%e3%83%9f%e3%82%b9%e3%81%a3%e3%81%9f%e8%a9%b1/)
