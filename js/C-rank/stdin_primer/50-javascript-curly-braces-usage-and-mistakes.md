# {} の扱いに関するミス！　→　✅ {} の意味と使い方をまとめた！

<br>

最近解いていた問題でよく`forEach()`を使っていたので、`=>`の後に`{}`を使うから癖でやってしまった。
ここで、改めて`{}`についてよくある失敗例や、ルールを整理したい！


## {} の扱いに関するミス！　気づいたきっかけ（実体験）
今日もいつも通りpaizaの問題に挑戦していた！

https://paiza.jp/works/mondai/stdin_primer/stdin_primer__matrix_data_boss

### <Br>問題概要

- 最初に整数 N（行数）が与えられる。
- N 行のデータが与えられ、各行はスペース区切りの整数のリスト。
- 各行の 最初の数字はカウント（不要な情報） で、それを除いた残りの数字をそのまま出力する。

<br>入力例
```
3
1 8
2 8 1
3 8 1 3
```
出力例
```
8
8 1
8 1 3
```
<br>

### 🌋ミスったコード

```js
const rl = require('readline').createInterface({input:process.stdin});
const lines = [];

rl.on('line',(input)=>{
    lines.push(input);
});

rl.on('close',()=>{
    const N = Number(lines[0]);
    const nums = lines.slice(1).map(num => {num.split(' ').map(Number)});
    
    for(let i = 0; i < N; i++){
        console.log(nums[i].slice(1).join(' '))
    }
    
});
```
<br>

🌋ミスったポイント

```js
const nums = lines.slice(1).map(num => {num.split(' ').map(Number)});
```
- `map()` 内で `{}` を使うと、関数の戻り値が `undefined` になる（`{}` をブロックとして扱ってしまう）。
- `.map(Number)` を適用しても結果を `return` していないため、`nums` の中身が `undefined` になってしまう。

<br>✅修正
`map()` の中で `{}` を使わず、`=>` のすぐ後ろに式を記述。
```js
lines.slice(1).map(line => line.split(' ').map(Number))
```
<br>
（このコードなら、別に数値型に変換しなくても良かったかもと、今更思ったり (´;ω;｀) ）


# <br><br>✅ {} の意味と使い方

JavaScript では`{}` は **2通りの意味** で使われる。

## ① ブロック（制御構造）

`if` や `forEach` などの制御構造で使われる `{}` は ブロック（処理のグループ化） の意味。
```js
const arr = [1, 2, 3];

arr.forEach(num => {
    console.log(num * 2); // {} はブロックなので return 必須
});
```

これは ブロック なので、関数の結果（戻り値）は 自動で `return` されない。
そのため、値を返したい場合は `return` を明示する必要がある。

## <br>② オブジェクトリテラル

`{}` は オブジェクト を表すこともある。
```js
const obj = { name: "Alice", age: 25 }; // これはオブジェクト
```

### ❌ よくある失敗例
1. `map()` で `{}` を使って `return` しない

間違い：
```js
const nums = [1, 2, 3];

const squared = nums.map(num => {
    num * 2; // return しないので undefined
});

console.log(squared); // [undefined, undefined, undefined]
```
正しくは：
```
const squared = nums.map(num => num * 2); // `{}` を省略
console.log(squared); // [2, 4, 6]
```
または `return` を明示：
```js
const squared = nums.map(num => { return num * 2; });
console.log(squared); // [2, 4, 6]
```

<br><br>2.オブジェクトを返したいのに `{}` がブロック扱いされる

間違い：
```js
const users = ["Alice", "Bob"];

const userObjects = users.map(name => { name: name }); // {} がブロック扱いされる

console.log(userObjects); // [undefined, undefined]
```
正しくは：
```js
const userObjects = users.map(name => ({ name: name })); // `()` でオブジェクトと明示
console.log(userObjects); // [{ name: "Alice" }, { name: "Bob" }]
```

## <br><br>✅ {} を使うかどうかのルール

![スクリーンショット 2025-04-09 224258.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/4018208/169b5d9b-15d0-42f5-a78c-61fd41302361.png)



### <br>📌 結論
- 処理が1行なら `{}` なしで `return` 省略OK
- `{}` を使うと `return` 必須
- オブジェクトを返すときは `({ key: value })` の形にする


## <br><br>✅ ブロック ({}) とは？

ブロック とは、 1つ以上のステートメント（処理）をまとめる構造 のこと。
JavaScript では`{}`（波かっこ）を使ってブロックを定義する。

### <br>📝 ブロックの主な用途
- 制御構造のブロック
- 関数のブロック
- スコープのブロック

#### <br>① 制御構造のブロック

`if` や `for` などの制御構造で使われる `{}`
```js
if (true) {  // ← ここから
    console.log("This is a block!"); // ここまでがブロック
}

for (let i = 0; i < 3; i++) {  // ← ここから
    console.log(i); // ここまでがブロック
}
```
- ブロックの中に複数の処理をまとめられる
- ブロック内の変数 (`let` や `const`) はブロック外からアクセス不可

#### <br>② 関数のブロック

関数の処理を `{}` で囲む
```js
function greet(name) {  // ← ここから
    console.log("Hello, " + name); // ここまでがブロック
}
greet("Alice");
```
- 関数の処理を `{}` でまとめる
- 関数のローカル変数もブロック内に閉じ込められる

#### <br>③ スコープのブロック

ブロックごとに変数のスコープ（範囲）が決まる
```js
{
    let x = 10; // ブロック内の変数
    console.log(x); // 10
}
console.log(x); // ❌ エラー (x is not defined)
```
- ブロック内で `let` や `const` を使うと、外からアクセスできない
- ブロックごとに変数のスコープが独立


### <br>❌ よくあるミス

1. `if` や `for` のブロックを省略して意図しない動作
```js
if (true) 
    console.log("Hello");
    console.log("World"); // ← `if` の外なので常に実行される
```
正しくは：
```js
if (true) {
    console.log("Hello");
    console.log("World"); // これで意図通り
}
```

<br>2. `{}` を使ったのに `return` し忘れる
```js
const double = num => { num * 2 }; // `{}` を使ったので return 必須
console.log(double(5)); // undefined
```
正しくは：
```js
const double = num => { return num * 2 };
console.log(double(5)); // 10
```
または：
```js
const double = num => num * 2; // `{}` を省略すれば return 不要
console.log(double(5)); // 10
```

### <br>✅ まとめ
- ブロック `{}` は処理をまとめるために使う
- `if` や for でブロックを省略するとバグの原因になる
- アロー関数で `{}` を使うと `return` が必要



## <Br><br>✅ ブロック {} vs. オブジェクト {} の違い

![スクリーンショット 2025-04-09 224356.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/4018208/c13cd73a-fd52-4c31-8843-7c5551a4c094.png)


### <br>🔹 見分け方

- `if` や `function` の後の `{}` → ブロック
- `=` の右側にある `{}` → オブジェクト


### <br>✅ まとめ
- ブロック `{}` は処理をまとめる
- オブジェクト `{}` はデータをまとめる
- 見た目は同じでも、用途とルールが異なるので注意！



    
## <br><br><br>おまけ　発端のforEach()について


✅ 省略形（1行なら `{}` なしでもOK）
```js
array.forEach(a => console.log(a));
```
→ `console.log(a);` だけなら `{}` なしで書ける


<br><br>✅`{}` を使った書き方（1行でも使える）
```js
array.forEach(a => { console.log(a); });
```
→ `{}` を使っても動くが、1行なら省略できる

<br><br> 🌋どっちでもいいけど、短く書けるなら {} なしの方がスッキリする

<br><br>

`map()` では 1行でも `{}` を使うと意図しない動作になる ことがある。

🔴 `map()` で `{}` を使うと `undefined` が返る
```js
const arr = [1, 2, 3];

const result = arr.map(num => { num * 2 });
console.log(result); // [undefined, undefined, undefined]
```
理由：
- `{}` を使うと、ブロック（処理のまとまり） と解釈される。
- `return` を明示しないと、値を返さない（`undefined` になる）。

<br><br>✅ 正しい書き方（1行なら `{}` を省略）
```js
const result = arr.map(num => num * 2);
console.log(result); // [2, 4, 6]
```

<br>✅ ブロックを使うなら `return` を書く
```js
const result = arr.map(num => { return num * 2; });
console.log(result); // [2, 4, 6]
```

<br><br>
### `forEach` と `map` の違い

- `forEach()` → 返り値なし（処理を実行するだけ）
- `map()` → 配列を返す（`return` が重要）


だから、

✔ `forEach()` は `{}` を使っても問題なし
✔ `map()` は `{}` を使うなら `return` が必要
という違いがあるんだ！


<br><br>[僕の失敗談(´;ω;｀)🚀](https://paizabeginner.wordpress.com/2025/04/11/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e2%9c%85-%e3%81%ae%e6%84%8f%e5%91%b3%e3%81%a8%e4%bd%bf%e3%81%84%e6%96%b9%e3%82%92%e3%81%be%e3%81%a8%e3%82%81%e3%81%9f/)

