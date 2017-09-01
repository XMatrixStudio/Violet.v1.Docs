\# Violet SDK使用说明 



目前暂时只支持mongodb+nodejs的后台，更多的数据库和服务端配合以后可能会推出



\#\#使用前准备



\#\#\# 1. 设置配置文件



在config文件夹下新建一个violet.json的文件



\`\`\`json

{

  "host": "oauth.xmatrix.studio",

  "key": "从管理中心获取",

  "webId": "从管理中心获取"

}

\`\`\`



\#\#\# 2.修改代码



修改verify.js的第一行代码，使其可以访问mongodb的用户数据库



\`\`\`js

// verify.js

const userDB = require\('../user.js'\).db;

\`\`\`



引号内为用户数据库模块文件地址



\`\`\`js

// user.js

exports.db = userDB; //把用户数据库设置为外部接口

\`\`\`



\#\#\# 3. 引用SDK



在需要使用sdk的js里面增加下列代码



\`\`\`js

const verify = require\('./sdk/verify.js'\);

\`\`\`











\#\# 常用API



\#\#\# makeASha\(str\[, sign\]\)



简介： 使用SHA512对字符串进行散列，常用于密码



参数： str\(需要进行Hash的字符串\)、sign\(加盐\)\(默认为配置文件里面的key\)



返回：Hash后的字符串



\#\#\#makeAMD5\(str\[, sign\]\)



简介： 使用MD5对字符串进行散列，常用于密码



参数： str\(需要进行Hash的字符串\)、sign\(加盐\)\(默认为配置文件里面的key\)



返回：Hash后的字符串



\#\#\# getUserId\(res\)



简介： 获取用户id，此函数需要在一次请求内，使用过checkToken\(\)之后才能使用



参数：res



返回：用户id



\#\#\# getUserData\(res\)



简介： 获取用户具体信息，此函数需要在一次请求内，使用过checkToken\(\)之后才能使用



参数：res



返回：用户具体信息的JSON



\#\#\# checkToken\(req ,res, next\)



简介：检查cookies里面token的有效性，并更新cookies里面的token，常用于中间件



参数：req, res, next



返回：无



\#\#\# getLoginState\(req\)



简介：获取登陆状态，检查请求中token是否存在



参数：req



返回：是否存在token的boolean



\#\#\# setCookies\(res, name, data, time ,callback\)



简介： 设置cookies



参数：res, name\(cookie名字\),data\(具体数据\), time\(有效时间，单位为秒\), callback



返回：无



\#\#\# logout\(req ,res, next\)



简介：退出登陆，清除token，设置状态



参数：req, res, next



返回：无



\#\#\# getUserInfo\(code,callback\)



简介：使用授权码获取用户信息



参数：code\(用户授权码\), callback\(data\)



返回： 回调第一个参数返回结果：data.state为状态、data.reason为错误原因、data.userData为成功后返回的用户数据JSON



\`\`\`json

// data.userData

{

  state: 'ok',

  uid: val.uid,

  name: val.name,

  email: val.email,

  phone: val.phone,

  detail: val.detail,

  web: val.web,

  sex: val.sex,

  birthTime: val.birthTime,

 }

\`\`\`







