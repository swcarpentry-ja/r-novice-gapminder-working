---
title: Seeking Help
teaching: 10
exercises: 10
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- To be able to read R help files for functions and special operators.
- To be able to use CRAN task views to identify packages to solve a problem.
- To be able to seek help from your peers.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I get help in R?

::::::::::::::::::::::::::::::::::::::::::::::::::



## ヘルプファイルを読む

R、そしてその他のパッケージには、関数のヘルプファイルが用意されています。 一般的な関数のヘルプを検索する構文は、（インタラクティブなRセッションで）自分の名前空間に読み込まれた パッケージに存在する特定の関数名、「function_name」を以下のように使って下さい：


``` r
?function_name
help(function_name)
```

For example take a look at the help file for `write.table()`, we will be using a similar function in an upcoming episode.


``` r
?write.table()
```

これで、RStudioにヘルプページ（Rの場合は、画面に簡素なテキスト）が表示されます

それぞれのヘルプページは、セクションに分けられます：

- - Description（説明）：関数がすることの詳しい説明
- - Usage（使い方）：関数の引数及びデフォルトの値
- - Arguments（引数）：それぞれの引数が取りうるデータの説明
- - Details（詳細）：気を付けるべき重要な詳細
- - Value（値）：関数が返すデータ
- - See Also（ついでにこっちも）：他に役立ちそうな関連する関数
- - Examples（例）：関数の使い方の例

関数によって、違うセクションがあるかもしれませんが、知っておくべき主要なものは以上でしょう。

Notice how related functions might call for the same help file:


``` r
?write.table()
?write.csv()
```

This is because these functions have very similar applicability and often share the same arguments as inputs to the function, so package authors often choose to document them together in a single help file.

:::::::::::::::::::::::::::::::::::::::::  callout

## Tip: Running Examples

From within the function help page, you can highlight code in the
Examples and hit <kbd>Ctrl</kbd>\+<kbd>Return</kbd> to run it in
RStudio console. This gives you a quick way to get a feel for
how a function works.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：ヘルプファイルを読む

Rの気が滅入る点は、多くの関数があるという点です。 自分が使う全ての関数の正しい使い方を覚えるのは不可能とまでは言いませんが、かなり大変でしょう。 Luckily, using the help files
means you don't have to remember that!

::::::::::::::::::::::::::::::::::::::::::::::::::

## 特別な演算子

特別な演算子を検索するためには、引用符を使いましょう：


``` r
?"<-"
?`<-`
```

## パッケージについてのヘルプを入手する

Many packages come with "vignettes": tutorials and extended example documentation.
多くのパッケージには、「ビニエット（vignette）」があります。これは、使い方の説明や詳細な例を掲載する資料です。 引数なしに、`vignette()` と入力すると、インストールされているパッケージの全てのビニエットの一覧が表示されます。 `vignette(package="package-name")` と入力すると、`package-name`についての全てのビニエットの一覧が表示され、 `vignette("vignette-name")` と入力すると、指定されたビニエットが開きます。

もしパッケージにビニエットがひとつもない場合、通常 `help("package-name")` と入力するとヘルプが表示されます。

RStudio also has a set of excellent
[cheatsheets](https://rstudio.com/resources/cheatsheets/) for many packages.

## When You Remember Part of the Function Name

関数がどのパッケージに入っているか、またはスペルが定かではないとき、あいまい検索をすることもできます：


``` r
??function_name
```

A fuzzy search is when you search for an approximate string match. For example, you may remember that the function
to set your working directory includes "set" in its name. You can do a fuzzy search to help you identify the function:


``` r
??set
```

## どうしていいか分からない場合

関数やパッケージが分からない場合、[CRAN Task Views](https://cran.at.r-project.org/web/views)という 分野ごとにまとめられたパッケージのリストがあります。 このページから探し始めるといいかもしれません。

## コードがうまく動作しない：仲間の助けが必要な場合

関数がうまく動かない場合、十中八九、探している問の答えは、 [Stack Overflow](https://stackoverflow.com/)に掲載されいます。 `[r]` タグを使って検索ができます。 Please make sure to see their page on
[how to ask a good question.](https://stackoverflow.com/help/how-to-ask)

もし答えが見つからない場合、仲間に質問をする際に 役に立つ関数がいくつかあります：


``` r
?dput
```

これで、あなたが使っているデータを誰にでもコピー・アンド・ペーストしてRセッションで使える 形式にすることができます。


``` r
sessionInfo()
```

``` output
R version 4.4.1 (2024-06-14)
Platform: x86_64-pc-linux-gnu
Running under: Ubuntu 22.04.5 LTS

Matrix products: default
BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.10.0 
LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.10.0

locale:
 [1] LC_CTYPE=C.UTF-8       LC_NUMERIC=C           LC_TIME=C.UTF-8       
 [4] LC_COLLATE=C.UTF-8     LC_MONETARY=C.UTF-8    LC_MESSAGES=C.UTF-8   
 [7] LC_PAPER=C.UTF-8       LC_NAME=C              LC_ADDRESS=C          
[10] LC_TELEPHONE=C         LC_MEASUREMENT=C.UTF-8 LC_IDENTIFICATION=C   

time zone: UTC
tzcode source: system (glibc)

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

loaded via a namespace (and not attached):
 [1] assertthat_0.2.1 R6_2.5.1         xfun_0.47        magrittr_2.0.3  
 [5] glue_1.8.0       knitr_1.48       sandpaper_0.16.8 lifecycle_1.0.4 
 [9] xml2_1.3.6       ps_1.8.0         cli_3.6.3        processx_3.8.4  
[13] callr_3.7.6      vctrs_0.6.5      renv_1.0.10      withr_3.0.1     
[17] compiler_4.4.1   purrr_1.0.2      tools_4.4.1      tinkr_0.2.0.9000
[21] evaluate_1.0.0   yaml_2.3.10      pegboard_0.7.6   rlang_1.1.4     
```

これは、現在使っている R のバージョン、そして読み込まれている全てのパッケージを表示させる関数です。 他の人が問題点を再現し、バグを見つける際にこの情報が役立つこともあります。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ１

Look at the help page for the `c` function. What kind of vector do you
expect will be created if you evaluate the following:


``` r
c(1, 2, 3)
c('d', 'e', 'f')
c(1, 2, 'f')
```

:::::::::::::::  solution

## チャレンジ３の解答

The `c()` function creates a vector, in which all elements are of the
same type. In the first case, the elements are numeric, in the
second, they are characters, and in the third they are also characters:
the numeric values are "coerced" to be characters.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ２

Look at the help for the `paste` function. You will need to use it later.
What's the difference between the `sep` and `collapse` arguments?

:::::::::::::::  solution

## チャレンジ３の解答

To look at the help for the `paste()` function, use:


``` r
help("paste")
?paste
```

The difference between `sep` and `collapse` is a little
tricky. The `paste` function accepts any number of arguments, each of which
can be a vector of any length. The `sep` argument specifies the string
used between concatenated terms — by default, a space. The result is a
vector as long as the longest argument supplied to `paste`. In contrast,
`collapse` specifies that after concatenation the elements are _collapsed_
together using the given separator, the result being a single string.

It is important to call the arguments explicitly by typing out the argument
name e.g `sep = ","` so the function understands to use the "," as a
separator and not a term to concatenate.
e.g.


``` r
paste(c("a","b"), "c")
```

``` output
[1] "a c" "b c"
```

``` r
paste(c("a","b"), "c", ",")
```

``` output
[1] "a c ," "b c ,"
```

``` r
paste(c("a","b"), "c", sep = ",")
```

``` output
[1] "a,c" "b,c"
```

``` r
paste(c("a","b"), "c", collapse = "|")
```

``` output
[1] "a c|b c"
```

``` r
paste(c("a","b"), "c", sep = ",", collapse = "|")
```

``` output
[1] "a,c|b,c"
```

(For more information,
scroll to the bottom of the `?paste` help page and look at the
examples, or try `example('paste')`.)

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ３

Use help to find a function (and its associated parameters) that you could
use to load data from a tabular file in which columns are delimited with "\\t"
(tab) and the decimal point is a "." (period). This check for decimal
separator is important, especially if you are working with international
colleagues, because different countries have different conventions for the
decimal point (i.e. comma vs period).
Hint: use `??"read table"` to look up functions related to reading in tabular data.

:::::::::::::::  solution

## チャレンジ３の解答

The standard R function for reading tab-delimited files with a period
decimal separator is read.delim(). You can also do this with
`read.table(file, sep="\t")` (the period is the _default_ decimal
separator for `read.table()`), although you may have to change
the `comment.char` argument as well if your data file contains
hash (#) characters.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## その他の資料

- [Quick R](https://www.statmethods.net/)
- [RStudio cheat sheets](https://www.rstudio.com/resources/cheatsheets/)
- [Cookbook for R](https://www.cookbook-r.com/)

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `help()` to get online help in R.

::::::::::::::::::::::::::::::::::::::::::::::::::
