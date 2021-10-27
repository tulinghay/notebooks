#### 黏包、半包

当数据传送过程中导致数据错位，源数据如下

```tex
huangda \n
this world is so beautiful\n
and you must to support \n
```

传送变成了,错位

```tex
huangda \nthis world 
is so beautiful\n
and you must to support \n
```

#### transferTo

使用此方法需要注意，其文件最大为2G，返回值当前执行传送的大小，可以通过以下方法处理

```java
public void transferTo(FileChannel in,FileChannel out) throws IOException {
    long size=0;
    for(size=in.size();size>0;){
        size-=in.transferTo(0,size,out);
    }
}
```

#### 阻塞

socket中的accept和read都会发生阻塞

#### SocketChannel事件类型

accept-请求连接时触发

connect-客户端连接建立触发

read-读事件

write-写事件