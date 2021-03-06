
# 基于django的单点登录系统

![image](https://github.com/myide/django-sso/blob/master/media/imgs/ssostruct.jpg)

## 实现机制
  当用户第一次访问应用系统1的时候，因为还没有登录，会被引导到认证系统中进行登录；根据用户提供的登录信息，认证系统进行身份校验，如果通过校验，应该返回给用户一个认证的凭据－－ticket；用户再访问别的应用的时候就会将这个ticket带上，作为自己认证的凭据，应用系统接受到请求之后会把ticket送到认证系统进行校验，检查ticket的合法性。如果通过校验，用户就可以在不用再次登录的情况下访问应用系统2和应用系统3了。


## 主要功能
- 系统共享
    - 统一的认证系统是SSO的前提之一。认证系统的主要功能是将用户的登录信息和用户信息库相比较，对用户进行登录认证；认证成功后，认证系统应该生成统一的认证标志（ticket），返还给用户。另外，认证系统还应该对ticket进行校验，判断其有效性。

- 信息识别
    - 要实现SSO的功能，让用户只登录一次，就必须让应用系统能够识别已经登录过的用户。应用系统应该能对ticket进行识别和提取，通过与认证系统的通讯，能自动判断当前用户是否登录过，从而完成单点登录的功能。
    - 另外：
        - 1、单一的用户信息数据库并不是必须的，有许多系统不能将所有的用户信息都集中存储，应该允许用户信息放置在不同的存储中，事实上，只要统一认证系统，统一ticket的产生和校验，无论用户信息存储在什么地方，都能实现单点登录。
        - 2、统一的认证系统并不是说只有单个的认证服务器
当用户在访问应用系统1时，由第一个认证服务器进行认证后，得到由此服务器产生的ticket。当他访问应用系统2的时候，认证服务器2能够识别此ticket是由第一个服务器产生的，通过认证服务器之间标准的通讯协议（例如SAML）来交换认证信息，仍然能够完成SSO的功能。


## 环境

- Python 3.6
    - Django 2.0
    - Django Rest Framework 3.8

## 使用

- sso服务端: django-sso
    - 启动服务端，端口8090，假定地址为 www.django-sso.com

- 接入sso的子系统(工程名:projectName):
    - 从django-sso服务端拷贝文件django-sso/auth.py, 放置在子系统的工程目录下

    - 设置 settings.py
    ```bash
        SSO_URL = 'http://www.django-sso.com:8090/api/account/users/'
        MIDDLEWARE = [
            ...
            'projectName.auth.AuthMiddleware',
            ...
        ]
    ```

- 各系统前端
    - 设置用户的登录地址 'http://www.django-sso.com:8090/api/account/login/'
    - 获取登录地址的token并保存，对访问子系统的请求头加入 {'Authorization': 'JWT ' + token值}

## 交流学习
- QQ群 630791951

## License

- BSD 3-Clause License

