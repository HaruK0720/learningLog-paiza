# Map×orderでスッキリ！出現順に単語カウントする方法

<br>

Paizaの問題で、半角スペース区切りの文字列を出現順に出力した後、それぞれの出現回数を出力するというタスクに挑戦した。とりあえず `Map` と `forEach` を駆使したのだけれど、これはどうも無駄が多い…。調べたら配列を使うと最適化できることがわかった。その実装を共有しよう！

## <br>問題概要

👉 要求
- 半角スペース区切りの文字列を受け取る
- その中のユニークな単語を、登場順に出力
- その後、個々の単語の出現回数を出力

<br>入力例：
```
red green blue blue green blue
```

出力例：
```
red
green
blue
1
2
3
```

https://paiza.jp/works/mondai/c_rank_skillcheck_archive/word-count_07


## <br>🚨 シンプルながら無駄が多いコード
```js
const rl = require('readline').createInterface({input: process.stdin});

rl.on('line', (input) => {
   const words = input.split(" ");
   const dict = new Map();
  
   words.forEach(a => {
       dict.set(a, 0); // まず0で初期化
   });

   words.forEach(a => {
       dict.set(a, dict.get(a) + 1); // カウントアップ
   });

   dict.forEach((value, key) => {
       console.log(key); // 単語を出力
   });

   dict.forEach(value => {
       console.log(value); // 出現回数を出力
   });

   rl.close();
});
```

## <br>🛠️ order 配列を利用
```js
const rl = require('readline').createInterface({ input: process.stdin });

rl.on('line', (input) => {
    const words = input.split(" ");
    const dict = new Map();
    const order = []; // 単語の出現順を記録

    words.forEach(word => {
        if (!dict.has(word)) {
            dict.set(word, 0);
            order.push(word); // 初出の単語を記録
        }
        dict.set(word, dict.get(word) + 1); // カウント更新
    });

    // 出現順に単語を出力
    order.forEach(word => console.log(word));
    
    // 出現順にカウントを出力
    order.forEach(word => console.log(dict.get(word)));

    rl.close();
});
```

### 🤔 なぜ order 配列が必要なの？
- `order`に初出の単語を記録すれば、出力順を保証できる
- ループを1回にまとめて、計算量を削減


## <br>📅 まとめ
- `Map` で単語の出現回数を管理
- `order`を利用して出現順を保証
- `forEach`を減らすだけで、キレイなコードに修正できる

🎧 この解決法で、パフォーマンスが向上した気がする！


<br>[僕の失敗談と解決談！🚀](https://paizabeginner.wordpress.com/2025/04/07/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9amap%e3%81%a7%e5%87%ba%e7%8f%be%e9%a0%86%e5%8d%98%e8%aa%9e%e3%82%ab%e3%82%a6%e3%83%b3%e3%83%88%ef%bc%81/)

