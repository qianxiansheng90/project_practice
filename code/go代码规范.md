# 代码规范

# 说明

代码规范是提供一个良好的编程规范，用于统一代码风格以及约束编程行为。
代码规范如果存在不合理之处或者有更好的建议，请联系**fangyuan.qian@qunai.com**。

### 约束说明
<font color='green'>[Good]</font> 表示强制，规范必须遵守。

<font color='red'>[Bad]</font> 表示避免，规范一定要避免。

<font color='blue'>[Advise]</font> 表示建议，不做强制性要求。

# 代码风格
### 命名
命名需要遵循以下原则：
+ 命名需要简洁，最好见名知义。
+ 采用通用的缩写命名，例如buf 而不是bufio。如果缩写会产生歧义，则应选择放弃或者换一个。
+ 禁止使用go保留关键字。

#### 文件命名
文件命名只能使用小写字母(a-z)、数字(0-9)、下划线(-)，并且只能以字母开头。
测试文件文件名需要以 "_test" 结尾。

<font color='green'>[Good]</font>
```
api.go
api_test.go
mysql_connect.go
handler_500.go
```
<font color='red'>[Bad]</font>
```
API.go
apiTest.go
MySQLConnect.go
HTTP_Handler.go
500_handler.go
404-handler.go
```

#### package 命名
package命名只能使用小写字母(a-z)、数字(0-9)、下划线(-)，并且只能以字母开头。

<font color='green'>[Good]</font>
```
package mysql_connect
```
<font color='red'>[Bad]</font>
```
package mysqlConnect
package mysql-Connect
```

<font color='blue'>[Advise]</font>

每个包需要有一个文件名和包名相同的文件，并在文件的头部注释说明当前包的用途。

#### 常量、结构体、变量、函数命名
命名只能使用小写字母(a-z)、大写字母(A-Z)、数字(0-9)，并且只能以字母开头。
首字母为小写字母则只允许内部包访问，首字母为大写字母则外部包可以访问。
禁止使用全大写，严格按照驼峰命名。
测试函数命名需要以Test开头。如果是对某个场景的测试用例分组，则可以使用下划线(_)，例如：TestMyFunction_WhenIDOverflow。

<font color='green'>[Good]</font>
```
const genParallel = 10

type AuditMySQL struct{}

var mysqlConnectDSN string

func MyFunction()

func TestMyFunction(t *testing.T)

func TestMyFunction_WhenIDOverflow(t *testing.T)
```
<font color='red'>[Bad]</font>
```
const gen_parallel = 10

type audit-MySQL struct{}

var _MySQL_CONNECT_DSN string

func MYFUNCTION()
```

#### json tag
json tag 推荐使用小写字母(a-z)、数字(0-9)、下划线(_)并且只能以字母开头。

<font color='blue'>[Advise]</font>
```go
type HelloWorld struct {
	ID          int64  `json:"id"`
	UserName    string `json:"user_name"`
	PhoneNumber string `json:"phone_number"`
}
```

### 文件layout
文件格式书写示例
``` go
/*
 * Copyright (c) 2011 Qunar.com. All Rights Reserved.
 * @Author: fangyuan.qian
 * @Create: 2021-09-28 09:46:15
 * @Modifier: fangyuan.qian
 * @Update: 2021-10-14 11:38:03
 * @Description: 这是测试文件
 */

package main

import (
	"context"
	"fmt"
	"time"

	"github.com/gin-gonic/gin"
	"github.com/pkg/errors"

	"gitlab.corp.qunar.com/fangyuan.qian/dubai_common/daemon/server"
	"gitlab.corp.qunar.com/fangyuan.qian/dubai_common/daemon/server"
)

const(
	genHour = 2
)

type TaskGenerator struct{
	name  string
}

var (
	genData       time.Time
)

func test() {
  ...
}

```
文件书写按照以下顺序

<font color='green'>[Good]</font>
```
comment
package
import
const
struct
var
func
```
<font color='red'>[Bad]</font>
```
package
import
struct
const
var
func
```

#### 文件头部
文件头部至少需要声明 版权、修改者、更新时间、文件内容说明。
<font color='green'>[Good]</font>
```
/*
 * Copyright (c) 2011 Qunar.com. All Rights Reserved.
 * @Author: fangyuan.qian
 * @Create: 2021-09-28 09:46:15
 * @Modifier: fangyuan.qian
 * @Update: 2021-10-14 11:38:03
 * @Description: 这是测试文件
 */
```


<font color='red'>[Bad]</font>
```
/*
 * Copyright (c) 2011 Qunar.com. All Rights Reserved.
 * @Author: fangyuan.qian
 * @Create: 2021-09-28 09:46:15
 */
```

#### import
导入库大致分为三类：系统库、开源库、公司内部库。所有导入的库需要分为这三块，每块用空行分割，每块内部按照字母排序。
单个文件内禁止使用多个import。

<font color='green'>[Good]</font>
```go
import(
	"context"
	"fmt"
	"time"

	"github.com/gin-gonic/gin"
	"github.com/pkg/errors"

	"gitlab.corp.qunar.com/fangyuan.qian/dubai_common/daemon/server"
	"gitlab.corp.qunar.com/fangyuan.qian/dubai_common/daemon/server"

)
```

<font color='red'>[Bad]</font>
```go
import (
	"context"
	"fmt"
)

import (
	"time"
)
```

<font color='red'>[Bad]</font>
```go
import (
	"context"
	"fmt"
	"github.com/gin-gonic/gin"
	"github.com/pkg/errors"
	"time"
)
```
#### 变量、常量
多个变量/常量需要分成一组，也可以按照相关性分为多组。

<font color='green'>[Good]</font>
```go
const(
	testName = "a"
	testID = 1
)

var(
	areaX,areaY  int
	volume float64
)
```

<font color='red'>[Bad]</font>
```go 
const testName = "a"
const testID = 1
var areaX int
var areaY int
var volume float64
```
#### struct
每个字段单独一行。如果struct需要输出到http接口，则struct必须包含 json tag。

<font color='green'>[Good]</font>
```go
type HelloWorld struct{
	ID   int
	Name string
}

type UserInfo struct {
	Name     string    `json:"name"`
	Sex      string    `json:"sex"`
	Age      int       `json:"age"`
	Address  string    `json:"address"`
	BirthDay time.Time `json:"birth_day"`
}
```
<font color='red'>[Bad]</font>
```go
type HelloWorld struct{
	ID   int; Name string
}
```

### 书写
#### 缩进和折行

<font color='green'>[Good]</font>
缩进采用四个空格，使用gofmt格式化。

<font color='blue'>[Advise]</font>
单行最长不超过120字符，除了长文本语句(```)。

<font color='green'>[Good]</font>
go语言不需要结尾冒号，多条语句应该分行（除了错误处理）；

<font color='green'>[Good]</font>
```go
var areaX int
var areaY int
```
<font color='red'>[Bad]</font>
```go
var areaX int; var areaY int
```

#### 代码规模

<font color='blue'>[Advise]</font>
单个函数代码行数不应超过200行。

<font color='blue'>[Advise]</font>
单个文件代码行数不应超过600行。

<font color='blue'>[Advise]</font>
结构体变量个数不应超过50个，否则考虑使用结构体嵌套。

<font color='blue'>[Advise]</font>
单个项目总体代码行数超过20w，否则考虑将项目拆分成子项目。

# 代码注释
长命名并不一定具有良好的可读性，良好的注释通常比长命名更有价值。

#### 注释风格
使用单行注释和块注释，但是块注释必须在单独的行。

<font color='green'>[Good]</font>
```go
// 单行注释
var a int

var b int // 单行注释

/*
 多行注释
*/
var c int
```
<font color='red'>[Bad]</font>
```go
func myFunction() {/*
 多行注释
*/
	...
}
```
#### 结构体注释
每个结构体都应该有注释说明，需要对其功能、用途做简单的介绍。

<font color='blue'>[Advise]</font>
```go
/*
 *  @Description: 用户的基本信息
 */
 type UserInfo struct {
	Name     string    `json:"name"`
	Sex      string    `json:"sex"`
	Age      int       `json:"age"`
	Address  string    `json:"address"`
	BirthDay time.Time `json:"birth_day"`
}
```
#### 函数注释
每个函数都应该有注释说明，函数注释应包括以下三个方面，并按照顺序写撰写：
Description：函数的功能介绍
param:每行一个参数，格式为 参数名:参数说明，没有参数时可以省略此项
return:每行一个返回值，格式为： 返回值：返回值说明，没有返回值时可以省略此项

<font color='blue'>[Advise]</font>
```go
/*
 *  @Description: 通过发送数据找到报告
 *  @param id:
 *  @param name:
 *  @return info:
 *  @return err:
 */
func (r *Receiver) myFunc(id int64, name string) (info string, err error) {
	return
}

```

#### tag注释
tag有助于表示此处可能需要做一些特殊的处理。
允许使用的tag:
+ **TODO**：表示此处需要在未来做一些事情。
+ **FIXME**：表示此处有问题，需要在接下来进行修复。
+ **DEPRECATED**：表示这个接口在未来会被弃用，不在维护。

<font color='blue'>[Good]</font>
```go
// TODO: ...
func myFunc() {}

// FIXME: ...
func myFunc() {}

// DEPRECATED: ...
func myFunc() {}
```
<font color='red'>[Bad]</font>
```go
// todo: ...
func myFunc() {}

// Fixme: ...
func myFunc() {}

// myFunc DEPRECATED: ...
func myFunc() {}
```
# 编程习惯
#### 函数返回最后一个参数为error
如果函数需要返回错误，则返回值的最后一个参数为error类型。不要使用string类型代替error。

<font color='green'>[Good]</font>

```go
func myFunction() (string, error) {
	...
}
```
<font color='red'>[Bad]</font>
```go
func myFunction() (string, string) {
	...
}
```

#### 错误处理
<font color='green'>[Good]</font>

```go
if err = myFunc(); err != nil {
	...
}
```
<font color='red'>[Bad]</font>
```go
err = myFunc()
err != nil {
	...
}
```
#### 不使用bool表达式

<font color='blue'>[Advise]</font>
```go
if exist {
	...
}
```

<font color='red'>[Bad]</font>
```go
if exist == true {
	...
}
```
#### 条件语句中尽量不使用else

<font color='blue'>[Advise]</font>
```go
if exist {
	...
	return
}
...
return
```

<font color='red'>[Bad]</font>
```go
if exist == true {
	...
	return
} else {
	...
	return
}
```
#### panic
<font color='blue'>[Advise]</font>
不要使用panic，除非你真的知道这样做会有什么问题。


#### 隐式返回
如果函数行数比较少，推荐使用这种写法，否则建议显示返回。

<font color='blue'>[Advise]</font>
```go
func myFunc() (err error){
	...
	return
}
```

#### 初始化函数
<font color='blue'>[Advise]</font>
初始化函数不要做耗时操作，初始化函数应该作为当前文件第一个函数。
