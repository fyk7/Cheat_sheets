## Installation and Basic Usage

```sh
$ python -m virtualenv venv
$ source venv/bin/activate # venvと同じインターフェース
$ pip install pytest

path <- cwd
	tasks
		test_three.py
		test_four.py

$ pytest tasks/test_three.py tasks/test_four.py # ファイルを指定して実行
$ pytest tasks

tasks <- cwd
	test_three.py
	test_four.py

$ pytest test_three.py
```

## Options

```sh
-v #詳細表示
-q #簡易表示
-h #ヘルプ表示
—collect-only #テスト予定一覧出力(テスト実行はしない?)
-k “word1 or word2” # フィルタ。word1, word2 を含む test func のみを実行
-m mark1 #マーカー。　@pytest.mark.mark1 が付いている test func のみを実行
-x #テストが失敗したらそこで停止
-tb=no # traceback を出力しない
—maxfail=n # n 回失敗したらテスト中止
-s # print 分も出力
—lf #最後に失敗したテストのみ実行
-l #local 変数表示
—tb=line #失敗している箇所１行
—tb=short
—dutations=3 #もっとも時間がかかった上位 3 つのテストを表示
```

## Features

```sh
src
tests
    conftest.py
    pytest.ini # プロジェクト全体の pytest の構成情報,振る舞いを変更するディレクティブ
    func
        some_func_test.py
    unit
        some_unit_test.py
```

- simple syntax

```python
assert something <- assertTrue(something)
assert a == b <- assertEqual(a, b)
assert a <= b <- assertLessEqual(a, b)
```

- Exception Handling

```python
def some_test():
    with pytest.raises(TypeError): #例外が発生しなかったら失敗
    raise_occuer_func()

def test_start_db_raises():
    with pytest.raises(ValueError) as exec:
        tasks.start_db(‘some/great/path’, ‘mysql’)
        exception_msg = exec.value.args[0]
        assert exception_msg == “db_type must be a ‘tiny’ or ‘mongo’”
```

- Test With Marker

```python
@pytest.marker.smoke
def test_list_raises():
    pass

@pytest.mark.get
@pytest.mark.smoke
def test_get_raises():
    pass
```

```sh
$ pytest -v -m ‘smoke’ test_exceptions.py # 両方実行
$ pytest -v -m ‘get’ test_exceptions.py #下のみ実行
$ pytest -v -m ‘smoke and get’ test_exceptions.py #上のみ実行
```

- Fixture

```python
@pytest.fixture(autouse=True) # autouse <-全テストでfixtureを使用
def init_db(tmpdir):
    tasks.start_db(str(tmpdir), ‘tidy’)
    yield # テスト実行
    tsks.stop_db()
```

- Skip the test

```python
@pytest.mark.skip(reason=‘some*reason’)
@pytest.mark.skipif(tasks.**version**<‘0.2.0’, reasone=‘some_reason’)
```

```sh
$ pytest -rs test\*.py # 失敗したreasonを表示する
```

- Expected to Fail Test

```python
@pytest.mark.xfail() # XPASS の場合、間違って成功してしまっている
```

```sh
・pytest -v -tb=no で特定のファイル・ディレクトリのテストを実行するためのパスを取得
```

- Test of Single Function

```sh
$ pytest -v test/func/test_add.py::test_add_returns_valid_id
$ pytest -v tests/func/test_api_exceptions.py::TestUpdate::test_bad_id
```

- Test with Keyword

```sh
$ pytest -v -k \_raises
$ pytest -v -k “\_raises and not delete” # and, or が含まれる場合はクオテーション
```

- Test with Parameter

```python
@pytest.mark.parametrize(‘<引数名(複数可)>’, ‘<引数値リスト>’)
def test_add(<引数名>): # 複数の引数値を代入し実行
    pass
```

- Fixture

```python
# 全ファイルで使用する共通フィクスチャを conftest.py に記述する
# import conftest のようにしないこと
# pytest は tmpdir というフィクスチャを提供している(一時ファイルを準備)
@pytest.fixture()
def tasks_db(tmpdir):
    tasks.start_tasks_db(str(tmpdir), ‘tiny’) #セットアップ # TinyDBを使用している
    yield #制御がテストに渡される
    taskl.stop_tasks_db() # ティアダウン
```

- 各テスト関数に書くと良いかもしれないコメント

```python
# GIVEN (前提) <- フィクスチャに定義
# WHEN (こうしたら)
# THEN (こうなる)

# テストの焦点を定める(前提条件を目立たせない)
```

```sh
pytest —setup-show test_file.py #フィクス茶がいつ実行されるかが表示される
```

```sh
@pytest.fixture()
def a_tuple():
    return (1, ‘foo’, None, {‘bar’: 23})

def test_a_tuple(a_tuple):
    assert a_tuple[3][‘bar’] == 32 #Error

# fixture側での不整合であるからFailではなくErrorを出力
```
