# 「count++」の極意：配列内の出現回数を正しくカウントする方法

<br>

Paizaの問題を解いていて、「`.includes()`」を使ったら大失敗…。

「含まれるか」じゃなくて「何回あるか」が大事な時は `count++` すべきだった！

<br><br>


## 📝 問題概要

N人の参加者が、M個の数字を紙に書く。

後で発表されるKと一致する数字の個数が、その人の得点になる。

<br>

入力例:
```
4 5 2
1 3 4 4 5
2 2 2 2 2
1 2 3 4 5
1 1 1 1 1
```

出力例:
```
0
5
1
0
```

<br><br><br><br><br>

## 💀 NG例（間違いコード）
```js
const nCount = lines.filter(num => num.includes(K)).length;
console.log(nCount);
```

<br><br>

### ❌ .includes(K) は「Kが含まれているか」しか判定できず、出現回数をカウントできない。
<br>

```js
lines.filter(num => num.includes(K))
```

- `lines` から、K を含む要素だけを抽出する。
- `num.includes(K)` が `true` の要素だけを返す。
- `filter` の結果は配列になる。

<br><br>

```js
.length
```

- `filter` の結果（配列）の要素数（＝Kを含む要素の個数）を取得。

<br><br>

このコードは `lines` の各行に `K` が含まれるかをチェックして、「 K を 1 つでも含む行の数」 をカウントしてしまい、この配列の `length` を取ると、「 K を含む行の数」 になっちゃう。
    
この問題では「K がある行の数」ではなく、「K が何回あるか」が必要！

<br><br><br>

## ✅ OK例（正しいコード）
```js
const [N, M, K] = lines[0].split(" ").map(Number);

for (let i = 1; i <= N; i++) {
    let count = 0;
    const numbers = lines[i].split(" ").map(Number);

    for (let j = 0; j < M; j++) {
        if (numbers[j] === K) {
            count++;
        }
    }
    console.log(count);
}
```

✅ `count++` を使ってKの出現回数をカウント

✅ 参加者ごとに `count = 0` をリセット

<br><br><br>

## 🎯 学んだこと
- `.includes()` は 存在チェック、カウントには向かない！
- `for` ループ + `count++` で 回数を正しくカウント！
・問題の意図を見極めることが大事！（僕は間違えました）

Paizaの問題で地味にハマったけど、配列の扱い方がスッキリ理解できた！
