questions focus on Golang itself，rather than algorithm and so on。If this repo gives you any sort of help,pleasee push the :star: button,thanks.

这里时我整理得一些 Golang 思考点,用来深理解何思考之用,如果这些问题给你了些启发，请打个:star:，谢谢。

# 1-make与new有什么区别?

<details>
  <summary>answer</summary>
new(T) 返回 T 的指针 *T 并指向 T 的零值。
make(T) 返回的初始化的 T，只能用于 slice，map，channel
</details>

# 2-byte与rune有什么区别？

<details>
  <summary>answer</summary>
 Go语言中byte和rune实质上就是uint8和int32类型。byte用来强调数据是raw data，而不是数字；而rune用来表示Unicode的code point。
</details>

# 3-运行go install 会发生什么？

<details>
  <summary>answer</summary>
go install只会检查“参数指定的包所在的GOPATH”内的源码是否有更新，如果有则重新编译。对于依赖的其他GOPATH下的包，如果存在已经编译好的.a文件，则不会再检查源码是否有更新，不会重新编译,将 main 包生成二进制文件放到GOBIN目录下。将非 main 包编译成.a文件放到项目对于的 pkg 目录下。
</details>


# 4-一个标准的 Golang 工程的目录结构是怎样的？

<details>
	<summary>answer</summary>
	like this https://github.com/golang-standards/project-layout
</details>
	

# 5-Golang 中的 package 命名有什么原则？

<details>
  <summary>answer</summary>
  小写、清晰、简明、一个单词、不要使用下划线等字符
</details>

# 6-Golang 中如何使用 goto 语句？

<details>
  <summary>answer</summary>
  Go语言中有标签这一概念，来配合 for、switch、select 进行使用，形式为某一行的第一个以冒号结尾的单词，比如
  <pre>
LABEL1:
    for i := 0; i <= 5; i++ {
        for j := 0; j <= 5; j++ {
            if j == 4 {
                continue LABEL1 //其执行效果相当于是 break
            }
            fmt.Printf("i is: %d, and j is: %d\n", i, j)
        }
    }

}
  </pre>
  本段代码中，continue将会指向 LABEL1 而不是继续,演示效果如下所示：
  <pre>
i is: 0, and j is: 0
i is: 0, and j is: 1
i is: 0, and j is: 2
i is: 0, and j is: 3
i is: 1, and j is: 0
i is: 1, and j is: 1
i is: 1, and j is: 2
i is: 1, and j is: 3
i is: 2, and j is: 0
i is: 2, and j is: 1
i is: 2, and j is: 2
i is: 2, and j is: 3
i is: 3, and j is: 0
i is: 3, and j is: 1
i is: 3, and j is: 2
i is: 3, and j is: 3
i is: 4, and j is: 0
i is: 4, and j is: 1
i is: 4, and j is: 2
i is: 4, and j is: 3
i is: 5, and j is: 0
i is: 5, and j is: 1
i is: 5, and j is: 2
i is: 5, and j is: 3
  </pre>
</details>

# 7-Golang 中的如何打印一个 Unicode 字符串中的每一个字？

<details>
  <summary>answer</summary>
<pre>
	for pos, value := range "我是中国人" {
		fmt.Printf("val:%#U, pos:%d", value, pos)
	}
</pre>
</details>

# 8-如何跨平台编译？
<details>
  <summary>answer</summary>
<pre>
CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build main.go
</pre>
缺点是无法使用 cgo，一个第三方工具是 xgo，其准备了各式各样的编译环境，详见 https://github.com/karalabe/xgo
</details>

# 9-Go中如何判断一个实例是否实现了某个接口？
<details>
  <summary>answer</summary>
因为接口与实现只依赖于判断两个类型的方法，所以没有必要定义一个具体类型和它实现的接口之间的关系。也就是说，尝试文档化和断言这种关系几乎没有用，所以并没有通过程序强制定义。下面的定义在编译期断言一个*bytes.Buffer的值实现了io.Writer接口类型:
<pre>
// *bytes.Buffer must satisfy io.Writer
var w io.Writer = new(bytes.Buffer)
</pre>
因为任意*bytes.Buffer的值，甚至包括nil通过(*bytes.Buffer)(nil)进行显示的转换都实现了这个接口，所以我们不必分配一个新的变量。并且因为我们绝不会引用变量w，我们可以使用空标识符来进行代替。总的看，这些变化可以让我们得到一个更朴素的版本：
<pre>
// *bytes.Buffer must satisfy io.Writer
var _ io.Writer = (*bytes.Buffer)(nil)
</pre>
</details>

# 10-写出四种初始化一个空字符串的方式
<details>
  <summary>answer</summary>

<pre>
s := ""
var s string
var s = ""
var s string = ""
</pre>
第一种形式，是一条短变量声明，最简洁，但只能用在函数内部，而不能用于包变量。第二种形式依赖于字符串的默认初始化零值机制，被初始化为""。第三种形式用得很少，除非同时声明多个变量。第四种形式显式地标明变量的类型，当变量类型与初值类型相同时，类型冗余，但如果两者类型不同，变量类型就必须了。实践中一般使用前两种形式中的某个，初始值重要的话就显式地指定变量的类型，否则使用隐式初始化。

# 11-嵌入式链表输入在golang中表达?
<details>
  <summary>answer</summary>

<pre>
介入式链表的存在主要时为了克服,在C语言链表中出现的将数据定义在strut中.
golang 中的实现,首先定义一个基础的可索引的interface,并定义一个链表struct "List"
该List 实现上面索引interface,并添加对应的链表方法
最终将之前的预定义的结构体中,使用匿名嵌套结构体的方式包含该List结构体即可
详见:https://sheepbao.github.io/post/golang_list/
</details>

