---
title: データの出力
teaching: 10
exercises: 10
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- To be able to write out plots and data from R.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I save plots and data created in R?

::::::::::::::::::::::::::::::::::::::::::::::::::



## Saving plots

`ggsave` 関数を使って、`ggplot2` を用いて作った直近の図を保存する方法は既に学んだ通りです。 おさらいしておきましょう。


``` r
ggsave("My_most_recent_plot.pdf")
```

RStudio から図を保存するには "Plot" ウィンドウにある "Export" ボタンを使います。 この機能を使うと、図を .pdf や .png、.jpg など様々な画像形式で保存することができます。

図を "Plot" ウィンドウに表示することなく保存したいこともあるでしょう。 ページごとに一つの図を含むような複数のページから成る PDF を出力したいこともあるでしょう。 あるいはループを使ってファイル中のデータの絞り込み条件を変えながら複数の図を作成していて、 一つずつ保存するためにいちいちループを止めて "Export" ボタンを押すことが不可能な場合もあるでしょう。

このような場合、より柔軟な方法を使います。 `pdf` 関数は新たに PDF デバイスを作ります。 この関数に引数を指定することで、サイズや解像度を調整できます。


``` r
~~~ pdf("Life_Exp_vs_time.pdf", width=12, height=4) ggplot(data=gapminder, aes(x=year, y=lifeExp, colour=country)) + geom_line() + theme(legend.position = "none") # PDF デバイスを確実に終了しておく必要があります！
```

このドキュメントを開いて内容を確認してみましょう。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ１

Rewrite your 'pdf' command to print a second
page in the pdf, showing a facet plot (hint: use `facet_grid`)
of the same data with one panel per continent.

:::::::::::::::  solution

## チャレンジ８の解答 1


``` r
pdf("Life_Exp_vs_time.pdf", width = 12, height = 4)
p <- ggplot(data = gapminder, aes(x = year, y = lifeExp, colour = country)) +
  geom_line() +
  theme(legend.position = "none")
p
p + facet_grid(~continent)
dev.off()
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

同様にして `jepg` や `png` などのコマンドを使うことで、他の形式のドキュメントを作成できます。

## データの出力

R からデータを出力したいこともあります。

これには、既に使った `read.table` 関数とよく似た `write.table` 関数を使います。

データを綺麗にするスクリプトを書きましょう。 今回の分析では、gapminder データの中のオーストラリアに該当する部分だけを使います。


``` r
aust_subset <- gapminder[gapminder$country == "Australia",]

write.table(aust_subset,
  file="cleaned-data/gapminder-aus.csv",
  sep=","
)
```

シェルに戻ってデータが正しく出力されているか確認しましょうOK:


``` bash
head cleaned-data/gapminder-aus.csv
```

``` output
"country","year","pop","continent","lifeExp","gdpPercap"
"61","Australia",1952,8691212,"Oceania",69.12,10039.59564
"62","Australia",1957,9712569,"Oceania",70.33,10949.64959
"63","Australia",1962,10794968,"Oceania",70.93,12217.22686
"64","Australia",1967,11872264,"Oceania",71.1,14526.12465
"65","Australia",1972,13177000,"Oceania",71.93,16788.62948
"66","Australia",1977,14074100,"Oceania",73.49,18334.19751
"67","Australia",1982,15184200,"Oceania",74.74,19477.00928
"68","Australia",1987,16257249,"Oceania",76.32,21888.88903
"69","Australia",1992,17481977,"Oceania",77.56,23424.76683
```

Hmm, that's not quite what we wanted. Where did all these
quotation marks come from? Also the row numbers are
meaningless.

ヘルプファイルを見てこの動作を変更しましょう。


``` r
?write.table
```

既定では、R は文字列をファイルに出力する時に引用符で囲みます。 また、行と列の名前も出力します。

以下のように修正しましょう。


``` r
write.table(
  gapminder[gapminder$country == "Australia",],
  file="cleaned-data/gapminder-aus.csv",
  sep=",", quote=FALSE, row.names=FALSE
)
```

もう一度シェルを使ってデータを確認してみましょう。


``` bash
head cleaned-data/gapminder-aus.csv
```

``` output
country,year,pop,continent,lifeExp,gdpPercap
Australia,1952,8691212,Oceania,69.12,10039.59564
Australia,1957,9712569,Oceania,70.33,10949.64959
Australia,1962,10794968,Oceania,70.93,12217.22686
Australia,1967,11872264,Oceania,71.1,14526.12465
Australia,1972,13177000,Oceania,71.93,16788.62948
Australia,1977,14074100,Oceania,73.49,18334.19751
Australia,1982,15184200,Oceania,74.74,19477.00928
Australia,1987,16257249,Oceania,76.32,21888.88903
Australia,1992,17481977,Oceania,77.56,23424.76683
```

よくなりました！

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ２

gapminder データを1990年以降のデータのみに絞り込むスクリプトを書いて下さい。

このスクリプトを使って絞り込んだ結果を `cleaned-data/` ディレクトリ中のファイルに出力しましょう。

:::::::::::::::  solution

## チャレンジ８の解答


``` r
チャレンジ1990の解答 ~~~ write.table( gapminder[gapminder$year , ], file = "cleaned-data/gapminder-after1990.csv", sep = ",", quote = FALSE, row.names = FALSE)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Save plots from RStudio using the 'Export' button.
- Use `write.table` to save tabular data.

::::::::::::::::::::::::::::::::::::::::::::::::::
