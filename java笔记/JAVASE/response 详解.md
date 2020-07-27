# response 详解

## getRequestURL

获取完整项目路径 返回URL

## getRequestURI

获取资源名部分

## URL 与 URI区别

URL是完整目录 URI是资源		URI是主机名后边数据前边

## getQueryString

请求的字符串  请求部分的数据   "?"后边的部分 参数名+值

http://localhost/web名?username=abc&pwd=123

request.getQueryString 会得到username=abc&pwd=123

## getRemoteAddr

等到主机IP地址 		比如127.0.0..1

## getRemoteHost

得到访问的主机名		比如localhost

## getRemotePort

客户机端口号

## getLocalPort getLocalAddr getLoalName

获得服务器的端口号, 地址, 主机名



