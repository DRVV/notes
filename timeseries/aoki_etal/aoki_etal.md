// [INCLUDE=PRESENTATION]

~ MathDefs
\newcommand{\paren}[1]{\left ( #1 \right ) }
\newcommand{\curly}[1]{\left \{ #1 \right \} }
\newcommand{\mean}[1]{\bar{#1}}
~

# 時系列データとは

- 「対象のある側面を特定の時間間隔で測定観測した結果の集まりです」

	- 例：東京の毎日の気温，証券の終値を営業日間隔で観測した結果

- **観測者によって観測の時間間隔が変わる**

## 事象が生起した時間に意味がある場合　→　時系列データとは言わない

- たとえば地震．発生時刻やその時間間隔が意味を持っている

-  発生間隔を観測者が設定できない →　時系列データの定義を外れている

    - 為替取引がリアルタイムに記録されたもの

- この種のデータは点過程(point process)，マーク付き点過程データと呼ばれる

    - マークとは：事象が生起した時に記録される，発生時間以外の値のこと

        - 取引金額，マグニチュードなどか

## 視覚化する際の区別

- 時系列　→　折れ線

    - 横軸：観測時間，縦軸：観測値

- マーク付き点過程　→　縦棒

- **棒グラフと呼ぶのは間違い**

    - 棒グラフは類別変量を計数データを表示するための手法（目的は類別変量の要約）

        - 類別変量とは……たとえば性別「男性・女性」，服のサイズ「L，M，S」など，
		観測対象の区別を与えるデータ　（ラベル？）

	- ヒストグラムの目的は**数値変量の要約**

        - ヒストグラムは棒をくっつけて描く　→　連続性を意識


## 少し発展的な話

- データがある確率変数の実現値の集まりとする

- 密度関数・確率関数の形状を調べる際，ヒストグラムや棒グラフを描くことがある

- 実は，「データは同一の確率分布からの独立に抽出した標本の集まりである」と決定したことになります（？）

	- ヒストグラムを描くということは，１つの確率分布の密度関数の形状を調べようということに他ならない

	- しかも，ヒストグラムは標本抽出の順序を考慮していない（＝無作為の復元抽出）　→　毎回の標本抽出は互いに独立と認めている

- 時系列データに対してこのようなヒスとgラムを作り，それを妥当なものだと判断したのならば，抽出順（時間情報）は全く無意味であるということ

- 時系列分析では，この時間情報も重要な情報源と考える

    - 活用法はさまざまあるが，もっともシンプルな方法は時系列データをうまく説明する時系列モデルをデータに適用することです．

	- 次節以降，そのための基礎知識である時系列データと確率分布の関係について整理する

# 時系列データと確率分布

- 時系列データを収集することは，時間とともに観測対象が連続時間上で変化しているということを暗に仮定している

- 時系列データの収集とは「観測対象ないしその属性が連続時間上で変化すると仮定し，それらを事前に定めた時間間隔で記録する」こと

## 時系列分析の主目的と困難

- 観測対象のモデル化，つまり観測対象をうまく表現する連続時間を引数とした関数（＝「連続時間の関数」とよぶ）をデータから見つけ出すこと

- これはとても困難，なぜなら

    - 連続時間の関数の形は不明．しかも連続時間の関数は無限に考えられる．→すべての関数のあてはめを試すことは(物理的に)不可能

    - 観測対象が従う関数の形は，時間の経過とともに変化する可能性がある．つまり，推定対象の関数は機関を通じて１つであるという保証はない
	- 関数の形が明示的にわかった（分析者が強制的に設定した）としても，観測にはノイズが混入するので関数のパラメタの正確な推定は容易ではない

- なので

    - 観測者や分析者の知見をいれる
	- 先行研究の知見を入れる
	- 予備的なデータの分析

を通じて，**関数のクラスを絞り込み**，

- その中の**なるべく単純化**した時系列モデルを使って観測対象を表現することを目指す

## 単純化の例

- 離散時間の関数に置き換えてしまう……観測時点ごとにある現象が発生するとみなす
たとえば，各事象の発生の裏に確率分布の存在を仮定する　→　対象の確率分布を用いたモデル化が可能

- 具体例：ある株式の日次収益率$R_t$

	- $t$日の株式の終値$S_t$に対して $R_t = \frac{S_t -S_{t-1}}{S_{t-1}}$と定義する

	- 一つの「サイコロ」（＝確率分布）で記述されているとする

	- 正規分布を仮定することで，$\mu, \sigma$の推定問題に帰着する

        - 正規分布を仮定して良いか，本来は確認する必要がある
            - QQプロット
			- 適合度検定

	- このモデルでは，時間の情報を用いていない＝単一のサイコロを繰り返し振っていると考えている＝独立同一性の仮定

	- 時系列モデルは各試行ごとに異なるサイコロを振ると考えている

	- あきらかに一つのサイコロでは表せないものを次節で見る

# 株式収益率の時系列データ

- 統計手法の多くは，データの独立同一性を仮定している

    - 独立同一性がないデータに対して，独立同一性を仮定している検定をおこなっても無意味

- みのまわりに独立同一でないデータはある

    - 例：マツダの株式収益率には独立同一性はない

        - 11月から12月：平均が正で分散が小さい

		- 1月以降：過去の収益率との関係性がみえない．分散大



- 独立同一性のない分布に独立同一性を仮定したモデルを適用すると予測精度は悪いだろう

# 時系列分析に向けて

先ほどの株式収益率のモデル化を考えると， いずれかの事実を実現する必要がある

1. 当日の収益率は過去の収益率に依存

2. バラツキが日々変化

- これらは確率分布が日々変化する＝サイコロがコロコロ変わると思えば実現可能．

## 自己回帰モデル (AutoRegressive model)

- 例：$R_t = a + b R_{t-1} + \epsilon_t$,
	ここで a, b は定数，$\epsilon_t$は平均$0$, 分散$\sigma^2$の確率変数とする．

	実現値が$r_{t-1}$であれば，$R_t$は平均$a + b r_{t-1}$, 分散$\sigma^2$となる．
	つまり，サイコロは，何が実現値かで毎日変わる

	- こういう枠組みのモデルは__自己回帰モデル__と呼ばれる(Chap. 2, 3)

## 自己回帰条件付き分散不均一モデル(AutoRegressive Conditional Heteroscedasticity model)

- 現在の分散を過去の分散で表現する$\sigma_t^2 = a + b \sigma_{t-1}^2$

- 「条件付き」は時刻$t$の分散が時刻$t-1$の情報で予測可能としているから

- 一般化したものは__GARCH (Generalized ARCH)__と呼ばれる（Chap. 4）

## 注意

- あえて時系列情報を無視，独立同一性を仮定することもある

- 独立同一性の仮定をはばかられるデータがしばしばある　→ Chap. 5　単位根過程

	- 単位根過程の系列を独立同一に取り扱うと，__見せかけの回帰__問題が発生する

	- 単位根過程に関連する概念，共和分，ペアトレーディングにもふれる

# 3-1 時間依存の表現


## 自己相関

時系列データの各時点間での依存関係を時間依存とよぶことにする．時間依存性はどのように表現すればよいか？

- 1時点前との値との相関　= __ラグ1の自己相関__

- 自己相関の相関係数 = 自己相関係数 ...　*ではない*

- ラグ$h$の自己相関係数は次式で定義する

~Equation

	\frac{\sum_{t=h+1}^{n} \paren{r_t - \mean{r}}\paren{r_{t-h} - \mean{r}}}
	{\sum_{t=1}^{n} \paren{r_t - \mean{r}}^2},

~ 

where

~Equation
	\mean{r} := \frac{1}{n} \sum_{t=1}^{n} r_t
~

## コレログラム

横軸にラグ$h$，縦軸に自己相関係数をプロットしたもの

- Rでは`acf` (Auto Correlation Function)関数で計算可能

	- デフォルトでは帰無仮説信頼度95%区間も出力する

## 偏自己相関

ラグ1の自己相関が積み重なっている場合には，ラグ$h$の自己相関が見えてしまう．つまり関係「自己相関がある」は推移律を満たす．

推移律で結ばれる相関を除去した相関係数を__偏自己相関係数__という尺度で測る（定義なし）

- Rでは`acf(data, type="partial")`で計算可能

## 検定による自己相関の発見

自己相関・偏自己相関を観察するのが一般的だが，統計的検定に頼ることもできる→__Ljung-Box検定__ （定義なし）

- 帰無仮説「ラグ$k$までの自己相関係数がゼロである」の検定

- Rでは`Box.test(data, type="Ljung-Box")`で実行可能


## AR(AutoRegressive) modelの導入

$AR(p)$-modelの定義(Wikipedia) ：

~~ Equation
X_t = c + \sum_{i=1}^{p} \phi_i X_{t-i} + \epsilon_t,
~~
where $\phi_i (i=1,\dots, p)$ are *parameters* of the model, $c$ is a constant, and $\epsilon_t$ is *white noise*.

## Stationarity

定義：

- strong/strict stationarity:  cumulantが時間に依存しないこと。 →　mean, variance, ... など全てのmomentが時間に依存しない

~~ Equation
\forall k, \tau \quad F_X \paren{x_{t_1 + \tau}, \dots, x_{t_k+\tau}} = F_X \paren{x_{t_1}, \dots, x_{t_k}},
~~
where $F_X$ is the cumulative distribution of the unconditional joint distribution of $\curly{X_t}$.

- weak/wide-sense stationarity:  mean，autocovarianceが時間依存しないこと

例：

- White noise

- $Y$をscalar random variableとして$X_t = Y (t \in \mathbb{R}$　→　一系列は全部同じ値(e.g. $\curly{3,3,3, \dots}$) 

## AR(1)の性質を簡単に調べる

~~ Equation
X_t = c + \phi X_{t-1} + \epsilon_t
~~

~~ Theorem
$\abs{\phi} < 1$のとき，AR(1)は（漸近的に）stationaryである。
~~


~~ Proof
確率変数に関する方程式を漸化式と見て解くと
~~~ Equation
  X_t = c \sum_{i=0}^{t-1}\phi^i + \phi^t X_0 + \sum_{i=0}^{t-1} \phi^i \epsilon_{t-i}
~~~
$t$が十分大のとき，$\abs{\phi} < 1$であれば収束し，
~~~ Equation
  X_t = \frac{c}{1-\phi} + \sum_{i=0}^{t-1} \phi^i \epsilon_{t-i}
~~~
~~

~~ Remark
これをみると，期待値は確かに時間依存していないが，$t \to \infty$はどの程度必然的なのかよくわからん。
~~


