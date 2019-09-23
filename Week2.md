# 有名な集合 - LaTeXコードを高い保守性の下で記述する例

## 動機

「そこそこ多くの標準のコマンドを覚え，LaTeXに慣れてきた人」にはまだ若干実感が湧かないかもしれませんが，長期にわたって書き足したり修正したりする文書を作成するにあたっては **“文書のメンテナンス性の高さ”** [^1] が求められることも多いのではないでしょうか。つまり，後で**全体を**容易に変更する余地を残した文書作成を意識する必要が出てくるのです。

## 実践

今回はそういった “修正を容易にする” 発想のごく初歩的な例として，有名な集合を書き表すシンプルなマクロを紹介しましょう。ここでいう有名な集合というのは，自然数[^2]，整数，有理数，実数，複素数といった，ラテン文字1字で書き表される集合です。“普通に” やるなら，例えば

~~~ latex
$n \in \mathbf{N}^{+}$，$p \in \mathbf{Q}$，$x \in \mathbf{R}$ とする．$
~~~

というコードを処理することで

![images/week2_1.png](https://qiita-image-store.s3.amazonaws.com/0/75927/65789940-4826-bd0f-60ef-597bd6982660.png)

と出力することが可能です。しかし，これらの集合の記法には複数の流儀があります。もしかしたら後で「太字イタリックにすべきだ」という要請があって

![images/week2_2.png](https://qiita-image-store.s3.amazonaws.com/0/75927/9827bef8-0f65-4057-8ee0-3455dc5ef589.png)

と修正したり，或いは「黒板太字にしてほしい」[^3]という依頼があって

![images/week2_3.png](https://qiita-image-store.s3.amazonaws.com/0/75927/653f31c8-5b54-c0eb-2b88-08d8ed36a8c0.png)

に修正することになるかもしれません。こうなるとこれらの集合に使われたすべての`\mathbf`を`\bm`に置き換えなければならないのですが，集合以外でも`\mathbf`を使っていると単純な文字列置換では済まなくなったりして修正が大変です。

そこで，最初に掲げたような “単に出力するための書き方” をするのではなく，まず

~~~ latex
\def\usebbsetcapital{\def\setcapital##1{\mathbb{##1}}}
\def\usebfsetcapital{\def\setcapital##1{\mathbf{##1}}}
\def\usebmsetcapital{\def\setcapital##1{\bm{##1}}}
\usebfsetcapital
\def\setC{\setcapital{C}}
\def\setH{\setcapital{H}}
\def\setN{\setcapital{N}}
\def\setNpos{\setcapital{N}^{\mathord{+}}}
\def\setP{\setcapital{P}}
\def\setQ{\setcapital{Q}}
\def\setR{\setcapital{R}}
\def\setRp{\setcapital{R}^{\mathord{+}}}
\def\setZ{\setcapital{Z}}
~~~

というスタイルファイル`my-style.sty`を保存しておきます。これを用いて，実際に記述する文書を

~~~ latex
\documentclass{jsarticle}
\usepackage{amssymb}
\usepackage{bm}
\usepackage{my-style}
\begin{document}
  \indent
    $n \in \setNpos$，$p \in \setQ$，$x \in \setR$ とする．
  \par
\end{document}
~~~

として処理すると，やはり

![images/week2_1.png](https://qiita-image-store.s3.amazonaws.com/0/75927/65789940-4826-bd0f-60ef-597bd6982660.png)

のように出力されますが，これが最初の “単に出力するための書き方” と違うのは，プリアンブル[^4] の`\usepackage{my-style}`以降に

~~~latex
\usebmsetcapital
~~~

を追加して処理するだけで

![images/week2_2.png](https://qiita-image-store.s3.amazonaws.com/0/75927/9827bef8-0f65-4057-8ee0-3455dc5ef589.png)

とすべて修正された出力となる点です。つまり，**最初から複数の記述方法をとりうるものは一斉に変更できる仕組みにしておこう**という考え方です。意味を明示した（セマンティックな）コマンド名にしておけば，**コードも見やすくなり**，一挙両得！

ただし，セマンティックなコードを目指すあまり複雑なマクロ化を施してしまうと，今度は打つのが大変になりますから，適度な範囲に留めておくことをおすすめします。

[^1]: 一般に **保守性** と呼ばれます。

[^2]: 0 は自然数。

[^3]: ただし「黒板太字は手書きに使われる字体であって印刷に用いる代物ではない」という主張があり，TeXの作者Donald. E. Knuthも活字として黒板太字を使用することには苦言を呈しているらしいです。

[^4]: `\documentclass{...}`と`\begin{document}`の間の部分を指します。
