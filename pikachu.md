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

本题使用allen，因为是POST，不能在url栏直接构造payload，我们需要构造一个恶意网页，这里直接使用BurpSuite自带的工具。抓包，生成CSRF POC

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/EFBY~AL[96T}%]SLPF)YGKN.png?raw=ture">

修改相关信息，生成HTML

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/L1[F{1ON4(V$S]~7%GEO6Z4.png?raw=true">

进入HTML，点击提交，修改成功

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/_ZK3%$BUZ_T8YWDUPFZZ6QU.png?raw=true">

## SQL-Injection

### 数字型注入(post)

直接抓包，发送到Repeater

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/5PP1$EF(BR}8F7QBFDQE[YQ.png?raw=true">

判断字段数

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/RW0MVY``KZA04{$EN8D}E]1.png?raw=true">

判断回显点

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/UUGHG)L~$HFS~F06TJB`~[O.png?raw=true">

查看数据库和版本

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/Y0Q`6B[B53YJ9[)MQUZX%R7.png?raw=true">

查询所有表

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/OJAPH@OKF_N%V6N[WRM((4C.png?raw=true">

查询users下所有字段

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/3)G[X@~UR@51E0@PFAF}R$U.png?raw=true">

查询字段内容

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/Y}C4~NZXVBY5O%``BRHA9HM.png?raw=true">

### 字符型注入(get)

判断注入点

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/QMYKI6KGR_(7NXBP0VPEOSJ.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/SAZ_[3JP(77}QYYTWY5@Q$6.png?raw=true">

确定存在注入点，且单引号闭合，判断字段数

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/3Z)`EM%PGV(PM0@H_K2{[H3.png?raw=true">

判断回显点

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/V}2]2K5~N_2~QG{CU]934XB.png?raw=true">

下面步骤省略

### 搜索型注入

根据报错信息，数字后多了个百分号

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/[FZZ`{E2B0@{9}`W)TPEZH7.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/6{%)51O761)I`E4RGNMKCI1.png?raw=true">

### xx型注入

引号后多了个括号

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/T8)77@E2I_T`G}1G%LFWKM6.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/0CM%7~O40YM(KL3%YUWVX4V.png?raw=true">

### insert/update注入

进入注册页面，构建报错注入的payload。查询数据库

```sql
1' and extractvalue(1,concat(0x5c,(database()))) and '1'='1
```

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/OJNL6{ZNM0E50JU6G`%C}6Y.png?raw=true">

查询表

```sql
1' and extractvalue(1,concat(0x5c,(select table_name from information_schema.tables where table_schema='pikachu' limit 0,1))) and '1'='1
```

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/A2E{THKIKFGC]XK)WR_%$[T.png?raw=true">

查询users下字段

```sql
1' and extractvalue(1,concat(0x5c,(select column_name from information_schema.columns where table_schema='pikachu' and table_name='users' limit 1,1))) and '1'='1
```

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/`N0]WK}5ECSSWKXZR]1RL6B.png?raw=true">

查询字段内容，这里因为长度限制，所以使用mid函数截取获取到的内容，每次十个字符长度

```sql
1' and extractvalue(1,concat(0x7e,mid((select password from pikachu.users limit 0,1),1,10))) and '1'='1
```

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/1N6E[P3(OOIC8USQKBK7YVI.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/C)4C~~LNFCOQ5O710S~[P}M.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/BV4E}8EW]W`M5%J4EOFNV$H.png?raw=true">

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/N{3AJ()KB@N}OB5A`N18K`O.png?raw=true">

*注： `mid(arg1, arg2, arg3)` ，mid函数可以截取一段文本中的内容，有三个参数，arg1是需要截取的文本内容，arg2是截取位置，从1开始，arg3是截取长度*

### delete注入

有一个留言板，提交后可以删除

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/HCZ%FT8)RI4NV0AG56NLDEC.png?raw=true">

当光标移动到删除上的时候，左下角出现请求

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/~VIZN~Q~A9[3`_79Q_IMI%4.png?raw=true">

抓包，发现是GET传参，发送到Repeater构建payload，注意需要转换为url编码

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/@Z{(3EIOY0KKJD]_5T9]P`P.png?raw=true">

查询数据库

```sql
1%20and%20extractvalue(1,concat(0x5c,(database())))
```

> <img src="https://github.com/Ki1z/Pikachu/blob/main/IMG/IAP@@PE2FM_Q56175~9KNX9.png?raw=true">

以下省略
