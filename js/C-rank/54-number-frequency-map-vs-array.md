# 数値の出現率　Map() vs 配列

<br>

paizaの「数字の出現回数カウント」の問題を解いた！出現回数をカウントする問題は前に解いたことあったけど、地味なミスを連発した(´;ω;｀)　あと、正解は一つじゃないんだと改めてわかったことを記録するよ！

## <br>🎯 問題概要：`0〜9`の出現回数を数えるだけ
入力例：
```
5  // N
1 2 3 3 6  
```
出力例：
```
0 1 1 2 0 0 1 0 0 0
```
N個の整数（0〜9）が与えられるので、それぞれの出現回数をカウントし、
0から9の順で1行に出力するだけ。

## <br>❌NG例
```js
const nums = lines[1].split(' ');
const count = new Map();

for (let i = 1; i <= 9; i++) {
    count.set(i, 0); // ❌ 0入れ忘れた
}

nums.forEach(a => {
    count.set(a, count.get(a) + 1); // ❌ aが文字列なのでNaNになる
});

count.forEach(v => console.log(v)); // ❌ 出力が改行されてしまう
```

ミス：
- 0 を初期化し忘れる
- `.split(' ')` で得た値が文字列 → `.map(Number)` を忘れると事故る
- 出力が 10行 に分かれる（`join`が必要）

## <br>✅ OK例：Mapをちゃんと使うならこう
```js
const nums = lines[1].split(' ').map(Number);
const count = new Map();

for (let i = 0; i <= 9; i++) {
    count.set(i, 0);
}

nums.forEach(a => {
    count.set(a, count.get(a) + 1);
});

const result = [];
count.forEach(value => result.push(value));
console.log(result.join(' '));
```

## <br>✨配列でスマートに！
```js
const nums = lines[1].split(' ').map(Number);
const counts = Array(10).fill(0);

for (const n of nums) {
    counts[n]++;
}

console.log(counts.join(' '));
```
解説：
- `Array(10).fill(0)`：0〜9に対応する配列を初期化
- `counts[n]++`：そのままインデックスで加算
- `console.log(counts.join(' '))`：出力は1行で整形済み

<br>なぜ配列がいいのか？
- キーが固定（0〜9）ならMapより配列が高速＆簡潔
- 書く量も読む量も少なくなる＝タイパがいい

## <br>🧠 まとめ：Mapに頼りすぎない！

Mapももちろん便利だけど、キーが固定なら配列で十分。

<Br>覚えておきたい構文：
- `Array(10).fill(0)`
- `.map(Number)`（文字列→数値）
- `.join(' ')`（出力整形）


<br><br>[僕の失敗談(´;ω;｀)と解決話🚀](https://paizabeginner.wordpress.com/2025/04/15/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e6%95%b0%e5%ad%97%e3%81%ae%e5%87%ba%e7%8f%be%e7%8e%87%ef%bc%81/)

