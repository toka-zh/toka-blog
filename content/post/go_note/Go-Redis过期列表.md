Date:20220524

在实际开发中,通过使用特征值来对数据进行去重复操作,而特征值首先需要具备过期时间和列表属性,但在redis中没有直接可用的数据结构,如果使用String则没有List特性,并且检索过程繁琐无法利用好redis,而List则无法使用过期时间。

查找资料后找到一种思路:使用有序集合进行排序,以时间作为值,定期执行清除过期数据任务

那么接下来开始实践

有序集合在Go中结构为`redis.Z`,包含`Member 元素名`与`Score 分数`

首先需要了需要用到的函数,用到的无非就是增,删,查三个函数(文章末尾有函数介绍):

1. ZAdd: 添加特征值元素,Member为特征值,Score为时间戳
2. ZRangeByScore: 通过分数范围取值,分数范围即为有效时间
3. ZRemRangeByScore: 通过时间范围删除特征值

除此之外,还需要一些时间函数,用于计算有效时间

1. 获取分数(时间戳string)内的值
2. 删除分数(时间戳float64)外的值

并且需要有两种返回形式,float64用于ZAdd的float型分数,而string用于检索时的分数

```go
// TimeBfFloat 获取日期时间戳
func TimeBfFloat(beforeDay int) float64{
  now := time.Now().Unix()
  sub := 7 * 24 * time.Hour.Second()
  return now - sub
}

// 获取string类型
func TimeBfString(beforeDay int) string{
  beforeTime := TimeBfFloat(beforeDay)
  return strconv.Itoa(int(beforeTime))
}
```



然后对Redis操作进行封装抽象

总的来说应该满足以下接口

```go
type Eigenvalues interface{
  // Set 设置特征值
  // 		key 				特征键
  //		eigenvalue	特征值
  //		timeout			超时时间(天数)
  Set(key, eigenvalue string, timeout int) error
  
  // Get 获取特征值
  // 		key 				特征键
  //		timeout			时间范围(天数)
  Get(key string, timeout int) []string
  
  // Del 删除特征值
  // 		key 				特征键
  //		timeout			超时时间(天数)
  Del(key string, timeout int) error
}
```



## Redis函数

### 1.ZAdd

添加一个或者多个元素到集合，如果元素已经存在则更新分数

```go
client.ZAdd("key", redis.Z{20220524,"tizi"}).Err()
```



### 2.ZRangeByScore/ZRevRangeByScore

返回集合中索引范围内的元素,

ZRangeByScore根据分数从小到大排序,

ZRevRangeByScore根据分数从大到小排序

```go
// 初始化查询条件， Offset和Count用于分页
op := redis.ZRangeBy{
  Min:"2",	// 最小分数
	Max:"10", // 最大分数
	Offset:0, // 起始位置
	Count:5, 	// 数据条数
}
// return: []string
vals, err := client.ZRangeByScoreWithScores("key", op).Result()
```



### 3.ZRemRangeByScore

根据分数范围删除元素

```go
// 删除范围： 2<=分数<=5 的元素
client.ZRemRangeByScore("key", "2", "5")

// 删除范围： 2<=分数<5 的元素
client.ZRemRangeByScore("key", "2", "(5")
```

