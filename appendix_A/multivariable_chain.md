## 逆伝播における逆伝播値の加算(p.281)の補足

ニューラルネットワークの順伝播において、ある中間変数(出力信号)が複数の経路に枝分かれする場合、逆伝播ではそれぞれの経路で計算された勾配を合算する。  
これは、「**多変数関数の連鎖律**」に依る.

---

## 多変数関数の連鎖律

関数  
$$
f = f\bigl(u(x,y),\;v(x,y)\bigr)
$$  
において、$(x,y)$ がまず$ (u,v)$ を決定し、つづいて $(u,v)$ から $f$ が決まる場合、多変数関数の連鎖律により

$$
\frac{\partial f}{\partial x}
= \frac{\partial f}{\partial u}\,\frac{\partial u}{\partial x}
+ \frac{\partial f}{\partial v}\,\frac{\partial v}{\partial x},
$$
$$
\frac{\partial f}{\partial y}
= \frac{\partial f}{\partial u}\,\frac{\partial u}{\partial y}
+ \frac{\partial f}{\partial v}\,\frac{\partial v}{\partial y}
$$

---

## Softmax-with-Loss レイヤへの適用

クロスエントロピー損失は  
$$
L = -\sum_{k} t_k \,\log y_k
$$  
と定義されており、Softmax の内部で  
$$
y_i = y_i\bigl(z\bigr),\quad z = \tfrac1S,\quad S = \sum_j \exp (a_j)
$$  
としていると考える。このとき

1. 中間変数 $(z=1/S)$ が分岐して各$ y_i$を決定する。  
2. さらに各 $y_i $が損失 $L$ を決定する。  

すなわち  
$$
L = L\bigl(y_1(z),\,y_2(z),\,y_3(z)\bigr).
$$  
多変数関数の連鎖律を適用すると、

$$
\frac{\partial L}{\partial z}
= \sum_{i=1}^3
   \frac{\partial L}{\partial y_i}\,
   \frac{\partial y_i}{\partial z} 
= 
   \frac{\partial L}{\partial y_1}\,
   \frac{\partial y_1}{\partial z} 
   +
   \frac{\partial L}{\partial y_2}\,
   \frac{\partial y_2}{\partial z} 
   +
   \frac{\partial L}{\partial y_3}\,
   \frac{\partial y_3}{\partial z}. 
$$

従って、「逆伝播のとき、枝分かれした各経路の勾配を加算する」ことになる. 

<!-- ---

## まとめ

- **順伝播**： (z) から複数の (y_i) が生じる（枝分かれ）。  
- **逆伝播**： 各 (y_i) の勾配 (\partial L/\partial y_i) をそれぞれ (z) へ戻し、合流点で和を取る。  
- これはまさに「多変数チェーンルール」による結果であり、合流時の加算はチェーンルールの一般形そのものです。

 -->
