# haserl 0.9.35 (binary only)
 - Package: [master/make/haserl/](https://github.com/Freetz-NG/freetz-ng/tree/master/make/haserl/)

"*Haserl is a small program that uses shell or Lua script to create cgi
web scripts. It is intended for environments where PHP or ruby are too
big.*

*It was written for Linux, but is known to run on FreeBSD. A typical use
is to run cgi scripts in an embedded environment, using a small web
server, such as mini-httpd, lighty, or the server built into busybox.*"

### Using busybox httpd / inet

Inetd custom config:

```
#:httpd-start: test web interface
8088    stream  tcp nowait  root    /var/media/ftp/uFlash/httpd/httpd-start httpd-start -i
```

httpd-start:

```
#!/bin/sh

export PATH=/sbin:/bin:/usr/sbin:/usr/bin:/mod/sbin:/mod/bin:/mod/usr/sbin:/mod/usr/bin
export LD_LIBRARY_PATH=/mod/lib

homedir=/var/media/ftp/uFlash/httpd/www
config=/var/media/ftp/uFlash/httpd/httpd.conf

cd "$homedir"
exec httpd "$@" -p 8088 -c "$config" -h "$homedir" -r "Freetz" 2>/dev/null
```

Allow execution:

```
chmod +x httpd-start
```

httpd.conf can be an empty file:

```
touch httpd.conf
```

/var/media/ftp/uFlash/httpd/www/cgi-bin/info.cgi:

```
#!/usr/bin/haserl --shell=lua
Content-Type: text/html; charset=UTF-8

<html>
<body>
<h1>Info</h1>
<% for n,v in pairs(ENV) do print(n, v, '<br />') end %>
</body>
</html>
```

Allow execution:

```
chmod +x info.cgi
```

Test:
[http://fritz.box:8088/cgi-bin/info.cgi](http://fritz.box:8088/cgi-bin/info.cgi)

### Weiterführende Links

-   [Homepage](http://haserl.sourceforge.net/)
-   [Patch for LUA support](https://trac.boxmatrix.info/freetz-ng/ticket/1326)

