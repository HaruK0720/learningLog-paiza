# toFixed() vs Math.round()：小数の丸め方を極める！

<br>

小数を M 桁まで丸めるなら Math.round() より toFixed() のほうが断然スマート。技術メモメモ！

## <br>問題: 小数を `M`桁まで表示

📌 仕様
実数 `N` を小数第 `M` 位まで四捨五入
小数部が `M` 位に満たない場合は `0` で埋める

<br>入力例 → 出力例
```
入力:  3.14159 3  
出力:  3.142  
```
https://paiza.jp/works/mondai/stdout_primer/stdout_primer__format_real_number_step4

## <br>NG例: `Math.round()` で分岐すると…
```js
const [N, M] = input.split(" ");
const num = Number(N);
const decimalPlaces = Number(M);
let result = "";

if (decimalPlaces === 1) {
    result = (Math.round(num * 10) / 10).toString();
} else if (decimalPlaces === 2) {
    result = (Math.round(num * 100) / 100).toString();
}
```
😱 問題点

✅ M の値ごとに if 文を分岐 → 可読性最悪
✅ 0 埋め処理がない → 表示がズレる可能性

## <Br>OK例: `toFixed(M)` で一発解決！
```js
const [N,M] = input.split(" ").map(Number);
console.log(N.toFixed(M));
```

🎯 `toFixed(M)` の強み

✔` M` 桁まで四捨五入 → `Math.round()` のような分岐不要
✔ 小数部が足りないときは自動 `0` 埋め

## <br>おまけ: `toFixed()` なしで解決する方法
```js
const [N, M] = input.split(" ");
const num = Number(N);
const decimalPlaces = Number(M);
let result = "";

let result = (Math.round(num * 10**M) / 10**M).toString();
if (!result.includes(".")) result += ".";
result += "0".repeat(M - (result.split(".")[1] || "").length);
console.log(result);
```
📌 何してる？
- `Math.round()` で `M` 位まで四捨五入
- `0` 埋めを手動処理

## <br>結論
🚀 toFixed(M) を使えば 一行で M 桁まで丸められる！
🚀 Math.round() は 0 埋め処理が必要で 冗長になりがち

<br>[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/29/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e5%b0%8f%e6%95%b0%e3%82%92m%e6%a1%81%e3%81%ab%e4%b8%b8%e3%82%81%e3%82%8b%e6%96%b9%e6%b3%95%ef%bc%81tofixed-vs-%e6%9d%a1/)

