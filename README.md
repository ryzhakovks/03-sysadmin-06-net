**1. Работа c HTTP через телнет.
Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
Отправьте HTTP запрос
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]**

Ответ 
```rb
HTTP/1.1 403 Forbidden
Connection: close
Content-Length: 1918
Server: Varnish
Retry-After: 0
Content-Type: text/html
Accept-Ranges: bytes
Date: Mon, 13 Feb 2023 17:45:11 GMT
Via: 1.1 varnish
X-Served-By: cache-fra-eddf8230053-FRA
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1676310312.877688,VS0,VE1
X-DNS-Prefetch-Control: off

<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Forbidden - Stack Exchange</title>
    <style type="text/css">
                body
                {
                        color: #333;
                        font-family: 'Helvetica Neue', Arial, sans-serif;
                        font-size: 14px;
                        background: #fff url('img/bg-noise.png') repeat left top;
                        line-height: 1.4;
                }
                h1
                {
                        font-size: 170%;
                        line-height: 34px;
                        font-weight: normal;
                }
                a { color: #366fb3; }
                a:visited { color: #12457c; }
                .wrapper {
                        width:960px;
                        margin: 100px auto;
                        text-align:left;
                }
                .msg {
                        float: left;
                        width: 700px;
                        padding-top: 18px;
                        margin-left: 18px;
                }
    </style>
</head>
<body>
    <div class="wrapper">
                <div style="float: left;">
                        <img src="https://cdn.sstatic.net/stackexchange/img/apple-touch-icon.png" alt="Stack Exchange" />
                </div>
                <div class="msg">
                        <h1>Access Denied</h1>
                        <p>This IP address (188.16.91.218) has been blocked from access to our services. If you believe this to be in error, please contact us at <a href="mailto:team@stackexchange.com?Subject=Blocked%20188.16.91.218%20(Request%20ID%3A%20486472251-FRA)">team@stackexchange.com</a>.</p>
                        <p>When contacting us, please include the following information in the email:</p>
                        <p>Method: block</p>
                        <p>XID: 486472251-FRA</p>
                        <p>IP: 188.16.91.218</p>
                        <p>X-Forwarded-For: </p>
                        <p>User-Agent: </p>

                        <p>Time: Mon, 13 Feb 2023 17:45:11 GMT</p>
                        <p>URL: stackoverflow.com/questions</p>
                        <p>Browser Location: <span id="jslocation">(not loaded)</span></p>
                </div>
        </div>
        <script>document.getElementById('jslocation').innerHTML = window.location.href;</script>
</body>
</html>Connection closed by foreign host.
```

**2. Повторите задание 1 в браузере, используя консоль разработчика F12.
откройте вкладку Network
отправьте запрос http://stackoverflow.com
найдите первый ответ HTTP сервера, откройте вкладку Headers
укажите в ответе полученный HTTP код
проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
приложите скриншот консоли браузера в ответ.***




![Image alt](https://github.com/ryzhakovks/03-sysadmin-06-net/blob/main/23.png)

![Image alt](https://github.com/ryzhakovks/03-sysadmin-06-net/blob/main/225.png)

**3. Какой IP адрес у вас в интернете?**

root@vagrant:~# dig TXT +short o-o.myaddr.l.google.com @ns1.google.com | awk -F'"' '{ print $2}'
188.16.91.xxx

**4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois**
root@vagrant:~# whois 188.16.91.xxx | grep ^descr
descr:          Dynamic distribution IP's for broadband services
descr:          OJSC RosteleÓom, regional branch "Urals"
descr:          Rostelecom networks

**5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute**
![Image alt](https://github.com/ryzhakovks/03-sysadmin-06-net/blob/main/2266.png)

AS12389
AS15169
AS263411

**6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?**

![Image alt](https://github.com/ryzhakovks/03-sysadmin-06-net/blob/main/2777.png)

**7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig**
```rb
root@MMRU59A0000:~# dig +short NS dns.google
ns4.zdns.google.
ns1.zdns.google.
ns2.zdns.google.
ns3.zdns.google.
```


**8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig**


```rb
root@MMRU59A0000:~# for ip in `dig +short A dns.google`; do dig -x $ip | grep ^[0-9].*in-addr; done
4.4.8.8.in-addr.arpa.   0       IN      PTR     4.4.8.8.in-addr.arpa.
8.8.8.8.in-addr.arpa.   0       IN      PTR     8.8.8.8.in-addr.arpa.
root@MMRU59A0000:~#
```
