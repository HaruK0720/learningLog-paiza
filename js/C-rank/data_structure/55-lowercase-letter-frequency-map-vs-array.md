# 英小文字の出現回数【Mapか配列か】charCodeAt()

<br>

英小文字の出現回数を数えるやつで、`Map`の扱いに失敗...。`undefined + 1`に泣いた経験から、学習記録をメモします！

## <br>💡 問題内容：a～zの出現回数をカウントせよ

 長さNの文字列Sが与えられる。
 それに含まれている「a〜z」までの各小文字の出現回数を、
 半角スペース区切りで1行に出力せよ！

<br>入力例：
```
13
aaabbbccdddde
```
出力例：
```
3 3 2 4 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
```
<br>

https://paiza.jp/works/mondai/data_structure/data_structure__dict_step2


## <br>🔥 初手でやらかした僕のコードの全体像
```js
const rl = require('readline').createInterface({input:process.stdin});
const lines = [];

rl.on('line',(input)=>{
    lines.push(input);
});

rl.on('close',()=>{
    const letters = lines[1].split('');
    const alphabet = new Map();

    for (let i = 0; i <= 26; i++) {
        alphabet.set(letters[i], 0);// ❌ letters から初期化しちゃってる
    }

    letters.forEach(a => {
        alphabet.set(a, alphabet.get(a) + 1);// ❌ 未定義 + 1 で NaN祭り
    });

    const result = [];
    alphabet.forEach(value => result.push(value));
    console.log(result.join(' '));
});
```

### <br>❌ NGポイント: Mapでキー初期化できてない
```js
for (let i = 0; i <= 26; i++) {
    alphabet.set(letters[i], 0);// ❌ letters から初期化しちゃってる
}
    
const alphabet = new Map();
letters.forEach(a => {
  alphabet.set(a, alphabet.get(a) + 1); // ← undefined + 1 → NaN
});
```
未出現の文字に `.get()` すると `undefined。`数値加算で NaN に。


## <br><br>✅最終的なコード（Mapを使った）の全体像
```js
const rl = require('readline').createInterface({ input: process.stdin });
const lines = [];

rl.on('line', (input) => {
    lines.push(input);
});

rl.on('close', () => {
    const letters = lines[1].split('');
    const alphabet = new Map();

    // a～z をキーとして0で初期化
    for (let i = 0; i < 26; i++) {
        const char = String.fromCharCode(97 + i); 
                                        // 97: 'a'
        alphabet.set(char, 0);
    }

    // カウント処理
    letters.forEach(a => {
        
    alphabet.set(a, alphabet.get(a) + 1);
       
    });

    // 出力
    const result = [];
    alphabet.forEach(value => result.push(value));
    console.log(result.join(' '));
});
```



## <br><br>🌟charCodeAt() で "配列" を使う方法
```js
const letters = lines[1].split('');
const counts = Array(26).fill(0);

letters.forEach(ch => {
  counts[ch.charCodeAt(0) - 97]++;
});

console.log(counts.join(' '));
```
- 'a'`.charCodeAt(0)` → 97
- `文字 - 97`で a〜z を 0〜25 にマッピング
- 連番キーなら `Map` より"配列"がベター（高速・簡潔）

## <br>📝まとめ
- `Map` は動的キー管理向け。a〜zのような固定キーなら不向き。
- 配列＋`charCodeAt()` の組み合わせで、数値インデックス管理が最適。
- 出現回数のカウント処理では、「連番キーかどうか」で構造を選ぶのがコツ。

<br><br>[僕の失敗談(´;ω;｀)🚀](https://paizabeginner.wordpress.com/2025/04/16/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc-%e8%8b%b1%e5%b0%8f%e6%96%87%e5%ad%97%e3%81%ae%e5%87%ba%e7%8f%be%e5%9b%9e%e6%95%b0%e3%80%90map%e3%81%8b%e9%85%8d%e5%88%97%e3%81%8b/)



### <br><br><br>おまけ解説🔢 英小文字を数字に変換する方法

JavaScript では、文字のコード番号（Unicode）を使って変換できる。

<br>🌟 `charCodeAt()` を使う！
```js
const char = 'a';
const code = char.charCodeAt(0);  // 97

console.log(code); // 出力: 97
```
'a' は 97
'b' は 98
'z' は 122

<br><br>✨ a を 0、b を 1、... z を 25 にしたい場合
```js
const char = 'e';
const index = char.charCodeAt(0) - 'a'.charCodeAt(0);  // 101 - 97 = 4

console.log(index); // 出力: 4
```
これで "a" → 0, "b" → 1, ..., "z" → 25 になる！

<br><br>🔁 複数文字を変換する例
```js
const str = 'hello';
const positions = [...str].map(c => c.charCodeAt(0) - 97);
console.log(positions);  // 出力: [7, 4, 11, 11, 14]
```

<br><br>🔄 逆に数字から文字に戻したい場合
```js
const num = 4;
const char = String.fromCharCode(num + 97);  // 'e'
console.log(char);
```

<br><br>🔍 `charCodeAt(index)` とは？

JavaScript では、`charCodeAt(index)` は 指定した位置の文字の「Unicode（文字コード）」を返す関数！
```js
const str = "abc";
console.log(str.charCodeAt(0)); // => 97 ('a')
console.log(str.charCodeAt(1)); // => 98 ('b')
console.log(str.charCodeAt(2)); // => 99 ('c')
```

<br>🎯 `charCodeAt(0)` の `0` は？

これは「最初の文字」って意味！
```js
const ch = "z";
console.log(ch.charCodeAt(0)); // => 122 ('z')
```

たとえば 'z' は1文字だから、`charCodeAt(0)` でOK。
でも 'abc' みたいに複数文字のときは、何番目の文字を見るかで結果が変わるよ。

<br><br>✍️ まとめると！
- "a"`.charCodeAt(0)` は 'a' の文字コード（97）を返す
- "abc"`.charCodeAt(1)` は 'b' の文字コード（98）を返す
- 0 は「何文字目を調べるか」のインデックス番号


