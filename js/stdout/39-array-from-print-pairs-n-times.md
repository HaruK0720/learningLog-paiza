# Array.from() でループ不要！ (A, B) をN回出力するスマートな方法

<br>

Paizaの問題を解いていたら、 `(A, B)` を`N`回出力するだけなのに、もっとスッキリ書ける方法があった！ `for`ループを使うのもアリだけど、`Array.from()` を使うと一発で解決できるぞ！

問題:`(A, B)` を`N`回出力せよ！

📌 入力例
```
3 5 7　　// N A B
```
📌 出力例
```
(5, 7), (5, 7), (5, 7)
```
👉 `N`回 `(A, B)` を出力し、各ペアは カンマ + 半角スペース で区切る。

https://paiza.jp/works/mondai/stdout_primer/stdout_primer__specific_format_step2


## <br>最初のNGコード: `for`ループでゴリ押し
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const [N, A, B] = input.split(" ").map(Number);
    const nums = [];
    for(let i = 0; i < N; i++){
        nums.push(`(${A}, ${B})`);
    }
    console.log(nums.join(", "));
});
```
💡 問題点
- `for`ループで `N` 回 `push()` するのがちょっと冗長
- 変数 `nums` を使わずにもっと短く書ける

## <br>改善版: `Array.from()` でループを消す！
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const [N, A, B] = input.split(" ").map(Number);
    console.log(Array.from({ length: N }, () => `(${A}, ${B})`).join(", "));
});
```
改善ポイント🚀
✅ `Array.from()` で配列を1行で生成
✅ ループ不要でスッキリ！
✅ 可読性UP！ 余計な変数なし

## <br>気づきメモ📝
- `Array.from({ length: N })` は長さ`N`の配列を作る
- マッピング関数 `() => \(${A}, ${B})`` ` で各要素をセット

    ループを書かなくても、スマートに配列を作れる！

## <br>まとめ

✔ `for`ループより `Array.from()` を使うと短く書ける
✔ 繰り返し処理はループ以外の方法も考えよう
✔ 可読性が上がるとバグも減る！

こういう小さな工夫が、"書きやすくて読みやすい" コードにつながるぞ！💡

<br>[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/31/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-a-b-%e3%82%92n%e5%9b%9e%e5%87%ba%e5%8a%9b%ef%bc%81%e3%83%ab%e3%83%bc%e3%83%97%e3%81%aa%e3%81%97%e3%81%a7%e3%82%b9%e3%83%9e/)
