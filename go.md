
## import 小訣竅

1. 相對路徑是不行的
2. import 可以有別名
```go
import something "github.com/FPNL/comment_board/pkg/setting"
// 或是
import (
  something "github.com/FPNL/comment_board/pkg/setting"
)

func main() {
  something.GetSomething();
}
```
3. 關於版本號
   1. 引用時採用版本號前面的命名
   2. 如果要 package 名稱為版本號，要用別名，因為版本號通常自動忽略
```go
import (
  "github.com/FPNL/comment_board/api/v1"
  v2 "github.com/FPNL/comment_board/action/v2"
)

func main() {
  api.GetSomething()
  v2.DoSomething()
}
```

4. 別名有兩種特別命名
   1. `.`
      1. 不用特別指名引用包的名稱
   2. `_`
      1. 只會執行該包的 init()
```go
import (
    "math"
    . "fmt"
    _ "image/jpeg" // 會自動執行此包的 init 函式
)

func init() {
        image.RegisterFormat("png", pngHeader, Decode, DecodeConfig)
}

func main() {
  Println(math.Pi)
}
```


參考 [Go tips and tricks: almost everything about imports](https://scene-si.org/2018/01/25/go-tips-and-tricks-almost-everything-about-imports/)

## Go Struct with Error

如果有一個 struct 實作 Error()，要利用 function return 此結構體，
可以指定 function return `error`。

```go
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
```

## Go slice  的 capacity

slice 表達式 `s[lo:hi]`

之所以 slice 的 capacity 等於 hi - lo 是因為他不想要你超過記憶體裡面的區塊，
什麼意思呢？
我們可以看到製作一個 slice，參數分別為：`型別`、`長度`、`記憶體容量`
```go
slice := make([]int, 5, 0)
// cap(slice) 一定大於等於 len(slice)
```

所以在另外做一個切片，
```go
slice2 := [:5]
cap(slice2) // 可以發現 cap 等於 5
```
但是
```go
slice3 := [3:5]
cap(slice3) // cap 等於 2
```
為何會變小？ slice 不就是指針指向原本的陣列嗎？指針應該要忠實呈現該陣列的大小才對啊。

但是 slice3 是從第三個區塊開始映射，所以他要是跟你說：嘿，容量是五喔，然後你放進了五個東西，就造成了 overflow 了；

但是如果他跟你說：嘿，容量是 2 喔，所以你知道你的切片最多放進 2 個東西，

若這時有個 slice4 ，我不跟你講他的 ｃｏｄｅ，你不曉得他是映射陣列的哪段到哪段，
但你知道 cap(slice4) == 4，至少你不會多放東西進去。

反過來說，若 slice4 把它映射的陣列 capcity 原原本本地跟你說： cap(slice4) == 6 ，
但是你不知道的是 slice4 其實是從原先陣列倒數的第二個值映射到最後的值，
接著，你放進了 6 個東西，這就出現了問題。

```go
package main

import "fmt"

func main() {
	y := []int{1, 2, 3}
	a := make([]int, 8)
	printSlice("a", a) // a len=8 cap=8 [0 0 0 0 0 0 0 0]
	x := y[1:2]
	fmt.Println(len(a), cap(y)) // 8 3
	printSlice("x", x) // x len=1 cap=2 [2]
	b := make([]int, 0, 5)
	printSlice("b", b) // b len=0 cap=5 []
	c := b[:2]
	printSlice("c", c) // c len=2 cap=5 [0 0]
	d := c[2:5]
	printSlice("d", d) // d len=3 cap=3 [0 0 0]
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```
