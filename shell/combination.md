```sh
# "kubectl"というワードを含むコマンドの履歴を抽出し、重複を解決して表示
# uniqは隣接した重複しか解決しないため先にsortする必要あり
history 100| awk '{$1=""; print $0}' | grep kubectl | sort | uniq -c | sort -r
```
