CAS 开发事项

## 加密策略
1. 当加密方式为自定义时，需配置如下
XXX.passwordEncoder.type=com.paladin.cas.server.PasswordEncryption //自定义加密类 必须实现spring的PasswordEncoder
XXX.passwordEncoder.characterEncoding=UTF-8							
XXX.passwordEncoder.encodingAlgorithm=MD5							//加密方式

## 数据库方式登录认证
1. 配置application.properties中,以cas.authn.jdbc.query[0]开头的值，多个数据库以query[X]配置。


## rest方式登录认证
1. 接口认证需提供一个rest接口，以做登录验证使用
2. 接口的接收参数以：@RequestHeader HttpHeaders httpHeaders来接收
3. httpHeaders中的authorization的值为Basic Base64编码后的值，需将值new String(Base64Utils.decodeFromString(baseCredentials), "UTF-8");反向编译成字符串，内里包含用户名和密码。
4. 接口返回值如200，则需包含{"@class":"org.apereo.cas.authentication.principal.SimplePrincipal","id":"casuser","attributes":{}}等值，否则不认为登录成功


## service Json配置
注意：Json文件名字规则为${name}-${id}.json，id必须为Json文件内容Json一致。
json文件解释：
1. @class：必须为org.apereo.cas.services.RegisteredService的实现类，对其他属性进行一个json反射对象，常用的有RegexRegisteredService，匹配策略为id的正则表达式
2. serviceId：唯一的服务id
3. name： 服务名称，会显示在默认登录页
4. id：全局唯一标志
5. description：服务描述，会显示在默认登录页
6. evaluationOrder： 匹配争取时的执行顺序，最好是比1大的数字
7. logoutType : 服务注册表中的每个注册服务都将包含描述如何提交注销请求的配置。此行为通过logoutType属性进行控制。该属性允许指定是应通过后/前通道提交注销请求还是为此应用程序关闭注销请求，关闭注销请求可以将logoutType设置为NONE。


## 自定义登录或退出样式页面
1. casLoginView.html文件不能乱命名，必须为casLoginView.html。这里是覆盖登录页面所以命名为casLoginView.html，如果要覆盖退出页面则是casLogoutView.html
