# LaTeX マクロで文書にメンテナンス性を

今週のマクロ第2回でも話題になりましたが，「文書のメンテナンス性の高さ」という点において LaTeX は優れています。このことを実感できるわかりやすい例を2つ取り上げてみたいと思います。

~~~ latex
\documentclass{jsarticle}
%%% definition of \tlcomb{}{} for combination
\newcommand{\tlcomb}[2]{%
  \ensuremath{{}_{#1}\mathrm{C}_{#2}}%   definition 1
%  \ensuremath{\mathrm{C}(#1,#2)}%        definition 2
%  \ensuremath{{}^{#1}\mathrm{C}_{#2}}%   definition 3
}
%%% definition of \tlperm{}{} for permutation
\newcommand{\tlperm}[2]{%
  \ensuremath{{}_{#1}\mathrm{P}_{#2}}%   definition 1
%  \ensuremath{\mathrm{P}(#1,#2)}%        definition 2
%  \ensuremath{{}^{#1}\mathrm{P}_{#2}}%   definition 3
}
%%% definition of \tlinner{}{} for inner product
\newcommand{\tlinner}[2]{%
  \vec{#1} \cdot \vec{#2}%                         definition 1
%  \left( \vec{#1}, \vec{#2} \right)%               definition 2
%  \left\langle \vec{#1}, \vec{#2} \right\rangle%   definition 3
}
\begin{document}
異なる6枚のカードから3枚を選ぶ組み合わせの総数は\tlcomb{6}{3}通りです。
異なる12枚のカードから5枚を選んで並べる順列の総数は\tlperm{12}{5}通りです。数式中でも使えます：
\[
  \tlcomb{6}{3} = \frac{6 \cdot 5 \cdot 4}{3 \cdot 2 \cdot 1} = 20
\]

2本のベクトル $\vec{a}$, $\vec{b}$ が次のように与えられているとき，それらの内積 $\tlinner{a}{b}$ を求めなさい。…
\end{document}
~~~

3つの定義をそれぞれ行頭の `%` でコメントアウトして，一つだけ使うようにしています。コメントアウトする行を切り替えて，その出力の違いを確認してみてください。

#### 定義1を使った場合

![equation-1.png](https://qiita-image-store.s3.amazonaws.com/0/80249/137192a9-8818-aaf5-6f5a-c47dce69dabb.png)

#### 定義2を使った場合

![equation-2.png](https://qiita-image-store.s3.amazonaws.com/0/80249/38b72150-53e6-1e60-aedf-889609b8139e.png)

#### 定義3を使った場合

![equation-3.png](https://qiita-image-store.s3.amazonaws.com/0/80249/f039e88e-18bb-1549-36f7-b424c714d86d.png)

## 「順列・組み合わせ」の記号

中学や高校の数学で必ずでてくるのが，順列や組み合わせという概念です。授業で一度は

- n 個の元から k 個を選んで並べる順列の総数は <sub>n</sub>P<sub>k</sub> と表す
- n 個の元から k 個を選ぶ組み合わせの総数は <sub>n</sub>C<sub>k</sub> と表す

のように習ったことと思います。しかし，意外と知られていないようですが，この記号は国際的に通用するわけではありません。あまり参照元として好ましくないかもしれませんが，一般の人がよく使用している記号を垣間みることができる Wikipedia を見てみると，このことが実感できます（以下，選ぶ個数を表す k の代わりに m や r が混在しますが，これは Wikipedia の記述が言語によって統一されていないことに由来していますので，気にしないでください）。

日本語版 Wikipedia「順列」では，順列の総数を表す記号として <sub>n</sub>P<sub>k</sub>, <sup>n</sup>P<sub>k</sub>, P<sub>n,k</sub>, P(n,k) が挙げられています。同様に，「組合せ」では，選び方の総数を表す記号として <sub>n</sub>C<sub>k</sub>, C(n,k) が挙げられています。英語の Permutation, Combination を基にしているので，一見世界共通のように誤解されがちですが，他の言語も見てみるとどうでしょう。

![equation-6.png](https://qiita-image-store.s3.amazonaws.com/0/80249/36bee479-881d-331e-f966-0ce61d4b2341.png)

![equation-7.png](https://qiita-image-store.s3.amazonaws.com/0/80249/c7db0b98-b1fa-4f76-f281-aae91bb95107.png)

順列については各言語の頭文字が使われ，一見して理解できないものもありますし，組み合わせについても，よく見ると指数の上下が逆になっている例も見受けられ，紛らわしいのが実情です。

確かに，これらの概念の拡張（例えば組み合わせに対応する「二項係数」）には比較的通用する記号が存在し，対応する LaTeX マクロが定義されていることもあります。しかし，標準の LaTeX には，日本の初等数学でよく使われる記法を簡略化するマクロがありません。仮に「長文の中で何度も `${}_n \mathrm{C}_k$` という記号を使っていたのに，急遽 `$\mathrm{C}(n,k)$` に変更しなければならなくなった！」という場合，どうすればよいでしょう。下付き文字や括弧といったありふれた記法だけを用いるため，ベタ書きしてしまうと探し出すのに一苦労です。

P や C を用いた記号だけでもいろいろな流儀があることを考えると，こういう場合は初めに LaTeX マクロを定義してマークアップするのが非常に重要です。

~~~ latex
%%% definition of \tlcomb{}{} for combination
\newcommand{\tlcomb}[2]{%
  \ensuremath{{}_{#1}\mathrm{C}_{#2}}%   definition 1
%  \ensuremath{\mathrm{C}(#1,#2)}%        definition 2
%  \ensuremath{{}^{#1}\mathrm{C}_{#2}}%   definition 3
}
%%% definition of \tlperm{}{} for permutation
\newcommand{\tlperm}[2]{%
  \ensuremath{{}_{#1}\mathrm{P}_{#2}}%   definition 1
%  \ensuremath{\mathrm{P}(#1,#2)}%        definition 2
%  \ensuremath{{}^{#1}\mathrm{P}_{#2}}%   definition 3
}
~~~

このマクロでは，本文中と数式環境中の両方で数式とみなされるように，`\ensuremath` の中に入れて定義しました。もし `\ensuremath` に入れずに定義していたならば，本文中で使用した場合に `_`, `^`, `\mathrm` が

~~~
! Missing $ inserted.
! LaTeX Error: \mathrm allowed only in math mode.
~~~

というエラーになるはずです。

なお，[emath パッケージ](http://emath.s40.xrea.com/ydir/Wiki/index.php?%BD%E7%CE%F3%A1%A6%C1%C8%B9%E7%A4%BB%B5%AD%B9%E6)にはこのような順列や組み合わせのマクロが組み込まれています。これらも本記事とほとんど同じ方法で定義されています。

## 「ベクトルの内積」の記号

こちらもマークアップが非常に有効な例です。高校数学で学ぶベクトルの内積ですが，2つのベクトルの内積の記法として，分野によってさまざまな流儀が考えられます。この場合も，ある記法から別の記法に置き換えるのはベタ書きでは困難です。そこで，マクロの出番です。

~~~ latex
%%% definition of \tlinner{}{} for inner product
\newcommand{\tlinner}[2]{%
  \vec{#1} \cdot \vec{#2}%                         definition 1
%  \left( \vec{#1}, \vec{#2} \right)%               definition 2
%  \left\langle \vec{#1}, \vec{#2} \right\rangle%   definition 3
}
~~~

一見 `\cdot` を使うほうが入力が楽だと思うかもしれませんが，メンテナンス性という点では「その `\cdot` が内積を表すのか，それとも積の記号なのか」がわからないという問題があります。マークアップすることで，`\tlinner` の定義さえ変更すれば，文書にでてくるすべての内積の記号を変更できるという安心感が生まれます。

## 終わりに

今回のマクロは，少し LaTeX に慣れた方ならすぐに中身を理解できるほど単純なものだと思います。マクロは難しいものだと思って敬遠しているとすれば，それはマークアップとしての LaTeX のメリットを十分活用できていないことになります。

今週のマクロでは会員がさまざまなレベルのマクロを披露しますが，本記事をきっかけにより広く LaTeX の良さが伝われば幸いです。