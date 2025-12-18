# 九九表をJavaScriptで出力せよ！

<br>

JavaScript で九九表を出力する問題に挑戦！シンプルな問題かと思いきや、出力のズレで撃沈…。でも `process.stdout.write() `を使えばスッキリ解決！

## <br>問題概要

自然数 `N` を入力し、N × N の九九表を出力する。

✅ 条件:

各行の `i` 行 `j`列目 は`i * j` の計算結果

数値の間は半角スペース区切り

各行の末尾は改行（最後の数値の後にはスペースを入れない）


<br>📌 入力例:
```
3
```
📌 出力例:
```
1 2 3
2 4 6
3 6 9
```

## <br>OKコード

`console.log()` では改行を制御しにくいので、
`process.stdout.write()` を使って行の最後の処理を分ける。

📌 コード:
```js
const readline = require('readline');
const rl = readline.createInterface({ input: process.stdin, output: process.stdout });

rl.on('line', (input) => {
    const N = Number(input);
    for(let i = 1; i <= N; i++){
        for(let j = 1; j <= N; j++){
            if(j === N){
                process.stdout.write(i * j + "\n");
            } else {
                process.stdout.write(i * j + " ");
            }
        }
    }
    rl.close();
});
```
<br>✅ ここがポイント

`process.stdout.write()` を使う → 改行を自由に制御！

`if (j === N)`で分岐 → 最後の数値の後ろにスペースを入れない！

<br>[🔗 僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/25/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e4%b9%9d%e4%b9%9d%e8%a1%a8%e3%82%92javascript%e3%81%a7%e5%87%ba%e5%8a%9b%e3%81%9b%e3%82%88%ef%bc%81/)



### <br>おまけ：基本の九九表（9×9バージョン）

九九表の基本形（`N=9`）なら、以下のコードでもOK。
```js
for(let i = 1; i <= 9; i++){
    for(let k = 1; k <= 9; k++){
        if(k === 9){
            process.stdout.write(i * k + "\n");
        } else {
            process.stdout.write(i * k + " ");
        }
    }
}
```
📌 出力:
```
1 2 3 4 5 6 7 8 9
2 4 6 8 10 12 14 16 18
3 6 9 12 15 18 21 24 27
4 8 12 16 20 24 28 32 36
5 10 15 20 25 30 35 40 45
6 12 18 24 30 36 42 48 54
7 14 21 28 35 42 49 56 63
8 16 24 32 40 48 56 64 72
9 18 27 36 45 54 63 72 81
```
