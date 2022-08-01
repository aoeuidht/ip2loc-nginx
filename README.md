# ip2loc-nginx
Add the `Point` of city center to downstream http request header based on user's IP address. The database of ip2loc is ipip.net's free ip data base.

Most of the logic copied from https://github.com/ipipdotnet/ipdb-luajit

## Usage

### deploy this plugin
Deploy details are listed in `setup` section.

* nginx.conf
* ipdb

### setup up a echo server on `127.0.0.1:8888`

use nc for testing

```
nc -l 8888
```

### curl the nginx

Try

```
curl 'ip-addr:8080'
```

and you can see the additional headers in output of nc

```
work@maptable-dev:~$ nc -l 8888
GET / HTTP/1.0
Host: 127.0.0.1:8888
Connection: close
User-Agent: curl/7.68.0
Accept: */*
X-Country-Name: 中国
X-Province-Name: 西藏
X-City-Name: 日喀则
X-Ip-Loc: POINT(88.87854614 29.27013032)

^C
work@maptable-dev:~$ 
```

4 headers are added to http request:
* X-Country-Name
* X-Province-Name
* X-City-Name
* X-Ip-Loc

If no geo info founded in ipdb, then there will be no headers added.

# Setup

## install nginx and lua
* nginx
* lua: https://docs.nginx.com/nginx/admin-guide/dynamic-modules/lua/

## deploy ipdb code to luajit path

```
cp -r ipdb $path_of_lua
```

## update the ipdb path in nginx.conf
update the path at line 3 to the path `ipipfree.ipdb` placed. Then copy `nginx.conf` to `site-enabled` or merge this config to your own nginx config files.
