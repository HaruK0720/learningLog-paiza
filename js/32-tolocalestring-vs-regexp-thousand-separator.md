# toLocaleString() vs 正規表現 - 3桁区切りの最適解はどっち？

<br>

Paizaの問題「大きな数の3桁区切り」で試行錯誤！
最初は正規表現で頑張ったけど、実は toLocaleString() の方がスマートだった！？
技術メモとしてまとめておくよ。

## <br>問題の概要

入力: 超デカい整数 
出力: 3桁ごとにカンマを挿入した文字列

※位の小さい方から 3 けたごとにカンマ区切りで出力
※Nの桁数は 3 の倍数とは限らない

例:
入力: 12345678901234567890
出力: 12,345,678,901,234,567,890

https://paiza.jp/works/mondai/stdout_primer/stdout_primer__specific_split_boss

## <br>解法①: 正規表現 + `replace()`
```js
console.log(input.replace(/\B(?=(\d{3})+(?!\d))/g, ","));
```
解説
`\B` → 先頭を除く位置にマッチ（\b の逆）
`(?=(\d{3})+(?!\d))` → 3桁ごとの数字のグループが一回以上繰り返されている、次が数字じゃない（＝３桁数字グループの間）位置にマッチ。

`.replace(..., ",") `でカンマを挿入

メリット: 柔軟にカスタム可能
デメリット: 正規表現ミスるとバグりやすい（僕も最初、先頭にカンマ入る事故発生💀）


## <Br>解法②: toLocaleString()
```js
console.log(BigInt(input).toLocaleString());
```
解説
`toLocaleString()` → ロケール対応 で数値をフォーマット
`BigInt` 必須 →（`Number` 型では処理できない大きな数を扱いたい場合、`BigInt` を使う必要がある）

メリット: 簡潔 & 国際対応OK
デメリット: カスタマイズ性低め
## <br>まとめ
汎用性なら正規表現
お手軽 & 国際対応なら `toLocaleString()`

[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/24/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e5%a4%a7%e3%81%8d%e3%81%aa%e6%95%b0%e3%82%923%e6%a1%81%e5%8c%ba%e5%88%87%e3%82%8a%ef%bc%81%e6%ad%a3%e8%a6%8f%e8%a1%a8%e7%8f%be-v/)

## <br><br>おまけ
```js
const rl = require('readline').createInterface({input: process.stdin});

rl.on('line',(input) => {

    function addCommas(num) {
    // 数字を文字列にしてから逆順にし、処理後に逆転させる
    let str = num.toString().split('').reverse().join('');
  
    let result = [];
  
    for (let i = 0; i < str.length; i++) {
    if (i > 0 && i % 3 === 0) {
      result.push(',');  // 3桁ごとにカンマを追加
    }
    result.push(str[i]);  // 現在の数字を追加
    }

    // 最後に文字列を逆順にして元に戻す
    return result.reverse().join('');
        
    }

    console.log(addCommas(input));  
    rl.close(); 
});
```
