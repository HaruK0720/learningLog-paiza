# Setとsizeで単語数を取得！

<br>

Paizaの問題を解いてたら、「単語数をカウントせよ！」みたいな問題が出てきた。
これ、`Set`と`.size`を使えばめっちゃスッキリ解けるんだよね。


## <br>問題概要

スペース区切りの文字列を改行して出力。ただし、
	•	同じ単語は1回だけ出力
	•	その後、出現した単語の種類の数だけ 1 を出力

<br>入力例
```
red green blue blue green blue
```
出力例
```
red  
green  
blue  
1  
1  
1  
```
https://paiza.jp/works/mondai/c_rank_skillcheck_archive/word-count_05

## <br>初めのコード
```js
const rl = require('readline').createInterface({input: process.stdin});

rl.on('line', (input) => {
    const words = input.split(' ');
    const seen = new Set();

    words.forEach(word => {
        if (!seen.has(word)) {
            console.log(word);
            seen.add(word);
        }
    });

    for (let i = 0; i < seen.size; i++) {
        console.log(1);
    }

    rl.close();
});
```
## <br>他のコード例（改善？）
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const words = input.split(' ');
    const seen = new Set();
    const output = [];

    words.forEach(word => {
        if (!seen.has(word)) {
            output.push(word);
            seen.add(word);
        }
    });

    console.log(output.join("\n"));  
    console.log(Array.from(seen).map(() => 1).join("\n"));  

    rl.close();
});
```

### <br>解説

✅ `Set` は 重複を自動で排除 してくれる！
✅ `.size` を使えば ユニークな単語数を取得 できる！
✅ `Array.from(seen).map(() => 1).join("\n")` で 1 の出力を`size`もループもなしで実装！

### <br>まとめ
•	`Set` の特性を活かせば、重複をチェックする手間が減る！
	•	`.size` を使えば、要素数のカウントも楽勝！
	•	`map(() => 1)` のテクニックで、出力もシンプルに！


<br>[僕の失敗談と解決談！](https://paizabeginner.wordpress.com/2025/04/05/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9aset%e3%81%a8size%e3%81%a7%e5%8d%98%e8%aa%9e%e6%95%b0%e3%82%92%e5%8f%96%e5%be%97%ef%bc%81/)

