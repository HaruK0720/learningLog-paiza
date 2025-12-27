# 単語のカウントFINAL問題　Map.keys()でシンプルに！

<br>

これまで解いてきたことを活かして、単語のカウント最終問題を解いたから解説するよ！

## <br>📌 問題概要

与えられた半角スペース区切りの単語列から、
登場順に単語とその出現回数を出力する。

<br>💡 入力例
```
red green blue blue green blue
```
✅ 出力例
```
red 1
green 2
blue 3
```

https://paiza.jp/works/mondai/c_rank_skillcheck_archive/word-count


## <br>💥 初めの実装（標準入力省略）
前回のコードをつかって
```js
const words = input.split(" ");
const dict = new Map();
const order = []; // 単語の出現順を記録

words.forEach(word => {
    if (!dict.has(word)) {
        dict.set(word, 0);
        order.push(word);
    }
    dict.set(word, dict.get(word) + 1);
});

// 出現順に単語とカウントを出力
order.forEach(word => console.log(word, dict.get(word)));
```


## <br>✨ 改善版コード

🚀 改善ポイント

✅カウントの初期化を `set(word, 0)` せずに `set(word, 1)` にする
　 初回の `set` 時に `0` にして、すぐに `+1` するのは無駄
　 `set(word, 1)` にすれば `+1` の処理が不要になる
<br> ✅ `order` を配列ではなく `Map.keys()` を使う
 　`order` 配列を使わなくても `Map.keys()` でキーの順番を保持できる
 　`Map` はキーを挿入順に保持する特性があるので `order` は不要

```js
const words = input.split(" ");
const dict = new Map();

words.forEach(word => {
    dict.set(word, (dict.get(word) || 0) + 1);
});

// 挿入順に単語と出現回数を出力
for (const word of dict.keys()) {
    console.log(word, dict.get(word));
}
```

### <br>🛠 改善点の説明
✅カウントの初期化を `(dict.get(word) || 0) + 1` で処理
 　`dict.get(word) || 0` により `undefined` の場合は `0` にする
 　`set(word, (dict.get(word) || 0) + 1)` にすれば `if` 文不要

✅`order` を削除し、`dict.keys()` を使う
 　`order` を作らなくても `Map.keys()` で挿入順にアクセス可能

✅`forEach` ではなく `for…of` を使う


### <br>💡 まとめ

Map の特性を活かせば、余計な変数を減らせてコードがスッキリ！ 「データ構造の理解 = コードの最適化」 だと実感した。

<br><br>[🔗 僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/04/08/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e5%8d%98%e8%aa%9e%e3%81%ae%e3%82%ab%e3%82%a6%e3%83%b3%e3%83%88final%e5%95%8f%e9%a1%8c/)

