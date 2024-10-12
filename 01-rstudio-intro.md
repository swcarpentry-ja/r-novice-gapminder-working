---
title: RとRStudio入門
teaching: 45
exercises: 10
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- RStudio IDE の各ウィンドウの使用目的と使い方が説明出来るようになりましょう。
- RStudio IDE のボタンやオプションの位置を理解しましょう。
- 変数が定義出来るようになりましょう。
- 変数に値の設定が出来るようになりましょう。
- R セッションのワークスペース管理が出来るようになりましょう。
- 算術演算子や比較演算子が使えるようになりましょう。
- 関数が呼び出せるようになりましょう。
- パーッケージをロードしましょう。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- RStudio はどのように操作したらよいですか？
- R とはどのようにやりとりしたらよいですか？
- 環境の管理はどうしたらよいですか？
- パッケージのインストールはどうしたらよいですか？

::::::::::::::::::::::::::::::::::::::::::::::::::



## ワークショップを始める前に

RとRStudioの最新バージョンが、自分のコンピューターにインストールされているか確認してください。 最新バージョンであることが重要である理由は、ワークショップで使うパッケージには、Rが最新でないと、正常に（または全く）インストールされないものがあるからです。

- [ここからRの最新バージョンをダウンロード及びインストール下さい](https://www.r-project.org/)
- [ここからRStudioをダウンロード及びインストール下さい](https://www.rstudio.com/products/rstudio/download/#download)

## R とRStudio を使う理由

Software CarpentryのR部分のワークショップへようこそ。

科学は複数の段階からなる作業です：実験を計画し、データを収集したら、 そこから本当の楽しみが始まります！ このレッスンでは、R言語の基礎と、知っていると後々かなり楽になる科学的プロジェクトのためにコードを整える最適な方法をお教えます。

Microsoft Excel やGoogle スプレッドシートを使用してデータを分析することも可能ですが、これらのツールは柔軟性とアクセス性に限界があります。 特に、生データの探索や変更を行うステップを共有するのが難しいため、[「再現可能」](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285)な研究には不向きです。

そのため、このレッスンではR とRStudio を使用してデータの探索を始める方法を学びます。 R プログラムはWindows、Mac、Linux のオペレーティングシステムで利用でき、無料で上記からダウンロード可能です。 R を実行するにはR プログラムだけで十分です。

ただし、R の使用をより簡単にするために、上記でダウンロードしたRStudio を使用します。 RStudio is a free, open-source, Integrated Development
Environment (IDE). このレッスンではRStudioを使います。RStudioは、無料であり、Rを組み込んだオープンソースの 総合開発環境（IDE: Integrated Development Environment）です。RStudioは、全てのプラットフォーム（サーバーも含む）で起動できること、 エディタが組み込まれていることや、プロジェクト管理やバージョン管理にも対応しているなど、 良いところがいっぱいがあります。

## 概要

We will begin with raw data, perform exploratory analyses, and learn how to plot results graphically. This example starts with a dataset from [gapminder.org](https://www.gapminder.org) containing population information for many
countries through time. Can you read the data into R? Can you plot the population for
Senegal? Can you calculate the average income for countries on the continent of Asia?
By the end of these lessons you will be able to do things like plot the populations
for all of these countries in under a minute!

**基本的なレイアウト**

最初にRStudioを開くと、３つのパネルが現れます：

- インタラクティブなRコンソール（左側全部）
- 環境／履歴（右上のタブ形式）
- Files/Plots/Packages/Help/Viewer（右下のタブ形式）

![](fig/01-rstudio.png){alt='RStudio layout'}

Rスクリプトなどのファイルを開くと、左上にエディタパネルも開きます。

![](fig/01-rstudio-script.png){alt='RStudio layout with .R file open'}

:::::::::::::::::::::::::::::::::::::::::  callout

## R scripts

Any commands that you write in the R console can be saved to a file
to be re-run again.  Files containing R code to be ran in this way are
called R scripts.  R scripts have `.R` at the end of their names to
let you know what they are.

::::::::::::::::::::::::::::::::::::::::::::::::::

## RStudioでの作業の流れ

RStudioで作業する２つの主な方法：

1. インタラクティブなRコンソールで試したり、確認したりした後、コードを.

- This works well when doing small tests and initially starting off.
- It quickly becomes laborious

2. Start writing in a .R file and use RStudio's short cut keys for the Run command
   to push the current line, selected lines or modified lines to the
   interactive R console.

- This is a great way to start; all your code is saved for later
- You will be able to run the file you create from within RStudio
  or using R's `source()`  function.

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：ひとかたまりのコードを走らせる

RStudio offers you great flexibility in running code from within the editor
window. There are buttons, menu choices, and keyboard shortcuts. To run the
current line, you can

1. click on the `Run` button above the editor panel, or
2. select "Run Lines" from the "Code" menu, or
3. hit <kbd>Ctrl</kbd>\+<kbd>Return</kbd> in Windows or Linux
   or <kbd>⌘</kbd>\+<kbd>Return</kbd> on OS X.
   (This shortcut can also be seen by hovering
   the mouse over the button). To run a block of code, select it and then `Run`.
   If you have modified a line of code within a block of code you have just run,
   there is no need to reselect the section and `Run`, you can use the next button
   along, `Re-run the previous region`. This will run the previous code block
   including the modifications you have made.

::::::::::::::::::::::::::::::::::::::::::::::::::

## R入門

Rを使う時間のほとんどは、Rのインタラクティブコンソールでの作業となるでしょう。 ここが、全てのコードを走らせる、また、.Rファイルにコードを加える前にアイディアを 試してみるのに使える環境となります。 このRStudioのコンソールが、コマンドライン環境に`R` と入力するのと同じ環境になります。

Rインタラクティブセッションでまず目に入ってくるのは、ひとかたまりの情報と その後に続く、「 」と点滅するカーソルです。これは多くの観点で、 シェルのレッスンで学んだシェル環境と似ています。 つまり、「読み、実行し、出力するループ」の考えと同じように動くので、 コマンドを入力すれば、Rは、それを実行しようとし、そして結果を返すのです。

## Rを計算機としての使用

一番単純なRの使い方は、演算です：


``` r
1 + 100
```

``` output
[1] 101
```

And R will print out the answer, with a preceding "[1]". [1] is the index of
the first element of the line being printed in the console. For more information
on indexing vectors, see [Episode 6: Subsetting Data](https://swcarpentry.github.io/r-novice-gapminder/06-data-subsetting/index.html).

バッシュのように、Rは、不完全なコマンドが入力されると、それが完成されるまで待ちます： If you are familiar with Unix Shell's bash, you may recognize this behavior from bash.

```r
> 1 +
```

```output
+
```

Any time you hit return and the R session shows a "+" instead of a ">", it
means it's waiting for you to complete the command. If you want to cancel
a command you can hit <kbd>Esc</kbd> and RStudio will give you back the ">" prompt.

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：コマンドの取り消し

RStudioではなく、コマンドラインからRを使う場合、 コマンドを取り消す場合、<kbd Esc</kbd の代わりに<kbd Ctrl</kbd +<kbd C</kbd を使う必要があります。これは、Macユーザーも同じです！ コマンドの取り消しは、不完全なコマンドを消す他にも使えます。

走っているコードを止めてとRに伝えるにも使え（例えば、予想よりも かなり長くかかっている場合）、その他にも、今書いているコードを 消去するにも使えます。

::::::::::::::::::::::::::::::::::::::::::::::::::

Rを計算機として使う場合、演算の順番は、学校で学んだだろうものと 同じです。

一番先に行われる処理から一番後に行われる処理：

- 括弧： `(`、`)`
- 累乗：`^`か` `
- 掛ける：` `
- 割る：`/`
- 加える：`+`
- 引く：`-`


``` r
3 + 5 * 2
```

``` output
[1] 13
```

デフォルトと違う順番で計算させたい場合や、意図を明確にしたい場合は、 演算をまとめるために、括弧を使いましょう。


``` r
(3 + 5) * 2
```

``` output
[1] 16
```

括弧をつける必要ではないときは、面倒かもしれませんが、そうすることで自分の意図 するところがはっきり伝わります。
自分が書いたコードを、後々他の人が読むことになるかもしれないことを忘れないように。


``` r
(3 + (5 (2 ^ 2))) # 読みづらい 3 + 5 2 ^ 2 # ルールを覚えている人には明確 3 + 5 (2 ^ 2) # ルールを忘れている人にも伝わるはず
```

コードの後の文は、「コメント」と呼ばれます。 シャープ（ナンバー）記号`#` の後に来るものは、Rがコードを実行する際は無視されます。

とても大きい、又は小さい数には、指数表記が使われます：


``` r
2/10000
```

``` output
[1] 2e-04
```

Which is shorthand for "multiplied by `10^XX`". So `2e-4`
is shorthand for `2 * 10^(-4)`.

数を指数記号で書くこともできます：


``` r
5e3 # マイナスがないことに注意
```

``` output
[1] 5000
```

## 数学関数

R has many built in mathematical functions. To call a function,
we can type its name, followed by open and closing parentheses.
Functions take arguments as inputs, anything we type inside the parentheses of a function is considered an argument. Depending on the function, the number of arguments can vary from none to multiple. 例えば：


``` r
getwd() #returns an absolute filepath
```

doesn't require an argument, whereas for the next set of mathematical functions we will need to supply the function a value in order to compute the result.


``` r
sin(1) # 三角関数
```

``` output
[1] 0.841471
```


``` r
log(1) # 自然対数
```

``` output
[1] 0
```


``` r
log10(10) # 10を底とする対数
```

``` output
[1] 1
```


``` r
exp(0.5) # e^(1/2)
```

``` output
[1] 1.648721
```

Googleで検索すればいいですし、関数の始まりさえ覚えていれば RStudioのタブ入力補完機能が使えます。

これがRStudioが、Rそのものを使うよりもいい理由のひとつです。 RStudioには、自動補完機能があり、関数、その引数、その取りうる値を より簡単に見つけることができます。

コマンド名の前に、`?` を付けることで、そのコマンドのヘルプのページを開くことができます。 When using RStudio, this will open the 'Help' pane;
if using R in the terminal, the help page will open in your browser.
The help page will include a detailed description of the command and
how it works. Scrolling to the bottom of the help page will usually
show a collection of code examples which illustrate command usage.
これについては、後ほど、例で見てみることにしましょう。

## 何かを比較する

Rを使って比較をすることもできます：


``` r
~~~ 1 == 1 # 等価（イコールが２つあることに注意。「は、～と等しい」と読む） ~~~
```

``` output
~~~1 == 1
```


``` r
~~~ 1 != 2 # 不等価（「は、～と等しくない」と読む） ~~~
```

``` output
~~~1 != 2
```


``` r
1 < 2 # より少ない
```

``` output
[1] TRUE
```


``` r
1 <= 1 # より少ない、又は等しい
```

``` output
[1] TRUE
```


``` r
1 0 # より大きい
```

``` error
Error in parse(text = input): <text>:1:3: unexpected numeric constant
1: 1 0
      ^
```


``` r
1 = -9 # より大きい、又は等しい
```

``` error
Error in 1 = -9: invalid (do_set) left-hand side to assignment
```

:::::::::::::::::::::::::::::::::::::::::  callout

## ##ヒント： 数の比較

A word of warning about comparing numbers: you should
never use `==` to compare two numbers unless they are
integers (a data type which can specifically represent
only whole numbers).

Computers may only represent decimal numbers with a
certain degree of precision, so two numbers which look
the same when printed out by R, may actually have
different underlying representations and therefore be
different by a small margin of error (called Machine
numeric tolerance).

Instead you should use the `all.equal` function.

Further reading: [http://floating-point-gui.de/](https://floating-point-gui.de/)

::::::::::::::::::::::::::::::::::::::::::::::::::

## 変数及び代入

代入演算子`<-`を用いて、次のように値を変数に入れることができます：


``` r
x <- 1/40
```

Notice that assignment does not print a value. Instead, we stored it for later
in something called a **variable**. `x` now contains the **value** `0.025`:


``` r
x
```

``` output
[1] 0.025
```

より正確には、記録された値は、[浮動小数点数](https://en.wikipedia.org/wiki/Floating_point) と呼ばれる 小数近似 (decimal approximation) です。

Look for the `Environment` tab in the top right panel of RStudio, and you will see that `x` and its value
have appeared. Our variable `x` can be used in place of a number in any calculation that expects a number:


``` r
log(x)
```

``` output
[1] -3.688879
```

変数は、再度、代入することもできます：


``` r
x <- 100
```

`x`は、0.025という値でしたが、今は、100になりました。

代入する値は、既に代入されている変数でもいいです：


``` r
x <- x + 1 #RStudioが、右上のタブにあるxの詳細をどう更新したかにも御留意下さい y <- x 2
```

代入の右側については、Rで使える表現であれば何でも大丈夫です。
右側については、代入される前に、 計算が完全に実施 されます。

変数名には、文字、数字、下線、ピリオドを含むことができます。 They
must start with a letter or a period followed by a letter (they cannot start with a number nor an underscore).
Variables beginning with a period are hidden variables.
長い変数名のつけ方は様々です。例えば、

- ピリオドを.単語の.間に入れる
- 下線を\\_単語の_間に入れる
- 単語の始まりを大文字にする（camelCaseToSeparateWords）

何を使うかは書き手次第ですが、 一貫性を持たせる ようにしましょう。

代入の演算子として、`=`を使うこともできます：


``` r
x = 1/40
```

But this is much less common among R users.  The most important thing is to
**be consistent** with the operator you use. There are occasionally places
where it is less confusing to use `<-` than `=`, and it is the most common
symbol used in the community. So the recommendation is to use `<-`.

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ１

次のうちどれがRの変数名として使えますか？


``` r
min_height
max.height
_age
.mass
MaxLength
min-length
2widths
celsius2kelvin
```

:::::::::::::::  solution

## チャレンジ８の解答 1

The following can be used as R variables:


``` r
min_height
max.height
MaxLength
celsius2kelvin
```

The following creates a hidden variable:


``` r
.mass
```

The following will not be able to be used to create a variable


``` r
_age
min-length
2widths
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## ベクトル化

One final thing to be aware of is that R is _vectorized_, meaning that
variables and functions can have vectors as values. In contrast to physics and
mathematics, a vector in R describes a set of values in a certain order of the
same data type. 例えば：


``` r
1:5
```

``` output
[1] 1 2 3 4 5
```

``` r
2^(1:5)
```

``` output
[1]  2  4  8 16 32
```

``` r
x <- 1:5
2^x
```

``` output
[1]  2  4  8 16 32
```

これは、とても使えるのですが、詳しくは後続のレッスンで述べることにします。

## 環境を管理する

Rセッションで使えるコマンドがいくつかあります。

`ls`は、グローバル環境（作業中のRセッション）にある全ての変数と関数をリスト化します：


``` r
ls()
```


``` error
Error: object 'y' not found
```

``` output
[1] "x"
```

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：隠れたオブジェクト

シェルのように、`ls`はデフォルトでは、"."で始まる変数と関数を表示しません。 全てのオブジェクトをリスト化するには、代わりに`ls(all.names=TRUE)`と書きます

::::::::::::::::::::::::::::::::::::::::::::::::::

ここでは、`ls`に特に引数を与えませんでしたが、それでもRに関数を 呼び出していることを伝えるために括弧は必要です。

If we type `ls` by itself, R prints a bunch of code instead of a listing of objects.


``` r
ls
```

``` output
function (name, pos = -1L, envir = as.environment(pos), all.names = FALSE, 
    pattern, sorted = TRUE) 
{
    if (!missing(name)) {
        pos <- tryCatch(name, error = function(e) e)
        if (inherits(pos, "error")) {
            name <- substitute(name)
            if (!is.character(name)) 
                name <- deparse(name)
            warning(gettextf("%s converted to character string", 
                sQuote(name)), domain = NA)
            pos <- name
        }
    }
    all.names <- .Internal(ls(envir, all.names, sorted))
    if (!missing(pattern)) {
        if ((ll <- length(grep("[", pattern, fixed = TRUE))) && 
            ll != length(grep("]", pattern, fixed = TRUE))) {
            if (pattern == "[") {
                pattern <- "\\["
                warning("replaced regular expression pattern '[' by  '\\\\['")
            }
            else if (length(grep("[^\\\\]\\[<-", pattern))) {
                pattern <- sub("\\[<-", "\\\\\\[<-", pattern)
                warning("replaced '[<-' by '\\\\[<-' in regular expression pattern")
            }
        }
        grep(pattern, all.names, value = TRUE)
    }
    else all.names
}
<bytecode: 0x557fc6ecee50>
<environment: namespace:base>
```

What's going on here?

Like everything in R, `ls` is the name of an object, and entering the name of
an object by itself prints the contents of the object. The object `x` that we
created earlier contains 1, 2, 3, 4, 5:


``` r
x
```

``` output
[1] 1 2 3 4 5
```

The object `ls` contains the R code that makes the `ls` function work! We'll talk
more about how functions work and start writing our own later.

もう必要ないオブジェクトを消去するには、`rm`が使えます：


``` r
rm(x)
```

もし、環境に、色々なものがいっぱいあって、それらを全て消去したい場合、 `ls`の結果を`rm`関数に渡すことで対応できます。：


``` r
rm(list = ls())
```

In this case we've combined the two. ここでは、二つを組み合わせました。演算の順番のように、一番内側の括弧の中のものが、 まず最初に実行され、実行が続きます。

ここでは、`ls`の結果を`rm`の引数のリストとして用いるべしと設定しました。 名前で引数の値を定めるときは、演算子`=`を 必ず 使わなければなりません！

もし代わりに`<-`を使うと、意図しない副作用が現れるか、エラーメッセージが現れます：


``` r
rm(list <- ls())
```

``` error
Error in rm(list <- ls()): ... must contain names or character strings
```

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：警告かエラーか

Pay attention when R does something unexpected! Errors, like above,
are thrown when R cannot proceed with a calculation. Warnings on the
other hand usually mean that the function has run, but it probably
hasn't worked as expected.

In both cases, the message that R prints out usually give you clues
how to fix a problem.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Rパッケージ

It is possible to add functions to R by writing a package, or by
obtaining a package written by someone else. As of this writing, there
are over 10,000 packages available on CRAN (the comprehensive R archive
network). R and RStudio have functionality for managing packages:

- インストールされているパッケージを以下を入力することで見ることができます。
- You can install packages by typing `install.packages("packagename")`,
  where `packagename` is the package name, in quotes.
- You can update installed packages by typing `update.packages()`
- You can remove a package with `remove.packages("packagename")`
- You can make a package available for use with `library(packagename)`

Packages can also be viewed, loaded, and detached in the Packages tab of the lower right panel in RStudio. Clicking on this tab will display all of the installed packages with a checkbox next to them. If the box next to a package name is checked, the package is loaded and if it is empty, the package is not loaded. Click an empty box to load that package and click a checked box to detach that package.

Packages can be installed and updated from the Package tab with the Install and Update buttons at the top of the tab.

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ２

次のプログラムのそれぞれの宣言の後、それぞれの変数の値は 何になるでしょうか？


``` r
mass <- 47.5
age <- 122
mass <- mass * 2.3
age <- age - 20
```

:::::::::::::::  solution

## チャレンジ８の解答


``` r
mass <- 47.5
```

This will give a value of 47.5 for the variable mass


``` r
age <- 122
```

This will give a value of 122 for the variable age


``` r
mass <- mass * 2.3
```

This will multiply the existing value of 47.5 by 2.3 to give a new value of
109.25 to the variable mass.


``` r
age <- age - 20
```

This will subtract 20 from the existing value of 122 to give a new value
of 102 to the variable age.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ３

チャレンジ２のコードを走らせ、massとageを比較するコマンドを書いて下さい。 massはageよりも大きいでしょうか？

:::::::::::::::  solution

## チャレンジ３の解答

One way of answering this question in R is to use the `>` to set up the following:


``` r
mass > age
```

``` output
[1] TRUE
```

This should yield a boolean value of TRUE since 109.25 is greater than 102.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ４

Clean up your working environment by deleting the mass and age
variables.

:::::::::::::::  solution

## チャレンジ８の解答

We can use the `rm` command to accomplish this task


``` r
rm(age, mass)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ５

Install the following packages: `ggplot2`, `plyr`, `gapminder`

:::::::::::::::  solution

## チャレンジ８の解答

We can use the `install.packages()` command to install the required packages.


``` r
install.packages("ggplot2")
install.packages("plyr")
install.packages("gapminder")
```

An alternate solution, to install multiple packages with a single `install.packages()` command is:


``` r
install.packages(c("ggplot2", "plyr", "gapminder"))
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: instructor

When installing ggplot2, it may be required for some users to use the dependencies flag as a result of lazy loading affecting the install. This suggestion is not tied to any known bug discussion, and is advised based off instructor feedback/experience in resolving stochastic occurences of errors identified through delivery of this workshop:


``` r
install.packages("ggplot2", dependencies = TRUE)
```

:::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use RStudio to write and run R programs.
- R has the usual arithmetic operators and mathematical functions.
- Use `<-` to assign values to variables.
- Use `ls()` to list the variables in a program.
- Use `rm()` to delete objects in a program.
- Use `install.packages()` to install packages (libraries).

::::::::::::::::::::::::::::::::::::::::::::::::::
