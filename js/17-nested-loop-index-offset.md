# 多重ループ問題！行番号とindexのズレに注意！

<br>

Paizaの多重ループ問題で「m+2」を使って爆死…。

行番号と配列のインデックスのズレに気づかず…。でも解決しました！

<br><br>

## 問題概要
m 個の文字 c_1, ..., c_m と n 個の文字列 S_1, ..., S_n が与えられ、

各 c_i が各 S_j に含まれるかを判定して "YES" / "NO" を出力する問題。

<br>

入力例
```
1
a
2
paiza
kyoko
```

出力例
```
YES
NO
```

<br><br><br><br>

## ⭐ミスったポイント

問題文に「`(m + 2)` 行目に `n` がある」と書いてあったから、
```js
const n = lines[m + 2]; // インデックスのズレでエラー！
```
と書いたけど、動かない…。

<br>

⭐️ 行番号は 1 から始まるけど、配列のインデックスは 0 から！

だから `m + 1` を使うのが正解だった。


<br><br><br>

## 解決コード
```js
const n = Number(lines[m + 1]); // 正しく n を取得！

for (let count = 1; count <= m; count++) {
    let charC = lines[count];
    for (let i = 1; i <= n; i++) {
        let stringS = lines[m + 1 + i];
        console.log(stringS.includes(charC) ? "YES" : "NO");
    }
}
```

ポイント

✅ `.includes()` で YES / NO をスマート判定

✅ `m+1` で `n` の取得ミスを回避

✅ 三項演算子 `? :` でコードを短縮

これで僕もエラー地獄から脱出！
