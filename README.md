Name
=======

纯lua实现的openresty上使用的redis集群实现,
依赖春哥的lua-resty-redis(https://github.com/openresty/lua-resty-redis)
代码逻辑基本上是redis的C实现照搬过来的,增加了些个人需要的功能,是长连接的.
redis命令都支持的,只是维护槽分配的节点,然后用metatable把所有命令执行都交给春哥的lua-resty-redis执行.
发现春哥修改过redis.lua而我从未更新过,不知道区别在那里!

但是这个库是本公司生产在使用的,目前没发现什么问题.剩下的看你们了^_^


Methods
=======
* [new](#new)
    * [get](#new)
    * [set](#new)
    * [keys](#new)
    * [...](#new)
    * [cluster_dels](#cluster_dels)
    * [cluster_status](#cluster_status)
    


new
-------
```lua
local redis_cluster = require "resty.redis_cluster"
local cluster_id = "redis_cluster"
local startup_nodes = { {"10.10.10.1", 10101},{"10.10.10.2", 10201},{"10.10.10.3", 10301}}
local opt = { 
	    timeout = 1000,
	    keepalive_size = 50,
	    keepalive_duration = 60000
	}
  
local rc = redis_cluster:new(cluster_id, startup_nodes, opt)
local res,err=rc:get("xuzz")
ngx.log(ngx.ERR,"res=",tostring(res))
```

cluster_dels
-------
`syntax: tres = rc:cluster_dels(regkey)`

集群执行的删除匹配键值,返回每个集群节点中删除的结果,是一个table.
原理为单独连接集群每个节点用eval执行一段lua脚本批量删除匹配的key


cluster_status
-------
`syntax: tres = rc:cluster_status(bmaster?)`

获得集群每个节点的状态,返回结果自己实验吧....哎呀我太懒了!
貌似这个命令有修改春哥的redis.lua这个坑了...只好你们自己实验出我修改了的redis.lua和春哥到底有什么不一样了...
