# 整数の積と比較！Number()の落とし穴

Paizaの「A × B ≦ C」を判定する問題に挑戦したんだけど、Number()を入れ忘れて大失敗…。前にも「差と積」の問題で同じミスをしたのに、またやらかした😂 そんな学びをまとめたよ！

<br><br>

## ミスったコード
```js
const [A, B, C] = line.split(" "); 
console.log(A * B <= C ? "YES" : "NO");  
```
これはA, B, Cが文字列のままなので、計算が正しく動かない！


<br>

## 修正後のコード
```js
const [A, B, C] = line.split(" ").map(Number);
console.log(A * B <= C ? "YES" : "NO");
```
`map(Number)`を使えば一発で数値に変換！シンプルで読みやすい！

この手のミス、初心者あるあるじゃない？僕は2回やらかしたけど、もう忘れない…はず💦
