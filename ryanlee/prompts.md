### 项目基线与约束

- 使用 go1.24.0 版本
- 跳过.gitignore 中文件的遍历
- 如果代码中有 //TODO 描述 根据描述更新代码逻辑
- 函数传参以及返回类型，如果是结构体那么传递以及返回指针类型。
- 魔鬼字符串及数字，要使用常量类型定义在文件中，只处理 switch case 语句中的常量， 以及 if 语句 中的常量判断等，其他不处理，空字符串不需要定义常量，0 数字也不定义常量 除非 0 是枚举的值
- 入口 `main/main.go` 已启动 Gin HTTP 服务与 metrics；保持这一结构，新增路由需通过 `routers` 统一注册。
- 配置由 `config/config.go` 与 `initialize/LoadConfig` 加载，复用现有 bootstrap 日志/配置能力，新增配置项请接入同一机制。
- 数据访问使用 GORM, 所有表结构体继承 gorm.Model ；
- 数据库使用 mysql；
- 所有跟数据库插入 查询 更新 删除 等逻辑都放在对应表结构定义的文件中封装函数统一管理。不要放在逻辑代码中。
- gorm tag 样例："GoodsId uint64 `json:"goods_id" gorm:"column:goods_id;type:bigint;not null;uniqueIndex:idx_goods_id;comment:'business-facing goods id'"`"

  都集成 gorm.Model，表名放到 const.go 中定义常量以"t\_"作为前缀，。 如有枚举定义在 enums.go 中
  统一封装在 `dao/`，需要初始化数据库连接（`initialize/mysql.go`）。 表结构体 GormXXX 命名 xxx 是表功能描述
  gorm 表封装的函数 都使用结构体内部函数封装。不使用全局格式函数定义
  gorm.DB 不存在统一返回 database.ErrorNilGormDatabase 错误

- 现有健康检查在 `routers/controller/consul`；保持可用并补充钱包相关 API。
  response 功能使用 `gitlab.xbit.trade/tootchain/commons/v2/web/common/response` 里的功能进行应答
  router 下的 gin 回调函数里的 log.WithContext 统一使用参数 （c \*gin.Context） 如 log.WithContext(c),不另外生成 context。
- Gin 处理函数统一以 handle 开头。同一个 group 下的处理函数放入同一个处理函数文件中，如 wallet 这个 group 下的处理函数都放入 wallet.go 文件中
- Request 结构体统一 Req 开头，Response 结构体统一 Ack 开头。不使用 c.Query("address") 这种方式 Query 数据也定义结构体，结构体 tag 使用 form + json
- 结构体 tag json + form 都使用小驼峰命名规则，gorm 使用下划线间隔定义
- 处理函数相关的数据结构定义在与处理函数所在文件的文件名加 "\_model.go" 结尾的文件中。
- 项目执行流程放在 work 目录下
- 所有日志统一使用 log.WithContext(ctx) 形式输出日志, log 库地址"gitlab.xbit.trade/tootchain/commons/v2/log"
- 如果是枚举类型 定义枚举类型 如 "type EnumChainName string" 然后再使用 const 定义枚举数据
- 整理文件结构，定义一个逻辑类型时，如果需要定义数据结构，定义在同目录的 types.go 文件中，常量定义在 consts.go 文件中，小的功能函数定在 utils.go 文件中 types.go 文件中只声明较简单的数据类型定义，需要声明较复杂的功能函数的类型单独文件定义。
- 如果有kafka模块，使用`gitlab.xbit.trade/tootchain/commons/v2/mq` 里的功能封装
- 如果有kvdb模块，使用`gitlab.xbit.trade/tootchain/commons/v2/kvdb/pebble` 里的功能封装
