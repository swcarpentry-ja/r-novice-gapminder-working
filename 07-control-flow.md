---
title: Control Flow
teaching: 45
exercises: 20
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- Write conditional statements with `if...else` statements and `ifelse()`.
- Write and understand `for()` loops.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I make data-dependent choices in R?
- How can I repeat operations in R?

::::::::::::::::::::::::::::::::::::::::::::::::::



コードを書く際、実行の流れを制御する必要がよくあります。 これは、ある条件、または一連の条件が満たされたときに実行されるようにすればできます。
あるいは、決まった回数実行されるよう設定することもできます。

There are several ways you can control flow in R.
For conditional statements, the most commonly used approaches are the constructs:


``` r
~~~ # if if (condition is true) {
  perform action
} # if ... else if (condition is true) { # 条件が満たされた場合 アクションを行う } else { # つまり、条件が満たされなかった場合 別のアクションを行う } ~~~
```

例えばRに、もし変数 `x` が特定の値を持っていた場合、メッセージを表示させたいとします。


``` r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
}

x
```

``` output
[1] 8
```

The print statement does not appear in the console because x is not greater than 10. To print a different message for numbers less than 10, we can add an `else` statement.


``` r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
} else {
  print("x is less than 10")
}
```

``` output
[1] "x is less than 10"
```

`else if` を使うと、複数の条件を試すこともできます。


``` r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
} else if (x > 5) {
  print("x is greater than 5, but less than 10")
} else {
  print("x is less than 5")
}
```

``` output
[1] "x is greater than 5, but less than 10"
```

**Important:** when R evaluates the condition inside `if()` statements, it is
looking for a logical element, i.e., `TRUE` or `FALSE`. This can cause some
headaches for beginners. 例えば：


``` r
x  <-  4 == 3
if (x) {
  "4 equals 3"
} else {
  "4 does not equal 3"
}
```

``` output
[1] "4 does not equal 3"
```

ここで見られるように、ベクトル x が `FALSE` であるため、不等号のメッセージが表示されました。


``` r
x <- 4 == 3
x
```

``` output
[1] FALSE
```

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ１

Use an `if()` statement to print a suitable message
reporting whether there are any records from 2002 in
the `gapminder` dataset.
Now do the same for 2012.

:::::::::::::::  solution

## チャレンジ３の解答

We will first see a solution to Challenge 1 which does not use the `any()` function.
We first obtain a logical vector describing which element of `gapminder$year` is equal to `2002`:


``` r
gapminder[(gapminder$year == 2002),]
```

Then, we count the number of rows of the data.frame `gapminder` that correspond to the 2002:


``` r
rows2002_number <- nrow(gapminder[(gapminder$year == 2002),])
```

The presence of any record for the year 2002 is equivalent to the request that `rows2002_number` is one or more:


``` r
rows2002_number >= 1
```

Putting all together, we obtain:


``` r
if(nrow(gapminder[(gapminder$year == 2002),]) >= 1){
   print("Record(s) for the year 2002 found.")
}
```

All this can be done more quickly with `any()`. The logical condition can be expressed as:


``` r
if(any(gapminder$year == 2002)){
   print("Record(s) for the year 2002 found.")
}
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

次のような警告メッセージをもらった人はいますか？


``` error
Error in if (gapminder$year == 2012) {: the condition has length > 1
```

The `if()` function only accepts singular (of length 1) inputs, and therefore
returns an error when you use it with a vector. The `if()` function will still
run, but will only evaluate the condition in the first element of the vector.
Therefore, to use the `if()` function, you need to make sure your input is
singular (of length 1).

:::::::::::::::::::::::::::::::::::::::::  callout

## Tip: Built in `ifelse()` function

`R` accepts both `if()` and `else if()` statements structured as outlined above,
but also statements using `R`'s built-in `ifelse()` function. This
function accepts both singular and vector inputs and is structured as
follows:


``` r
# ifelse function
ifelse(condition is true, perform action, perform alternative action)
```

where the first argument is the condition or a set of conditions to be met, the
second argument is the statement that is evaluated when the condition is `TRUE`,
and the third statement  is the statement that is evaluated when the condition
is `FALSE`.


``` r
y <- -3
ifelse(y < 0, "y is a negative number", "y is either positive or zero")
```

``` output
[1] "y is a negative number"
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：`any()` と `all()`

`any()` 関数は、ベクトルの中に少なくとも１つ `TRUE` の値がある場合、 `TRUE` を返し、 そうでない場合は、 `FALSE` を返します。
これは、 `%in%` 演算子でも同様に使えます。
関数 `all()` は、その名前が示唆しているように、ベクトル内の全ての値が `TRUE` である時のみ、 `TRUE` となります。

::::::::::::::::::::::::::::::::::::::::::::::::::

## 繰り返し行う処理

If you want to iterate over
a set of values, when the order of iteration is important, and perform the
same operation on each, a `for()` loop will do the job.
We saw `for()` loops in the [shell lessons earlier](https://swcarpentry.github.io/shell-novice/05-loop.html). This is the most
flexible of looping operations, but therefore also the hardest to use
correctly. In general, the advice of many `R` users would be to learn about
`for()` loops, but to avoid using `for()` loops unless the order of iteration is
important: i.e. the calculation at each iteration depends on the results of
previous iterations. If the order of iteration is not important, then you
should learn about vectorized alternatives, such as the `purrr` package, as they
pay off in computational efficiency.

`for()` ループの基本構造は：


``` r
for (iterator in set of values) {
  do a thing
}
```

例えば：


``` r
for (i in 1:10) {
  print(i)
}
```

``` output
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
[1] 10
```

`1:10` の部分は、ベクトルをその場で作るものです。 他のベクトルの中身を繰り返すこともできます。

`for()` ループを、もうひとつの `for()` ループと入れ子となる形にすれば、 ２つ同時に繰り返すこともできます。


``` r
for (i in 1:5) {
  for (j in c('a', 'b', 'c', 'd', 'e')) {
    print(paste(i,j))
  }
}
```

``` output
[1] "1 a"
[1] "1 b"
[1] "1 c"
[1] "1 d"
[1] "1 e"
[1] "2 a"
[1] "2 b"
[1] "2 c"
[1] "2 d"
[1] "2 e"
[1] "3 a"
[1] "3 b"
[1] "3 c"
[1] "3 d"
[1] "3 e"
[1] "4 a"
[1] "4 b"
[1] "4 c"
[1] "4 d"
[1] "4 e"
[1] "5 a"
[1] "5 b"
[1] "5 c"
[1] "5 d"
[1] "5 e"
```

We notice in the output that when the first index (`i`) is set to 1, the second
index (`j`) iterates through its full set of indices. Once the indices of `j`
have been iterated through, then `i` is incremented. This process continues
until the last index has been used for each `for()` loop.

結果を表示させずに、ループの結果を新しいオブジェクトに書き込むこともできます。


``` r
output_vector <- c()
for (i in 1:5) {
  for (j in c('a', 'b', 'c', 'd', 'e')) {
    temp_output <- paste(i, j)
    output_vector <- c(output_vector, temp_output)
  }
}
output_vector
```

``` output
 [1] "1 a" "1 b" "1 c" "1 d" "1 e" "2 a" "2 b" "2 c" "2 d" "2 e" "3 a" "3 b"
[13] "3 c" "3 d" "3 e" "4 a" "4 b" "4 c" "4 d" "4 e" "5 a" "5 b" "5 c" "5 d"
[25] "5 e"
```

このアプローチが役に立つこともありますが、'結果を太らせる' （結果のオブジェクトを 徐々に積み上げる）と、演算する上で非効率になります。 ゆえに、多くの値の間を繰り返すときは避けましょう。

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：結果を太らせないようにしましょう

One of the biggest things that trips up novices and
experienced R users alike, is building a results object
(vector, list, matrix, data frame) as your for loop progresses.
Computers are very bad at handling this, so your calculations
can very quickly slow to a crawl. It's much better to define
an empty results object before hand of appropriate dimensions, rather
than initializing an empty object without dimensions.
So if you know the end result will be stored in a matrix like above,
create an empty matrix with 5 row and 5 columns, then at each iteration
store the results in the appropriate location.

::::::::::::::::::::::::::::::::::::::::::::::::::

よりよい方法は、（空の）出力オブジェクトを、値を埋める前に宣言することです。
この例では、より複雑に見えますが、より効率的です。


``` r
output_matrix <- matrix(nrow = 5, ncol = 5)
j_vector <- c('a', 'b', 'c', 'd', 'e')
for (i in 1:5) {
  for (j in 1:5) {
    temp_j_value <- j_vector[j]
    temp_output <- paste(i, temp_j_value)
    output_matrix[i, j] <- temp_output
  }
}
output_vector2 <- as.vector(output_matrix)
output_vector2
```

``` output
 [1] "1 a" "2 a" "3 a" "4 a" "5 a" "1 b" "2 b" "3 b" "4 b" "5 b" "1 c" "2 c"
[13] "3 c" "4 c" "5 c" "1 d" "2 d" "3 d" "4 d" "5 d" "1 e" "2 e" "3 e" "4 e"
[25] "5 e"
```

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：while ループ

時には、ある条件が満たされるまで繰り返す必要があります。 これは、 `while()` ループを使えばできます。


``` r
while(this condition is true){
  do a thing
}
```

R will interpret a condition being met as "TRUE".

```while(this condition is true) \~\~\~  例として、このwhileループは 一様分布（&#x60;runif()&#x60; 関数）から0.1よりも小さい数を得るまで、 ０から１の間で乱数を生成します。
```

```r
z <- 1
while(z > 0.1){
  z <- runif(1)
  cat(z, "\n")
}
```

`while()` loops will not always be appropriate. You have to be particularly careful
that you don't end up stuck in an infinite loop because your condition is always met and hence the while statement never terminates.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ２

Compare the objects `output_vector` and
`output_vector2`. Are they the same? If not, why not?
How would you change the last block of code to make `output_vector2`
the same as `output_vector`?

:::::::::::::::  solution

## チャレンジ３の解答

We can check whether the two vectors are identical using the `all()` function:


``` r
all(output_vector == output_vector2)
```

However, all the elements of `output_vector` can be found in `output_vector2`:


``` r
all(output_vector %in% output_vector2)
```

and vice versa:


``` r
all(output_vector2 %in% output_vector)
```

therefore, the element in `output_vector` and `output_vector2` are just sorted in a different order.
This is because `as.vector()` outputs the elements of an input matrix going over its column.
Taking a look at `output_matrix`, we can notice that we want its elements by rows.
The solution is to transpose the `output_matrix`. We can do it either by calling the transpose function
`t()` or by inputting the elements in the right order.
The first solution requires to change the original


``` r
output_vector2 <- as.vector(output_matrix)
```

into


``` r
output_vector2 <- as.vector(t(output_matrix))
```

The second solution requires to change


``` r
output_matrix[i, j] <- temp_output
```

into


``` r
output_matrix[j, i] <- temp_output
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ３

`gapminder` データを大陸ごとにループし、平均余命が50歳以上かどうかを表示する スクリプトを書きましょう。

:::::::::::::::  solution

## チャレンジ３の解答

**Step 1**:  We want to make sure we can extract all the unique values of the continent vector


``` r
gapminder <- read.csv("data/gapminder_data.csv")
unique(gapminder$continent)
```

**Step 2**: We also need to loop over each of these continents and calculate the average life expectancy for each `subset` of data.```gapminder <- read.csv("data/gapminder\_data.csv") unique(gapminder$continent) \~\~\~ {: .language-r} 手順2 ：これらの大陸のそれぞれにループをし、その &#x60;部分集合&#x60; データごとに平均余命を出す必要があります。
```

1. それは次のようにすればできます： 1.
2. '大陸（continent）' の固有の値のそれぞれについてループする 2.
3. Return the calculated life expectancy to the user by printing the output:


``` r
for (iContinent in unique(gapminder$continent)) {
  tmp <- gapminder[gapminder$continent == iContinent, ]
  cat(iContinent, mean(tmp$lifeExp, na.rm = TRUE), "\n")
  rm(tmp)
}
```

**Step 3**: The exercise only wants the output printed if the average life expectancy is less than 50 or greater than 50.
ゆえに、結果を表示させる前に `if` 条件をつけて、演算された平均余命が基準値以上か、基準値未満かを判別し、結果によって正しい出力を表示させる必要があります。
これを踏まえて、上の (3) を修正する必要があります： 3a.

3a. If the calculated life expectancy is less than some threshold (50 years), return the continent and a statement that life expectancy is less than threshold, otherwise return the continent and a statement that life expectancy is greater than threshold:


``` r
thresholdValue <- 50

for (iContinent in unique(gapminder$continent)) {
   tmp <- mean(gapminder[gapminder$continent == iContinent, "lifeExp"])

   if (tmp < thresholdValue){
       cat("Average Life Expectancy in", iContinent, "is less than", thresholdValue, "\n")
   } else {
       cat("Average Life Expectancy in", iContinent, "is greater than", thresholdValue, "\n")
   } # end if else condition
   rm(tmp)
} # end for loop
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ４

チャレンジ３のスクリプトをそれぞれの国ごとにループする形に直してください。 今回は、平均余命は50歳未満か、50歳以上70歳未満か、70歳以上かを 表示しましょう。

:::::::::::::::  solution

## チャレンジ３の解答

We modify our solution to Challenge 3 by now adding two thresholds, `lowerThreshold` and `upperThreshold` and extending our if-else statements:


``` r
チャレンジ４の解答 チャレンジ３の解答を、 `lowerThreshold` と `upperThreshold` の２つの基準値を加え、if-else 宣言を拡張する形で修正します： ~~~ lowerThreshold <- 50 upperThreshold <- 70 for( iCountry in unique(gapminder$country) ){ tmp <- mean(subset(gapminder, country==iCountry)$lifeExp) if(tmp < lowerThreshold){ cat("Average Life Expectancy in", iCountry, "is less than", lowerThreshold, "\\n") } else if(tmp lowerThreshold && tmp < upperThreshold){ cat("Average Life Expectancy in", iCountry, "is between", lowerThreshold, "and", upperThreshold, "\\n") } else{ cat("Average Life Expectancy in", iCountry, "is greater than", upperThreshold, "\\n") } rm(tmp) } ~~~ {: .language-r}
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ5 - 上級

Write a script that loops over each country in the `gapminder` dataset,
tests whether the country starts with a 'B', and graphs life expectancy
against time as a line graph if the mean life expectancy is under 50 years.

:::::::::::::::  solution

## チャレンジ３の解答

We will use the `grep()` command that was introduced in the [Unix Shell lesson](https://swcarpentry.github.io/shell-novice/07-find.html)
to find countries that start with "B."
Lets understand how to do this first.
Following from the Unix shell section we may be tempted to try the following


``` r
grep("^B", unique(gapminder$country))
```

But when we evaluate this command it returns the indices of the factor variable `country` that start with "B."
To get the values, we must add the `value=TRUE` option to the `grep()` command:


``` r
grep("^B", unique(gapminder$country), value = TRUE)
```

We will now store these countries in a variable called candidateCountries, and then loop over each entry in the variable.
Inside the loop, we evaluate the average life expectancy for each country, and if the average life expectancy is less than 50 we use base-plot to plot the evolution of average life expectancy using `with()` and `subset()`:


``` r
thresholdValue <- 50
candidateCountries <- grep("^B", unique(gapminder$country), value = TRUE)

for (iCountry in candidateCountries) {
    tmp <- mean(gapminder[gapminder$country == iCountry, "lifeExp"])

    if (tmp < thresholdValue) {
        cat("Average Life Expectancy in", iCountry, "is less than", thresholdValue, "plotting life expectancy graph... \n")

        with(subset(gapminder, country == iCountry),
                plot(year, lifeExp,
                     type = "o",
                     main = paste("Life Expectancy in", iCountry, "over time"),
                     ylab = "Life Expectancy",
                     xlab = "Year"
                     ) # end plot
             ) # end with
    } # end if
    rm(tmp)
} # end for loop
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Use `if` and `else` to make choices.
- Use `for` to repeat operations.

::::::::::::::::::::::::::::::::::::::::::::::::::
