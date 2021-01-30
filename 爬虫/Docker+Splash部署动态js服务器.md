# Docker+Splash部署动态js服务器

> Splash是一个Javascript渲染服务。它是一个实现了HTTP API的轻量级浏览器，Splash是用Python实现的，同时使用Twisted和QT。Twisted（QT）用来让服务具有异步处理能力，以发挥webkit的并发能力。

## 1. Splash在爬虫中应用

在Python爬虫脚本中， 经常遇到动态js渲染的html网页（Ajax）,通常作法是使用Selenium（浏览器驱动工具）请求html并在渲染网页之后，查找元素或获取网页源码。因为Selenuim本身的特性，需要加载不同浏览器驱动程序才能打开当前系统下不同的浏览器，有很多的限制。如果只是单纯执行ajax并渲染页面，可以使用Splash完全可以取代复杂的Selenium工具。当然，如果实现UI自动化程序的话，Selenium还是非常优秀的。



## 2. Docker下部署splash服务

```
docker pull scrapinghub/splash
```

```
sudo docker run -dit --name splash-server -p 5052:5023 -p 8050:8050 -p 8051:8051 scrapinghub/splash
```

启动splash服务，并通过http，https，telnet提供服务

通常一般使用http模式 ，可以只启动一个8050(http)就好 。也可以指定 8051 (https) 和 5023 (telnet).



## 3. Splash的HTTP API

> 详细参考： https://splash.readthedocs.io/en/latest/api.html

### 3.1 渲染 render.html

**参数说明**

- **url** **:** **string** **:** **required**

- **baseurl** **:** **string** **:** **optional**

- **timeout** **:** **float** **:** **optional**

   目标网页渲染的超时时长， 默认为30 秒。默认情况下最大允许值是90。在启动镜像是可以通过 `--max-timeout 300` 指定。

```shell
$ docker run -it -p 8050:8050 scrapinghub/splash --max-timeout 300
```

- **resource_timeout** **:** **float** **:** **optional**

  目标网页请求的超时时长。

- **wait** **:** **float** **:** **optional**

  Time (in seconds) to wait for updates after page is loaded (defaults to 0). Increase this value if you expect pages to contain setInterval/setTimeout javascript calls, because with wait=0 callbacks of setInterval/setTimeout won’t be executed. Non-zero [wait](https://splash.readthedocs.io/en/latest/api.html#arg-wait) is also required for PNG and JPEG rendering when doing full-page rendering (see [render_all](https://splash.readthedocs.io/en/latest/api.html#arg-render-all)).

  Wait time must be less than [timeout](https://splash.readthedocs.io/en/latest/api.html#arg-timeout).

- **proxy** **:** **string** **:** **optional**

  Proxy profile name or proxy URL. See [Proxy Profiles](https://splash.readthedocs.io/en/latest/api.html#proxy-profiles).

  A proxy URL should have the following format: `[protocol://][user:password@]proxyhost[:port]`

  Where protocol is either `http` or `socks5`. If port is not specified, the port 1080 is used by default.

- js : string : optional

  Javascript profile name. See [Javascript Profiles](https://splash.readthedocs.io/en/latest/api.html#javascript-profiles).

- js_source : string : optional

  JavaScript code to be executed in page context. See [Executing custom Javascript code within page context](https://splash.readthedocs.io/en/latest/api.html#execute-javascript).

- filters : string : optional

  Comma-separated list of request filter names. See [Request Filters](https://splash.readthedocs.io/en/latest/api.html#request-filters)

- allowed_domains : string : optional

  Comma-separated list of allowed domain names. If present, Splash won’t load anything neither from domains not in this list nor from subdomains of domains not in this list.

- allowed_content_types : string : optional

  Comma-separated list of allowed content types. If present, Splash will abort any request if the response’s content type doesn’t match any

- forbidden_content_types : string : optional

  Comma-separated list of forbidden content types. If present, Splash will abort any request if the response’s content type matches any of the content types in this list. Wildcards are supported using the [fnmatch](https://docs.python.org/3/library/fnmatch.html) syntax.

- viewport : string : optional

  View width and height (in pixels) of the browser viewport to render the web page. Format is “<width>x<height>”, e.g. 800x600. Default value is 1024x768.

  ‘viewport’ parameter is more important for PNG and JPEG rendering; it is supported for all rendering endpoints because javascript code execution can depend on viewport size.

  For backward compatibility reasons, it also accepts ‘full’ as value; `viewport=full` is semantically equivalent to `render_all=1` (see [render_all](https://splash.readthedocs.io/en/latest/api.html#arg-render-all)).

- images : integer : optional

  Whether to download images. Possible values are `1` (download images) and `0` (don’t download images). Default is 1.

  Note that cached images may be displayed even if this parameter is 0. You can also use [Request Filters](https://splash.readthedocs.io/en/latest/api.html#request-filters) to strip unwanted contents based on URL.

- headers : JSON array or object : optional

  HTTP headers to set for the first outgoing request.

  This option is only supported for `application/json` POST requests. Value could be either a JSON array with `(header_name, header_value)` pairs or a JSON object with header names as keys and header values as values.

  “User-Agent” header is special: is is used for all outgoing requests, unlike other headers.

- body : string : optional

  Body of HTTP POST request to be sent if method is POST. Default `content-type` header for POST requests is `application/x-www-form-urlencoded`.

- http_method : string : optional

  HTTP method of outgoing Splash request. Default method is GET. Splash also supports POST.

- save_args : JSON array or a comma-separated string : optional

  A list of argument names to put in cache. Splash will store each argument value in an internal cache and return `X-Splash-Saved-Arguments` HTTP header with a list of SHA1 hashes for each argument (a semicolon-separated list of name=hash pairs):

  ```
  name1=9a6747fc6259aa374ab4e1bb03074b6ec672cf99;name2=ba001160ef96fe2a3f938fea9e6762e204a562b3
  ```

  Client can then use [load_args](https://splash.readthedocs.io/en/latest/api.html#arg-load-args) parameter to pass these hashes instead of argument values. This is most useful when argument value is large and doesn’t change often ([js_source](https://splash.readthedocs.io/en/latest/api.html#arg-js-source) or [lua_source](https://splash.readthedocs.io/en/latest/api.html#arg-lua-source)are often good candidates).

- load_args : JSON object or a string : optional

  Parameter values to load from cache. `load_args` should be either `{"name": "<SHA1 hash>", ...}` JSON object or a raw `X-Splash-Saved-Arguments` header value (a semicolon-separated list of name=hash pairs).

  For each parameter in `load_args` Splash tries to fetch the value from the internal cache using a provided SHA1 hash as a key. If all values are in cache then Splash uses them as argument values and then handles the request as usual.

  If at least on argument can’t be found Splash returns **HTTP 498** status code. In this case client should repeat the request, but use [save_args](https://splash.readthedocs.io/en/latest/api.html#arg-save-args) and send full argument values.

  [load_args](https://splash.readthedocs.io/en/latest/api.html#arg-load-args) and [save_args](https://splash.readthedocs.io/en/latest/api.html#arg-save-args) allow to save network traffic by not sending large arguments with each request ([js_source](https://splash.readthedocs.io/en/latest/api.html#arg-js-source) and [lua_source](https://splash.readthedocs.io/en/latest/api.html#arg-lua-source) are often good candidates).

  Splash uses LRU cache to store values; the number of entries is limited, and cache is cleared after each Splash restart. In other words, storage is not persistent; client should be ready to re-send the arguments.

- html5_media : integer : optional

  Whether to enable HTML5 media (e.g. `<video>` tags playback). Possible values are `1` (enable) and `0` (disable). Default is 0.HTML5 media is currently disabled by default because it may cause instability. Splash may enable it by default in future, so pass `html5_media=0` explicitly if you don’t want HTML5 media.See also: [splash.html5_media_enabled](https://splash.readthedocs.io/en/latest/scripting-ref.html#splash-html5-media-enabled).

- http2 : integer : optional

  Enable or disable HTTP2 support. Possible values are `1` (enable) and `0` (disable). Default is 1. Set it to 0 if website fails to render with http2 due to bugs (usually network 399 errors).

- engine : string : optional

  Browser engine to use. Allowed values are `webkit` (default) and `chromium`.

  Warning

  engine=chromium is in pre-alpha stage: many features don’t work, there are known bugs, including Splash crashes. Use on your own risk!

  Allowed values also depend on Splash startup options: `--browser-engines` startup option can be used to disable one of them. Start Splash with `--browser-engines=webkit` option to disallow Chromium.



## 4. 应用案例

### 4.1 智联招聘

### 4.2 腾讯招聘



