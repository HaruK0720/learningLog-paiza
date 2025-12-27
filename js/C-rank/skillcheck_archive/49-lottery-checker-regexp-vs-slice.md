# 宝くじの当選判定を作ろう！　正規表現 vs slice()

<br>

Paizaの問題「宝くじ当選判定」を解いてみたら、最初に書いたコードが動かなかったんですよね……。試行錯誤してシンプルで読みやすいコードに改善したので、初心者の方にも分かりやすいようにシェアします！

## <br>🎰 宝くじのルール

宝くじには 100000 以上 199999 以下の6桁の番号がついています。当選番号が 142358 だった場合、以下のように当たりが決まります：

1等：完全一致 (例: 142358)

前後賞：当選番号の前後 (例: 142357, 142359)

2等：下4桁が一致 (例: x42358)

3等：下3桁が一致 (例: xxx358)

外れ：上記以外

<br>入力例:
```
142358
3
195283
167358
142359
```

出力例:
```
blank
third
adjacent
```

https://paiza.jp/works/mondai/c_rank_skillcheck_archive/lottery

では、実際にコードを書いていきましょう！

## <br>🚨 初めに書いたコード（NG例）
```js
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => {
    lines.push(input);
});

rl.on('close', () => {
    const winningNumber = Number(lines[0]); // 一等の当選番号
    const ticketCount = Number(lines[1]); // 宝くじの枚数
    const lotteryNumbers = lines.slice(2).map(Number); // 宝くじの番号
    
    const winningNumber_4 = String(winningNumber).match(/.{1,4}$/g); // 下4桁
    const winningNumber_3 = String(winningNumber).match(/.{1,3}$/g); // 下3桁
    
    lotteryNumbers.forEach(num => {
        const lotteryNumbers_4 = String(lotteryNumbers).match(/.{1,4}$/g);
        const lotteryNumbers_3 = String(lotteryNumbers).match(/.{1,3}$/g);
        
        if (winningNumber === num) {
            console.log("first");
        } else if (winningNumber - num === 1 || winningNumber - num === -1) {
            console.log("adjacent");
        } else if (winningNumber_4 === lotteryNumbers_4) {
            console.log("second");
        } else if (winningNumber_3 === lotteryNumbers_3) {
            console.log("third");
        } else {
            console.log("blank");
        }
    });
});
```

❌ 失敗ポイント
・正規表現の使い方ミス: `match(/.{1,4}$/g)` は配列を返すので、単純比較 (`===`) ではうまく動かない。

・誤った変数の使い方: `lotteryNumbers` から `num` を取得しているのに `String(lotteryNumbers).match(/.{1,4}$/g)` を実行してしまっている。

・ロジックが複雑: 正規表現で頑張ろうとしたけど、もっとシンプルな方法がある。

## <br>✅ 修正後のコード（OK例）
```js
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => {
    lines.push(input);
});

rl.on('close', () => {
    const winningNumber = Number(lines[0]); // 一等の当選番号
    const ticketCount = Number(lines[1]); // 宝くじの枚数
    const lotteryNumbers = lines.slice(2).map(Number); // 宝くじの番号
    
    lotteryNumbers.forEach(num => {
        if (num === winningNumber) {
            console.log("first");
        } else if (num === winningNumber - 1 || num === winningNumber + 1) {
            console.log("adjacent");
        } else if (String(winningNumber).slice(-4) === String(num).slice(-4)) {
            console.log("second");
        } else if (String(winningNumber).slice(-3) === String(num).slice(-3)) {
            console.log("third");
        } else {
            console.log("blank");
        }
    });
});
```
✨ 改善ポイント
- 正規表現の使用をやめ、`slice()` を活用
`String(winningNumber).slice(-4)` を使えば、下4桁を簡単に取得できる！
`match()` の結果が配列になってしまう問題を回避できる。

- ロジックをシンプルに整理
それぞれの判定を分かりやすくまとめ、ミスが減る。

### <br>📝 気づきメモ

`match()` は 配列 で返るので、単純な比較には使いにくい。

`slice(-4)` を使えば、一発で下桁が取得できて便利！

### <br>まとめ
`.match()` は配列を返すから相依するのは危険
`.slice()` ならシンプルで実装できる
次回、下桁チェックの問題がきたら `slice()` を思い出そう


<br><br>[僕の失敗談(´;ω;｀)と解決話！]()



## <br><br>🎁 おまけ: 正規表現で書くなら？
```js
const winningNumber_4 = String(winningNumber).match(/\d{4}$/)[0];
const winningNumber_3 = String(winningNumber).match(/\d{3}$/)[0];

lotteryNumbers.forEach(num => {
    const num_4 = String(num).match(/\d{4}$/)[0];
    const num_3 = String(num).match(/\d{3}$/)[0];

    if (num === winningNumber) {
        console.log("first");
    } else if (num === winningNumber - 1 || num === winningNumber + 1) {
        console.log("adjacent");
    } else if (num_4 === winningNumber_4) {
        console.log("second");
    } else if (num_3 === winningNumber_3) {
        console.log("third");
    } else {
        console.log("blank");
    }
});
```
🚀 改善点

- 正規表現 `/\d{4}$/` を使い、下4桁・下3桁を取得
- `match()` の結果は配列なので `[0]` で値を取り出す
- それ以外のロジックはそのままにして、分かりやすく整理

<br>📌`String()`してるのに数字？

確かに `String()` を使っているので、数字が文字列に変換されているのに `\d`（数字）を使うことに違和感を感じるかもしれないが、実は正規表現では 文字列内の数字を対象にすることができる。

<br>📌なぜ `\d` を使っているのか？
正規表現の `\d` は、数字の文字（0〜9）を意味しますが、文字列として数字が格納されている場合でも、それにマッチ。たとえば：
```js
const str = "142358"; // 文字列
const result = str.match(/\d{4}$/); // 下4桁を抽出
console.log(result); // ["8358"]
```

どういうことか？
`String()` を使っても、数字は文字列として扱われます。例えば 142358 という数値は “142358” という文字列に変換される。
文字列 “142358” に対して \d を使うと、その文字列の中にある数字を対象にできるので、ちゃんと4桁の数字部分 (8358) がマッチ。

<br>まとめ
`String()` で数字を文字列に変換したとしても、正規表現の `\d` は文字列内の 数字として表現されている文字 にマッチ。つまり、文字列 “142358” の中にある数字を対象に検索することができる!

<br><br>[僕の失敗談(´;ω;｀)と解決話！](https://paizabeginner.wordpress.com/2025/04/10/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e5%ae%9d%e3%81%8f%e3%81%98%e5%bd%93%e9%81%b8%e5%88%a4%e5%ae%9a%e3%82%92%e4%bd%9c%e3%82%8d%e3%81%86%ef%bc%81/)

