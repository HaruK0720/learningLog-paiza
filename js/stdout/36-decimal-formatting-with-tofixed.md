# toFixed(3) で小数フォーマットを爆速攻略！

<br>

「小数第3位まで表示＆不足分は0埋め」って意外と面倒じゃない？
でも `toFixed(3)` を知ったら、一瞬で解決したからシェアする！

## <br>問題概要
入力: 実数 `N`
出力: `N` を小数第3位まで四捨五入し、不足分は0埋め

<br>入力例
```
3.14159
```
出力例
```
3.142
```

https://paiza.jp/works/mondai/stdout_primer/stdout_primer__format_real_number_step3

## <br>NG例（僕がやらかしたコード）
```js
console.log(Number(input).toPrecision(3)); // ❌ "3.14"（小数第3位が消えた…）
```
ミスの原因
✅ `toPrecision(3)` は「有効桁数3桁」なので、小数の扱いがズレる
✅ 3.14159 は 3.14 になって、小数第3位が表示されないことがある

## <br>OK例（正しいコード）
```js
console.log(Number(input).toFixed(3)); // ✅ "3.142"
```
解説
✅ `toFixed(3)` なら 小数第3位まで表示＆不足分は0埋め
✅ 自動で四捨五入（3.14159 → 3.142）
✅ 文字列として返るので、計算に使うなら `parseFloat()` で数値化

## <br>結論
✔ 小数点以下3桁が欲しいなら `toFixed(3)` 一択！
✔ `toPrecision(n)` は「有効桁数指定」なので用途が違う
✔ 「小数フォーマット問題」= `toFixed(n)` を思い出せ！

<br>[僕の失敗談と解決話!](https://paizabeginner.wordpress.com/2025/03/28/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-tofixed%e3%81%a7%e5%b0%8f%e6%95%b0%e3%82%92%e3%82%ad%e3%83%ac%e3%82%a4%e3%81%ab%e6%95%b4%e5%bd%a2%ef%bc%81/)
