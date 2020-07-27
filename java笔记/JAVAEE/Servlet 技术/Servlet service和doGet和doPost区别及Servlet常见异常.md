## service和doGet和doPost方法的区别

### service方法

不管是 get 方式还是 post 方式的请求，如果 Servlet 类中有
service 方法，则优先调用 Service 方法。 

### doGet方法

在没有 service 方法的情况下如果是 get 方式的请求所调用
的处理请求的方法 

### doPost方法

在没有 service 方法的情况下如果是 post 方式的请求所调
用的处理请求的方法 

## Servlet中常见的错误总结

Servlet的常见错误：

* 404错误:资源未找到

  原因一：在请求地址中的servlet的别名书写错误。

  原因二：虚拟项目名称拼写错误

* 500错误：内部服务器错误

  错误一：java.lang.ClassNotFoundException:
  

  解决：

  在web.xml中校验servlet类的全限定路径是否拼写错误。

  错误二：

  因为service方法体的代码执行错误导致

  解决：

  根据错误提示对service方法体中的代码进行错误更改。

* 405错误:请求方式不支持

  原因：

  请求方式和servlet中的方法不匹配所造成的 

  解决：

  尽量使用 service 方法进行请求处理，并且不要再 service 方法中调用父类的
  service。 

