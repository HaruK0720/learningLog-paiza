# Array(N).fill() .join(" ")で一発！ 簡単な問題ほど シンプルな解法を考えるクセ が大事！
<br>

今回解いた問題は、
「整数 N を受け取り、N 回 paiza を半角スペース区切りで出力せよ」
`while` で N 回ループを書いたら、なぜか `N-1` 回しか回らないバグ に遭遇した。

<Br>
→ 原因は `count = 1` で始めたせい。ループは `count < N` だから、1 から N-1 までの N-1 回しか回らない ってオチだった。

<br><br>

## NGコード（N-1回しか出力されない）
```js
let count = 1;  
while (count < N) {  
    process.stdout.write("paiza ");  
    count++;  
}  
process.stdout.write("paiza");  
```

<br>

## OKコード（ちゃんとN回出力）
```js
let count = 0;  
while (count < N) {  
    process.stdout.write(count === N - 1 ? "paiza" : "paiza ");  
    count++;  
}  
console.log("");  
```

または、`count <= N` にしておけばよかった。


<br>

## さらに、`Array(N).fill().join()`  を使うと超スマートかも。

```js
console.log(Array(N).fill("paiza").join(" "));  
```

こっちなら `while` ループなしで N回出力完了！

・`Array(N)` で N 個の空の要素を持つ配列を作成
・`.fill("paiza")` で全部 paiza にする
・`.join(" ")` で半角スペース区切りで結合



<br><br>
## まとめ
回数って、１からカウントするじゃんってなったのと、条件式をしっかり考えずに書いたのがNGを招いた。
