# 配列そのまま渡してない？Math.max()の正しい使い方。スプレッド構文 (...) ってなに？

<br>

Paizaの問題で`Math.max()`を使ったら、まさかのエラー。
「配列をそのまま渡せない」 という基本的なポイントを見落としてた…。
同じミスをしないように、サクッと整理！


## <br>問題概要

問題: A, B, C の最大値と最小値の差を求める
```
入力: 30 50 10
出力: 40
```
https://paiza.jp/works/mondai/data_structure/data_structure__array_step3


## <br>NG例（間違いコード）
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const nums = input.split(' ').map(Number);
    console.log(Math.max(nums) - Math.min(nums));  // ここが間違い！
    rl.close();
});
```
→ `Math.max()` や `Math.min()` に 配列そのまま 渡すとエラー！

## <br>OK例（正しいコード）
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const nums = input.split(' ').map(Number);
    console.log(Math.max(...nums) - Math.min(...nums));  // スプレッド構文で展開
    rl.close();
});
```
→ `...`（スプレッド構文）を使うことで、配列の要素を 個別の引数 として渡せる！

### <br>解説
- `Math.max()` や `Math.min()` は 個別の引数 を受け取る関数
- 配列をそのまま渡すと 型が合わずエラー
- スプレッド構文 (`...`) を使うと、配列の中身が展開される！


### <br>まとめ

✅ `Math.max()` や `Math.min()` に 配列を直接渡さない
✅ スプレッド構文 (`...`) で配列を展開 して渡す
✅ デバッグ時、「配列のまま渡してない？」をチェック！


<br><br>[僕の失敗談(´;ω;｀)🚀](https://paizabeginner.wordpress.com/2025/04/12/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e3%80%8c%e6%9c%80%e5%a4%a7%e5%80%a4%e3%81%a8%e6%9c%80%e5%b0%8f%e5%80%a4%e3%80%8d%e8%a8%88%e7%ae%97%e3%83%9f%e3%82%b9/)


## おまけ：　スプレッド構文 (...) ってなに？
<br>
### ❓ なぜ `Math.min()` や `Math.max()` に使うの？

`Math.min()` や `Math.max()` は 複数の数を引数として渡す関数。
でも 配列をそのまま渡してもダメ だから、スプレッド構文でバラしてあげる！

<br>

#### ❌ うまくいかない例
```js
const numbers = [3, 5, 1, 9, 2];

console.log(Math.min(numbers)); // NaN（計算できない）
console.log(Math.max(numbers)); // NaN
```
→ 配列のまま渡すと 1つの値（配列）と認識されてしまう からエラーになる💦

#### ⭕ 正しいやり方（スプレッド構文を使う）
```js
const numbers = [3, 5, 1, 9, 2];

console.log(Math.min(...numbers)); // 1
console.log(Math.max(...numbers)); // 9
```
→ `...numbers` で 配列の中身をバラして渡せる からOK！

実際には `Math.min(3, 5, 1, 9, 2)` になってるよ✨

<br>

### スプレッド構文ってどんなときに使うの？

#### ① 配列をくっつけるとき
```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]
```
→ `concat()` を使わずに簡単にくっつけられる！

#### <br>② 配列をコピーするとき
```js
const original = [10, 20, 30];
const copy = [...original];

console.log(copy); // [10, 20, 30]
```
→ `copy = original` だと 同じデータを参照 しちゃうけど、
`...` を使うと 新しい配列を作れる！

#### <br>③ 関数の引数に配列を渡すとき
```js
function sum(a, b, c) {
    return a + b + c;
}

const values = [5, 10, 15];

console.log(sum(...values)); // 30
```
→ `sum(5, 10, 15)` と同じになるよ！

### <br>まとめ
- スプレッド構文 (`...`) は配列やオブジェクトの中身をバラして使う方法
- `Math.min()` や `Math.max()` に配列を渡すときに便利！
- 配列の結合・コピー・関数の引数として使う ことができる

スプレッド構文を使うと、コードが スッキリ・簡単・ミスしにくい からどんどん使おう！ 🎉


### ❓...numbers で 配列の中身をバラして渡せる からOK！とあるけど、③の例にあたる？
✅ そう！ `Math.min(...numbers)` は、スプレッド構文を使って「関数の引数として配列を展開する」例だから、③の例にあたる！

詳しく説明すると…

#### ✅ Math.min(...numbers) の仕組み
```js
const numbers = [3, 5, 1, 9, 2];

console.log(Math.min(...numbers)); 
// ↓ 展開されてこうなる
console.log(Math.min(3, 5, 1, 9, 2)); // 1
```
→ `Math.min()` は 複数の数を引数として受け取る関数 だから、スプレッド構文を使って `numbers` の 中身を展開 することで正しく動く！

#### <br>✅ ③の sum(...values) の仕組み
```js
function sum(a, b, c) {
    return a + b + c;
}

const values = [5, 10, 15];

console.log(sum(...values));
// ↓ 展開されてこうなる
console.log(sum(5, 10, 15)); // 30
```
→ ここでも、スプレッド構文を使って 配列の要素をバラして関数に渡している から `Math.min(...numbers)` と同じ考え方！

#### <br>✅ Math.min(...numbers) と sum(...values) 

![スクリーンショット 2025-04-10 224757.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/4018208/c06787cc-08f8-409c-abc2-23494cbe3750.png)


どちらも「関数の引数として配列を展開する」用途だから、③の例にあたるよ！💡

