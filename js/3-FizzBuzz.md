
# FizzBuzzを制す者、基礎を制す！

<br>

「FizzBuzz？ 簡単そう」と思った僕。でも、いざ書くと if の順番 でハマった！間違えると FizzBuzzがFizzになる という罠…。「なぜ？」を考えながら、正しい書き方を整理してみた。
条件の順番が大事！

<br>

## FizzBuzzの基本ルール:

3の倍数 → Fizz
5の倍数 → Buzz
3と5の倍数 → FizzBuzz（これを最優先に！）

## NG例（FizzBuzzが出ない）:
```js
if (count % 3 === 0) { 
    console.log("Fizz"); 
} else if (count % 5 === 0) { 
    console.log("Buzz"); 
} else if (count % 3 === 0 && count % 5 === 0) {  
    console.log("FizzBuzz"); }  
```

☠ if の順番ミス！3の倍数でFizzが出てしまい、FizzBuzzになるはずの数がFizzに…

## →　✅条件が厳しいものから順に並べる！
