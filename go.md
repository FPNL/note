
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
