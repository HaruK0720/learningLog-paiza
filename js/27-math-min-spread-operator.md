# Math.min() + スプレッド構文で最小値取得を最適化

<br>

Paizaで「5つの正の整数から最小値を出せ」という問題に挑戦した。最初は「配列にしてソートすればOK！」と思ったけど、「Math.max()があるならMath.min()もある？」 と気づき、スプレッド構文を組み合わせると一発解決できると分かったのでメモ！

## 問題概要

入力: 5つの自然数（改行区切り）
出力: 最小の数

入力例:
```
10
12
4
8
46
```
出力例:
```
4
```

<br><br>📌初めに思いついたコード
```js
const lines = ["10", "12", "4", "8", "46"];
lines.sort((a, b) => a - b); // 数値比較にする
console.log(lines[0]); 　// ✅ → "4"
```
## <br>❌ NGコード（forループでゴリ押し）
```js
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin", "utf-8");
const numbers = input.split("\n").map(Number);

let mi = 101; // 初期値を適当に大きく
for (let i = 0; i < 5; i++) {
  if (mi > numbers[i]) {
    mi = numbers[i]; // 最小値を更新
  }
}

console.log(mi);
```
👎 問題点:
- ループ不要（もっとシンプルに書ける）
- mi = 101 って…100超えたらバグる

## <br>✅ OKコード（Math.min() + スプレッド構文）
```js
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

const lines = [];
rl.on('line', (input) => lines.push(input));
rl.on('close', () => console.log(Math.min(...lines.map(Number))));
```
👍 改善ポイント:
- スプレッド構文 (...lines) で配列を展開 → Math.min() に直接渡せる
- map(Number) で文字列→数値変換（← これ忘れると NaN 地獄）
- ループ不要 → シンプル＆可読性UP

## <Br>💡 まとめ

✅ Math.min(...配列) で一発解決
✅ map(Number) を忘れると NaN のワナ
✅ ループは勉強にはなるけど、実務ではもっとシンプルに！

[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/19/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%e3%82%b9%e3%83%97%e3%83%ac%e3%83%83%e3%83%89%e6%a7%8b%e6%96%87%e3%81%a7%e6%a5%bd%e3%80%85%e6%9c%80%e5%b0%8f%e5%80%a4%ef%bc%81math/)
