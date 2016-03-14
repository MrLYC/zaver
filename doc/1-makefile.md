# zaver Makefile 文件学习
## 涉及函数
### wildcard
用法： `$(wildcard <pattern>)`

搜索符合 _<pattern>_ 的文件，并展开成空格分割的文件名列表。

### patsubst
用法： `$(patsubst <pattern>,<replacement>,<text>)`

将 _<text>_ 中的独立单词（有空白符分隔）分别匹配 _<pattern>_，若符合则替换为 _<replacement>_ _<pattern>_ 中可用 `%` 来表示任意字符，可将匹配结果传递到 _<replacement>_ 中。

## 生成
### 构建
在根目录执行`make`即可生成，相当于`make all` 我们注意到 _Makefile_ 中有这样一段：

```text
$(TARGET): build $(OBJECTS)
    $(LINK) $(TARGET) $(OBJECTS) $(LIBS)
```

构建目录为 _objs/src_

其中`$(TARGET)`为 _objs/zaver_，`$(OBJECTS)`展开为 _objs/src/_ 对应到 _src/_ 下的 _.o_ 文件。 例如 _src/_ 下有两个文件 _a.c_ 和 _b.c_，则这段相当于：

```text
objs/zaver: build objs/a.o objs/b.o
    $(LINK) $(TARGET) $(OBJECTS) $(LIBS)
```

### 编译
_Makefile_ 中还有这样一段：

```text
objs/%.o: %.c
    $(CC) -c $(CFLAGS) -o $@ $<
```

这段其实对应了`$(OBJECTS)`这个依赖，作用是将 _.C_ 文件编译成对应的 _.o_ 文件。 `$(OBJECTS)`展开后成为一个文件列表，这个方式成为多目标，会对每个目标都调用这个规则。

其中`$@`代表所有目标，`$<`代表所有依赖，如果目标是 _objs/src/a.o_，则这段相当于：

```text
objs/src/a.o: src/a.c
    gcc -c -g -Wall -Isrc -Wextra -o objs/src/a.o src/a.c
```

### 连接
连接命令为：

```text
$(LINK) $(TARGET) $(OBJECTS) $(LIBS)
```

如果目标是 _objs/src/a.o_，展开相当于：

```text
gcc -o objs/zaver objs/src/a.o -ldl -lpthread
```
