# forEachよりfor-of！フラグ管理でループを最適化

<br>

「フラグ管理」問題に挑戦した高2の僕。最初は7の個数をカウントしてたけど、実はもっとシンプルな方法があった！


<br><br>

## 問題概要
与えられた数列に「7」が含まれているか判定し、1つでもあれば "`YES`" 、なければ "`NO`" を出力する。


<br><br><br><br>

## NG例（非効率）
```js
rl.on('close', () => {
    let seven = 0;
    lines.slice(1).map(Number).forEach(num => {
        if (num === 7) seven++;
    });
    console.log(seven > 0 ? "YES" : "NO");
});
```

❌ 問題点:
- 7 を数える必要はなく、1つ見つかった時点で確定できる
- `forEach` は途中で止められず、無駄なループが発生

<br><br><br>

## OK例（最適化）
```js
rl.on('close', () => {
    let foundSeven = false;

    for (let num of lines.slice(1).map(Number)) {
        if (num === 7) {
            foundSeven = true;
            break;
        }
    }

    console.log(foundSeven ? "YES" : "NO");
});
```

✅ 改善点:

✔ フラグ (foundSeven) で 存在確認のみに 絞る

✔ `break` を使い、最短で処理終了

✔ `for-of` は `forEach` と違い、途中でループを抜けられる

<br><br><br><br>

## まとめ
「カウント」ではなく「存在確認」ならフラグ管理が最適！
