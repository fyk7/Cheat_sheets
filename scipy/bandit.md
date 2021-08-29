## Banditアルゴリズム周り
### beta分布(事後分布)
```python
# beta分布(コイントスで表が出る回数の分布)
a = 4 # 表が出た回数
b = 10 - 4 # 裏が出た回数
p = 0.2 # 表が出る確率

# 表が出る確率が0.2における確率密度
scipy.stats.beta.pdf(p, a + 1, b + 1)
# 表が出る確率の累積確率密度(0~0.2の和)
scipy.stats.beta.cdf(p, a + 1, b + 1)
# 累積確率密度の逆関数(累積確率密度 -> 確率)
scipy.stats.beta.ppf(p, a + 1, b + 1)
```

### 正規分布、t分布(事後分布)
```python
# 基本的には beta -> t, beta -> normに変更するだけ
# t分布
scipy.stats.t.pdf(p, a + 1, b + 1)
# 正規分布
scipy.stats.norm.pdf(p, a + 1, b + 1)
```

### 確率密度関数の描画
```python
a = 4
b = 10 - 4
x = linspace(0, 1, 10000)
# beta分布に従った確率密度関数の描画
plt.plot(x, scipy.stats.beta.pdf(x, a + 1, b +1))

# 以下を試してみると大数の法則を感じることができる
(a, b) = (4, 6)
(a, b) = (40, 60)
(a, b) = (400, 600)
# 90%信頼区間が徐々に狭くなっていくことが確認できる
```
