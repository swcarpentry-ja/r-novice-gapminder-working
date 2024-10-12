---
title: Writing Good Software
teaching: 15
exercises: 0
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- Describe best practices for writing R and explain the justification for each.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I write software that other people can use?

::::::::::::::::::::::::::::::::::::::::::::::::::

## プロジェクトフォルダーを構造化する

Keep your project folder structured, organized and tidy, by creating subfolders for your code files, manuals, data, binaries, output plots, etc. It can be done completely manually, or with the help of RStudio's `New Project` functionality, or a designated package, such as `ProjectTemplate`.

:::::::::::::::::::::::::::::::::::::::::  callout

## ヒント：解決策の一つ：ProjectTemplate

One way to automate the management of projects is to install the third-party package, `ProjectTemplate`.
This package will set up an ideal directory structure for project management.
This is very useful as it enables you to have your analysis pipeline/workflow organised and structured.
Together with the default RStudio project functionality and Git you will be able to keep track of your
work as well as be able to share your work with collaborators.

1. Install `ProjectTemplate`.
2. Load the library
3. Initialise the project:


``` r
install.packages("ProjectTemplate")
library("ProjectTemplate")
create.project("../my_project_2", merge.strategy = "allow.non.conflict")
```

For more information on ProjectTemplate and its functionality visit the
home page [ProjectTemplate](https://projecttemplate.net/index.html)

::::::::::::::::::::::::::::::::::::::::::::::::::

## コードを読めるようにする

The most important part of writing code is making it readable and understandable.
コードを書く際に一番重要なのが、読みやすくて理解のしやすくすることです。 他の誰が自分のコードを取り上げ、何をするものかを理解してもらわなければなりません。 多くの場合、この誰かさんは、6か月後の自分なので、もしコードが読めない、 理解できない場合は、昔の自分に悪態をつくことになるでしょう。

## 文書化：「どうやって」ではなく「なぜ」を伝えて下さい

When you first start out, your comments will often describe what a command does,
since you're still learning yourself and it can help to clarify concepts and
remind you later. However, these comments aren't particularly useful later on
when you don't remember what problem your code is trying to solve. Try to also
include comments that tell you _why_ you're solving a problem, and _what_ problem
that is. The _how_ can come after that: it's an implementation detail you ideally
shouldn't have to worry about.

## コードをモジュール化しましょう

関数を分析スクリプトと分けて、別のファイルに保存しておき、 プロジェクトでRのセッションを開いたときに、 `source` することをオススメします。 このアプローチを取ると、分析スクリプトに無駄がなくなり、プロジェクト内のどの分析スクリプトでも 使える関数をストックする場所ができるので、便利です。 また、同じような関数をまとめるのが簡単になります。

## 問題をひとくち大に分ける

初めは、問題解決と関数の記述は気が滅入るタスクで、 コードの経験不足とは別の問題として分けることができないと思うかもしれません。問題を消化できる塊に分け、 導入の詳細についての心配は後回しにしましょう： 問題をコードで解決できるまで、どんどん小さい関数に分けていきしましょう。 そして、解決されたものを積み上げていけば、問題を解決できるでしょう。

## コードが正しいことをしているか確かめましょう

関数をテストすることを、くれぐれも忘れないように！

## 同じことを繰り返さないようにしましょう

Functions enable easy reuse within a project. 関数は、プロジェクトの中で簡単に再利用できます。もし、プロジェクトで同じようなコードの行の塊を 見つけたら、それらを関数に移す候補にしましょう。

もし、計算が一連の関数で行われていた場合、プロジェクトはよりモジュール化され、変更するのが簡単になります。 これは、特定のインプットが必ず特定のアウトプットを返す場合など、特にそうです。

## 常にスタイリッシュであろうとする

自分のコードに一貫性のあるスタイルを適用しましょう。

:::::::::::::::::::::::::::::::::::::::: keypoints

- Keep your project folder structured, organized and tidy.
- Document what and why, not how.
- Break programs into short single-purpose functions.
- Write re-runnable tests.
- 同じことを繰り返さないようにしましょう.
- Be consistent in naming, indentation, and other aspects of style.

::::::::::::::::::::::::::::::::::::::::::::::::::
