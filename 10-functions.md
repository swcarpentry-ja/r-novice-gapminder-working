---
title: Functions Explained
teaching: 45
exercises: 15
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- Define a function that takes arguments.
- Return a value from a function.
- Check argument conditions with `stopifnot()` in functions.
- Test a function.
- Set default values for function arguments.
- Explain why we should divide programs into small, single-purpose functions.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I write a new function in R?

::::::::::::::::::::::::::::::::::::::::::::::::::



分析したいデータセットが一つだけなら、ファイルを表計算ソフトで読み込み、単純な統計値をプロットした方が早いでしょう。 しかし、gapmider データは定期的に更新されるので、後から新しい情報を読み込み分析し直したくなります。 また、将来的には似たようなデータを違う場所から入手することもあるでしょう。

この講義では関数の書き方を学ぶことで、同じ操作を一つのコマンドで繰り返せるようになります。

:::::::::::::::::::::::::::::::::::::::::  callout

## 関数とは何でしょう？

関数は連続した操作を一つに纏め、後から使うときのために保存しておきます。 Functions provide:

- a name we can remember and invoke it by
- relief from the need to remember the individual operations
- a defined set of inputs and expected outputs
- rich connections to the larger programming environment

As the basic building block of most programming languages, user-defined
functions constitute "programming" as much as any single abstraction can. 関数を書いた時点であなたはコンピュータープログラマーです。

::::::::::::::::::::::::::::::::::::::::::::::::::

## 関数を定義しましょう。

`functions/` ディレクトリ内に新しく functions-lesson.R と名付けた R スクリプトを作成して開きましょう。

The general structure of a function is:


``` r
my_function <- function(parameters) {
  # perform action
  # return value
}
```

華氏をケルビンに変換する `fahr_to_kelvin()` という関数を定義しましょう。


``` r
fahr_to_kelvin <- function(temp) {
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}
```

`fahr_to_kelvin()` を定義するには、`fahr_to_kelvin` に `function` の出力を指定します。 引数の名前の一覧は括弧に中に書きます。   次に関数の[本文 (body)]({}/reference/#function-body) として 走らせた時の実行内容を波括弧 (`{}`) の中に記述します。 本文はスペース二つでインデントしておきます。 これによりコードの操作内容を変更せずに可読性を向上させます。

It is useful to think of creating functions like writing a cookbook. First you define the "ingredients" that your function needs. In this case, we only need one ingredient to use our function: "temp". After we list our ingredients, we then say what we will do with them, in this case, we are taking our ingredient and applying a set of mathematical operators to it.

関数を呼び出す時、引数に指定した値は関数内で用いられる変数に与えられます。 関数の中では関数を呼び出した相手に結果を送るために [return 文]({}/reference/#return-statement) を用います。

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント

return 文が不要なことは R の変わった特徴の一つです。
R では関数の本文の最終行に記述された変数が自動的に返り値になります。 しかし、わかりやすくするために return 文を明示的に記述します。

::::::::::::::::::::::::::::::::::::::::::::::::::

関数を実行してみましょう。
自分の関数を呼び出す方法は他の関数を呼び出す方法と同じです。


``` r
# 水の凝固点 fahr_to_kelvin(32)
```


``` r
# 水の沸点 fahr_to_kelvin(212)
```

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ１

Write a function called `kelvin_to_celsius()` that takes a temperature in
Kelvin and returns that temperature in Celsius.

Hint: To convert from Kelvin to Celsius you subtract 273.15

:::::::::::::::  solution

## チャレンジ８の解答 1

Write a function called `kelvin_to_celsius` that takes a temperature in Kelvin
and returns that temperature in Celsius


``` r
kelvin_to_celsius <- function(temp) {
 celsius <- temp - 273.15
 return(celsius)
}
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 関数を組み合わせましょう

関数の真髄を発揮するのは、関数を混ぜ合わせ組み合わせてより多きな塊にすることで、望み通りの効果を得る時です。

華氏をケルビンに変換する関数とケルビンをセ氏に変換する関数の2つを定義しましょう。


``` r
fahr_to_kelvin <- function(temp) {
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}

kelvin_to_celsius <- function(temp) {
  celsius <- temp - 273.15
  return(celsius)
}
```

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ２

Define the function to convert directly from Fahrenheit to Celsius,
by reusing the two functions above (or using your own functions if you
prefer).

:::::::::::::::  solution

## チャレンジ８の解答

Define the function to convert directly from Fahrenheit to Celsius,
by reusing these two functions above


``` r
fahr_to_celsius <- function(temp) {
  temp_k <- fahr_to_kelvin(temp)
  result <- kelvin_to_celsius(temp_k)
  return(result)
}
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 幕間: 防衛的プログラミング

関数を書くことで R のコードを効率的に再利用したりモジュール化する方法を理解し始めたところですが、 関数は想定した用途でのみ機能するように確実に設計することが重要です。 関数の引数を検査することは 防衛的プログラミング の考え方に繋がります。
防衛的プログラミングでは状況を頻繁に検査し何かおかしなことがあればエラーを返すことを推奨します。 このような検査は、プログラム実行を継続する前に現状が `TRUE` であることをアサート（表明・断言）するので、アサーション文と呼ばれます。
アサーション文により、エラーがどこで起きているか分かりやすくなりデバッグが容易になります。

### `stopifnot()` を用いて状態を検査しましょう

華氏をケルビンに変換する `fahr_to_kelvin()` 関数について再検討してみましょう。 この関数の定義は以下の通りです。


``` r
fahr_to_kelvin <- function(temp) {
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}
```

For this function to work as intended, the argument `temp` must be a `numeric`
value; otherwise, the mathematical procedure for converting between the two
temperature scales will not work. To create an error, we can use the function
`stop()`. For example, since the argument `temp` must be a `numeric` vector, we
could check for this condition with an `if` statement and throw an error if the
condition was violated. We could augment our function above like so:


``` r
fahr_to_kelvin <- function(temp) {
  if (!is.numeric(temp)) {
    stop("temp must be a numeric vector.")
  }
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}
```

複数の状態や引数を検査する必要があると、全てを検査するためのコードは何行にも渡ります。 幸いなことに R は `stopifnot` という便利な関数を提供しています。 `TRUE` と評価されるべき要件を必要なだけ列挙すると、 `stopifnot()` は一つでも `FALSE` がある場合にエラーを返します。 検査項目を列挙すると、追加のドキュメント化という2つ目の目的としても機能します。

`stopifnot()` を用いて `fahr_to_kelvin()` に入力を検査するアサーション文を追加し、 防衛的プログラミングに挑戦しましょう。

`temp` が数値ベクトルであることをアサートしたいとします。 以下のようにしましょう。


``` r
fahr_to_kelvin <- function(temp) {
  stopifnot(is.numeric(temp))
  kelvin <- ((temp - 32) * (5 / 9)) + 273.15
  return(kelvin)
}
```

入力が適切であればこれでも機能します。


``` r
# 水の凝固点 fahr_to_kelvin(temp = 32)
```

しかし不適切な入力があるとすぐに失敗します。


``` r
# Metric is a factor instead of numeric
fahr_to_kelvin(temp = as.factor(32))
```

``` error
Error in fahr_to_kelvin(temp = as.factor(32)): is.numeric(temp) is not TRUE
```

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ３

防衛的プログラミングにより、`fahr_to_celsius()` 関数の `temp` 引数に不適切な 値が指定されたらすぐにエラーを返すよう念押しして下さい。

:::::::::::::::  solution

## チャレンジ３の解答

チャレンジ３の解答 明示的に `stopifnot()` を呼ぶことで先述の関数の定義を拡張しましょう。 `fahr_to_celsius()` は2つの他の関数から構成されているので、 ここでの検査は2つの関数の検査に追加され冗長になります。


``` r
fahr_to_celsius <- function(temp) {
  stopifnot(is.numeric(temp))
  temp_k <- fahr_to_kelvin(temp)
  result <- kelvin_to_celsius(temp_k)
  return(result)
}
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## もっと関数を組み合わせましょう

ここで我々のデータセットで利用できるデータからある国の国内総生産を計算するための関数を定義します。


``` r
# データセットを受け取り、人口の列と一人あたりのGDPをかけます。 calcGDP <- function(dat) { gdp <- dat$pop dat$gdpPercap return(gdp}
```

`calcGDP()` を定義するために、`function` の結果を `calcGDP` に代入します。 引数の名前の一覧は括弧に中に書きます。 次に、本文 -- 関数を読んだ時に実行される命令文 -- は波括弧 (`{}`) の中に書きます。

本文中の命令文は2つのスペースでインデントしました。 これにより関数の動作に影響を及ぼさずに可読性を向上できます。

関数を呼び出す時に、関数に渡した値は引数に指定され、 関数の本文中における変数になります。

関数の中では `return()` 関数を用いて結果を返します。
`return()` 関数は必須ではなく、R は 関数の最終行で実行されたコマンドの結果を自動的に返します。


``` r
calcGDP(head(gapminder))
```

``` error
Error in calcGDP(head(gapminder)): could not find function "calcGDP"
```

That's not very informative. これでは情報に乏しいです。いくつか引数を追加して、年ごとと国ごとの情報を得られるようにしましょう。


``` r
# データセットを受け取り、人口の列と一人あたりのGDPの列をかけます。 calcGDP <- function(dat, year=NULL, country=NULL) { if(!is.null(year)) { dat <- dat[dat$year %in% year, ] } if (!is.null(country)) { dat <- dat[dat$country %in% country,] } gdp <- dat$pop dat$gdpPercap new <- cbind(dat, gdp=gdp) return(new}
```

もしこれらの関数を別の R スクリプトに書いているなら (グッドアイディア！)、 `source()` 関数を使って関数を R セッションに読み込むことができます。


``` r
source("functions/functions-lesson.R")
```

Ok, so there's a lot going on in this function now. In plain English, the
function now subsets the provided data by year if the year argument isn't empty,
then subsets the result by country if the country argument isn't empty. Then it
calculates the GDP for whatever subset emerges from the previous two steps. The
function then adds the GDP as a new column to the subsetted data and returns
this as the final result. You can see that the output is much more informative
than a vector of numbers.

`year` を指定した時に何が起きるか見てみましょう。


``` r
head(calcGDP(gapminder, year=2007))
```

``` error
Error in calcGDP(gapminder, year = 2007): could not find function "calcGDP"
```

また `country` を指定するとどうなるでしょうか。


``` r
calcGDP(gapminder, country="Australia")
```

``` error
Error in calcGDP(gapminder, country = "Australia"): could not find function "calcGDP"
```

あるいは両方指定してみましょう。


``` r
calcGDP(gapminder, year=2007, country="Australia")
```

``` error
Error in calcGDP(gapminder, year = 2007, country = "Australia"): could not find function "calcGDP"
```

関数の本文を順番に見ていきましょう。


``` r
calcGDP <- function(dat, year=NULL, country=NULL) {
```

ここで `year` と `country` の二つの引数を追加しました。 `=` 演算子を関数定義時に用いることで、両者の 既定値 には `NULL` を指定しています。 これにより、ユーザーが値を指定しない限り、これらの引数の値は `NULL` になることを意味します。


``` r
  if(!is.null(year)) {
    dat <- dat[dat$year %in% year, ]
  }
  if (!is.null(country)) {
    dat <- dat[dat$country %in% country,]
  }
```

ここでは、追加した引数それぞれについて値が `NULL` であるか確認し、 `NULL` でなければ `dat` に格納されたデータセットを非 `NULL` な引数の値を用いて絞り込み上書きします。

Building these conditionals into the function makes it more flexible for later.
この関数を用いて、以下の様々な場合のGDPを計算できます。

- データセット全体
- ある年
- ある国
- ある年とある国の組み合わせ

代わりに `%in%` を使うことによって、`year` と `country` に複数の値を指定できるようになっています。

:::::::::::::::::::::::::::::::::::::::::  callout

## Tip: 値渡し

Functions in R almost always make copies of the data to operate on
inside of a function body. When we modify `dat` inside the function
we are modifying the copy of the gapminder dataset stored in `dat`,
not the original variable we gave as the first argument.

This is called "pass-by-value" and it makes writing code much safer:
you can always be sure that whatever changes you make within the
body of the function, stay inside the body of the function.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Tip: 関数のスコープ

Another important concept is scoping: any variables (or functions!) you
create or modify inside the body of a function only exist for the lifetime
of the function's execution. 関数の本文中で作成したり変更したいかなる変数 (関数を含む！) は、 関数を実行している間だけ存在します。`calcGDP()` を呼んだ時に、 `dat`、`gdp`、そして `new` という変数は関数の本文中でのみ存在します。 対話的な R のセッションにおいて同名の変数が存在していたとして、 それらは関数実行時に変更されることはありません。

::::::::::::::::::::::::::::::::::::::::::::::::::


``` r
  gdp <- dat$pop * dat$gdpPercap
  new <- cbind(dat, gdp=gdp)
  return(new)
}
```

Finally, we calculated the GDP on our new subset, and created a new data frame
with that column added. 最終的に、絞り込んだデータからGDPを計算し、その結果を列に追加した新しいデータフレームを作成しました。 これは関数を呼び出した後でも返り値のGDPの値が持つ文脈がわかることを意味します。 従って、最初に試した数値のベクトルを返す方法よりもずっと良いものです。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ４

GDP を計算する関数をテストするため、1987年の New Zealand の GDP を計算して下さい。 1952 年の New Zealand の GDP とはどう違いますか？

:::::::::::::::  solution

## チャレンジ８の解答


``` r
  calcGDP(gapminder, year = c(1952, 1987), country = "New Zealand")
```

GDP for New Zealand in 1987: 65050008703

GDP for New Zealand in 1952: 21058193787

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ５

The `paste()` function can be used to combine text together, e.g:


``` r
best_practice <- c("Write", "programs", "for", "people", "not", "computers")
paste(best_practice, collapse=" ")
```

``` output
[1] "Write programs for people not computers"
```

Write a function called `fence()` that takes two vectors as arguments, called
`text` and `wrapper`, and prints out the text wrapped with the `wrapper`:


``` r
fence(text=best_practice, wrapper="***")
```

_Note:_ the `paste()` function has an argument called `sep`, which specifies
the separator between text. The default is a space: " ". The default for
`paste0()` is no space "".

:::::::::::::::  solution

## チャレンジ８の解答

Write a function called `fence()` that takes two vectors as arguments,
called `text` and `wrapper`, and prints out the text wrapped with the
`wrapper`:


``` r
fence <- function(text, wrapper){
  text <- c(wrapper, text, wrapper)
  result <- paste(text, collapse = " ")
  return(result)
}
best_practice <- c("Write", "programs", "for", "people", "not", "computers")
fence(text=best_practice, wrapper="***")
```

``` output
[1] "*** Write programs for people not computers ***"
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント

R より複雑な演算を行う時に利用できる変わった機能があります。 ここでは発展的な概念を知っておく必要のあることは書きません。 将来的に R で関数を書くことに慣れたら、 [R Language Manual][man] や Hadley Wickham による [Advanced R Programming][adv-r] のこの\[章]\[]を読んで学んで下さい。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Tip: テストとドキュメント

It's important to both test functions and document them:
Documentation helps you, and others, understand what the
purpose of your function is, and how to use it, and its
important to make sure that your function actually does
what you think.

When you first start out, your workflow will probably look a lot
like this:

1. Write a function
2. Comment parts of the function to document its behaviour
3. Load in the source file
4. Experiment with it in the console to make sure it behaves
   as you expect
5. Make any necessary bug fixes
6. Rinse and repeat.

Formal documentation for functions, written in separate `.Rd`
files, gets turned into the documentation you see in help
files. The [roxygen2] package allows R coders to write documentation
alongside the function code and then process it into the appropriate `.Rd`
files. You will want to switch to this more formal method of writing
documentation when you start writing more complicated R projects. In fact,
packages are, in essence, bundles of functions with this formal documentation.
Loading your own functions through `source("functions.R")` is equivalent to
loading someone else's functions (or your own one day!) through
`library("package")`.

Formal automated tests can be written using the [testthat] package.

::::::::::::::::::::::::::::::::::::::::::::::::::

[man]: https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Environment-objects
[chapter]: https://adv-r.had.co.nz/Environments.html
[adv-r]: https://adv-r.had.co.nz/
[roxygen2]: https://cran.r-project.org/web/packages/roxygen2/vignettes/rd.html
[testthat]: https://r-pkgs.had.co.nz/tests.html

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `function` to define a new function in R.
- Use parameters to pass values into functions.
- Use `stopifnot()` to flexibly check function arguments in R.
- Load functions into programs using `source()`.

::::::::::::::::::::::::::::::::::::::::::::::::::
