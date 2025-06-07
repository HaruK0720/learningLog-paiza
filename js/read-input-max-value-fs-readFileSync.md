# fs.readFileSync() + Math.max()　標準入力と最大値の求め方を学んだ！



Paizaの「最大値を求めよ」という問題で、なぜか `sort()` を使って遠回り(´;ω;｀) 
調べたら `Math.max()` で一発だったし、`fs.readFileSync()` を使えば競プロ向きのやり方を知ったから使ってみた！

<br>

## NG①：sort()で最大値？無駄に遠回り
```js
const lines = [...標準入力...];  
const sorted = lines.slice(1).map(Number).sort((a, b) => b - a);  
console.log(sorted[0]);  
```
・比較のコストが無駄（最大値だけ知りたいのに全体を並べ替えてる）。
・`sort()` はデフォルトだと文字列比較なので、意図しない並びになることも。

<Br>

何でこうなったか…　まずは、調べずに今知っている知識で頑張ってみようと頭をひねった結果、最近 `.sort` で並び替えをできると学んだからこれで一番大きい数字を配列の一番初めに入れて出力すればいいじゃん！ってなったから(/ω＼)

<br>

## ↓readlineで1行ずつ読むのは遅い
```js
const readline = require('readline');
const rl = readline.createInterface({ input: process.stdin });

const lines = [];
rl.on('line', (line) => lines.push(line));
rl.on('close', () => {
    console.log(Math.max(...lines.slice(1).map(Number)));
});
```
・非同期処理で1行ずつ読み込むため、行数が増えると遅くなる。
・競技プログラミングではこの方法は非推奨らしい。

<br>

## OK：fs.readFileSync() + Math.max()で最適化！
```js
const fs = require("fs");

const input = fs.readFileSync("/dev/stdin", "utf-8").trim();
const lines = input.split("\n");
console.log(Math.max(...lines.slice(1).map(Number)));
```
・同期処理なので一発で入力を読み取れる。
・`.split("\n")`  で即配列化、 `map(Number)`  で型変換もスッキリ。
・`Math.max(...配列)`  で最大値を瞬時に取得（`sort()` いらない！）。

結論：競プロなら `fs.readFileSync()`  +  `Math.max()` 一択！
