# 迷路判定！実は文字列はインデックスでアクセス可能！２次元配列使わない！

<br>

迷路のマスが `#` か `.` か見るだけの問題で、僕がまさかの 迷路に迷った件について話す!

## <br>🧩 問題内容（超ざっくり）
`H`×`W`マスの迷路があり、`r`行`c`列のマスが「`#`（壁）」かどうか判定せよ！

<br>入力例：
```
3 4 1 2　　// H,W,r,c
..#.
#.##
....
```

このとき、1行目の2文字目は `.` なので答えは No！
<br>

https://paiza.jp/works/mondai/data_structure/data_structure__string_boss

## <br>🔥 始めのコード（NG）
```js
const rl = require('readline').createInterface({input:process.stdin});
const lines = [];

rl.on('line',(input)=>{
    lines.push(input);
});

rl.on('close',()=>{
    const[H,W,r,c] = lines[0].split(' ').map(Number);
    const maze = lines.slice(1).map(a => a.split(' ')); // ❌

    if(maze[r][c] === "#"){  //❌
        console.log("Yes");
    } else {
        console.log("No");
    }
});
```

🙅‍♂️ NGポイント
- `split(' ')` の誤用: 迷路の行を分割しようとしたけど、スペースがないので、すべて1要素の配列に。
- インデックスのズレ: `r` と `c` は 1始まり なのに、そのまま使っていたため、ズレが生じた。


## <br>✅ 修正コード（OK）
```js
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => {
    lines.push(input);
});

rl.on('close', () => {
    const [H, W, r, c] = lines[0].split(' ').map(Number);
    const maze = lines.slice(1); // 文字列の配列でOK

    if (maze[r - 1][c - 1] === "#") {  // インデックス修正
        console.log("Yes");
    } else {
        console.log("No");
    }
});
```

### 🧠 解説
- 文字列へのアクセス: JavaScriptでは文字列も配列のようにインデックスでアクセスできる。そのため、`maze[r-1][c-1]` と書くだけで、指定した行・列のマスにアクセス可能。



### 💡 迷路の2次元配列化

もし迷路を完全な2次元配列として使いたい場合は、次のようにすることもできる。
```js
const maze = lines.slice(1).map(line => line.split(''));
```
<br>
この方法では、各行が配列になるが、今回の問題では読み取りだけなので、最初のように文字列のままで使うのがベターかも。


## <br>🏁 まとめ
✅文字列へのアクセスはインデックスでできるから、`split('')` は不要！

✅1 始まり → 0 始まり のインデックス補正を忘れずに。



<br><br>[僕の失敗談(´;ω;｀)🚀](https://paizabeginner.wordpress.com/2025/04/14/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9apaiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e8%bf%b7%e8%b7%af%e3%81%ae%e5%a3%81%e5%88%a4/)




