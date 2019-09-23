# 「新たな税込価格」の計算

今週のマクロ第３週では，カウンタの取り扱いが話題として取り上げられましたが，そこでは「あらかじめ用意されたカウンタを用いて四則計算できるのは，整数どうしに限られる」という制限がありました。

そこで今回は，消費税増税によって値段がどれくらい値上がりしたかを計算するマクロを作製し，小数点以下を四捨五入するという操作をTeX上で実現しました。

~~~latex

\documentclass[uplatex]{jsarticle}

\makeatletter

\newcount\zeikomi

\def\newzeikomi{\@ifstar\@newzeikomi\@@newzeikomi}%
\def\@newzeikomi#1{\zeikomi= #1\relax
\multiply \zeikomi by 108\relax
\divide \zeikomi by 105\relax
\@tempcnta=#1\relax
\multiply \@tempcnta by 1080\relax
\divide \@tempcnta by 105\relax
\@tempcntb=\zeikomi\relax
\multiply \@tempcntb by 10 \relax
\advance \@tempcnta by -\@tempcntb\relax
\ifnum \@tempcnta>4\relax
\advance \zeikomi by 1\relax
\fi
\the\zeikomi}%
\def\@@newzeikomi#1{\zeikomi= #1\relax
\multiply \zeikomi by 108\relax
\divide \zeikomi by 105\relax
\the\zeikomi}%

\makeatother

\begin{document}

2014年4月1日の消費税増税に伴い，それまで税込価格210円だった商品は税込価格\newzeikomi{210}円となってしまった．

また，それまで160円で乗車できた電車にICカードを用いて乗車する際の運賃は，\newzeikomi*{160}円となった．

\end{document}

~~~

これをコンパイルすると，以下のようになります。

![スクリーンショット 2015-05-19 13.51.42.png](https://qiita-image-store.s3.amazonaws.com/0/62358/5b33b0fa-3f14-2f29-0a12-a98bea79c1a5.png)

##マクロ解説

それでは，今回のマクロにおいて使用した技（と言えるものもないですが）をご紹介いたします。

###四捨五入する機能の装填

このマクロでは「小数点以下を四捨五入する」操作は以下のような順序で実現しています。

(Ⅰ)　小数点以下を切り捨てた整数値をとりあえず出力する
(Ⅱ)　10倍してから小数点以下を切り捨てた値を計算し，その値を暫定カウンタ`\@tempcnta`に入力する
(Ⅲ)　(Ⅰ)で計算した整数値を10倍したものを求めて暫定カウンタ`\@tempcntb`に入力し，その値をカウンタ`\@tempcnta`の値から引く
(Ⅳ)　(Ⅲ)で出て来た1桁の数値が5以上か否かを判定し，もし5以上だったら小数点以下の切り上げが必要なので，(Ⅰ)の値に1を足す操作を行う

四捨五入する機能のみを抜粋した部分について，マクロのどの部分が上の４つの操作のどれに対応しているのかを説明すると，以下のようになります。

~~~latex
（\newcount\zeikomi）
{\zeikomi= #1\relax
\multiply \zeikomi by 108\relax
\divide \zeikomi by 105\relax
%ここまでが(Ⅰ)に相当
\@tempcnta=#1\relax
\multiply \tempcnta by 1080\relax
\divide \@tempcnta by 105\relax
%ここまでが(Ⅱ)に相当
\@tempcntb=\zeikomi\relax
\multiply \@tempcntb by 10\relax
\advance \@tempcnta by -\@tempcntb \relax
%ここまでが(Ⅲ)に相当
\ifnum \@tempcnta>4 \relax
\advance \zeikomi by 1 ¥\relax
\fi 
%ここまでが(Ⅳ)に相当
\the\zeikomi}
~~~

今回のマクロは，四捨五入ののち整数値を出力する命令でしたが，これを応用することで「小数第２位を四捨五入して，小数第１位までの値を出力する」マクロも作製することができるでしょう。

###\the\カウンタ名のご紹介

TeXにおいては
~~~latex
\newcount\hoge
~~~
というように\hogeというカウンタを定義した際，
~~~latex
\the\hoge
~~~
という命令によってカウンタ値を出力することができます。

###\ifstarによる分岐

このマクロは，小数点以下を「切り捨てる」場合と「四捨五入する場合」の２つが文脈によって使い分けられることを想定しています。このような場合は，制御綴の文字列が似ている方が使用する際に便利ですよね。そこで，アスタリスクが文末にあるかないかで条件分岐を行う事ができ，

~~~latex
\def\hoge{\@ifstar\@hoge\@@hoge}
\def\@hoge#1#2...{アスタリスクがある場合の命令}
\def\@@hoge#1#2{アスタリスクがない場合の命令}
~~~

とコマンドを書くことで条件分岐が可能になります。

消費税引き上げによりこのマクロが過去の産物となってしまわないことを願って，筆を置かせていただきます。最後までご覧いただきありがとうございました。