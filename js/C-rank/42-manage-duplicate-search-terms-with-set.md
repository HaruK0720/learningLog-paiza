# Set で一発解決！検索履歴の重複ワードを効率的に管理する方法

<br>

PaizaのCランク問題「検索履歴」を解いていて、最初は `Map` でゴリ押ししようとしたけど、実は `Set` を使えばめちゃくちゃシンプルに書けると気づいた！

今回は、検索履歴の重複ワードを排除して、最適な履歴リストを作る方法を解説する。

## <br>📚 問題概要

与えられた検索ワードを以下のルールで管理しよう。

既に履歴にあるワードが再度入力された場合 → 履歴から削除し、先頭に追加。

履歴にないワードが入力された場合 → そのワードを履歴の先頭に追加。

すべての入力を処理した後の履歴を表示。

<br>📚 入力例
```
5
book
candy
apple
book
candy
```

📃 出力例
```
candy
book
apple
```

## <br>❌ NG例: 配列だけでゴリ押ししたパターン

最初は `Array` の `includes()` を使って頑張ろうとした。
```js
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => { lines.push(input); });

rl.on('close', () => {
    const N = Number(lines[0]);
    const words = [];
    
    for (let i = 1; i <= N; i++) {
        // 既にある場合は削除して追加
        if (words.includes(lines[i])) {
            words.splice(words.indexOf(lines[i]), 1);
        }
        words.unshift(lines[i]);
    }
    words.forEach(word => console.log(word));
});
```
❌ 問題点:

`includes()` の計算量が O(N)  なので、大量データだと遅くなる。

`splice()` で要素削除すると、配列全体をシフトするためコストが高い。

## <br>✅ OK例: Set で最適化！

`Set` を使うことで、高速に重複チェックが可能！
```
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => { lines.push(input); });

rl.on('close', () => {
    const N = Number(lines[0]);
    const words = lines.slice(1).reverse(); // 逆順に取得
    const seen = new Set(); // 出力済みの単語を記録

    words.forEach(word => {
        if (!seen.has(word)) {
            console.log(word);　//出力
            seen.add(word); //Setに追加
        }
    });
});
```
✅ 改善点:

`Set` の `has()` は O(1) で超高速チェック！

`reverse()` を使うことで、リストを逆順にし `unshift()` を使わずに済む。

## <br>📝 新しく学んだこと

✅ `Set` で一瞬で重複チェック！

`Set` は 重複を許さない コレクション。だから `has()` で存在確認しつつ、重複なくデータを保存できる。
検索履歴の問題では、`Set` を使ってすでに出力した単語を記録し、二重出力を防いでいる！

✅ `reverse()` で順番をひっくり返す！

配列の要素を 逆順に並び替える 便利メソッド。
検索履歴は「最後に入力した単語が先に出る」から、`reverse()` で逆順にして処理するのがポイント！

✅ `unshift()` で配列の先頭に追加！

`push()` が末尾に追加するのに対して、`unshift()` は 先頭に追加する。
ただし `unshift()` は 配列全体をずらす から、大量データには向かない！

## <br>🔍 まとめ

✅ `Set` を使えば、O(1) の高速な重複チェックができる！
✅ `reverse()` で履歴を逆順にすれば `unshift()` を使わずスッキリ！

検索履歴を管理する問題を通して、 データ構造の選び方でパフォーマンスが大きく変わる ことを学んだ！

<br>[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/04/03/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e6%a4%9c%e7%b4%a2%e5%b1%a5%e6%ad%b4%e3%82%92%e7%ae%a1%e7%90%86%ef%bc%81%e9%87%8d%e8%a4%87%e3%83%af%e3%83%bc%e3%83%89/)

