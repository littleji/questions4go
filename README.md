questions focus on Golang itself，rather than algorithm and so on。

# 1-make与new有什么区别?

# 2-byte与rune有什么区别？

# 3-运行go install 会发生什么？

# 4-一个标准的 Golang 工程的目录结构是怎样的？

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

# 7-Golang 中的如何打印一个 Unicode 字符串中的每一个子？

<details>
  <summary>answer</summary>
<pre>
	for pos, value := range "我是中国人" {
		fmt.Printf("val:%#U, pos:%d", value, pos)
	}
</pre>
</details>
