# process.stdout.write() の罠?!：バッファリング問題と解決法

<br>

僕がPaizaの出力問題を解いてるとき、あることに気づいた。小さな入力だとサクサク動くのに、大きな数字を入れると出力が途中で消えちゃう…！？その原因は、Node.jsの`process.stdout.write()`に潜むバッファリング問題なるものかも？！今回はその問題と解決策をシンプルに紹介するよ！

問題概要

与えられた `H`, `W`, `A`, `B` を使って、`H`×`W` の `(A, B)` の表を作成せよ。横は` | `区切り、縦は`=`で区切る。ただし、縦の区切り線は上の行と同じ長さにすること！

<br>入力:
```
2 3 7 8  
```
出力:
```
(7, 8) | (7, 8) | (7, 8)  
=======================  
(7, 8) | (7, 8) | (7, 8)  
```

https://paiza.jp/works/mondai/stdout_primer/stdout_primer__specific_format_step4

## <br>バッファリングの罠

最初は`process.stdout.write()`を使って出力してたんだけど、小さい数字だと問題なく動いてたのに、大きい数字（例えば `92 79 7 8`）だと出力が途中で消えちゃう…！これがバッファリングの問題かもしれない(´;ω;｀)

### <br>バッファリングって何？

簡単に言うと、バッファリングは大量のデータを一時的にメモリにためてから出力する仕組み。でも、このせいで、ループ内で何度も`stdout.write()`を使うと、バッファがいっぱいになっちゃって、出力が途切れることがあるんだ。

## <Br>NG例: process.stdout.write() を使いすぎるとどうなる？

まず、これがNGなコード。`process.stdout.write()`を使いすぎて、出力が途切れる原因になってる。
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const [H, W, A, B] = input.split(" ").map(Number);
    for (let i = 1; i <= H; i++) {
        for (let j = 1; j <= W; j++) {
            if (j === W) {
                process.stdout.write(`(${A}, ${B})\n`);
            } else {
                process.stdout.write(`(${A}, ${B}) | `);
            }
        }
        if (i !== H) {
            console.log("=".repeat(W * (4 + W - 1 + String(A).length + String(B).length)));
        }
    }
     rl.close();
});
```
このコードだと、大きな入力値を処理する際にバッファが溢れて、途中で出力が途切れちゃうんだ。

## <br>解決策：console.log()とArray.from()を使ってスッキリ解決

バッファリング問題を回避しつつ、コードをもっとシンプルにする方法がある！

`console.log()`を使えば、バッファリングの影響を受けずに安定した出力が得られるし、`Array.from()`を使うとコードがめっちゃスッキリするよ。

### <br>OK例：console.log()とArray.from()を活用
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const [H, W, A, B] = input.split(" ").map(Number);
    const row = Array.from({ length: W }, () => `(${A}, ${B})`).join(" | ");
    const separator = "=".repeat(row.length);

    console.log(Array.from({ length: H }, () => row).join("\n" + separator + "\n"));
});
```
これで、バッファリング問題を回避しつつ、コードがスッキリした！
`Array.from()`を使うことでループもシンプルになって、可読性も向上。

### <br>改善ポイント
- `process.stdout.write()`はバッファリングの影響を受けやすいから、`console.log()`を使ったコードを考えてみよう。
- `Array.from()`を使うと、2重ループなしでスッキリコード！
- `repeat()`を使えば区切り線の計算も楽々！

## <br>まとめ
Paizaの問題を解いてると、意外なところでバグに遭遇することもある。でも問題をしっかり理解すれば、簡単に解決策が見つかるよ。`console.log()`や`Array.from()`を活用して、コードをスッキリさせよう！🚀

<br>[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/04/02/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e3%80%80%e3%80%80%e3%83%90%e3%83%83%e3%83%95%e3%82%a1%e3%83%aa%e3%83%b3%e3%82%b0%e3%81%ae%e7%bd%a0%ef%bc%81%ef%bc%9fnode-j/)

