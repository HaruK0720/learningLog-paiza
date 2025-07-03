# +=（複合代入演算子） と padStart() で時刻計算をスマートに！

<br>

時刻計算問題でズレまくった話。`+=` の力を知らずに…。でも `+=` と `padStart()` を使えば一発解決した！

<br><br>

## 📌 問題概要
「XX:XX」形式の時刻を受け取り、30分後の時刻を同じ形式で出力する。

🔹 入力例: "12:31"

🔹 出力例: "13:01"

<br><br><br><br>

## NGコード例（僕の最初の挑戦）
```js
rl.on('line',(line) => {
    const time = line.split(":");
    const Time = time.map(Number);
   
    if (Time[1] + 30 < 60){
        console.log(Time[0] + ":" + (Time[1] + 30));
    }else {
        console.log((Time[0] + 1) + ":" +  (Time[1] - 30));
    }
})
```

❌ 60分超えたときの処理ミス

❌ "1:32" みたいにゼロ埋めされない

<br><br><br>

## 改善版コード例
```js
let [hours, minutes] = line.split(":").map(Number);
minutes += 30;
if (minutes >= 60) {
   minutes -= 60;
   hours += 1;
}
const formattedTime = String(hours).padStart(2, '0') + ":" + String(minutes).padStart(2, '0');
console.log(formattedTime);
```

✅ `+= 30` で一気に足す

✅ `minutes >= 60` なら `-60 & hours += 1`

✅ `padStart(2, '0')` で "01:32" のように 2桁 固定

<br><br><br>

## 💡 まとめ
🔹 `+=` / `-=` で数値を直接更新してスッキリ

🔹 `padStart(2, '0')` でゼロ埋めフォーマット
🔹 String() で数値を文字列に変換し padStart() を適用
