# Introduction
A distribution of Nginx with c++,python,lua and php web development. 


[hi-nginx-demo](https://github.com/webcpp/hi_demo).

[hi-nginx-doc](https://github.com/webcpp/hi-nginx-doc).

# Features
- All features of nginx(latest release) are inherited, i.e., it is 100% compatible with nginx.


# python and lua api
## hi_req
- uri
- method
- client
- param
- user_agent
- has_header
- get_header
- has_form
- get_form
- has_session
- get_session
- has_cookie
- get_cookie
## hi_res
- status
- content
- header
- session

# hello,world

## cpp servlet class

```
#include "servlet.hpp"
namespace hi{
class hello : public servlet {
    public:

        void handler(request& req, response& res) {
            res.headers.find("Content-Type")->second = "text/plain;charset=UTF-8";
            res.content = "hello,world";
            res.status = 200;
        }

    };
}

extern "C" hi::servlet* create() {
    return new hi::hello();
}

extern "C" void destroy(hi::servlet* p) {
    delete p;
}

```

### cpp compile

```
g++ -std=c++11 -I/usr/local/nginx/include  -shared -fPIC hello.cpp -o hello.so
install hello.so /usr/local/nginx/hi

```


## php servlet class

see `php/hi/request.php`,`php/hi/response.php` and `php/hi/servlet.php`

```php

<?php

require_once 'hi/servlet.php';

class hello implements \hi\servlet {

    public function handler(\hi\request &$req, \hi\response &$res) {
        $res->content = 'hello,world';
        $res->status = 200;
    }

}

```

### php demo

see `contrbi/php`

# Dependency
- linux
- gcc,g++(c++11)
- hiredis-devel
- python-devel
- boost-devel
- luajit-devel
- PHP 7.0 or later(--enable-embed=shared)

## centos
```
sudo yum install gcc gcc-c++ make pcre-devel zlib-devel openssl-devel hiredis-devel python-devel boost-devel luajit-devel

```
## ubuntu
```
sudo apt-get install build-essential libpcre3-dev zlib1g-dev libssl-dev libhiredis-dev python-dev libboost-all-dev libluajit-5.1-dev 

```

## About php7+ install
Please change `configure` :

```txt
ac_fn_c_check_decl "$LINENO" "isfinite" "ac_cv_have_decl_isfinite" "#include <math.h>
"
if test "x$ac_cv_have_decl_isfinite" = xyes; then :
  ac_have_decl=0
else
  ac_have_decl=0
fi

```



# Installation
see `install_demo.sh` or `--add-module=ngx_http_hi_module`

# 3rd party module
  -  array-var-nginx-module-0.05
  -  form-input-nginx-module-0.12
  -  headers-more-nginx-module-0.32
  -  iconv-nginx-module-0.14
  -  memc-nginx-module-0.18
  -  ngx_coolkit-0.2
  -  ngx_devel_kit-0.3.0
  -  rds-csv-nginx-module-0.07
  -  rds-json-nginx-module-0.14
  -  redis2-nginx-module-0.14
  -  set-misc-nginx-module-0.31
  -  srcache-nginx-module-0.31
  -  xss-nginx-module-0.05
  -  nginx-http-concat-1.2.2
  -  nginx-push-stream-module-0.5.4
  -  nginx-rtmp-module-1.2.0
  -  nchan-1.1.7
  -  echo-nginx-module-0.61
  -  nginx-upload-module-2.255

# ngx_http_hi_module directives
- directives : content: loc,if in loc
    - hi,default: ""

    example:
    
```
            location = /hello {
                hi hi/hello.so ;
            }
```
- directives : content: http,srv,loc,if in loc ,if in srv
    - hi_need_cache,default: on

    example:

```
        hi_need_cache on|off;
```

- directives : content: http,srv,loc,if in loc ,if in srv
    - hi_cache_size,default: 10

    example:

```
        hi_cache_size 10;
```

- directives : content: http,srv,loc,if in loc ,if in srv
    - hi_cache_expires,default: 300s

    example:

```
        hi_cache_expires 300s;
```

- directives : content: http,srv,loc,if in loc ,if in srv
    - hi_need_headers,default: off

        example:

```
        hi_need_headers on|off;
```

- directives : content: http,srv,loc,if in loc ,if in srv
    - hi_need_cookies,default: off

        example:

```
        hi_need_cookies on|off;
```

- directives : content: http,srv,loc,if in loc ,if in srv
    - hi_need_session,default: off

        example:

```
        hi_need_session on|off;
```

- directives : content: http,srv,loc,if in loc ,if in srv
    - hi_session_expires,default: 300s

    example:

```
        hi_session_expires 300s;
```
     

- directives : content: http,srv ,if in srv
    - hi_redis_host,default: ""

    example:

```
        hi_redis_host 127.0.0.1;
```

- directives : content: http,srv,if in srv
    - hi_redis_port,default: 0

    example:

```
        hi_redis_port 6379;
```


- directives : content: loc,if in loc
    - hi_python_content,default: ""

    example:
    
```
            location = /pyecho {
                hi_python_content "hi_res.status(200)\nhi_res.content('hello,world')" ;
            }
```
- directives : content: loc,if in loc
    - hi_python_script,default: ""

    example:
    
```
            location ~ \.py$  {
                hi_python_script python;
            }
```

- directives : content: loc,if in loc
    - hi_lua_content,default: ""

    example:
    
```
            location = /luaecho {
                hi_lua_content "hi_res:status(200)\nhi_res:content('hello,world')" ;
            }
```
- directives : content: loc,if in loc
    - hi_lua_script,default: ""

    example:
    
```
            location ~ \.lua$  {
                hi_lua_script lua;
            }
```

- directives : content: loc,if in loc
    - hi_php_script,default: ""

    example:
    
```
            location ~ \.php$  {
                hi_php_script php;
            }
```

## nginx.conf

[hi_demo_conf](https://github.com/webcpp/hi_demo/blob/master/demo.conf)