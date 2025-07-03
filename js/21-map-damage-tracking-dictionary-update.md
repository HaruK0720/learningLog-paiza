# Mapでダメージ管理！（辞書のデータ更新）

<br>

Paizaの問題を解いていて、「攻撃された人のダメージ合計を計算する」ってタスクにハマった話。

最初は `Map` をうまく使えず、攻撃ログを見事にスルー…。でも `set` と `get` を活用すれば、スッキリ解決できた！先に言っちゃうと、今回の最大の失敗は、「問題文をちゃんと読んでいなかったこと」(´;ω;｀)

<br><br>

## 問題の概要

入力
- n 人の名前（初期ダメージは0）
- m 回の「攻撃された人の名前」と「受けたダメージ」
- ダメージ合計を知りたい人の名前 S

出力
- S の合計ダメージ

<br>

入力例
```
2　　　　　//n
Kirishima
Kyoko
2　　　　　//m
Kyoko 1
Kyoko 2
Kyoko　 　//S
```

出力例
```
3
```

<br><br><br><br><br>


## NG例（ミスったコード）
```js
const n = Number(lines[0]);
const S = lines[n + 1]; // m（攻撃回数）を読み飛ばしてる！
const dict = new Map();

for (let i = 1; i <= n; i++) {
    const [key, value] = lines[i].split(" "); // 名前リストを攻撃データと誤認
    dict.set(key, Number(value));
}
```

何がダメ？

✅ m（攻撃回数）を無視 → そもそも攻撃が記録されない

✅ n の名前リストをダメージデータと誤認

<br><br><br>

## OK例（修正後）
```js
const damageMap = new Map();

// 初期化（全員のダメージ = 0）
for (let i = 1; i <= n; i++) {
    damageMap.set(lines[i], 0);
}

// ダメージ集計
for (let i = n + 2; i < n + 2 + m; i++) {
    let [person, damage] = lines[i].split(" ");
    damageMap.set(person, damageMap.get(person) + Number(damage));
}

console.log(damageMap.get(S));
```

ポイント

✅ `Map` を使い、キー（名前）→ 値（ダメージ） を管理

✅ 初期値 0 をセットし、undefined を防ぐ

✅ `get` で現在値を取得し、`set` で更新


<br><br><br>

## まとめ
問題文をちゃんと読めば、`Map` で楽にデータを管理できる！

辞書的なデータ更新には `Map` を活用しよう！

問題文はちゃんと読もう！(´;ω;｀)
