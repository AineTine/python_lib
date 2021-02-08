# Yagmail

> yagmail是由[Pascal van Kooten](mailto:kootenpv@gmail.com)制作的简单邮件系统，目的是让发送电子邮件的过程尽可能的简单
>
> yagmail支持通过密码访问发送者账户，虽然文档中显示可以支持keyring/OAuth2，但对于大部分人来说这并不适合，因此本文仅讲述通过密码的方式连接
>
> [yagmail文档支持库](https://yagmail.readthedocs.io/en/latest/index.html)
>
> [pip界面](https://pypi.org/project/yagmail/)

## 基本流程

### 1.安装Yagmail支持库(pip方式)

> 如果在国内，建议在安装任何库前先修改镜像库地址，具体请参阅**pip**篇

python2(Linux)

```shell
pip install yagmail
```

python3

```shell
pip3 install yagmail
```

> 这两种安装方式本质上都是pip安装，如果实在不知道自己计算机上安装的是Python2还是python3，可以都试一遍…
>
> 查看python的版本请参阅**python编译器**篇

### 2.写一个简单的示例

#### 使用密码

```python
# 首先必须要导入yagmail库
import yagmail

# 准备凭证
# user 代表用户名
# password 代表邮箱密码
# host 代表发信服务器
# port 发信端口
# smtp_ssl 使用ssl协议（默认为true）
# encoding 编码方式（默认utf-8）
yag = yagmail.SMTP(user="youremail@126.com", password="123456", host='smtp.126.com')
# 发送邮件
yag.send(
    # to 收件人，如果一个收件人用字符串，多个收件人用列表即可
    to=['sendto@outlook.com', 'sendto@126.com'],
    # cc 抄送，含义和传统抄送一致，使用方法和to 参数一致
    cc='123456789@qq.com',
    # subject 邮件主题（也称为标题）
    subject='邮件主题',
    # contents 邮件正文
    contents='邮件正文',
    # attachments 附件，和收件人一致，如果一个附件用字符串，多个附件用列表
    attachments=[r'c://text.txt', r'd://mmp.exe'])
# 记得关掉链接，避免浪费资源
yag.close()
```

## 重要的方法

### SMTP的init方法

```python
def __init__(
    # 用户名
    user=None,
    # 密码
    password=None,
    # 发信服务器
    host='smtp.gmail.com',
    # 默认端口
    port=None,
    smtp_starttls=None,
    # 使用ssl协议
    smtp_ssl=True,
    # debug等级
    smtp_set_debuglevel=0,
    # 跳过登录
    smtp_skip_login=False,
    # 编码格式
    encoding='utf-8',
    # 开放授权协议（比如想要登录百度，可以用微博认证一样）
    # 此处不常用
    oauth2_file=None,
    soft_email_validation=True,
    **kwargsyagmail.SMTP
)
```

### send

```python
yag.send(
    # 发送到，发送给
    to=None,
    # 邮件主题，邮件标题
    subject=None,
    # 邮件内容
    contents=None,
    # 附件
    attachments=None,
    # 抄送
    cc=None,
    # 密送
    bcc=None,
    # 仅预览
    preview_only=False,
    #头部信息
    headers=None,
    # 修饰为html
    prettify_html=True,
    # 消息id
    message_id=None,
    # 组消息
    group_messages=True
)
```

### close

```python
# 链接用完后要关闭
yag.close()
```

### send_unsent

```python
# 如果发送失败，调用send_unsent方法可以再次尝试发送失败的邮件
yag.send_unsent()
```

## 异常

### exception yagmail.error.YagAddressError

#### 原因：无法识别的电子邮件，一般的，指电子邮件格式错误

#### 解决方案：检查电子邮件的格式

### exception yagmail.error.YagConnectionClosed

#### 原因：链接被销毁

#### 解决方案：在链接建立时发送邮件（一般是因为发信前调用了close方法）

### exception yagmail.error.YagInvalidEmailAddress

#### 原因：邮件地址中存在语法错误

#### 解决方案：检查地址是否正确