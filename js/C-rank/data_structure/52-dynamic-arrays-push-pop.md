# 動的配列！push/popで配列を自在に操ろう！

<br>

Paizaで「動的配列」の問題をやってたときの話。Setで書いたら一見うまくいってたけど、「末尾削除(`pop_back`)」が面倒だった。

よく考えたら問題が動的"配列"なんだから、配列を使えばいいじゃん...。
ちゃんと配列(`Array`)を使えば一発だった。サクッと整理！


## <br>📘 問題概要

N個の要素を持つ配列Aに対して、以下の3つの操作を実行するプログラムを作成せよ。

- `0 x` : A の末尾に x を追加 （push_back）
- `1` : A の末尾を削除 （pop_back）
- `2` : A を半角スペース区切りで出力 （print）

<br>入力例
```
3 5
1 2 3
0 10
0 12
2
1
2
```
出力例
```
1 2 3 10 12
1 2 3 10
```

https://paiza.jp/works/mondai/data_structure/data_structure__array_boss


## <br>❌ NGコード（Setでやってみた）
```js
const rl = require('readline').createInterface({input:process.stdin});
const lines = [];

rl.on('line',(input)=>{
    lines.push(input);
});

rl.on('close',()=>{
    const arrayA = new Set();
    
    lines[1].split(' ').forEach(a => arrayA.add(a));
    
    lines.slice(2).map(a => a.split(' ')).forEach(num => {
        
        if(Number(num[0]) === 0){
            arrayA.add(num[1]);  // 追加はできる
        }
        
        else if(Number(num[0]) === 1){
            const arr = Array.from(arrayA);
            arrayA.delete(arr[arr.length - 1]);  // 末尾削除のつもりが微妙に面倒
        }
        
        else if(Number(num[0]) === 2){
            const arrA = Array.from(arrayA);
            console.log(arrA.join(' '));
        }
    });
});
```
- `.add()`で追加はできるけど、末尾から削除がめちゃくちゃやりにくい
- `Set`は`index`使えないから、`.pop()`みたいな操作がない
- `.delete()`で最後の要素を消すには毎回`Array`に変換する必要あり → 非効率＆読みにくい
- 重複NGなのも地味に面倒。今回の問題に向いてない


## <br>✅ OKコード（配列使ったver）
```js
const rl = require('readline').createInterface({ input: process.stdin });

const lines = [];

rl.on('line', (input) => {
    lines.push(input);
});

rl.on('close', () => {
    const arrayA = lines[1].split(' ');

    lines.slice(2).forEach(a => {
        const num = a.split(' ');

        if (Number(num[0]) === 0) {
            arrayA.push(num[1]);  // 末尾追加：これでOK
        }
        
        else if (Number(num[0]) === 1){
            arrayA.pop();  // 末尾削除：これだけ！
        }
        
        else if (Number(num[0]) === 2) {
            console.log(arrayA.join(' '));  // 出力もシンプル
        }
    });
});
```
- `push()` と `pop()`で末尾操作が直感的＆簡潔
- `join(' ')`で一発出力、余計な変換いらない
- `Set`使ってたときのゴチャゴチャが全部なくなった


## <br>💡まとめ
- `Set`じゃ末尾操作できない（`pop`がない！）
- 配列でやるのが正解：`push()`と`pop()`で完結
- 無理やりな変換は、たいてい設計ミスのサイン
- 出力は `.join(' ')` でスパッといける


「`Set`使った方がかっこよさそう」って思った(´;ω;｀)。でも今回みたいに素直に配列が正解な場面も多い！


<br><br>[僕の失敗談(´;ω;｀)🚀](https://paizabeginner.wordpress.com/2025/04/13/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e9%85%8d%e5%88%97%e6%93%8d%e4%bd%9c%ef%bc%9apush-pop%e3%81%a7%e9%85%8d%e5%88%97%e3%82%92%e8%87%aa%e5%9c%a8%e3%81%ab%e6%93%8d/)

