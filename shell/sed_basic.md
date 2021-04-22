```sh
# 置換基本
# 行の先頭の#を削除
# gをつけないと最初に一致した箇所のみしか置換が適応されない
sed "s/#^//g" file_name.txt

# 変数を正規表現で利用する際には'シングルクオテーション'で
# -eで拡張正規表現??
sed -e 's/$SOME_VAR_1/$SOME_VAR_2/g' file_name.json

# -iでファイルを上書き。
#.dumpはバックアップファイルの拡張子
sed -i.dump "s/^#//g" file_name.yml
```