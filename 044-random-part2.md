## ポアソン分布

ポアソン分布(Poisson distribution)とは、シメオン・ドニ・ポアソン（Siméon Denis Poisson 1781-1840）が1837年に発表した論文、「刑事民事の判決における確率の調査」で初めて公開されたものだ。この論文でポアソンは、ある国における冤罪の数について、ある時間間隔における冤罪の発生数を乱数とし、そのような乱数の分布について考察した。その結果がポアソン分布だ。

ある時間間隔に発生する離散的な事象の多くがポアソン分布に従う。例えば以下は具体的な例だ。


+ 1年間に発生する冤罪の数
+ 1ヶ月に発生する交通事故の数
+ 1年間で地球に飛来する隕石の数
+ 1時間である放射性同位体が放射性崩壊する回数


冤罪や交通事故の発生件数は乱数ではないように考えるかも知れない。しかし、結果的にみれば乱数のように振る舞っている。一ヶ月に発生する交通事故が10件であったとする。これは平均すると約3日に1回交通事故が起こっていることになるが、実際に3日に一回交通事故が起こったわけではない。交通事故の発生は離散的で、1日に複数件起こることもあれば、一週間無事故のときもある。なので3日に一回交通規制を敷いても交通事故を防ぐことはできない。

ポアソン分布に従う乱数の特徴としてもう一つ、無記憶性というものがある。3日間に1件の割合で交通事故が起こっているとしよう。その場合、常に今から3日以内に1件の交通事故が起きることが期待できるだけであって、3日以内に必ず起こるわけではない。そして、2日待ったから明日交通事故が起こるというわけでもない。交通事故が起こる確率は常に今から3日間につき1件だ。ポアソン分布は無記憶性なので、すでに2日間待っているという過去は未来に影響しない。

具体的なC++ライブラリのポアソン分布の使い方としては、ある所定の時間に平均して起こる事象の回数`mean`を指定すると、その所定の時間に起こった事象が乱数で返される。

### ポアソン分布(poisson_distribution<T>)


`std::poisson_distribution<T>`は整数型`T`の乱数$i$, $i \geq 0$を以下の離散確率関数に従って分布する。

$$
P(i\,|\,\mu) = \frac{e^{-\mu} \mu^{i}}{i\,!} \text{ .}
$$

ここで$\mu$を`mean`とする。

変数の宣言は以下の通り。

~~~c++
std::poisson_distribution<T> d( mean ) ;
~~~

`T`は整数型でデフォルトは`int`、`mean`は浮動小数点数型の値で所定の時間に平均して発生する事象の回数だ。

ポアソン分布が生成する乱数は0以上の事象が発生した回数となる。

例えば、1ヶ月に交通事故が平均して10件発生するとする。1ヶ月に発生した交通事故の件数を乱数で返す関数`traffic_accidents`は以下のようになる。

~~~cpp
template < typename Engine >
auto traffic_accidents( Engine & e )
{
    std::poisson_distribution d(10.0) ;
    return d(e) ;
}
~~~

これを10回呼び出すと以下のような乱数列が生成された。

~~~
14, 6, 11, 8, 8, 14, 7, 16, 12, 17, 
~~~

