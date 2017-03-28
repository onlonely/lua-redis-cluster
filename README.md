Name
=======

纯lua实现的openresty上使用的redis集群实现,依赖春哥的lua-resty-redis(https://github.com/openresty/lua-resty-redis)
代码逻辑基本上是redis的C实现照搬过来的,增加了些个人需要的功能


* [Methods](#methods)
    * [new](#new)
    
    
Methods
=======

```lua
local redis_cluster = require "resty.redis_cluster"
local cluster_id = "marketing_cluster"
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

