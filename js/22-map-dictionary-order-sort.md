# Map の辞書順ソートでハマらない方法

<br>

Paizaの問題を解いていたら、「攻撃された人のダメージを記録し、名前の辞書順で出力せよ！」という指示に見事にハマった。
辞書順って何！？ Map に直接ソートができない！？試行錯誤の末、「キーを配列化してソートすればいい」 という学びを得たので、その知見をシェアする！

<br><br>


## 問題概要

攻撃された人のダメージを記録し、名前の辞書順で出力。

<br>

入力例:
```
3　　　　　//n
PAIIZA
PAIZA
PAIIIZA
2　　　　　//m
PAIIZA 2
PAIIIZA 3
```

出力例:
```
3
2
0
```

<br><br><br><br><br>


## NG例: Map に頼りすぎたコード
```js
const damageMap = new Map();
for (let i = 1; i <= n; i++) {
    damageMap.set(lines[i], 0);
}
for (let i = n + 2; i < n + 2 + m; i++) {
    let [person, damage] = lines[i].split(" ");
    damageMap.set(person, damageMap.get(person) + Number(damage));
}
damageMap.forEach((value, key) => console.log(value)); // 辞書順にならない！
```

ミスのポイント

❌ `Map` の挿入順に保証されている（辞書順にならない）


<br><br><br>

## OK例1: Array.from() でソート
```js
const sortedNames = Array.from(damageMap.keys()).sort();
sortedNames.forEach(name => console.log(damageMap.get(name)));
```

✅ `damageMap.keys()` でキーを取り出し、`Array.from` で配列化して、`sort()` で辞書順ソート

✅ `get()` を使い、順番に出力

<br><br>


## OK例2: slice() で無駄を省く
```js
const sortedNames = lines.slice(1, n + 1).sort();
sortedNames.forEach(name => console.log(damageMap.get(name)));
```

✅ すでに `lines` に名前があるので `Map.keys()` 取得不要

✅ キーアクセス回数が減り、わずかにパフォーマンス向上


<br><br><br>

## まとめ
`Map` は順序を保証しない！辞書順ソートが必要なら、

- `Array.from(damageMap.keys()).sort()`
- もともとのリストを `slice().sort()` で利用
