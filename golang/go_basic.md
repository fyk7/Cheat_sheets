## 変数・型について

```go
var x int
var x, y int
var x int = 1
var x, y int = 1, 2
x := 1

// キャスト
var x int32 = 100
var y int64 = int64(x)
var z float32 = float32(y)
// 暗黙の型変換は禁止されている

// 文字列->整数
// ascii to integer
var s string = strconv.Atoi(5)
// integer to ascii
var n int = strcov.Itoa("5")

// 定数
cosnt x int = 1
```

## Input 関数について

```go
package main

import (
    "buffio"
    "fmt"
    "os"
)

func main() {
    name := input("type your name")
    fmt.Println("Hello, " + name + "!!")
}

func input(msg string) {
    scanner := buffio.NewScanner(os.Stdin)
    fmt.Print(msg)
    // Scanすることによって、scannerの内部にスキャンした情報が保存される
    scanner.Scan()
    return scanner.Text()
}
```

## スライスの操作

```go
// スライスは配列の参照
// 長さと容量があり、容量は参照している配列のサイズ

func push(a []int, v int) {
    return append(a, v)
}

func pop(a []int) []int {
    return a[:len(a)-1]
}

func unshift(a []int, v int) []int {
    // 先頭に追加
    return append([]int{v}, a...)
}

func shift(a []int) []int {
    // 右にシフトして先頭を取り除く
    return a[1:]
}

func insert(a []int, v int, p int) []int {
    // スライスを1文字分拡張
    a = append(a, 0)
    //a[p]は一時的に重複するが、後で置き換えるから問題ない
    a = append(a[:p+1], a[p:len(a)-1])
    a[p] = v
    return a
}

func remove(a []int, p int) []int {
    return append(a[:p], a[p+1:]...)
}

// スライスのアンパックについて
// a... → a[0], a[1], a[2]
func push(a []string, v ...string) (s []string) {
    s = append(a, v...)
    return
}
```

## Map について

```go
func main() {
    m := map[string]int{
        "a": 100,
        "b": 200,
        "c": 300
    }
    m["total"] = m["a"] + m["b"] + m["c"]

    var sum int = 0
    for k, v := range m {
        fmt.Println(k, v)
        sum += v
    }

    // mの状態そのものが更新される
    // キーが無くてもエラーにはならない
    delete(m, "total")

    // mapは順序は保証しない
}
```

## 関数について

```go
// 関数は値である

// シグニチャ
// func 関数(arg　type) 戻り値

// 複数の戻り値(複数の戻り値は()で括る)
// func 関数(arg1 type1, arg2 type2) (戻り値1, 戻り値2)

// 名前付き戻り値は()で括る
// (s []string)が名前付き戻り値
func insert(a []string, v string, p int) (s []string) {
    s = append(a, "")
    // s[p]は一時的に重複するが、置き換えるため問題ない
    s = append(s[:p+1], s[p:len(s)-1]...)
    s[p] = v
    // 明示的にsを返さなくて良い
    return
}

//可変長引数について
func push(a [string, v ...string]) (s []string) {
    // バラバラの引数をまとめてスライスvに格納してから、v...でアンパックしているイメージ
    s = append(a, v...)
    return
}
// push(m, "1", "2", "3")のように使用する
// (変数 ...値) → (変数　[]型)と考える

// goでは関数は値
func main() {
    f := func(a []string) ([]string, string){
        retunr a[1:], a[0]
    }
}

// 高階関数
// pythonでいう functool.partial
// "stringのスライスを引数にとって、stringのスライスを返す"関数を引数に取る
modify := func(a []string, f func([]string) []string) []string {
    return f(a)
}

// クロージャについて
// 代入された関数は、周辺の環境もそのまま保持している(関数の外側にある変数など)
func main() {
    // dataが関数の外にある値
    data := "新しい値"
    m1 := modify(data)
    data = "new data"
    m2 := modify(data)

    // m1, m2のdataではそれぞれ違う値を参照している。関数の外にあるものまで一緒に保持している。まさにクロージャ。
    // data := "新しい値"を参照
    fmt.Println(m1)
    // data := "new data"を参照
    fmt.Println(m2)
}

func modify(d string) func() []string {
    m := []string{
        "1st", "2nd",
    }
    return func() []string {
        return append(m, d)
    }
}

// フォーマット出力
func main() {
    n := 123
    b := true
    s := "string"
    fmt.Printf("number:%d, bool:%t, string:%s.", n, b, s)
}
```

## ポインタ関連

```go
// ポインタ型(intのポインタ、stringのポインタ)が違っても、中身はint値
// ポインタでスライスを操作
func main() {
    ar := []int{10, 20, 30}
    fmt.Println(ar)
    initial(&ar)
    fmt.Println(ar)
}
func initial(ar *[]int) {
    for i := 0; i < len(*ar); i++ {
        // *ar[i]とすると、i番目のスライスの実体・値のアドレスとなってしまう
        (*ar)[i] = 0
    }
}
```

## 構造体関連

```go
// type mydata struct {} 意外にもvarで変数として宣言できる
var mydata {
    Name string
    Data []int
}

// typeで型として定義できる
type myint int

// 外部から利用可能な型は関数などは必ずコメント
// Mydata is structure.
type Mydata struct {
    Name string
    Data []int
}

func main() {
    taro := MyData{"Taro", []int{1, 2, 3}}
    // 変数の数が多い場合は名前を指定すると良いかもしれない
    hanako := MyData{
        Name: "Hanako",
        Data: []int{4, 5, 6},
    }
}

// 構造体を関数の引数として渡す場合に、普通に構造体を引数として渡すと、構造体のコピーがどんどん作成されてしまう。そのため、ポインタで渡すことが多い

//構造体の生成
taro := new(Mydata) // ポインタが返る, 構造体内の変数は初期化されない
taro := Mydata{"Taro", []int{0,0,0,0,0}} // 実体(値)が返る

// makeは配列・スライス、マップ、チャンネルに飲み使用できる初期化関数
// 値を作成するというよりかは"初期化"といったイメージが近い
taro.Name = "Taro"
// 長さ5, 容量5の空のスライスを作成した
taro.Data = make([]int, 5, 5)
// &{Taro [0 0 0 0 0]}

// 構造体には"値"意外にも関数を追加できる
// この時構造体は関数の"レシーバ"となり、この関数はメソッドと呼ばれる
// Mydata is structure.
type Mydata struct {
    Name string
    Data []int
}
func (md Mydata) PrintName() {
    fmt.Println(md.Name)
}
// taro.PrintName

// 直接ポインタからメソッドを呼び出せる
// (*md).Nameとしなくて良い
func (md *Mydata) PrintName() {
    fmt.Println(md.Name)
}
// taro.PrintName
// (*taro).PrintName
```

## インターフェースについて

```go
package main

import (
    "fmt"
)

// Data is interface.
type Data interface {
    Initial(name string, data []int)
    PrintData()
}

// Mydata is Struct.
type Mydata struct {
    Name string
    Data []int
}

func (md *Mydata) Initial(name string, data []int) {
    md.Name = name
    md.Data = data
}

func (md *Mydata) PrintData() {
    fmt.Println(md.Name, md.Data)
}

func main() {
    var ob Mydata = Mydata{}
    ob.Initial("Taro", []int{1, 2, 3})
    ob.PrintData()
}

// MydataはDataのインターフェースを満たしているため、Mydata型はDataとしても扱える
func main() {
    // newで初期化 // obに代入する変数の型にあわせて値が扱われる
    // var ob Data = Mydata{}はできない
    // Mydataの値をData型変数に代入できないから
    // var ob Data = Data{}もできない
    var ob Data = new(Mydata)
    // obはData型だが、obの値はMydata型
    ob.Initial("Taro", []int{1, 2, 3})
    ob.PrintData()
}

// interfaceで宣言しているメソッド以外のメソッドを定義
func (md *Mydata) Check() {
    fmt.Printf("Check! [%s]", md.Name)
}
// 問題ない
func main() {
    var ob Mydata = Mydata{}
    ob.Initial("Taro", []int{1, 2, 3})
    ob.Check()
}
// ob.Check undefied error
func main() {
    // obはData型でData型にはCheckは無い(obの値はMydata型であるが)
    var ob Data = new(Mydata)
    ob.Initial("Taro", []int{1, 2, 3})
    ob.Check()
}

```
