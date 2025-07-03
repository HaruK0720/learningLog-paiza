# 工事終了時刻問題！split(), Number(), padStart(),%=剰余代入演算子を活用

前回に引き続き、Paizaの「パイザ君の家の前の工事」問題に挑戦したときにハマったポイントをシンプルに解説！

ちなみに前回の応用？発展？です。ちょっとしたミスで大混乱に…。

<br>

問題概要

与えられた「開始時刻」と「継続時間」に基づき、工事終了時刻を24時間制で出力する問題です。
例えば、開始時刻が「15:59」で、継続時間が「0時間1分」なら、出力は「16:00」となる。

<br>

入力例:

    2
    15:59 0 1
    23:20 1 0

出力例:

    16:00
    00:20

<br><br><br><br><br>

## NG例 (初心者がやりがちな (僕の(´;ω;｀) )ミス)
### ミス1: 配列の分割を間違える
```j
let [time, hoursPassed, minutesPassed] = lines.slice(1).split(" "); // ❌ エラー！
```

#### 修正:
```js
let [time, hoursPassed, minutesPassed] = lines[count].split(" "); // ⭑OK！
```

<br><br>

### ミス2: Number() を使わずに計算する
```js
let newHours = hours + hoursPassed; // ❌ 文字列連結になる
```

#### 修正:
```js
let newHours = Number(hours) + Number(hoursPassed); // ⭑OK！
```

<br><br>

### ミス3: padStart() の誤用
```js
String(newHours.padStart(2, '0')); // ❌ エラー
```

#### 修正:
```js
String(newHours).padStart(2, '0'); // ⭑OK！
```

<br><br><br><br>

## 正答例コード
```js
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

const lines = [];

rl.on('line', (line) => {
    lines.push(line);
});

rl.on('close', () => {
    const N = Number(lines[0]);
    for (let count = 1; count <= N; count++) {
        let [time, hoursPassed, minutesPassed] = lines[count].split(" ");
        let [hours, minutes] = time.split(":");
        
        let newHours = Number(hours) + Number(hoursPassed);
        let newMinutes = Number(minutes) + Number(minutesPassed);
        
        if (newMinutes >= 60) {
            newMinutes -= 60;
            newHours += 1;
        }
        
        newHours %= 24;
        
        let formattedTime = `${String(newHours).padStart(2, '0')}:${String(newMinutes).padStart(2, '0')}`;
        console.log(formattedTime);
    }
});
```
✅ ループ処理を活用して、1行ずつ処理するのが大事！

✅ `%=` を使うことで、24時間を超える計算を簡潔に書ける！

✅ いきなりコードを書かず、設計図（考え方）を整理するとスムーズ！

<br>

[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/06/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc/)  ←これと合わせた方がわかりやすいかも(m´・ω・｀)m ｺﾞﾒﾝ…
