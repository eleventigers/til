# WebRequest Restricted Headers

When using [`System.Net.WebRequest`](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx) some HTTP headers cannot be set/modified. Some "smart" dudes at MS decided that developers do not know web security therefore manually setting HTTP headers is a serious thing only to be done by the system.

Here is the list of restricted headers:

- `Accept`
- `Connection`
- `Content-Length`
- `Content-Type`
- `Date`
- `Expect`
- `Host`
- `If-Modified-Since`
- `Range`
- `Referer`
- `Transfer-Encoding`
- `User-Agent`
- `Proxy-Connection`

I found out about this when trying to set the `Date` header using `System.Net.WebRequest` in a [Unity3d](https://unity3d.com/) project. Even though some headers such as `Accept` can be modified using a dedicated method on `System.Net.HttpWebRequest` class, `Date` is not one of them. What's even worse is that the `Date` header does not get set by the system at all! This is super frustrating when you try to use `Date` to sign a request to a REST API which requires `path` + `headers` + `parameters` hash based authentication...

More info on [Stack Overflow](https://stackoverflow.com/questions/239725/cannot-set-some-http-headers-when-using-system-net-webrequest) and [Google Groups](https://groups.google.com/forum/embed/#!topic/zeep-mobile/VK1_fGClNFQ).