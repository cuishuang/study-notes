---
title: '使用pstree,查看进程树'
date: 2017-03-11 19:20:39
tags: Linux
---


在我的腾讯云CVM服务器上:


### `pstree` 查看进程树


<br>

**pstree**


```shell
systemd─┬─YDLive───{YDLive}
        ├─YDService───12*[{YDService}]
        ├─accounts-daemon─┬─{gdbus}
        │                 └─{gmain}
        ├─acpid
        ├─2*[agetty]
        ├─atd
        ├─barad_agent─┬─barad_agent
        │             └─barad_agent───3*[{barad_agent}]
        ├─containerd─┬─containerd-shim─┬─teacher───3*[{teacher}]
        │            │                 └─10*[{containerd-shim}]
        │            └─9*[{containerd}]
        ├─cron
        ├─dbus-daemon
        ├─dockerd─┬─docker-proxy───5*[{docker-proxy}]
        │         └─9*[{dockerd}]
        ├─gunicorn───gunicorn───{gunicorn}
        ├─2*[iscsid]
        ├─lvmetad
        ├─mdadm
        ├─mongod───9*[{mongod}]
        ├─mysqld───28*[{mysqld}]
        ├─named───3*[{named}]
        ├─nginx───nginx
        ├─ntpd
        ├─php-fpm7.2───3*[php-fpm7.2]
        ├─polkitd─┬─{gdbus}
        │         └─{gmain}
        ├─python───4*[{python}]
        ├─rsyslogd─┬─{in:imklog}
        │          ├─{in:imuxsock}
        │          └─{rs:main Q:Reg}
        ├─sgagent───{sgagent}
        ├─shuang───4*[{shuang}]
        ├─sshd───sshd───sshd───bash───pstree
        ├─systemd───(sd-pam)
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        ├─unattended-upgr───{gmain}
        └─uuidd

```

<br>

### `pstree -p` 查看进程树，并打印每个进程的PID


<br>

**pstree -p**

```shell
systemd(1)─┬─YDLive(23458)───{YDLive}(23488)
           ├─YDService(23429)─┬─{YDService}(23467)
           │                  ├─{YDService}(23468)
           │                  ├─{YDService}(23469)
           │                  ├─{YDService}(23750)
           │                  ├─{YDService}(23751)
           │                  ├─{YDService}(23762)
           │                  ├─{YDService}(23763)
           │                  ├─{YDService}(23764)
           │                  ├─{YDService}(23765)
           │                  ├─{YDService}(23766)
           │                  ├─{YDService}(23767)
           │                  └─{YDService}(23769)
           ├─accounts-daemon(1011)─┬─{gdbus}(1098)
           │                       └─{gmain}(1077)
           ├─acpid(1099)
           ├─agetty(1314)
           ├─agetty(1320)
           ├─atd(1123)
           ├─barad_agent(2068)─┬─barad_agent(2074)
           │                   └─barad_agent(2075)─┬─{barad_agent}(2086)
           │                                       └─{barad_agent}(2091)
           ├─containerd(1160)─┬─containerd-shim(1925)─┬─teacher(1942)─┬─{teacher}(2009)
           │                  │                       │               ├─{teacher}(2010)
           │                  │                       │               └─{teacher}(2011)
           │                  │                       ├─{containerd-shim}(1926)
           │                  │                       ├─{containerd-shim}(1927)
           │                  │                       ├─{containerd-shim}(1928)
           │                  │                       ├─{containerd-shim}(1929)
           │                  │                       ├─{containerd-shim}(1930)
           │                  │                       ├─{containerd-shim}(1931)
           │                  │                       ├─{containerd-shim}(1932)
           │                  │                       ├─{containerd-shim}(1933)
           │                  │                       ├─{containerd-shim}(1987)
           │                  │                       └─{containerd-shim}(2043)
           │                  ├─{containerd}(1359)
           │                  ├─{containerd}(1360)
           │                  ├─{containerd}(1361)
           │                  ├─{containerd}(1386)
           │                  ├─{containerd}(1387)
           │                  ├─{containerd}(1403)
           │                  ├─{containerd}(1404)
           │                  ├─{containerd}(1427)
           │                  └─{containerd}(19570)
           ├─cron(1128)
           ├─dbus-daemon(1052)
           ├─dockerd(1189)─┬─docker-proxy(1918)─┬─{docker-proxy}(1919)
           │               │                    ├─{docker-proxy}(1920)
           │               │                    ├─{docker-proxy}(1921)
           │               │                    ├─{docker-proxy}(1922)
           │               │                    └─{docker-proxy}(1923)
           │               ├─{dockerd}(1378)
           │               ├─{dockerd}(1379)
           │               ├─{dockerd}(1380)
           │               ├─{dockerd}(1428)
           │               ├─{dockerd}(1542)
           │               ├─{dockerd}(1543)
           │               ├─{dockerd}(1547)
           │               ├─{dockerd}(1566)
           │               └─{dockerd}(1567)
           ├─gunicorn(1376)───gunicorn(1840)───{gunicorn}(1924)
           ├─iscsid(1153)
           ├─iscsid(1154)
           ├─lvmetad(427)
           ├─mdadm(1277)
           ├─mongod(1009)─┬─{mongod}(1353)
           │              ├─{mongod}(1354)
           │              ├─{mongod}(1355)
           │              ├─{mongod}(1565)
           │              ├─{mongod}(1568)
           │              ├─{mongod}(1574)
           │              ├─{mongod}(1575)
           │              ├─{mongod}(1576)
           │              └─{mongod}(1577)
           ├─mysqld(30985)─┬─{mysqld}(30995)
           │               ├─{mysqld}(30996)
           │               ├─{mysqld}(30997)
           │               ├─{mysqld}(30998)
           │               ├─{mysqld}(30999)
           │               ├─{mysqld}(31000)
           │               ├─{mysqld}(31001)
           │               ├─{mysqld}(31002)
           │               ├─{mysqld}(31003)
           │               ├─{mysqld}(31004)
           │               ├─{mysqld}(31005)
           │               ├─{mysqld}(31006)
           │               ├─{mysqld}(31008)
           │               ├─{mysqld}(31009)
           │               ├─{mysqld}(31010)
           │               ├─{mysqld}(31011)
           │               ├─{mysqld}(31012)
           │               ├─{mysqld}(31013)
           │               ├─{mysqld}(31014)
           │               ├─{mysqld}(31015)
           │               ├─{mysqld}(31016)
           │               ├─{mysqld}(31017)
           │               ├─{mysqld}(31018)
           │               ├─{mysqld}(31019)
           │               ├─{mysqld}(31022)
           │               ├─{mysqld}(31023)
           │               ├─{mysqld}(31024)
           │               └─{mysqld}(6445)
           ├─named(1046)─┬─{named}(1074)
           │             ├─{named}(1075)
           │             └─{named}(1076)
           ├─nginx(10919)───nginx(7623)
           ├─ntpd(1455)
           ├─php-fpm7.2(1100)─┬─php-fpm7.2(1488)
           │                  ├─php-fpm7.2(31760)
           │                  └─php-fpm7.2(31762)
           ├─polkitd(1184)─┬─{gdbus}(1200)
           │               └─{gmain}(1198)
           ├─python(1520)─┬─{python}(1536)
           │              ├─{python}(1537)
           │              ├─{python}(1539)
           │              └─{python}(1540)
           ├─rsyslogd(1018)─┬─{in:imklog}(1072)
           │                ├─{in:imuxsock}(1071)
           │                └─{rs:main Q:Reg}(1073)
           ├─sgagent(2056)───{sgagent}(2057)
           ├─shuang(8155)─┬─{shuang}(8156)
           │              ├─{shuang}(8157)
           │              ├─{shuang}(8158)
           │              └─{shuang}(24887)
           ├─sshd(1141)───sshd(660)───sshd(727)───bash(731)───pstree(3190)
           ├─systemd(3179)───(sd-pam)(3185)
           ├─systemd-journal(390)
           ├─systemd-logind(1118)
           ├─systemd-udevd(460)
           ├─unattended-upgr(1183)───{gmain}(1429)
           └─uuidd(1579)

```

<br>


###  `pstree -p <pid>` 查看某个进程树型结构 


<br>

**pstree -p 1189**

```shell

dockerd(1189)─┬─docker-proxy(1918)─┬─{docker-proxy}(1919)
              │                    ├─{docker-proxy}(1920)
              │                    ├─{docker-proxy}(1921)
              │                    ├─{docker-proxy}(1922)
              │                    └─{docker-proxy}(1923)
              ├─{dockerd}(1378)
              ├─{dockerd}(1379)
              ├─{dockerd}(1380)
              ├─{dockerd}(1428)
              ├─{dockerd}(1542)
              ├─{dockerd}(1543)
              ├─{dockerd}(1547)
              ├─{dockerd}(1566)
              └─{dockerd}(1567)
```

<br>

 **pstree -p 1378**

 ```shell
{dockerd}(1378)
```
