```sh
# aws s3基本
# file_pathはそれぞれs3上でもローカル上でも問題ない(両方ローカルはわからない)
aws s3 cp <from_file_path> <to_file_path>

# フォルダをコピーしたい場合
# --recursiveオプションをつける必要がある
aws s3 cp <from_file_path> <to_file_path> --recursive

aws s3 ls --profile another-profile
aws s3 sync local/path s3://XXXXXXXXXXXXX/locationData --delete --include "*" --acl public-read
aws s3 rm s3://XXXXXXXXXXXXX --recursive
```
