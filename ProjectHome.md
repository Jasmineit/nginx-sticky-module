# DEPRECATED #

see https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/overview

This project has been forked and will no longer be updated. See https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/overview for updates.


# Old Description #

When dealing with several backend servers, it's sometimes useful that one client (browser) is always served by the same backend server (for session persistance for example).

Using a persistance by IP (with the ip\_hash upstream module) is maybe not a good idea because there could be situations where a lot of different browsers are coming with the same IP address (behind proxies) and the load balancing system won't be fair.

Using a cookie to track the upstream server makes each browser unique.

When the sticky module can't apply, it switchs back to the classic Round Robin or returns a "Bad Gateway" (depending on the no\_fallback flag).

Sticky module can't apply when cookies are not supported by the browser.

**Sticky module is based on a "best effort" algorithm. Its aim is not to handle security somehow. It's been made to ensure that normal users are always redirected to the same  backend server: that's all!**

Documentation: [here](http://code.google.com/p/nginx-sticky-module/wiki/Documentation)


---

```
(client)                             (nginx)                      (upstream servers)
    >-- GET /URI1 HTTP/1.0 -----------> |
                                        | *** nginx choose one upstream by RR ***
                                        | >----- GET /URI1 HTTP/1.0   ----> |
                                        | <------- HTTP/1.0 200 OK -------< |
    <-- HTTP/1.0 200 OK --------------< |
        Set-Cookie: route=md5(upstream) |
                                        |
    >-- GET /URI2 HTTP/1.0 -----------> |
        Cookies: route                  |
                                        | *** nginx redirect to "route" ***
                                        | >----- internal fetch /URI2 ----> |
                                        | <--- internal response /URI2 ---< |
    <-- HTTP/1.0 200 OK --------------< |
                                      (...)
```