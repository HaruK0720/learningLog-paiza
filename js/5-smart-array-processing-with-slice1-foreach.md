
# slice(1) × forEach() でスマートに配列処理！


高2の僕が「配列の要素を出力するだけでしょ？」と油断したら、思わぬ落とし穴が…。
`for` 文でループしたら、余計な値まで出力されてしまった！
解決策は `slice(1) × forEach()` のコンボ。

<br><br>

## NG例（失敗コード）
```js
rl.on('close',() => {
    for (let i = 0; i < lines.length; i++) {
        console.log(lines[i]);  // n（最初の要素）まで出力されてしまう！
    }
});
```
<br>

## OK例（改善コード）
```js
rl.on('close',() => {
    lines.slice(1).forEach(num => {
        console.log(num);
    });
});
```


## ポイント
✅ `slice(1)` で最初の要素（n）を除外
✅ `forEach()` でシンプルにループ処理

<br><br>

## 学んだこと
💡 `slice(1)` は、2つ目の要素から最後までの新しい配列を返す
💡 `forEach()` は、配列の要素ごとに処理を実行するのに便利
💡 意図しない出力を防ぐには、データ構造をしっかり理解することが大事！
