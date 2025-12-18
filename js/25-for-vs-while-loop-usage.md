# for か while か？使い分けは目的に合わせて

<br>

プログラミングをやっていると、「`for` で書いたほうがいいのか？それとも `while`？」と迷うこと、ありませんか？僕は実際に Paizaの問題で、`for` で書いたら無駄に複雑になりました。

## 問題概要

最初の状態：
パイザ君 と 霧島京子 は、それぞれ `1` を持っている。

操作ルール：
パイザ君の番: 自分の数を `a` 倍 し、京子の数に足す。
霧島京子の番: 自分の数を `b` で割った余り を、パイザ君の数に足す。

ゲームの終了条件：
京子の数が `n` より大きくなったら 終了！
このときのパイザ君の操作回数を求める。

📥 入力例:
```
6  //n
3 2  //a b
```
📤 出力例:
```
2  
```

## <br>❌ 最初に書いたコード（for を使った）
```js
for (let count = 1; ; count++) { // 条件なしの for
    kyoko += paiza * a;
    if (n < kyoko) { 
        console.log(count);
        break;
    }
    paiza += kyoko % b;
}
```

for なのに「何回回るかわからない」設計
    → 普通 for は 「回数が決まっている」 ときに使うもの。
    
break で無理やりループを抜ける書き方
    → while にすれば break なしでスッキリ書ける。


## <br>✅while を使う
```js
while (kyoko <= n) { // 明確な条件を指定
    count++;
    kyoko += paiza * a;
    paiza += kyoko % b;
}
console.log(count);
```
✅ 「条件が明確」
→ 「京子の数が n 以下の間ループする」 ので break 不要！
✅ カウンタ変数 (count) の管理が自然
→ while 内で count++ すれば OK

## <br>まとめ：for と while の選び方

for → 「回数が決まっている」場合
while → 「条件が続く限りループ」する場合

「とりあえず for で書こう」は危険！ ループの目的を考えて使い分ける のが大事です。

<br>[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/17/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e3%83%ab%e3%83%bc%e3%83%97%e3%81%ae%e9%81%b8%e6%8a%9e%e3%83%9f%e3%82%b9-for%e3%81%a8while%e3%81%ae%e4%bd%bf%e3%81%84%e5%88%86/)
