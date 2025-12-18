# すべての "行の長さと値が不定" な 2 次元配列の出力

<br>



Paizaの問題を解いてたら、最初のコードで全部 1 から順に並んじゃって大混乱。よく考えたら `index` の管理をミスってた…。`Iterator` や `slice()` を使えばスッキリ解決するから、複数の解法を紹介するよ！

## 問題の概要

**入力:**

- 1行目：数列の長さ `N` と行グループの数 `M`
- 2行目：`N` 個の数列 `A`
- 3行目：各グループのサイズ `B`

**出力:**

- `M` 個のグループごとに `A` の一部を出力（各行の末尾にはスペースなし）


https://paiza.jp/works/mondai/stdout_primer/stdout_primer__variable_array_boss


### 入力例

```
5 2
1 2 3 4 5
2 3
```

### 出力例

```
1 2
3 4 5
```

## <br>NG例（僕の最初の失敗）

```javascript
for (let i = 0; i < M; i++){
    for(let k = 0; k < row[i]; k++){
        if(k+1 === row[i]){
            process.stdout.write(nums[k] + "\n");
        }else {
            process.stdout.write(nums[k] + " ");
        }
    }
}
```

**ミス:**

- `nums[k]` を毎回 0 から参照してるせいで、出力が全部 1 から始まる…。

## <br>OK例（解決法）

### 解法1：Iterator を使う

```javascript
const [N, M] = lines[0].split(" ").map(Number);
const nums = lines[1].split(" ").map(Number);
const row = lines[2].split(" ").map(Number);

const iter = nums.values();
for (let i = 0; i < M; i++){
    for(let j = 0; j < row[i]; j++){
        if(j + 1 === row[i]){
            process.stdout.write(iter.next().value + "\n");
        }else{
            process.stdout.write(iter.next().value + " ");
        }
    }
}
```

**ポイント:**

- `nums.values()` で `Iterator` を作成
- `iter.next().value` を使って値を順番に取得

### <br>解法2：index で管理

```javascript
let index = 0;
for (let i = 0; i < M; i++) {
    let output = [];
    for (let j = 0; j < row[i]; j++) {
        output.push(nums[index++]);
    }
    console.log(output.join(" "));
}
```

**ポイント:**

1. `output` 配列を用意し、`row[i]` の個数分だけ値を取得
2. `join(" ")` でスペース区切りの文字列に変換し、一括で `console.log`
3. `index` で `nums` のインデックスを管理

### <br>解法3：`slice()` を使う

```javascript
const [N, M] = lines[0].split(" ").map(Number);
const A = lines[1].split(" ").map(Number);
const B = lines[2].split(" ").map(Number);

let begin = 0;
for (let i = 0; i < M; i++) {
    let end = begin + B[i];
    console.log(A.slice(begin, end).join(" "));
    begin = end;
}
```

**解説:**

1. `N, M` を取得（`N`: 数列の長さ, `M`: グループの数）
2. `A` を数列として取得（例: `[1, 2, 3, 4, 5]`）
3. `B` をグループのサイズとして取得（例: `[2, 3]`）
4. `begin` を 0 に初期化
5. `end = begin + B[i]` を計算し、適切な範囲を取得 (`slice(begin, end)`)。
6. `begin = end` に更新して、次のグループの処理へ進む。

### 処理の流れの例：

- `begin = 0, end = 2` → `A[0]` から `A[1]` を出力 → `1 2`
- `begin = 2, end = 5` → `A[2]` から `A[4]` を出力 → `3 4 5`

### ポイント

- `begin` はグループの開始位置、 `end` はグループの終了位置を表す。
- `slice(begin, end)` を使うことで簡潔に部分配列を取得できる。

## まとめ

✅ `Iterator` を使うとスッキリ解決！ 
✅ `index` を管理すればシンプルに書ける！ 
✅ `slice()` を使うと直感的！


<br>[僕の失敗談と解決話！](https://paizabeginner.wordpress.com/2025/03/27/paiza%e3%81%a7%e5%9f%ba%e6%9c%ac%e3%83%9e%e3%82%b9%e3%82%bf%e3%83%bc%ef%bc%9a%e3%81%99%e3%81%b9%e3%81%a6%e3%81%ae%e8%a1%8c%e3%81%ae%e9%95%b7%e3%81%95%e3%81%a8%e5%80%a4%e3%81%8c%e4%b8%8d%e5%ae%9a/)

