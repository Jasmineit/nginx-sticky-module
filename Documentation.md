# Compilation #
The nginx sticky module has to be compiled into nginx as every other nginx modules:
```
cd /path/to/nginx-sources
tar -xzf /path/to/nginx-sticky-module-$version.tar.gz
./configure ... --add-module=nginx-sticky-module-$version
make && make install
```
# Usage #
The sticky module has only one command: "sticky".
## sticky ##

### Context ###
The "sticky" command can only be used in a upstream block:
```
upstream {
  sticky;
  server 127.0.0.1:9000;
  server 127.0.0.1:9001;
  server 127.0.0.1:9002;
}
```
### Arguments ###
The "sticky" command can take several arguments to control its behaviour:
```
  sticky [name=route] [domain=.foo.bar] [path=/] [expires=1h] [hash=index|md5|sha1] [no_fallback];
```
| **Configuration** | **Description** | **Parameters** | **Default Value** |
|:------------------|:----------------|:---------------|:------------------|
| name              | the name of the cookie used to track the persistant upstream srv | can be any string | "route"           |
| domain            | the domain in which the cookie will be valid | can be any string | nothing. Let the browser handle this. |
| path              | the path in which the cookie will be valid | can be any path | nothing. Let the browser handle this. |
| expires           | the validity duration of the cookie | must be a duration greater than one second | nothing. It's a session cookie |
| hash              | the hash mechanism to encode upstream server. It cant' be used with hmac | md5|sha1       | md5               |
| hmac              | The HMAC hash mechanism to encode upstream server. It's like the hash mechanism but it uses hmac\_key to secure the hashing. It can't be used with hash. | md5|sha1       | none              |
| hmac\_key         | The key to use with hmac. It's mandatory when hmac is set. | can be any string | none              |
| no\_fallback      | When this flag is set, nginx will return a 502 (Bad Gateway orProxy Error) if a request comes with a cookie and the corresponding backend is unavailable. | no arguments   | none              |

# Warning #
The nginx sticky module CAN'T be used with ip\_hash in the same upstream block, behaviour is unknown !