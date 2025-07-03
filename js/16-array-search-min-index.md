# 配列検索! 配列から最小 index を探す方法！

<br>

Paizaで「特定の財産額を持つ人を探す」問題に挑戦！

でも `.length` で `k` を取得しようとしてズッコケ、`Number()` を忘れて比較エラー…。

配列検索の基礎をガッツリ学んだのでシェアするよ！

<br><br>

## 問題概要
1から n まで番号がついた人々の財産リスト a_1, ..., a_n がある。

財産が k 円の人の番号を出力する。ただし、同じ k を持つ人が複数いた場合は 最も小さい番号を出力する。

<br>

入力例
```
5　//n
3
7
5
7
2
7　// k
```

出力例
```
2
```

<br><br><br><br>

## NG例（今回やらかしたコード）
```js
const N = lines[0]; // ❌ Number()を忘れた
const k = Number(lines.length); // ❌ .length は配列の要素数！
for (count = 1; count <= N; count++) { // ❌ let なし
    if (lines[count] === k) { // ❌ 数値変換忘れ
        console.log(count);
        break;
    }
}
```

<br><br><br>

## OK例（修正コード）
```js
const N = Number(lines[0]);  // ✅ しっかり数値変換！
const k = Number(lines[N + 1]);  // ✅ k の正しい取得方法！
for (let count = 1; count <= N; count++) {
    if (Number(lines[count]) === k) {
        console.log(count);
        break;
    }
}
```

### ミスしないためのポイント
✅ `Number()` を忘れると文字列比較になってバグる！

✅`for` ループの `let` を忘れるとグローバル変数になってエラー！

✅ `break;` を使えば最初に見つけた `k` でループを抜けられて効率的！

✅ `.length` は配列の要素数であり、`k` の取得には関係ない！
→配列の最後の要素を取りたいなら `lines[lines.length - 1]` を使うべき？

<br><br><br>

## まとめ
配列検索で「最初に見つけた位置」を出力するならインデックス記録が最適解！

<br>

🔹 フラグ vs インデックス記録：f`ound` フラグで判定する方法もあるけど、

この問題ではインデックス記録の方がシンプル！

🔹 forEach は break できない → for ループを使おう！
