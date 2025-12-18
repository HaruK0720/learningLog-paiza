
# 🗝️ Map のキー、型変換でミスった！

<br>

今日はpaizaの問題を解いて「キーの型変換」でハマりかけた話をサクッと解説！

## <br>**問題の概要**
A, B, C の3つのグループがあり、各メンバーには番号が振られている。
A グループの各メンバーは B グループの特定のメンバーに仕事を依頼し、B グループのメンバーは C グループのメンバーに仕事を依頼する。
その結果、A グループのメンバーの仕事は最終的に C グループのメンバーによって処理される。

このとき、A グループの各メンバーが最終的に C グループのどのメンバーに仕事を頼むことになるのかを求める問題！
```
p q r
i_1 j_1
...
i_p j_p
j'_1 k_1
...
j'_q k_q
```
- 1行目: `p`,`q`, `r`（A, B, C グループの人数）
- 2行目から (`p + 1`) 行目:
`i_a j_a`（A グループ `i_a` の人が B グループ `j_a` の人に仕事を頼む）
- (`p + 2`) 行目から (`p + q + 2`) 行目:
`j'_b k_b`（B グループ `j'_b` の人が C グループ `k_b` の人に仕事を頼む）

⚠ 注意点
B グループの情報には、A グループから仕事を頼まれていない B のメンバーの情報も含まれる場合がある（不要な情報がある）。

入力例：
```
2 3 4
1 3
2 1
2 3
3 3
1 4
```
出力例：
```
1 3
2 4
```

## <Br>❌ NGコード全文

まず、最初に書いた 失敗コード をどうぞ👇
```js
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
});

const lines = [];

rl.on('line', (input) => {
    lines.push(input.trim());
});

rl.on('close', () => {
    const iter = lines.values();
    const [a, b, c] = iter.next().value.split(" ").map(Number);
    
    const groupA = Array.from({length: a}, () => iter.next().value);
    const mapA = new Map(groupA.map(item => {
        const [key, value] = item.split(" ").map(Number); // ❌ キーを数値化
        return [key, value];
    }));
    
    const groupB = Array.from({length: b}, () => iter.next().value);
    const mapB = new Map(groupB.map(item => {
        const [key, value] = item.split(" ").map(Number); // ❌ キーを数値化
        return [key, value];
    }));
    
    const sortedKeys = groupA
        .map(item => item.split(" ")[0])
        .sort((a, b) => Number(a) - Number(b)); // ❌ 数値ソート
    
    sortedKeys.forEach(a => {
        console.log(a, mapB.get(mapA.get(a)));
    });
});
```
## <br>🚨 ミスポイント解説
このコード、パッと見は正しく動きそうに見えるけど… 落とし穴が3つ！

### 1️⃣ キーを .map(Number) で数値化してしまった！
```js
const mapA = new Map(groupA.map(item => {
    const [key, value] = item.split(" ").map(Number); // ❌
    return [key, value];
}));
```
🔴 文字列 "1" と 数値 1 は別のキー扱い！
```js
const map = new Map();
map.set("1", "A");

console.log(map.get(1));  // ❌ undefined
console.log(map.get("1")); // ⭕ "A"
```
キーを "1" のまま扱わないと、後で .get() したときに undefined になる！

### <br><br>2️⃣ ソートで Number(a) - Number(b) してしまった！
```js
const sortedKeys = groupA
    .map(item => item.split(" ")[0])
    .sort((a, b) => Number(a) - Number(b)); // ❌
```
🔴 Map のキーは文字列なのに、無理やり数値ソート！
文字列の "10" と "2" を Number() で数値変換すると、
本来 "10" > "2" になるはずが、 10 > 2 で 順番が崩れる！

### <Br><br>3️⃣ イテレータ (iter.next()) を使っている
```js
const iter = lines.values();
const [a, b, c] = iter.next().value.split(" ").map(Number);
```
🔴 .next() を多用するとコードがゴチャつく！
配列の .slice() を使えば、もっとスッキリ書ける！

## <br><br>✅ 修正コード（スマート版）
ミスを直して より短く、わかりやすく！
```js
const readline = require('readline');
const rl = readline.createInterface({ input: process.stdin });
const lines = [];

rl.on('line', input => lines.push(input));
rl.on('close', () => {
    const [a, b, c] = lines[0].split(" ").map(Number);
    
    // Mapを作成（キーはそのまま文字列）
    const mapA = new Map(lines.slice(1, a + 1).map(x => x.split(" ")));
    const mapB = new Map(lines.slice(a + 1, a + 1 + b).map(x => x.split(" ")));
    
    // Aのキーを辞書順に並べて出力
    [...mapA.keys()].sort().forEach(a => {
        console.log(a, mapB.get(mapA.get(a)));
    });
});
```

## <br>🎯 改善点まとめ
- キーを数値化しない（"1" と 1 は違う！）
- split(" ") で文字列のまま Map に格納
- スプレッド構文 ([...map.keys()]) でソートも簡単
- slice() を使い、イテレータ不要に！



<br>[🔗 僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/16/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-map-%e3%82%92%e4%bd%bf%e3%81%a3%e3%81%a6%e4%bb%95%e4%ba%8b%e3%81%ae%e6%b5%81%e3%82%8c%e3%82%92%e8%bf%bd%e3%81%88%ef%bc%81/)
