# jwt_epl_dll_wechat
+ jwt.e 源码文件
+ Jwt.ec 易语言模块
+  jwt.dll 动态链接库


三张截图说明了公开的使用方法

源码作者Ym_gg(修改) 黎明（主承）

QQ1195557458(Ym_gg)

直接使用此源码内容需要遵守GPL协议

### 以下为源码主要内容
### 完整源码请下载Jwt.zip
```

.版本 2

.程序集 程序集1

.子程序 _启动子程序, 整数型, , 请在本子程序中放置易模块初始化代码

' 源码作者Ym_gg(修改) 黎明（主承）
' QQ1195557458(Ym_gg)
' 直接使用此源码内容需要遵守GPL协议
_临时子程序 ()  ' 在初始化代码执行完毕后调用测试代码
返回 (0)  ' 可以根据您的需要返回任意数值

.子程序 _临时子程序
.局部变量 token, 文本型
.局部变量 post, 字节集
.局部变量 post_text, 文本型
.局部变量 EncodingAESKey, 文本型

' token ＝ “”
' EncodingAESKey ＝ “”

' post ＝ 网页_访问 (“https://openai.weixin.qq.com/openapi/message/” ＋ token, 1, “query=” ＋ jwt (#payload, EncodingAESKey))
' post_text ＝ 编码_utf8到gb2312 (到文本 (post))

' 输出调试文本 (post_text)

.子程序 jwt, 文本型, 公开, 基于HMACSHA256 URLBASE64
.参数 payload, 文本型, , 请传入json文本msg username
.参数 key, 文本型, , EncodingAESKey
.局部变量 json, 类_json
.局部变量 i, 整数型
.局部变量 前两部分, 文本型
.局部变量 结果, 文本型


json.解析 (payload)
.如果真 (json.属性是否存在 (“jti”) ＝ 假)
    json.置属性 (“jti”, “5de35dec-9104-47aa-b04d-4fd723d00e7a”)  ' jti暂无加密方法
.如果真结束
.如果真 (json.属性是否存在 (“iat”) ＝ 假)
    json.置属性数值 (“iat”, 到长整数 (时间_取现行时间戳 (真)))
.如果真结束
.如果真 (json.属性是否存在 (“exp”) ＝ 假)
    json.置属性数值 (“exp”, 到长整数 (时间_取现行时间戳 (真)) ＋ 3600)
.如果真结束
前两部分 ＝ URLbase64_字节集 (到字节集 (#header)) ＋ “.” ＋ URLbase64_字节集 (编码_Ansi到Utf8 (json.取数据文本 ()))
json.清除 ()
结果 ＝ 前两部分 ＋ “.” ＋ URLbase64_字节集 (字节集_十六进制到字节集 (HmacSHA256 (前两部分, key)))
输出调试文本 (结果)
返回 (结果)


.子程序 URLbase64_字节集, 文本型
.参数 预编码文本, 字节集

返回 (文本_替换 (编码_BASE64编码 (预编码文本), , , , “=”, “”, “+”, “-”, “/”, “_”))


.子程序 URLbase64, 文本型, 公开
.参数 预编码文本, 文本型

返回 (文本_替换 (编码_BASE64编码 (到字节集 (预编码文本)), , , , “=”, “”, “+”, “-”, “/”, “_”))



.子程序 HmacSHA256, 文本型, 公开, https://www.eyuyan.la/post/1954.html
.参数 加密文本, 文本型
.参数 密钥, 文本型
.局部变量 脚本对象, 对象
.局部变量 局部文本, 文本型

CoInitialize (0)
脚本对象.创建 (“ScriptControl”, )
脚本对象.写属性 (“Language”, “JScript”)
脚本对象.数值方法 (“ExecuteStatement”, #sha256)
局部文本 ＝ 脚本对象.文本方法 (“Run”, “HMAC”, 加密文本, 密钥)
CoUninitialize ()  ' 返回之前使用 必须释放掉对象才可用
返回 (局部文本)
···
