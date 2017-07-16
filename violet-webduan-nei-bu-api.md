

# Violet Web端内部API

## 登陆前API

### POST /api/register

简介：

注册账号、注册成功后直接登陆

前端也要正则检测

name：/^[a-zA-Z0-9]{3,10}$/

email:  /^(\w)+(\.\w+)*@(\w)+((\.\w{2,9}))$/

password:  /^.{6,24}$/

参数：

name

email

password

返回:

用户名重复 {state: 'failed',reason:‘MULTIPLE_NAME’}  

邮箱重复{state: 'failed',reason:‘MULTIPLE_EMAIL’}  

非法用户名{state: 'failed',reason:‘ILLEGAL_NAME’}  

非法邮箱{state: 'failed',reason:‘ILLEGAL_EMAIL’}  

成功{ state: 'ok',name,email } //转到激活邮箱界面

### POST /api/login

简介：

参数：

user // 用户名或邮箱（自动识别）

password

sid //要登陆的站点id

ps：设置cookie里面的remember //是否记住登陆

返回：

邮箱不存在{state: 'failed',reason:‘NO_EMAIL’}  

用户不存在{state: 'failed',reason:‘NO_USERNAME’}  

密码错误{state: 'failed',reason:‘ERR_PWD’}  

没有激活邮箱 {state: 'failed',reason:‘VALID_EMAIL’ ,email,name}  //转到激活邮箱界面

成功{ state: 'ok',siteName: val.name,email,name} // 转到授权

​	{state: 'ok',siteName:‘VIOLET’,email,name}   // 进入用户中心



### POST /api/getCode

简介： 获取验证码

参数：

email

返回：

邮箱格式不合法{state: 'failed',reason:ILLEGAL_EMAIL’}  

等待60秒后才能发{state: 'failed',reason:‘WAITING_TIME’}  

邮箱不存在{state: 'failed',reason:‘NO_EMAIL’}  

成功{ state: 'ok' }



### POST /api/reset/

简介：重置密码

参数：

email

vCode //验证码

password

返回：

邮箱格式不合法{state: 'failed',reason:ILLEGAL_EMAIL’}  

邮箱不存在{state: 'failed',reason:‘NO_EMAIL’}  

验证码错误{state: 'failed',reason:‘ERR_CODE’}  

成功{ state: 'ok' } // 跳转到登陆界面



### POST /api/validEmail/

简介：激活邮箱

参数：

email

vCode //验证码

sid //站点id

返回：

邮箱不存在{state: 'failed',reason:‘NO_EMAIL’}  

验证码错误{state: 'failed',reason:‘ERR_CODE’}  

验证码超时{ state: 'failed', reason: 'TIMEOUT' }

成功{ state: 'ok' ,siteName: 站点名称,name, email} // 跳转到授权界面

成功{ state: 'ok'， siteName：'VIOLET', name, email} // 转到用户中心









### POST /api/getInfo 

简介： 根据授权码获取用户信息（这在应用服务器上部署）

参数：

userToken 授权码

webToken 网站令牌

返回：

授权码无效{ state: 'failed', reason: 'USER_ERR' }

网站令牌无效{ state: 'failed', reason: 'WEB_ERR' }

成功{

​          state: 'ok',

​          uid: val.uid,

​          name: val.name,

​          email: val.email,

​          phone: val.phone,

​          detail: val.detail,

​          sex: val.sex,

​          birthTime: val.birthTime

 }



## 登陆后API （需要cookie的Token）

---

**以下所有API,如果Token检测不通过返回：**

用户令牌无效{ state: 'failed', reason: 'ERR_TOKEN' }

用户令牌过期{ state: 'failed', reason: 'TIMEOUT' }

---



### POST /api/auth

简介： 授权

参数：

sid //站点id

返回：

成功{ state: 'ok', url: siteUrl, code: createCode() } // 回调



### POST /api/getUser

简介： 获取用户和网站信息

参数：

sid

返回：

没有激活邮箱 {state: 'failed',reason:‘VALID_EMAIL’ ,email,name}  //转到激活邮箱界面

成功{ state: 'ok'， siteName, name, email} // 转到授权界面

成功{ state: 'ok'， siteName：'VIOLET', name, email} // 转到用户中心



### POST /api/logout

简介：退出登陆

参数:

无

返回：

成功{ state: 'ok }

---

**以下为管理界面专属API**

### POST /api/noAuth

简介：取消授权

参数:

sid

返回：

成功{ state: 'ok }





### POST /api/setUserInfo

简介：设置用户信息

参数:

web;

birthDate;

detail;

phone;

sex(0: 男1: 女2: 未确定)

返回：

成功{ state: 'ok }



### POST /api/getUserInfo

简介：获取用户信息

参数:

无

返回：

成功

{ 

state: 'ok‘

 userData: {

​          name: val.name,

​          email: val.email,

​          web: val.web,

​          birthDate: val.birthDate,

​          detail: val.detail,

​          phone: val.phone,

​          sex: val.sex,

​          sites: val.sites,

​        },

WebData:[{

​          sid: data[index].sid,

​          name: data[index].name,

​          url: data[index].url,

​        }...]

 }



