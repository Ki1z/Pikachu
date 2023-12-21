# Pikachu

鉴于已经完成了DVWA，该靶场的讲解将会比较简洁，当作复习DVWA的各项操作

## 暴力破解

### 基于表单的暴力破解

输入admin和admin回车，提示username or password is not exists，看来不能先确定用户名

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/(AJ%WCQDV]A8_$R_5)(W$4I.png?raw=true">

直接爆破两者

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/@7XN%}%KS51[VJI}4({O0M8.png?raw=true">

最后得到用户名为admin，密码为123456

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/8(ZH4GT]8A7EODED7{OZUZW.png?raw=true">

### 验证码绕过(on server)

进行抓包，这里抓包需要输入正确验证码。为了节省时间，假设已知用户名为admin，爆破密码。

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/7AIF(]%M2T(8RTPN4ZA~`GY.png?raw=true">

因为服务器不刷新验证码，直接得到密码为12456

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/G8AAFP_[3$R(NAC864[_1IW.png?raw=true">

### 验证码绕过(on client)

前端的验证码形同虚设，直接删掉参数即可，但是在抓包的时候同样需要输入正确的验证码

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/(%)X%5$D$1LU@575H8)C](B.png?raw=true">

### token防爆破

这个在DVWA里我们也见过了，简单带过，首先抓包标记payload

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/1XBMCKR%}85H6`S5[T6OMFF.png?raw=true">

payload1填入常见密码，重点是payload2，首先获取token

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/I[2C7$8_PA`O0WOU(NU41JN.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/W$$UJ~15VIMHYU8PY7OSOC0.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/OXSE}B[`6DQ5DRGKFI37GOJ.png?raw=true">

注意，带有token的爆破只能使用单线程

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/5091LKBWFZN@T_4`~SL]0ET.png?raw=true">

密码为123456

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/ZT7}[4MUTOMUFWD{%~HC]DD.png?raw=true">

## CSRF

### CSRF(get)

根据提示，登录其中一个账号，这里我使用vince

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/]VUKEUW88B1D{DGBEHOEVNJ.png?raw=true">

登陆进入后，发现是修改个人信息的页面

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/WW$6`8%LD25[M_@HHZ$[%]L.png?raw=true">

点击修改信息，并进行抓包

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/6M~~06L4%{5]CMTU}]`STRA.png?raw=true">

修改sex为male并放行，发现修改成功

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/2EQ{]%A[N4[((LY]}{$32~I.png?raw=true">

直接构造CSRF攻击的payload

```url
http://127.0.0.1/pikachu/vul/csrf/csrfget/csrf_get_edit.php?sex=male&phonenum=U HAS BEEN HACKED&add=chain&email=vince%40pikachu.com&submit=submit
```

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/$9561SMFA%@0(JBJP3[VN1I.png?raw=true">

### CSRF(POST)

本题使用allen，因为是POST，不能在url栏直接构造payload，我们需要构造一个恶意网页