# 📌 Set と Map の違いまとめ (JavaScript)  　初心者なりにまとめてみた！

<br>

ここ最近paizaの単語のカウント問題を解いていたけど、最終問題まで解けた！
段階的に解いていくうちに学んだ`Set`と`Map`について改めてまとめたい！

`JavaScript` の `Set` と `Map` はどちらもデータを格納するけど、用途や特性が異なる！
（いろいろ間違ってたらごめんなさい。(m´・ω・｀)m ｺﾞﾒﾝ…ぜひご教示ください(^^ゞ）

# <br>✅ Set（集合）

### 💡 特徴
- 値のみを格納（キーがない）
- 重複を許さない（同じ値は 1 つだけ）
- 要素の順序を保持（挿入順）
- 配列のように扱えるが、重複を排除したい場合に適している

### <br>🛠 主な操作
![スクリーンショット 2025-04-07 230554.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/4018208/4f29b9fa-355b-4f0e-af0f-e750e06edd38.png)
※見にくかったら、画像をクリックしたら見やすくなるかも

### <br>🔹 使用例
```js
const mySet = new Set();

// 値を追加
mySet.add("apple");
mySet.add("banana");
mySet.add("apple");  // 重複は無視される

console.log(mySet);  // Set(2) { 'apple', 'banana' }
console.log(mySet.has("apple"));  // true
console.log(mySet.size);  // 2

// 値を削除
mySet.delete("banana");

console.log(mySet);  // Set(1) { 'apple' }

// 全削除
mySet.clear();
console.log(mySet.size);  // 0
```

# <Br>✅ Map（連想配列 / ハッシュマップ）
### 💡 特徴
- キーと値のペア を格納できる
- キーにあらゆるデータ型を使える（オブジェクトや関数もOK）
- キーの順序を保持
- `Object` の代わりに使えるが、`Map` はキーを安全に扱える

### <Br>🛠 主な操作

![スクリーンショット 2025-04-07 230604.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/4018208/037210e6-4887-460d-bb83-9d4ae43956d2.png)

### <br>🔹 使用例
```js
const myMap = new Map();

// キーと値を追加
myMap.set("name", "Alice");
myMap.set(123, "Number Key");
myMap.set({ obj: true }, "Object Key");

console.log(myMap.get("name"));  // Alice
console.log(myMap.get(123));  // Number Key

// キーの存在をチェック
console.log(myMap.has("name"));  // true

// キーを削除
myMap.delete(123);

console.log(myMap.size);  // 2
```

## <br>✅ Set と Map の違いまとめ

![スクリーンショット 2025-04-07 230157.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/4018208/2c3a5525-38b0-4cca-9aed-d778c0e9389e.png)


## <Br>✅ どちらを使うべきか？

![スクリーンショット 2025-04-07 230207.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/4018208/95877825-e9f3-40c3-b23e-8a6333b90960.png)


## <br>✅ Set と Map を使った実践例
🔹 例1: 文字列の重複チェック（`Set`）
```js
const words = ["apple", "banana", "apple", "orange", "banana"];
const uniqueWords = new Set(words);
console.log([...uniqueWords]); // ['apple', 'banana', 'orange']
```
🔹 例2: 単語の出現回数カウント（`Map`）
```js
const words = ["apple", "banana", "apple", "orange", "banana"];
const wordCount = new Map();

words.forEach(word => {
    wordCount.set(word, (wordCount.get(word) || 0) + 1);
});

console.log(wordCount); 
// Map(3) { 'apple' => 2, 'banana' => 2, 'orange' => 1 }
```

## <Br>✅ まとめ

✔ `Set` は重複を排除し、値のコレクションを管理するのに最適
✔ `Map` はキーと値のペアを管理するのに最適
✔ 順序を保持したい場合、どちらも `for...of` で順番通り取得可能

これで `Set` と `Map` の違い がバッチリ理解できたね！ 🎯


<br><br><br>[僕の失敗談と解決談🚀](https://paizabeginner.wordpress.com/)

