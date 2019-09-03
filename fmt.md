## 3.2.3 日志包

Go 在标准库中有一个 log 包和 logger 类型。使用 log 包将为您提供成为优秀公民 (译注：指 log 包兼容性非常好) 所需的一切。您将能够写入所有标准设备，自定义文件或支持 io.Writer 接口的任何目标。

主要结构体是：

```
type Logger struct {
  mu sync.Mutex
  prefix string
  flag int
  out io.Writer
  buf []byte
}


func New(out io.Writer, prefix string, flag int) *Logger {
  return &Logger{out: out, prefix: prefix, flag: flag}
}
```

在log包中通过New函数得到一个Logger结构体指针，这个函数的三个参数分别是out，prefix，flag。pfefix可以指定日志信息的前缀，比如“[Debug]”等，一般根据实际需要定义，可根据情况随时通过SetPrefix()函数修改。flag是日志的前缀信息（在prefix之后），包括可配置的时间格式等，一般默认为LstdFlags就可以了。out是日志输出的目标，只要实现了io.Writer接口就可以作为out，log包中默认指定stderr为out，所以log包默认都是输出到标准设备。

```
var std = New(os.Stderr, "", LstdFlags)

func Println(v ...interface{}) {
  std.Out,put(2, fmt.Sprintln(v...))
}
```
也可以按照上面的思路，把信息填写到文件里
```
logfile, err := os.OpenFile("my.log", os.O_RDWR|os.O_CREATE|os.O_APPEND, 0644)
if err != nil {
	log.Fatalln("fail to create log file!")
}
defer logfile.Close()

l:=log.New(logfile, "", log.LstdFlags)
l.Println("test")
num:=5
l.Printf("test %d",num)
```

因为logfile已经实现了io.Writer，所以这里用做out，日志信息被写入到文件。log的方法Printf()可以把信息按照一定格式来写入。另外，在写入日志信息时都有加入并发锁，这是mu sync.Mutex的作用。

最后，log包的日志功能基本上能满足一般的开发需要，但相对还是比较简单，缺少日志分层控制，缺少对json格式的支持等，所以如果有需要灵活定制或大并发、大吞吐量的日志开发需求，建议考虑使用其他方法或途径来实现。


```
  Ldate         = 1 << iota     // 当地时区的日期：2009/01/23
  Ltime                         // 在当地时区的时间：01:23:23
  Lmicroseconds                 // 微秒分辨率：01：23：23.123123。 假设Ltime。
  Llongfile                     // 完整的文件名和行号：/a/b/c/d.go:23
  Lshortfile                    // 最终文件名元素和行号：d.go:23。 覆盖Llongfile
  LUTC                          // 如果设置了日期或时间，请使用UTC而不是本地时区
  LstdFlags     = Ldate | Ltime // 标准记录器的初始值)
```


