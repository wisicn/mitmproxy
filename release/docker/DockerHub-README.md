# mitmproxy

Containerized version of [mitmproxy](https://mitmproxy.org/): an interactive, SSL/TLS-capable intercepting proxy for HTTP/1, HTTP/2, and WebSockets.

## Usage

```sh
$ docker run --rm -it [-v ~/.mitmproxy:/home/mitmproxy/.mitmproxy -e PUID=1001 -e PGID=1001] -p 8080:8080 mitmproxy/mitmproxy
[terminal user interface of mitmproxy is launched...]
```

The *volume mount* is optional: It's to store the generated CA certificates. And if you specific the -v parameter to mount your local storage, you should also set the PUID and PGID envionment variable to spcific the UID and GID of the local storage.  

Once started, mitmproxy listens as a HTTP proxy on `localhost:8080`:

```sh
$ http_proxy=http://localhost:8080/ curl http://example.com/
$ https_proxy=http://localhost:8080/ curl -k https://example.com/
```

You can also start `mitmdump` by just adding that to the end of the command-line:

```sh
$ docker run --rm -it -p 8080:8080 mitmproxy/mitmproxy mitmdump
Proxy server listening at http://*:8080
[...]
```

For `mitmweb`, you also need to expose port 8081:

```sh
# this makes :8081 accessible to the local machine only
$ docker run --rm -it -p 8080:8080 -p 127.0.0.1:8081:8081 mitmproxy/mitmproxy mitmweb --web-host 0.0.0.0
Web server listening at http://0.0.0.0:8081/
No web browser found. Please open a browser and point it to http://0.0.0.0:8081/
Proxy server listening at http://*:8080
[...]
```

You can also pass options directly via the CLI:

```sh
$ docker run --rm -it -p 8080:8080 mitmproxy/mitmproxy mitmdump --set ssl_insecure=true
Proxy server listening at http://*:8080
[...]
```

For further details, please consult the mitmproxy [documentation](http://docs.mitmproxy.org/en/stable/).

## Tags

The available release tags can be seen
[here](https://hub.docker.com/r/mitmproxy/mitmproxy/tags/).

* `dev` always tracks the git-master branch and represents the unstable development tree.
* `latest` always points to the same image as the most recent stable release, including bugfix releases (e.g., `4.0.0` and `4.0.1`).
* `X.Y.Z` tags contain the mitmproxy release with this version number.

## Security Notice

Dependencies in the Docker images are frozen on release, and can’t be updated in
situ. This means that we necessarily capture any bugs or security issues that
may be present. We don’t generally release new Docker images simply to update
dependencies (though we may do so if we become aware of a really serious issue).
