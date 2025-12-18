# カウンター魔法バトルの計算ミス（シュミレーション問題）

<br>

Paizaのカウンター魔法バトルを解いてたら、見事にハマった。
攻撃がフィボナッチっぽいと思ったら…。
同じミスをしないよう、技術メモを残しておく。

## 問題概要

ターン制バトルで、パイザ君とモンスターが「カウンター魔法」で攻撃し合う。
ただし、攻撃計算が特殊！

📌 攻撃ルール

- 1回目・2回目の攻撃は 固定で1
- 3回目以降は以下の計算：
    パイザ君の攻撃 = モンスターの直近2回の合計
    モンスターの攻撃 = (パイザ君の前回攻撃 × 2) + (前々回の攻撃)
- パイザ君の体力 H が 0 以下になったターンを出力

## <br>NGコード：
```js
let H = Number(input);
let n = 0;  
let damageFromMonster = [];
let damageFromPaiza = [];
damageFromMonster.push(1);
damageFromPaiza.push(1);

while (H > 0) {
    n++;
    let attackPaiza = damageFromMonster[n - 1] + damageFromMonster[n - 2];
    let attackMonster = damageFromPaiza[n - 1] * 2 + damageFromPaiza[n - 2];

    H -= attackMonster;
    damageFromPaiza.push(attackPaiza);
    damageFromMonster.push(attackMonster);
}

console.log(n);
```
💀 ミスのポイント
- 初回の攻撃を考慮してない！
    → `H` がズレる
- 配列のインデックスエラー発生！
    → `n=1` のとき `n-2` で `-1` になり、エラー

## <br>OKコード：修正後
```js
let H = Number(input) - 2;  // 最初の2回分を引く
let n = 2;  // 3回目からカウント
let damageFromPaiza = [1, 1];  
let damageFromMonster = [1, 1];  

while (H > 0) {
    let attackPaiza = damageFromMonster[n - 1] + damageFromMonster[n - 2];  
    let attackMonster = damageFromPaiza[n - 1] * 2 + damageFromPaiza[n - 2];  

    H -= attackMonster;  
    damageFromPaiza.push(attackPaiza);
    damageFromMonster.push(attackMonster);

    n++;
}

console.log(n);
```
✅ 修正ポイント
- 最初の 2回分の攻撃を`H`から引く ことでズレを修正
- 配列の 初期値を `[1,1]` にしてインデックスエラーを防ぐ
- `n=2` から開始 し、3回目以降を計算

## <br>さらに高速化

累積ダメージ（`sum`）を使うと、毎回 `H -= attackMonster`しなくてOK！
```js
const rl = require('readline').createInterface({input: process.stdin});
rl.on('line',(input) => {
    
    const H = Number(input);

    const damageFromPaiza = [1, 1]; // パイザ君から受けた攻撃
    const damageFromMonster = [1, 1]; // モンスターから受けた攻撃

    let i = 2;  
    let sum = 2; // すでにモンスターの1回目と2回目のダメージを加算済み

    while (sum < H) {  
        damageFromPaiza.push(damageFromMonster[i - 2] + damageFromMonster[i - 1]); // パイザ君の攻撃 = 前2回のモンスターの攻撃の合計
        damageFromMonster.push(damageFromPaiza[i - 2] + 2 * damageFromPaiza[i - 1]); // モンスターの攻撃 = (前回のパイザ君の攻撃 * 2) + (前々回のパイザ君の攻撃)
        sum += damageFromMonster[i]; // モンスターの攻撃を累積ダメージに加算
        i += 1;  // 次のターンへ
    }
    console.log(i);
    rl.close()
})
```
🚀 改善点
- `sum` を使って 累積ダメージを管理（ループの条件判定がシンプル）
- `H -= attackMonster` を `sum < H` に変更（無駄な計算を減らす）

## <br>まとめ
- 配列の初期値 を設定しないと インデックスエラー になる
- 最初のダメージ を考慮しないと `H` の減り方がズレる
- 累積ダメージ `sum` を使うと高速化できる！



[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/18/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e3%82%ab%e3%82%a6%e3%83%b3%e3%82%bf%e3%83%bc%e9%ad%94%e6%b3%95%e3%83%90%e3%83%88%e3%83%ab%e7%b7%a8/)
