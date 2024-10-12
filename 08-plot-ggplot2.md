---
title: Creating Publication-Quality Graphics with ggplot2
teaching: 60
exercises: 20
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- To be able to use ggplot2 to generate publication-quality graphics.
- To apply geometry, aesthetic, and statistics layers to a ggplot plot.
- To manipulate the aesthetics of a plot using different colors, shapes, and lines.
- To improve data visualization through transforming scales and paneling by group.
- To save a plot created with ggplot to disk.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I create publication-quality graphics in R?

::::::::::::::::::::::::::::::::::::::::::::::::::



データをプロットすることは、データとその変数間の様々な関係をクイックに探索する最良の方法の一つです。

Rには、主に3つのプロットシステムがあります。 [R組み込みplot関数][base]、 [lattice][lattice] パッケージ、[ggplot2][ggplot2] パッケージです。

今回、私たちはggplot2パッケージについて学んでいきます。 なぜなら、ggplot2パッケージは出版品質並のグラフィック作成に最も効果的だからです。

ggplot2 is built on the grammar of graphics, the idea that any plot can be
built from the same set of components: a **data set**,
**mapping aesthetics**, and graphical **layers**:

- **Data sets** are the data that you, the user, provide.

- **Mapping aesthetics** are what connect the data to the graphics.
  They tell ggplot2 how to use your data to affect how the graph looks,
  such as changing what is plotted on the X or Y axis, or the size or
  color of different data points.

- **Layers** are the actual graphical output from ggplot2. Layers
  determine what kinds of plot are shown (scatterplot, histogram, etc.),
  the coordinate system used (rectangular, polar, others), and other
  important aspects of the plot. このアイディアは、Photoshop、Illustrator、Inkscapeなどの画像編集ソフトを使用する場面でお馴染みかもしれません。

Let's start off building an example using the gapminder data from earlier.
The most basic function is `ggplot`, which lets R know that we're
creating a new plot. Any of the arguments we give the `ggplot`
function are the _global_ options for the plot: they apply to all
layers on the plot.


``` r
library("ggplot2")
ggplot(data = gapminder)
```

<img src="fig/08-plot-ggplot2-rendered-blank-ggplot-1.png" alt="Blank plot, before adding any mapping aesthetics to ggplot()." style="display: block; margin: auto;" />

Here we called `ggplot` and told it what data we want to show on
our figure. This is not enough information for `ggplot` to actually
draw anything. It only creates a blank slate for other elements
to be added to.

Now we're going to add in the **mapping aesthetics** using the
`aes` function. `aes` tells `ggplot` how variables in the **data**
map to _aesthetic_ properties of the figure, such as which columns
of the data should be used for the **x** and **y** locations.


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp))
```

<img src="fig/08-plot-ggplot2-rendered-ggplot-with-aes-1.png" alt="Plotting area with axes for a scatter plot of life expectancy vs GDP, with no data points visible." style="display: block; margin: auto;" />

Here we told `ggplot` we want to plot the "gdpPercap" column of the
gapminder data frame on the x-axis, and the "lifeExp" column on the
y-axis. Notice that we didn't need to explicitly pass `aes` these
columns (e.g. `x = gapminder[, "gdpPercap"]`), this is because
`ggplot` is smart enough to know to look in the **data** for that column!

The final part of making our plot is to tell `ggplot` how we want to
visually represent the data. We do this by adding a new **layer**
to the plot using one of the **geom** functions.


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) +
  geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-vs-gdpPercap-scatter-1.png" alt="Scatter plot of life expectancy vs GDP per capita, now showing the data points." style="display: block; margin: auto;" />

Here we used `geom_point`, which tells `ggplot` we want to visually
represent the relationship between **x** and **y** as a scatterplot of points.

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ１

Modify the example so that the figure shows how life expectancy has
changed over time:


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) + geom_point()
```

Hint: the gapminder dataset has a column called "year", which should appear
on the x-axis.

:::::::::::::::  solution

## チャレンジ８の解答 1

Here is one possible solution:


``` r
ggplot(data = gapminder, mapping = aes(x = year, y = lifeExp)) + geom_point()
```

<div class="figure" style="text-align: center">
<img src="fig/08-plot-ggplot2-rendered-ch1-sol-1.png" alt="Binned scatterplot of life expectancy versus year showing how life expectancy has increased over time"  />
<p class="caption">Binned scatterplot of life expectancy versus year showing how life expectancy has increased over time</p>
</div>

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ２

チャレンジ ２ 先の例題とチャレンジでは、`aes` 関数を使用して、各点の x と y の位置について散布図 geom を指定しました。
修正できるもう1つのエステティック属性は、点の色です。 先のチャレンジのコードを修正して、“continent” 列で点に色付けして下さい。 データにどはどのような傾向が見られますか？ それらの傾向は、あなたが期待したものですか？

:::::::::::::::  solution

## チャレンジ８の解答

The solution presented below adds `color=continent` to the call of the `aes`
function. The general trend seems to indicate an increased life expectancy
over the years. On continents with stronger economies we find a longer life
expectancy.


``` r
ggplot(data = gapminder, mapping = aes(x = year, y = lifeExp, color=continent)) +
  geom_point()
```

<div class="figure" style="text-align: center">
<img src="fig/08-plot-ggplot2-rendered-ch2-sol-1.png" alt="Binned scatterplot of life expectancy vs year with color-coded continents showing value of 'aes' function"  />
<p class="caption">Binned scatterplot of life expectancy vs year with color-coded continents showing value of 'aes' function</p>
</div>

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## レイヤー

散布図を使用することは、時間経過による変化を視覚化するのに、おそらく最適ではありません。
代わりに、データを線グラフとして可視化するようggplotに指示しましょう。


``` r
ggplot(data = gapminder, mapping = aes(x=year, y=lifeExp, color=continent)) +
  geom_line()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-line-1.png" style="display: block; margin: auto;" />

`geom_point`レイヤーを追加する代わりに、`geom_line`レイヤーを追加しました。

However, the result doesn't look quite as we might have expected: it seems to be jumping around a lot in each continent. Let's try to separate the data by country, plotting one line for each country:


``` r
ggplot(data = gapminder, mapping = aes(x=year, y=lifeExp, group=country, color=continent)) +
  geom_line()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-line-by-1.png" style="display: block; margin: auto;" />

by エステティックを追加し、各国ごとに線を描くよう`ggplot`に指示します。

しかし、線と点の両方をプロット上に視覚化したい場合はどうすればよいでしょうか？ プロットに別のレイヤーを追加するだけです。


``` r
ggplot(data = gapminder, mapping = aes(x=year, y=lifeExp, group=country, color=continent)) +
  geom_line() + geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-line-point-1.png" style="display: block; margin: auto;" />

It's important to note that each layer is drawn on top of the previous layer. In
this example, the points have been drawn _on top of_ the lines. Here's a
demonstration:


``` r
ggplot(data = gapminder, mapping = aes(x=year, y=lifeExp, group=country)) +
  geom_line(mapping = aes(color=continent)) + geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-layer-example-1-1.png" style="display: block; margin: auto;" />

この例では、 color エステティックマッピングが、`ggplot`のグローバルプロットオプションから`geom_line`レイヤーに移動されたため、 点には色が適用されなくなりました。 これで、点が線の上に描画されていることがわかります。

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：エステティック属性に、マッピングの代わりに値を設定する

So far, we've seen how to use an aesthetic (such as **color**) as a _mapping_ to a variable in the data. For example, when we use `geom_line(mapping = aes(color=continent))`, ggplot will give a different color to each continent. But what if we want to change the color of all lines to blue? You may think that `geom_line(mapping = aes(color="blue"))` should work, but it doesn't. Since we don't want to create a mapping to a specific variable, we can move the color specification outside of the `aes()` function, like this: `geom_line(color="blue")`.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ３

Switch the order of the point and line layers from the previous example. 何が起こったのでしょう？Rは、csvファイルを読み込む際、 列にある全てのものが同じ基本の型であるべきだと主張します。もし、列の 全て が、 double型であることが確認できない場合、その列の だれも double型にならないのです。

:::::::::::::::  solution

## チャレンジ３の解答

The lines now get drawn over the points!


``` r
ggplot(data = gapminder, mapping = aes(x=year, y=lifeExp, group=country)) +
 geom_point() + geom_line(mapping = aes(color=continent))
```

<img src="fig/08-plot-ggplot2-rendered-ch3-sol-1.png" alt="Scatter plot of life expectancy vs GDP per capita with a trend line summarising the relationship between variables. The plot illustrates the possibilities for styling visualisations in ggplot2 with data points enlarged, coloured orange, and displayed without transparency." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 変換と統計

ggplot2を使用すると、統計モデルをデータに適用することが容易になります。 デモのために、最初の例に戻ります。


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) +
  geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-vs-gdpPercap-scatter3-1.png" style="display: block; margin: auto;" />

Currently it's hard to see the relationship between the points due to some strong
outliers in GDP per capita. We can change the scale of units on the x axis using
the _scale_ functions. These control the mapping between the data values and
visual values of an aesthetic. We can also modify the transparency of the
points, using the _alpha_ function, which is especially helpful when you have
a large amount of data which is very clustered.


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) +
  geom_point(alpha = 0.5) + scale_x_log10()
```

<div class="figure" style="text-align: center">
<img src="fig/08-plot-ggplot2-rendered-axis-scale-1.png" alt="Scatterplot of GDP vs life expectancy showing logarithmic x-axis data spread"  />
<p class="caption">Scatterplot of GDP vs life expectancy showing logarithmic x-axis data spread</p>
</div>

The `scale_x_log10` function applied a transformation to the coordinate system of the plot, so that each multiple of 10 is evenly spaced from left to right. For example, a GDP per capita of 1,000 is the same horizontal distance away from a value of 10,000 as the 10,000 value is from 100,000. This helps to visualize the spread of the data along the x-axis.

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒントのリマインダ：エステティック属性に、マッピングの代わりに値を設定する

`geom_point(alpha = 0.5)`を使用したことに注目してください。 先のヒントで触れたように、`aes()`関数以外の設定を使用すると、この値がすべての点で使用されます。 この場合、この値が必要です。しかし、他のエステティック設定と同様に、 alpha はデータ内の変数にマッピングすることもできます。 たとえば、`geom_point(aes(alpha = continent))`を使用して、各大陸に異なる透明度を与えることができます。

::::::::::::::::::::::::::::::::::::::::::::::::::

`geom_smooth`という別のレイヤーを追加することで、データに単純な関係を当てはめることができます。


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) +
  geom_point(alpha = 0.5) + scale_x_log10() + geom_smooth(method="lm")
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-lm-fit-1.png" alt="Scatter plot of life expectancy vs GDP per capita with a blue trend line summarising the relationship between variables, and gray shaded area indicating 95% confidence intervals for that trend line." style="display: block; margin: auto;" />

`geom_smooth`レイヤーで size エステティック属性を設定することによって、 線を太くすることができます。


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) +
  geom_point(alpha = 0.5) + scale_x_log10() + geom_smooth(method="lm", linewidth=1.5)
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-lm-fit2-1.png" alt="Scatter plot of life expectancy vs GDP per capita with a trend line summarising the relationship between variables. The blue trend line is slightly thicker than in the previous figure." style="display: block; margin: auto;" />

エステティック属性を指定する方法は2つあります。 Here we _set_ the **linewidth** aesthetic by passing it as an argument to `geom_smooth` and it is applied the same to the whole `geom`. これまでのレッスンでは、データの変数とその視覚表現の間のマッピングを定義するために`aes`関数を使用しました。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 4a

前の例を用いて、点レイヤー上の点の色とサイズを変更して下さい。

ヒント：'aes'関数を使用しないでください。

Hint: the equivalent of `linewidth` for points is `size`.

:::::::::::::::  solution

## チャレンジ８の解答

Here a possible solution:
Notice that the `color` argument is supplied outside of the `aes()` function.
This means that it applies to all data points on the graph and is not related to
a specific variable.


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) +
 geom_point(size=3, color="orange") + scale_x_log10() +
 geom_smooth(method="lm", linewidth=1.5)
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-ch4a-sol-1.png" alt="Scatter plot of life expectancy vs GDP per capita with a trend line summarising the relationship between variables. The plot illustrates the possibilities for styling visualisations in ggplot2 with data points enlarged, coloured orange, and displayed without transparency." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ 4b

点を異なる形にし、また大陸毎に色分けと傾向線の描画をするために、 チャレンジ4aの回答を変更して下さい。  ヒント：color引数は、aes関数内で使用することができます。

:::::::::::::::  solution

## チャレンジ８の解答

Here is a possible solution:
Notice that supplying the `color` argument inside the `aes()` functions enables you to
connect it to a certain variable. The `shape` argument, as you can see, modifies all
data points the same way (it is outside the `aes()` call) while the `color` argument which
is placed inside the `aes()` call modifies a point's color based on its continent value.


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp, color = continent)) +
 geom_point(size=3, shape=17) + scale_x_log10() +
 geom_smooth(method="lm", linewidth=1.5)
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-ch4b-sol-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 複数パネルの図

先の例では、すべての国の平均余命の変化を1つのプロットで視覚化しました。 一方、 facet パネルのレイヤーを追加することで、複数のパネルに分割することができます。

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント

We start by making a subset of data including only countries located
in the Americas.  This includes 25 countries, which will begin to
clutter the figure.  Note that we apply a "theme" definition to rotate
the x-axis labels to maintain readability.  Nearly everything in
ggplot2 is customizable.

::::::::::::::::::::::::::::::::::::::::::::::::::


``` r
americas <- gapminder[gapminder$continent == "Americas",]
ggplot(data = americas, mapping = aes(x = year, y = lifeExp)) +
  geom_line() +
  facet_wrap( ~ country) +
  theme(axis.text.x = element_text(angle = 45))
```

<img src="fig/08-plot-ggplot2-rendered-facet-1.png" style="display: block; margin: auto;" />

`facet_wrap`レイヤーは引数として“formula”をとり、チルダ(~)で表記されます。 これは、gapminderデータセットのcountry列にある各々の一意な値のパネルを描画するようRに指示します。

## テキストの変更

分析結果の発表に向けてこの図を整理するにあたり、いくつかのテキスト要素を変更する必要があります。 x軸はあまりにも雑然としており、y軸はデータフレームの列名ではなく、“Life expectancy”と読み替えるべきです。

これを行うには、いくつかのレイヤーを追加する必要があります。 theme レイヤーは、軸テキストと全体のテキストサイズを制御します。 軸、プロットタイトル、および任意の凡例のラベルは、`labs`関数を使用して設定できます。 凡例のタイトルは、`aes`関数で使用したものと同じ名前を設定します。 したがって、color凡例のタイトルは`color = "Continent"`を用いて設定され、 fill凡例のタイトルは`fill = "任意のタイトル"`を使用して設定されます。


``` r
ggplot(data = americas, mapping = aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country) +
  labs(
    x = "Year",              # x axis title
    y = "Life expectancy",   # y axis title
    title = "Figure 1",      # main title of figure
    color = "Continent"      # title of legend
  ) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

<img src="fig/08-plot-ggplot2-rendered-theme-1.png" style="display: block; margin: auto;" />

## プロットのエクスポート

`ggsave()`関数を使用すると、ggplotで作成したプロットをエクスポートすることができます。 出版、公開のための高品質グラフィックを作成するために、 適切な引数（`width`、`height`、および`dpi`）を調整してプロットの寸法と解像度を指定できます。 上記のように、そのプロットを保存するには、最初にそのプロットを変数`lifeExp_plot`に割り当て、 `ggsave`にそのプロットを`png`形式で`results`というディレクトリに保存するよう指示します。 （作業ディレクトリに'results /'フォルダがあることを確認してください。）




``` r
lifeExp_plot <- ggplot(data = americas, mapping = aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country) +
  labs(
    x = "Year",              # x axis title
    y = "Life expectancy",   # y axis title
    title = "Figure 1",      # main title of figure
    color = "Continent"      # title of legend
  ) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

ggsave(filename = "results/lifeExp.png", plot = lifeExp_plot, width = 12, height = 10, dpi = 300, units = "cm")
```

`ggsave`には素晴らしい点が二つあります。 一つ目は、最後のプロットがデフォルトになるので、`plot`引数を省略すると、`ggplot`で作成した最後のプロットが自動的に保存されることです。 二つ目は、ファイル名に指定したファイル拡張子（例：`.png`または`.pdf`）からプロットを保存するフォーマットを決定しようとします。 必要な場合は、`device`引数に明示的にフォーマットを指定できます。

This is a taste of what you can do with ggplot2. RStudio provides a
really useful [cheat sheet][cheat] of the different layers available, and more
extensive documentation is available on the [ggplot2 website][ggplot-doc]. All RStudio cheat sheets are available from the [RStudio website][cheat_all].
Finally, if you have no idea how to change something, a quick Google search will
usually send you to a relevant question and answer on Stack Overflow with reusable
code to modify!

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ５

Generate boxplots to compare life expectancy between the different continents during the available years.

Advanced:

- Rename y axis as Life Expectancy.
- Remove x axis labels.

:::::::::::::::  solution

## チャレンジ３の解答

Here a possible solution:
`xlab()` and `ylab()` set labels for the x and y axes, respectively
The axis title, text and ticks are attributes of the theme and must be modified within a `theme()` call.


``` r
ggplot(data = gapminder, mapping = aes(x = continent, y = lifeExp, fill = continent)) +
 geom_boxplot() + facet_wrap(~year) +
 ylab("Life Expectancy") +
 theme(axis.title.x=element_blank(),
       axis.text.x = element_blank(),
       axis.ticks.x = element_blank())
```

<img src="fig/08-plot-ggplot2-rendered-ch5-sol-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

[base]: https://www.statmethods.net/graphs/index.html
[lattice]: https://www.statmethods.net/advgraphs/trellis.html
[ggplot2]: https://www.statmethods.net/advgraphs/ggplot2.html
[cheat]: https://www.rstudio.org/links/data_visualization_cheat_sheet
[cheat_all]: https://www.rstudio.com/resources/cheatsheets/
[ggplot-doc]: https://ggplot2.tidyverse.org/reference/

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `ggplot2` to create plots.
- Think about graphics in layers: aesthetics, geometry, statistics, scale transformation, and grouping.

::::::::::::::::::::::::::::::::::::::::::::::::::
