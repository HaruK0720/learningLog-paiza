# indexOf で一発検索！ループ不要の解決法

<br>

Paizaの問題を解いてたら、「配列の中で単語がどこにあるか？」を調べるシンプルな方法を発見したのでシェアします。最初は `for` ループを使ったんだけど、実は `indexOf` を使えば1行で解決 できるんです！

## <br>問題概要

1行目に 半角スペース区切りの文字列 が与えられ、
2行目に検索する単語 S が与えられる。

S が 1行目の文字列の何番目（インデックス）にあるかを出力せよ！
ただし、複数回登場する場合は最初の登場位置 を出力する。

<br>📌 入力例
```
red green blue blue green blue  
red  
```
📌 出力例
```
0  
```

https://paiza.jp/works/mondai/c_rank_skillcheck_archive/word-count_06

## <Br> 🚨 最初のコード（for ループ版）

最初に書いたコードがこれ👇
```js
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => {
    lines.push(input);
});

rl.on('close', () => {
    const words = lines[0].split(" ");
    const S = lines[1];
    
    for (let i = 0; i < words.length; i++) {
        if (S === words[i]) {
            console.log(i);
            break; // 最初に見つかったらループを抜ける
        }
    }
});
```
✅ 動作はするけど…
- `for` ループを使うと コードが長い
- `break` を忘れると 無駄なループ が発生
- もっとスッキリ書ける方法 ないの？

## <br>✅ indexOf でスッキリ解決！

実は `indexOf` を使えばループ不要！
```js
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => {
    lines.push(input);
});

rl.on('close', () => {
    const words = lines[0].split(" ");
    const S = lines[1];
    console.log(words.indexOf(S)); // 一発取得！
});
```

### <br>🤔 indexOf を使うメリット

✅ コードがスッキリ！
　→ `words.indexOf(S)` だけで検索できるので、`for` ループ不要！

✅ 可読性が向上！
　→ 最初の出現位置を一発で取得 できるので、無駄な処理を減らせる！

✅ エラーが減る！
　→ `for` ループを回して `break` を忘れるミスを防げる！

### <br>💡 学んだことメモ

🔹 `indexOf(S)` は配列内で最初に `S` が出てくるインデックスを返す！
🔹 ループを回さず、シンプルに検索できる！
🔹 検索アルゴリズムを意識するとコードがもっと洗練される！

## <br>📌 まとめ

`indexOf` を使えば、ループなしで単語の位置を取得できる！
次のステップは、`lastIndexOf` を使って 最後の登場位置を取る 応用問題も（あるのかな？）やってみたい！

<br>[僕の失敗談と解決話！ 🚀](https://paizabeginner.wordpress.com/2025/04/06/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9aindexof%e3%81%a7%e3%82%b7%e3%83%b3%e3%83%97%e3%83%ab%e3%81%ab%e6%96%87%e5%ad%97%e5%88%97%e6%a4%9c%e7%b4%a2%ef%bc%81/)

