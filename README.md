# jquery.cookie 使用介绍

一个轻量级的cookie 插件，可以读取、写入、删除 cookie。



## jquery.cookie.js 的配置

###第一种

首先包含jQuery的库文件，在后面包含 jquery.cookie.js 的库文件。

	<script type="text/javascript" src="js/jquery-1.6.2.min.js"></script>
	<script type="text/javascript" src="js/jquery.cookie.js"></script>

###第二种

支持fis的component的组件生态直接

	require('jquery-cookie');


## 使用方法

1.新添加一个会话 cookie：:

	$.cookie('the_cookie', 'the_value');

    注：当没有指明 cookie有效时间时，所创建的cookie有效期默认到用户关闭浏览器为止，所以被称为 “会话cookie（session cookie）”。

2.创建一个cookie并设置有效时间为 7天:

	$.cookie('the_cookie', 'the_value', { expires: 7 });

	注：当指明了cookie有效时间时，所创建的cookie被称为“持久 cookie （persistent cookie）”。


3.创建一个cookie并设置 cookie的有效路径：:


	$.cookie('the_cookie', 'the_value', { expires: 7, path: '/' });

	注：在默认情况下，只有设置 cookie的网页才能读取该 cookie。如果想让一个页面读取另一个页面设置的cookie，必须设置cookie的路径。cookie的路径用于设置能够读取 cookie的顶级目录。将这个路径设置为网站的根目录，可以让所有网页都能互相读取 cookie （一般不要这样设置，防止出现冲突） 。

4.读取cookie:


	$.cookie('the_cookie'); // => "the_value"
	$.cookie('not_existing'); // => undefined

5.读取所有有效的cookie:


	$.cookie(); // => { "the_cookie": "the_value", "...remaining": "cookies" }


这个插件默认的过期是按天数计算的，我们可以修改下，按毫秒计算，修改如下：

	//按天算
    t.setTime(+t + days * 86400000);
    //按毫秒算
    //t.setTime(t.getTime() + days);



Delete cookie:

```javascript
// Returns true when cookie was found, false when no cookie was found...
$.removeCookie('the_cookie');

// Same path as when the cookie was written...
$.removeCookie('the_cookie', { path: '/' });
```

*Note: when deleting a cookie, you must pass the exact same path, domain and secure options that were used to set the cookie, unless you're relying on the default options that is.*

## Configuration

### raw

By default the cookie value is encoded/decoded when writing/reading, using `encodeURIComponent`/`decodeURIComponent`. Bypass this by setting raw to true:

```javascript
$.cookie.raw = true;
```

### json

Turn on automatic storage of JSON objects passed as the cookie value. Assumes `JSON.stringify` and `JSON.parse`:

```javascript
$.cookie.json = true;
```

## Cookie Options

Cookie attributes can be set globally by setting properties of the `$.cookie.defaults` object or individually for each call to `$.cookie()` by passing a plain object to the options argument. Per-call options override the default options.

### expires

    expires: 365

Define lifetime of the cookie. Value can be a `Number` which will be interpreted as days from time of creation or a `Date` object. If omitted, the cookie becomes a session cookie.

### path

    path: '/'

Define the path where the cookie is valid. *By default the path of the cookie is the path of the page where the cookie was created (standard browser behavior).* If you want to make it available for instance across the entire domain use `path: '/'`. Default: path of page where the cookie was created.

**Note regarding Internet Explorer:**

> Due to an obscure bug in the underlying WinINET InternetGetCookie implementation, IE’s document.cookie will not return a cookie if it was set with a path attribute containing a filename.

(From [Internet Explorer Cookie Internals (FAQ)](http://blogs.msdn.com/b/ieinternals/archive/2009/08/20/wininet-ie-cookie-internals-faq.aspx))

This means one cannot set a path using `path: window.location.pathname` in case such pathname contains a filename like so: `/check.html` (or at least, such cookie cannot be read correctly).

### domain

    domain: 'example.com'

Define the domain where the cookie is valid. Default: domain of page where the cookie was created.

### secure

    secure: true

If true, the cookie transmission requires a secure protocol (https). Default: `false`.

## Converters

Provide a conversion function as optional last argument for reading, in order to change the cookie's value
to a different representation on the fly.

Example for parsing a value into a number:

```javascript
$.cookie('foo', '42');
$.cookie('foo', Number); // => 42
```

Dealing with cookies that have been encoded using `escape` (3rd party cookies):

```javascript
$.cookie.raw = true;
$.cookie('foo', unescape);
```

You can pass an arbitrary conversion function.

## Contributing

Check out the [Contributing Guidelines](CONTRIBUTING.md)

## Authors

[Klaus Hartl](https://github.com/carhartl)
