# includes()で文字列検索をスッキリ解決！

<br>

Paizaの問題を解いていて、文字列検索でハマった話。`includes()` という便利メソッドがあるらしいけど、最初はゴリ押しで書いたらミス連発。
結局、シンプルに `includes()` を使えば解決する話だった。

<br><br>

# ❌ NGコード：無駄なループ & バグ
```js
const charA = lines[0]();
const stringS = lines[1].split('');

let found = false;
for (let i = 0; i < stringS.length; i++) {
    if (stringS[i] === charA) {
        found = true;
        break;
    }
}
console.log(found ? "YES" : "NO");
```

🚨 ループ不要 → `includes() `なら一発で判定

🚨 変数管理が面倒 → `found` いらない

🚨 コードが長い → 三項演算子でスッキリ

<br>
<br>
<br>

# ✅ OKコード：スマートに解決！
```js
const charA = lines[0].trim();
const stringS = lines[1].trim();

console.log(stringS.includes(charA) ? "YES" : "NO");
```

✨ `includes()` で即判定！
✨ `trim()` でバグ防止！
✨ 三項演算子 `(? :)` で簡潔！

「特定の文字が含まれるか？」を判定するときは、迷わず `includes()` を使おう！🔥
