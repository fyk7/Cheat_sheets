## 日本語の分かち書き

### MeCab

```sh
# linux
$ sudo apt install mecab libmecab-dev mecab-ipadic
# mac
$ brew install mecab mecab-ipadic
# for python
$ pip install mecab-python3
```

### MeCab

```python
import MeCab
tagger = MeCab.Tagger()
# 以下のように辞書を-dで選択することも可能
# tagger = MeCab.Tagger('-d /usr/lib/mecab/dic/mecab-ipadic-neologd')
# 分かち書きのみ
# tagger = MeCab.Tagger('-Owakati')
node = tagger.parseToNode('私は私のことが好きなあなたが好きです')
while True:
    print(node.surface)
    # 連結リストのようなインターフェース
    node = node.next
```

### janome

## 日本語の正規化

### NEologdn

```sh
$ pip install neologdn
```

```python
import neologdn
neologdn.normalize('何かしらの全角半角を含む 日本語　の文章123１２３')
```

```python
import unicodedata
normalized = unicodedata.normalize('NFKC', '（株）')
# NFKCはNFD, NFCより大胆な変換を行う(強い正規化)
```

### Mecab, neologdn, unicodedata を用いた tokenizer

```python
import MeCab
import neologdn
import unicodedata

def tokenize(text):
    text = unicodedata.normalize('NFKC', text)
    text = neologdn.normalize(text)
    text = text.lower()

    node = MeCab.Tagger().parsenode(text)
    result = []
    while node:
        features = node.feature.split(',')
        if features[0] != 'BOS/EOS':
            if features[0] not in ['助詞', '助動詞']:
                # 6番目は原型
                token = features[6] if features[6] != * else node.surface
            result.append(token)
        node = node.next
```

## Bag of Word

### CountVectorizer, TfidfVectorizer

```python
from sklearn.feature_extension.text import CountVectorizer
vec = CountVectorizer(tokenizer=some_tokenizer)
# vec = TfidfVectorizer(tokenizer=some_tokenizer, ngram_range=(1, 3))
# list of texts.
vec.fit(texts)
bow = vec.transform(texts)
# bow's class is scipy.sparse.csr.csr_matrix
print(bow.__class__)
```

- 文章を one-hot に表現(単語が出現したら 1。逆は 0)
- TfidfVectorizer も CountVectorizer と全く同じインターフェースで使用可能

  - なお tf-idf は一文中の単語の出現頻度と全文章中の単語の出現頻度を考慮している
  - 前者が大きければ単語の重みは大きくなるが、後者が大きくなると単語の重みが小さくなる

## 言語の行列表現ベクトル化

### trancatedSVD

```python
from sklearn.decomposition import TruncatedSVD
svd = TruncatedSVD(n_components=4, random_state=42)
# bowはCountVectorizer等で算出済みとする
svd.fit(bow)
decomposed_features = svd.transform(bow)
```

### gensim

```sh
$ pip install gensim
```

### 例 1

```python
import gensim.downloader as api
model = api.load('glove-wiki-gigaword-50)
tokyo = model['tokyo']
japan = model['japan']
france = model['france']
v = tokyo - japan -france
print(model.wv.similar_by_vector(v, topn=1)[0])
```

### 例 2

```python
from gensim import Word2Vec
# https://github.com/Kyubyong/wordvectorsから日本語Word2Vecのモデルをダウンロード
model = Word2Vec.load('./ja/ja.bin')
tokyo = model['東京']
japan = model['フランス']
france = model['日本']
v = tokyo - japan -france
print(model.wv.similar_by_vector(v))
```

## その他言語系の前処理に関連した

### ハッシュタグの削除

```python
def clean_hashtag(text):
    cleaned_text = re.sub(r'( #[a-zA-Z]+)+$', '', text)
    cleaned_text = re.sub(r'( #[a-zA-Z]+) ', r'¥1', cleaned_text)
```

### 英語の除去

```python
import string
def filter_by_ascii_rate(text, threshold=0.9):
    ascii_letters = set(string.printable)
    rate = sum(c in ascii_letters for c in text) / len(text)
    return rate <= threshold
```

## ディープラーニングで使用する utils

### pathlib の使い方

```python
from pathlib import Path
p = Path('dir1)
list(p.glob('*'))
list(p.glob('*.txt'))
list(p.glob('**/*.txt'))
```

### tensorflow モデルの保存・呼び出し

```python
# モデルの構造・重みを同時に保存・呼び出し
model.save('model.h5')
from tensorflow.keras.models import load_model
model = load_model('model.h5')
```

- シンプルにモデルをそのまま保存するには model.save()を呼べば良い

```python
# モデルの構造・重みを同時に保存・呼び出し
json_string = model.to_json()
with open('model_arch.json', 'w') as f:
    f.write(json_string)
model.save_weight('weight.h5')

# モデルの読み込み
from tensorlfow.keras.models import model_from_json
with open('model_arch.json', 'r') as f:
    json_string = f.read()
    model = model.model_from_json(json_string)
model.load_weight(weight.h5)
```

- モデルの構造は json ファイル、モデルの重みは h5 ファイルに保存する
