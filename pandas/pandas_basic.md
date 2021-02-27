```python
# df生成
df = pd.DataFrame({“a”: [4, 5], “b”: [6, 7], “c”: [8, 9]}, index=[1, 2, 3])
df = pd.DataFrame([[4, 5], [6, 7], [8, 9]], index=[1, 2, 3], columns=[“a”, “b”, “c”])
# multiIndex  -> index部分を index = pd.MultiIndex.from_tuples([(‘d’, 1), (‘d’, 2), (‘e’, 2)],names=[’n’, ’v’])
# method chain
df = (pd.melt(df).rename(columns={‘variable’: ’var’, ‘value’: ’val’}).query(‘val >= 200’))

pd.melt(df) <= > df.pivot(columns=‘var’, values=‘val’)
pd.concat([df1, df2])  # 縦に結合
pd.concat([df1, df2], axis=1)  # 横に結合

# df変更
df.sort_values(‘mpg’)
df.sort_values(‘mpg’, ascending=False)  # inplace=Trueと同じ意味
df.rename(columns={‘y’: ‘year’})
df.sort_index(axis=1, ascending=True)
df.reset_index(drop=True)
df.drop(columns=[‘Length’, ‘Height’], inplace=True)


# df抽出
df[df.length > 7]
df.drop_duplicates()
df.sample(frac=0.5)
df.sample(n=10)
df.iloc[10:20]
df.iloc[1:3, [0, 4]]
df.nlargest(n, ‘value’)
df.nsmallest(n, ‘value’)
df[[‘width’, ‘length’, ’species’]]
df[‘width’] or df.width
df.filter(regex=‘regex’)
df.loc[:, ‘x2’: ‘x4’]
df.loc[1:3, [‘Age’, ‘Height’]]
df[[‘Age’, ’Name’]][[1:3]]
df.loc[df[’a’] > 10, [‘a’, ‘c’]]
df[df > 0]  # 0以下はNaN
df.where(df > 0)
df.mask(df > 0)  # 0以上はNaN
df[df['Grade'].isin(['A', 'B'])]

# 要約
df[‘w’].value_connts()
len(df)
df[‘w’].nunique()
df.describe()
sum, count, median, quantile([0.25, 0.75]), apply(
    func), min, max, mean, var, std

# 欠損
df.dropna()
df.fillna(value)

df.assign(Area=lambda df: df.length*df.Height)
df[‘Valume’] = df.Length*df.Height
pd.qcut(df.col, n, labels=False)  # n個に区分け
pd.clip(-10, 10)
pd.abs()

# グループ化
df.groupby(by=“col”)
df.groupby(level=“ind”)  # indexレベルのグループ化
size, agg(func),
shift(1), shift(-1),  # groupの中でシフト
rank(method=‘dense’), rank(method=‘min’), rank(pct=True), rank(method=‘first’),
cumsum(), cummax(), cummin(), cumprod()  # 累積系
df.expanding()
df.rolling(n)  # groupbyからのrolling

# プロット
df.plot.hist()
df.plot.scatter(x=‘w’, y=‘h’)

# 結合
pd.merge(df_a, df_b, how=‘left’, on=‘x1’)
pd.merge(df_a, df_b, how=‘right’, on=‘x1’)
pd.merge()df_a, df_b, how =‘inner’, on =‘x1’)
pd.merge()df_a, df_b, how=‘outer’, on=‘x1’)

df_a[df_a.x1.isin(df_b.x1)]
df_a[~df_a.x1.isin(df_b.x1)]

pd.merge(df_y, df_z)  # 両方に現れる行
pd.merge(df_y, df_z, how=‘outer’)  # 片方もしくは両方に現れる行
# df_yにあるがdf_zにはない
pd.merge(df_y, df_z, how=‘outer’, indicator=True).query(‘_merge ==“left only”’).drop(columns=[‘_merge’])

# 基本設定
pd.options.display.max_columns=None
pd.set_option(‘display.max_columns’, None)
pd.options.display.max_rows=10

# データI/O
pd.read_csv(‘.csv’, header=0)
df.to_csv(‘.csv’, index=False)
pd.read_excel(‘.xlsx’, sheetname=’Sheet1’)
df.to_excel(‘.xlsx’, sheet_name=’Sheet1’)
pd.read_hdf(‘.h5’, ‘df’)
df.to_hdf(‘.h5’, ‘df’)

# インデックス、カラム、値
.index, .columns, .values

# 基本情報
.info(), .shape, .count(), .dtypes, len(df) - df.count(), describe()

# よく行う操作
.T, any(), .all(), .astype(‘float64’), .map({‘Female’: 0, ‘Male’: 1}), .value_counts(), .get_dummies(df[‘Club’])

# columns関連
df[‘AB’]=df[‘A’] + df[‘B’]

# 欠損値
.dropna(), dropna(how=‘all’), fillna(0), fillna(method=‘ffill’)
```
