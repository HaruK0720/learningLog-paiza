# process.stdout.write() vs repeat() 最適解はどっち？

<br>

Paizaの問題「`N`個の `*` を並べる」で `process.stdout.write()` を使ったら、まさかの非効率…。`repeat()` を知っていれば一瞬だった ので、メモしておく。

## 問題概要

入力された整数 `N` に対して、`N` 個の `*` を横一列に出力する。例えば `N=5` ならこうなる。
```
*****
```
## <br><br>💥 初めの実装（`for` ループ + `process.stdout.write()`)
```js
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', (input) => {
    const N = Number(input);
    for (let count = 0; count < N; count++) {
        process.stdout.write("*");
    }
    rl.close();
});
```
😢 何が問題？
- I/O負荷が高い → `process.stdout.write()` を毎回呼ぶため、パフォーマンスが悪い
    ループが冗長 → `*` を `N` 回出力するだけなのに、明らかに大げさ

## <br>✨ `repeat()` を使えば一発！
```js
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.on('line', (input) => {
    console.log('*'.repeat(Number(input)));
    rl.close();
});
```

### <br><br> ✅ `repeat()` が最適な理由
- シンプルで読みやすい → `'*'.repeat(N) `だけで `N` 個の `*` を生成
- パフォーマンスが良い → 文字列を一括生成するため、無駄なループが不要
- I/Oコストが低い → `console.log()` 1回だけで済む


## 📝 まとめ
もし `repeat()` を知らなかったら、ずっと無駄な `for` ループを書いていたかも…。
「短いコード ＝ 速いコード」じゃないかもだけど、無駄を省けるなら省こう！

<br>最近は復習のような感じで標準出力の基礎問題を解いている。初めて解いたときなら、`process.stdout.write`で改行せずに出力できるんだ、と学んだだけで満足していたけど、少しレベルアップした今なら、もっとシンプルなコードをかけないかな？ と試行錯誤する余裕が生まれた。今後も頑張っていきたい(^^ゞ

[👉 僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/20/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-process-stdout-write-vs-repeat-%e6%9c%80%e9%81%a9%e8%a7%a3%e3%81%af%e3%81%a9%e3%81%a3%e3%81%a1%ef%bc%9f/)
