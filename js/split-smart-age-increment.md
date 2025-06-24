# split()でスマート配列操作！社員リストの年齢+1を一発で

  
<br>

今日もpaizaの問題に挑戦した！社員リストの年齢を+1するだけのシンプルな処理で、配列を分けすぎて沼った。`map()`を使わずスッキリ書く方法を共有！

<br><br><br>

## NG例 : 無駄な配列分割と push() 地獄
```js
const syain = lines.slice(1);

let name = [];  
let age = [];  

syain.forEach(sya => {
    let sy = sya.split(" ");
    name.push(sy[0]);
    age.push(Number(sy[1]));
});

const newAge = age.map(a => a + 1);
for (let count = 0; count < Number(lines[0]); count++){
    console.log(name[count], newAge[count]);
}
```

### 💀 問題点:
- 名前と年齢を別の配列にする必要なし
- map() で無駄に新しい配列を作ってる
- Number(lines[0]) を毎回変換（コスト大）

<br><br>

## OK例 : split()で一発分割！
```js
const n = Number(lines[0]);

for (let i = 1; i <= n; i++) {
  const [s, a] = lines[i].split(" ");
  console.log(`${s} ${Number(a) + 1}`);
}
```

### ✨ 改善点:
- `[s, a] = split(" ")` で即分割
- `Number(a) + 1` を直接計算
- `for` ループがスッキリ

あと、変数名どうにかしたかったなぁ。syとかsyaとか意味わかんないし(´;ω;｀)
