# 苏州农行自动升级方案

## image-syncer

`image-syncer` 是一个docker镜像同步工具，可用来进行多对多的镜像仓库同步，支持目前绝大多数主流的docker镜像仓库服务。

### 编译

~~~bash
git clone https://github.com/AliyunContainerService/image-syncer.git
make
# 为了方便使用，可以把二进制文件放到bin目录下
mv image-syncer $GOPATH/bin
~~~

### 使用

~~~bash
image-syncer --proc=6 --auth=./auth.json --images=./images.json --retries=3
# proc: 并发数
# auth: 指定仓库认证文件位置
# images: 指定镜像同步文件位置
# retries: 重试次数
# registry: 目标仓库为空时，使用它作为默认registry
# namespace: 目标仓库namespace为空时，使用它作为默认registry
~~~

这里主要涉及到两个配置文件:

* auth.json：用于配置docker registry相关信息，用户名、密码和使用的协议（http/https)
* images.json：用于定义需要同步那些镜像，格式为`"registry/namespace/image":"registry/namespace/image"`

#### auth.json

~~~json
{
    // 可以定义一个或多个docker registry
	"127.0.0.1:5000": { // docker registry地址
		"username":"admin", // 登录用户名
		"password":"123456", // 登录密码
		"insecure":true // 使用http访问，inscure=true,使用https为false或者缺省
	},
	"127.0.0.1:5001": {
        // 如果可以匿名访问registry,可以忽略username与password
		"insecure":true
	}
}
~~~

#### images.json

~~~json
{
    // 左边为源仓库，右边为目标仓库，格式与docker pull/push命令相似
    // 源仓库不能为空，如果需要同步多个镜像，需要配置多条
    // 目标仓库可以为空，如果目标仓库为空，默认使用命令行中的--registry和--namespace的值
    "127.0.0.1:5000/postgres":"127.0.0.1:5001/postgres",
    "127.0.0.1:5000/redis":"127.0.0.1:5001/redis",
    // 源仓库可以指定同步的tag，用‘，’分割，目标仓库不允许指定tag
    // 如果没有设置tag，默认同步所有tag
	"127.0.0.1:5001/golang:1.15.12":"127.0.0.1:5000/golang"
}
~~~

### 总结

需要手动触发同步，建议配合crontab使用。

