## Package

```go
1、安装redigo
go get github.com/garyburd/redigo/redis
```

2、import redigo

```go
import （
    "github.com/garyburd/redigo/redis"
)
```

3：连接redis

连接方式一：

```go

		c, err := redis.Dial("tcp", "192.168.2.225:6379")
    if err != nil {
        fmt.Println("connect to redis err", err.Error())
        return
    }
    defer c.Close()

```

连接方式二： 采用连接池的方式

```go

 var redisPool *redis.Pool
	func RedisPollInit() *redis.Pool {
	//var redisPool *redis.Pool
	redisPool = &redis.Pool{
		MaxIdle:   3,
		MaxActive: 5,
		Dial: func() (redis.Conn, error) {
			c, err := redis.Dial("tcp", "127.0.0.1:6379")
			if err != nil {
				return nil, err
			}
			return c, err
		},
	}
	return redisPool
}
```

4：命令执行

```go
n,err := c.Do("hset","key","field","value")  //写
result,err := redis.Values(c.Do("hgetall","key"))//读
```

5：hash常见命令

```go
hset(key, field, value) // 向名称为key的hash中添加元素field
hget(key, field)				// 返回名称为key的hash中field对应的value
hmget(key, (fields))   //  返回名称为key的hash中field i对应的value
hmset(key, (fields))	//  向名称为key的hash中添加元素field
hincrby(key, field, integer)	//  将名称为key的hash中field的value增加integer
hexists(key, field)		//  名称为key的hash中是否存在键为field的域
hdel(key, field)		//  删除名称为key的hash中键为field的域
hlen(key)						//  返回名称为key的hash中元素个数
hkeys(key)					//  返回名称为key的hash中所有键
hvals(key)					//  返回名称为key的hash中所有键对应的value
hgetall(key)				//  返回名称为key的hash中所有的键（field）及其对应的value
```

6：示例
6.1：hset示例

```go
_, err = c.Do("hset", "myhash", "bike1", "mobike")
if err != nil {
    fmt.Println("haset failed", err.Error())
}
```

6.2：hget示例

```go
res, err := c.Do("hget", "myhash", "bike1")
fmt.Println(reflect.TypeOf(res))
if err != nil {
    fmt.Println("hget failed", err.Error())
} else {
    fmt.Printf("hget value :%s\n", res.([]byte))
}
```

6.3：hmset/hmget

```go
_, err = c.Do("hmset", "myhash", "bike2", "bluegogo", "bike3", "xiaoming", "bike4", "xiaolan")
if err != nil {
    fmt.Println("hmset error", err.Error())
} else {
    value, err := redis.Values(c.Do("hmget", "myhash", "bike1", "bike2", "bike3", "bike4"))
    if err != nil {
        fmt.Println("hmget failed", err.Error())
    } else {
        fmt.Printf("hmget myhash's element :")
        for _, v := range value {
            fmt.Printf("%s ", v.([]byte))
        }
        fmt.Printf("\n")
    }
}
```

6.4：hincrby

```mysql
_, err = c.Do("hmset", "myhash", "bike2", "bluegogo", "bike3", "xiaoming", "bike4", "xiaolan")
if err != nil {
    fmt.Println("hmset error", err.Error())
} else {
    value, err := redis.Values(c.Do("hmget", "myhash", "bike1", "bike2", "bike3", "bike4"))
    if err != nil {
        fmt.Println("hmget failed", err.Error())
    } else {
        fmt.Printf("hmget myhash's element :")
        for _, v := range value {
            fmt.Printf("%s ", v.([]byte))
        }
        fmt.Printf("\n")
    }
}
```

6.5：hexists

```go
isExist, err := c.Do("hexists", "myhash", "tmpnum")
if err != nil {
    fmt.Println("hexist failed", err.Error())
} else {
    fmt.Println("exist or not:", isExist)
}
```

6.6：hlen

```go
ilen, err := c.Do("hlen", "myhash")
if err != nil {
    fmt.Println("hlen failed", err.Error())
} else {
    fmt.Println("myhash's len is :", ilen)
}
```

6.7：hkeys

```go
resKeys, err := redis.Values(c.Do("hkeys", "myhash"))
if err != nil {
    fmt.Println("hkeys failed", err.Error())
} else {
    fmt.Printf("myhash's keys is :")
    for _, v := range resKeys {
        fmt.Printf("%s ", v.([]byte))
    }
    fmt.Println()
}
```

6.8：hvals

```go
resValues, err := redis.Values(c.Do("hvals", "myhash"))
if err != nil {
    fmt.Println("hvals failed", err.Error())
} else {
    fmt.Printf("myhash's values is:")
    for _, v := range resValues {
        fmt.Printf("%s ", v.([]byte))
    }
    fmt.Println()
}
```

6.9：hdel

```go
_, err = c.Do("HDEL", "myhash", "tmpnum")
if err != nil {
    fmt.Println("hdel failed", err.Error())
}
```

6.10：hgetall

```go
result, err := redis.Values(c.Do("hgetall", "myhash"))
if err != nil {
    fmt.Println("hgetall failed", err.Error())
} else {
    fmt.Printf("all keys and values are:")
    for _, v := range result {
        fmt.Printf("%s ", v.([]byte))
    }
    fmt.Println()
}
```



### Gin 案例

添加

```go
type Location struct {
	DeviceId 	string `json:"deviceId"`
	Longitude 	string `json:"longitude"`
	Latitude	string `json:"latitude"`
}


func AddCache(c *gin.Context) {

	var params Location
	err := c.ShouldBindJSON(&params)
	if err != nil {
		c.JSON(400,gin.H{"error": "ParamsError", "errorMessage": "请求参数名称有误或参数类型错误"})
		return
	}

	conn := redisPool.Get()
	fmt.Printf("Connent Info --- %+v",conn)
	defer conn.Close()

	node, _ := api.NewWorker(1)

	if _, err := conn.Do("HMSET", redis.Args{}.Add(node.GetId()).AddFlat(&params)...); err != nil {
		fmt.Println("error -- ",err)
		return
	}
	c.JSON(200,gin.H{"msg":"success"})
}

```

查询

```go
func GetCache(c *gin.Context) {
	var params Location
	key := c.Request.FormValue("key")
	if key == "" {
		c.JSON(400,gin.H{"error": "ParamsError", "errorMessage": "key is emptys"})
		return
	}

	conn := redisPool.Get()
	//fmt.Printf("Connent Info --- %+v",conn)
	//resKeys,err := redis.Values(conn.Do("hgetall",key))
	for _, id := range []string{key} {
		v, err := redis.Values(conn.Do("HGETALL", id))
		if err != nil {
			panic(err)
		}
		if err := redis.ScanStruct(v, &params); err != nil {
			panic(err)
		}

		fmt.Printf("%+v\n", params)
	}
	c.JSON(200,gin.H{"msg":"success","data":params})
}

// 映射
// 摘抄来自 https://developer.aliyun.com/article/318872
```

GitHub other Example:

```go
// 用id1 和 id2 作为key 索引
// 分别用Struct 和 Map 写入 Redis
// 遍历查询

func argsExample() {
	var p1, p2 struct {
		Title  string `redis:"title"`
		Author string `redis:"author"`
		Body   string `redis:"body"`
	}

	p1.Title = "Example"
	p1.Author = "Gary"
	p1.Body = "Hello"

	if _, err := c.Do("HMSET", redis.Args{}.Add("id1").AddFlat(&p1)...); err != nil {
		fmt.Println(err)
		return
	}

	m := map[string]string{
		"title":  "Example2",
		"author": "Steve",
		"body":   "Map",
	}

	if _, err := c.Do("HMSET", redis.Args{}.Add("id2").AddFlat(m)...); err != nil {
		fmt.Println(err)
		return
	}

	for _, id := range []string{"id1", "id2"} {

		v, err := redis.Values(c.Do("HGETALL", id))
		if err != nil {
			fmt.Println(err)
			return
		}

		if err := redis.ScanStruct(v, &p2); err != nil {
			fmt.Println(err)
			return
		}

		fmt.Printf("%+v\n", p2)
	}
}
```











