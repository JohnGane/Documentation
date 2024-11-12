Чтобы настроить клиентскую часть OpenVPN, необходимо создать файл .ovpn — он понадобится для подключения к VPN. Добавьте в него следующее содержимое:


**client**  # Роль 

**remote 123.123.123.123**  # IP сервера OpenVPN
  
**port 1194** # Порт сервера OpenVPN, совпадает с конфигурацией сервера 
  
**dev tun** # Интерфейс  

**;proto tcp** # Протокол OpenVPN, как на сервере   
**proto udp**    
  
**;remote my–server–1 1194**   # Имя хоста, IP-адрес и порт сервера  
**;remote my–server–2 1194**  

**;remote–random**  # Случайный выбор хостов. Если не указано, выбор производится по порядку 
  
**resolv-retry infinite**  # Преобразование имени хоста 

**nobind** # Привязка к локальному порту  
  
**redirect-gateway def1 bypass-dhcp** # Шлюз по умолчанию  
  
**persist-key**    # При падении туннеля не выключать интерфейсы, не перечитывать ключи
**persist-tun**  

**;http–proxy–retry** # retry on connection failures  # Настройка HTTP прокси при подключении OpenVPN серверу  
**;http–proxy [proxy server] [proxy port #]**  
  
**;mute–replay–warnings**  # Отключение предупреждений о дублировании пакетов
  
**remote-cert-tls server**  # Дополнительная защита
  
**key-direction 1**  # Ключ HMAC
 
**cipher AES-256-CBC**  # Шифрование 

**;comp–lzo** # Сжатие. Если на сервере отключено, не включается    
 
**verb 3** # Вербальность журнала    


<ca>  
Содержимое файла /etc/openvpn/certs/ca.crt  
</ca>  
  
  
<cert>  
Содержимое файла /etc/openvpn/easy-rsa/pki/issued/client-1.crt  
</cert>  
  
  
<key>  
Содержимое файла /etc/openvpn/easy-rsa/pki/private/client-1.key  
</key>  
  
  
<tls-auth>  
Содержимое файла /etc/openvpn/certs/ta.key  
</tls-auth>

Пример конфигурации пользователя

```
# Роль
client

# IP сервера OpenVPN
remote 80.90.183.134

# Порт сервера OpenVPN, совпадает с конфигурацией сервера
port 1194

# Интерфейс
dev tun

# Протокол OpenVPN, как на сервере
;proto tcp
proto udp

# Имя хоста, IP-адрес и порт сервера

;remote my–server–1 1194
;remote my–server–2 1194

# Случайный выбор хостов. Если не указано, выбор производится по порядку
;remote–random

# Преобразование имени хоста
# (в случае непостоянного подключения к интернету)
resolv-retry infinite

# Привязка к локальному порту
nobind

# Шлюз по умолчанию
redirect-gateway def1 bypass-dhcp

# При падении туннеля не выключать интерфейсы, не перечитывать ключи
persist-key
persist-tun

# Настройка HTTP прокси при подключении OpenVPN серверу
;http–proxy–retry # retry on connection failures
;http–proxy [proxy server] [proxy port #]

# Отключение предупреждений о дублировании пакетов
;mute–replay–warnings

# Дополнительная защита
remote-cert-tls server

# Ключ HMAC
key-direction 1

# Шифрование
cipher AES-256-CBC

# Сжатие. Если на сервере отключено, не включается
#comp–lzo

# Вербальность журнала
verb 3

# Сертификаты

<ca>
-----BEGIN CERTIFICATE-----
MIIDUTCCAjmgAwIBAgIUJCspslKZJTpIvreDpvaAhuFkMX8wDQYJKoZIhvcNAQEL
BQAwGDEWMBQGA1UEAwwNVWJ1bnR1X1NlcnZlcjAeFw0yNDEwMTAxNjUzNTZaFw0z
NDEwMDgxNjUzNTZaMBgxFjAUBgNVBAMMDVVidW50dV9TZXJ2ZXIwggEiMA0GCSqG
SIb3DQEBAQUAA4IBDwAwggEKAoIBAQC29euTN6dnXvs86ZOjnUnsjmhhuSV275R5
PLGn9kKSO5UsvneRrV9TFMkzstbJ2AsQaFTnYXh+xpePDduaTNXEhIOpB5AAH7Jx
EsyGUI6KKwZB0kb7hAZhFMJ6K4sD0tkIWoieH+GCPL98zVmZQEz2Dy0HnetnBNBF
nYrzzcC88G7a6hPEWyptJdOj244DWzTQKsKtTzQfjQ5Bli8UDirOJNYnNomvQE/9
Q0udYIH/2tR1xs2HcCdd1r/yaeVmR3AEYgAnt7RDjBE4T21Gw8EgnQCOmS2ziaz6
uCMAteNcOzm7greWX+qEyi01tjJ5CfcIMeZcJIt9TZ93QzdxiBsRAgMBAAGjgZIw
gY8wDAYDVR0TBAUwAwEB/zAdBgNVHQ4EFgQU32HnXMqMQ2NcRdJTB0eWhNi68ycw
UwYDVR0jBEwwSoAU32HnXMqMQ2NcRdJTB0eWhNi68yehHKQaMBgxFjAUBgNVBAMM
DVVidW50dV9TZXJ2ZXKCFCQrKbJSmSU6SL63g6b2gIbhZDF/MAsGA1UdDwQEAwIB
BjANBgkqhkiG9w0BAQsFAAOCAQEAj9hgl9FoFlRa+Gdy0aq4dWs4AIhHAePZzXNq
sBtV1aNT14HB9S+jt54+bMv4Z6N/1y0n+QnQ5fvwxPs88Ofweuyu+5QGHIIuS1mb
AEysXUEku/Q1Dp1ODUTgCwK0vSDeTzPSbgHgPHf0uTutSjfIZ4UDodK+XcanpDoV
GX6JtqwKuyvoceDLgHSpoVo0bM0D9tmaLPcyyHlMOn5An7vKVulVgPSdWIaR2Jeb
TLwbe+gNzV+HzQbtG9zdQEa7D+JWR31pjsk2zMeA0elQwiWRuSjbcyUOCbW7V198
jNlEnM0PhjWQrYxbjbb2S/hIWkOQ4ViWEMjeFvIDkI54QHkS5g==
-----END CERTIFICATE-----
</ca>


<cert>
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            45:e1:96:ba:63:5a:3b:76:aa:ca:1d:67:8e:c3:4b:ce
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN=Ubuntu_Server
        Validity
            Not Before: Oct 10 16:59:49 2024 GMT
            Not After : Jan 13 16:59:49 2027 GMT
        Subject: CN=client-1
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:d1:68:c1:e0:ea:1b:d7:51:dc:54:a4:92:a4:9f:
                    a5:cf:4c:8c:a6:62:4d:6f:61:4b:26:da:cf:d5:9c:
                    d1:2d:f0:fc:88:23:c7:f0:da:02:b1:10:2a:9d:62:
                    0f:a9:c2:78:38:8e:4a:12:0c:e3:9f:27:67:6e:72:
                    16:e8:cc:ca:2a:28:de:c6:6a:05:ce:a0:a2:d0:3d:
                    6a:a5:7d:09:3d:e1:f7:10:e3:10:45:b8:20:20:01:
                    1e:5b:0d:cd:7c:b6:c6:f8:43:a2:35:28:fb:87:81:
                    a9:5c:89:c6:ca:f2:04:6f:6b:ac:37:71:33:d6:3d:
                    2b:96:ff:c3:63:39:37:a0:94:94:fb:54:bb:e7:b9:
                    ec:0c:ec:2b:4f:45:8d:24:c5:15:1c:0a:e4:78:3b:
                    64:58:7f:39:5b:6e:e5:07:91:ce:57:ac:ac:e6:cf:
                    55:4d:e2:d2:92:e7:c6:fd:c2:0e:59:73:71:e8:47:
                    24:91:bc:4f:05:0e:2d:00:57:a0:3a:d1:77:74:4b:
                    53:6e:d0:fb:aa:b5:db:37:ba:78:0c:ec:9a:fc:bd:
                    91:06:1f:68:14:a2:72:82:df:b1:0e:a9:44:07:da:
                    9b:f7:9c:f9:68:0a:7b:97:ef:70:22:f1:f5:28:84:
                    7f:4f:c9:2e:a3:5e:43:f2:fb:7d:a8:74:f2:1f:59:
                    08:bb
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 Subject Key Identifier:
                07:C9:FC:AF:52:4D:C8:5B:F0:53:B4:DF:A1:DB:C8:28:80:E4:78:69
            X509v3 Authority Key Identifier:
                keyid:DF:61:E7:5C:CA:8C:43:63:5C:45:D2:53:07:47:96:84:D8:BA:F3:27
                DirName:/CN=Ubuntu_Server
                serial:24:2B:29:B2:52:99:25:3A:48:BE:B7:83:A6:F6:80:86:E1:64:31:7F
            X509v3 Extended Key Usage:
                TLS Web Client Authentication
            X509v3 Key Usage:
                Digital Signature
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        0b:1b:b0:12:18:03:3d:be:09:9d:e9:0b:53:0c:da:8b:1c:35:
        b6:e1:a5:51:e8:38:da:b7:be:12:2d:ce:61:2c:7f:7b:8f:8c:
        69:fd:ef:49:49:88:b5:30:85:5a:ee:16:2e:b8:55:ca:be:14:
        18:b5:63:aa:77:43:db:1d:f8:73:91:d9:9c:a7:0c:37:5c:2f:
        71:ec:25:c4:da:4f:59:b2:88:93:d3:8d:51:03:03:94:05:60:
        56:d6:21:cf:f6:3e:12:2e:61:44:4b:e7:59:46:79:6c:0c:5d:
        6a:e9:71:2f:b1:4b:6b:6a:dc:8a:6a:46:af:7c:c8:fe:9a:6d:
        00:36:69:19:db:95:07:3f:ee:81:72:72:1d:48:86:45:31:0d:
        fc:17:78:21:af:c6:06:2d:6e:c5:dc:a7:8d:81:03:69:61:bd:
        f3:1e:a9:75:fa:b8:92:da:0b:dc:1a:e4:e5:7d:3a:9e:f8:3c:
        df:cb:99:d1:fb:14:4c:1d:5e:7b:92:71:b6:95:b8:45:2f:fe:
        10:d4:80:aa:9e:ef:c5:d4:ae:19:59:10:a0:62:48:a1:23:cd:
        85:be:26:e3:d4:42:c4:98:8a:d7:87:ba:b0:f4:96:e8:f5:ce:
        fa:db:de:9a:6e:86:3a:bb:df:f9:51:f8:5a:68:d7:74:d8:07:
        b1:3c:3d:fc
-----BEGIN CERTIFICATE-----
MIIDWjCCAkKgAwIBAgIQReGWumNaO3aqyh1njsNLzjANBgkqhkiG9w0BAQsFADAY
MRYwFAYDVQQDDA1VYnVudHVfU2VydmVyMB4XDTI0MTAxMDE2NTk0OVoXDTI3MDEx
MzE2NTk0OVowEzERMA8GA1UEAwwIY2xpZW50LTEwggEiMA0GCSqGSIb3DQEBAQUA
A4IBDwAwggEKAoIBAQDRaMHg6hvXUdxUpJKkn6XPTIymYk1vYUsm2s/VnNEt8PyI
I8fw2gKxECqdYg+pwng4jkoSDOOfJ2duchbozMoqKN7GagXOoKLQPWqlfQk94fcQ
4xBFuCAgAR5bDc18tsb4Q6I1KPuHgalcicbK8gRva6w3cTPWPSuW/8NjOTeglJT7
VLvnuewM7CtPRY0kxRUcCuR4O2RYfzlbbuUHkc5XrKzmz1VN4tKS58b9wg5Zc3Ho
RySRvE8FDi0AV6A60Xd0S1Nu0Puqtds3ungM7Jr8vZEGH2gUonKC37EOqUQH2pv3
nPloCnuX73Ai8fUohH9PyS6jXkPy+32odPIfWQi7AgMBAAGjgaQwgaEwCQYDVR0T
BAIwADAdBgNVHQ4EFgQUB8n8r1JNyFvwU7TfodvIKIDkeGkwUwYDVR0jBEwwSoAU
32HnXMqMQ2NcRdJTB0eWhNi68yehHKQaMBgxFjAUBgNVBAMMDVVidW50dV9TZXJ2
ZXKCFCQrKbJSmSU6SL63g6b2gIbhZDF/MBMGA1UdJQQMMAoGCCsGAQUFBwMCMAsG
A1UdDwQEAwIHgDANBgkqhkiG9w0BAQsFAAOCAQEACxuwEhgDPb4JnekLUwzaixw1
tuGlUeg42re+Ei3OYSx/e4+Maf3vSUmItTCFWu4WLrhVyr4UGLVjqndD2x34c5HZ
nKcMN1wvcewlxNpPWbKIk9ONUQMDlAVgVtYhz/Y+Ei5hREvnWUZ5bAxdaulxL7FL
a2rcimpGr3zI/pptADZpGduVBz/ugXJyHUiGRTEN/Bd4Ia/GBi1uxdynjYEDaWG9
8x6pdfq4ktoL3Brk5X06nvg838uZ0fsUTB1ee5JxtpW4RS/+ENSAqp7vxdSuGVkQ
oGJIoSPNhb4m49RCxJiK14e6sPSW6PXO+tvemm6GOrvf+VH4WmjXdNgHsTw9/A==
-----END CERTIFICATE-----
</cert>


<key>
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDRaMHg6hvXUdxU
pJKkn6XPTIymYk1vYUsm2s/VnNEt8PyII8fw2gKxECqdYg+pwng4jkoSDOOfJ2du
chbozMoqKN7GagXOoKLQPWqlfQk94fcQ4xBFuCAgAR5bDc18tsb4Q6I1KPuHgalc
icbK8gRva6w3cTPWPSuW/8NjOTeglJT7VLvnuewM7CtPRY0kxRUcCuR4O2RYfzlb
buUHkc5XrKzmz1VN4tKS58b9wg5Zc3HoRySRvE8FDi0AV6A60Xd0S1Nu0Puqtds3
ungM7Jr8vZEGH2gUonKC37EOqUQH2pv3nPloCnuX73Ai8fUohH9PyS6jXkPy+32o
dPIfWQi7AgMBAAECggEAAIzbtyfGUcC3guPTAtDskmirSFcJYkAi/p1XXtweBjBH
T/0QFG27BHrimC4R+0QCJUJMDYbjiwW2e6k1e2a2WhM1BD5yrEIdy2at6UHOP8+T
lgOSuMXGewilt/i6tn4tQNyLCF3tABEmJpKyGmxoxV/G2kQKewcHUbFAWS1lHDkZ
tGvY79XfIkIywM0YGr6nNI0dYENV5S8EyZpbStKD/pJTaxpa+3SFEgyZn8OwvEp/
P8d/7kxnXViCjX8UPlqkSfmTSGo6e6Z5wg8mabq0dRinPad2FsgtKZwA0434CMZX
ts4Rc6EYKExnf4W4gaMQyxYurTkRXwl49dLD4DZ8GQKBgQDotwwVdwSGRsWfg6Jf
UfWUf+2fiqAkfqqK8dNGXgUlyFAD7Ft+qs4TqenwfP2A5hJdegLqOimc7MDi0EiH
9qHGadkdtlEcCbIXNyLYdmNEKXnwrkXDuRQclxRa5KkEeaPbs91zgBdhBnmbqPLE
S5+PmJ/DdyOyx2O+E7fOMVme9QKBgQDmXLxfjYlwSMdiK5qyX86vSK8RhfhgFSCK
/4tsJdI1XTtfyus85wSA0IFyDwSggeZfPnj9GFbWvc2W45klAJDapF3n1cUBaElU
AZ7zBPeq96vI6uBEjbUSJrdcxJ5xlysF6t+M5aa1yNIBx5JTanbhN6TdVtRLcIIM
Jj5UuW3a7wKBgCDrnBsBPjOcmWJKZdLkLkB2pG/YVXU0Mf373a5rqIDCyIb1ja/q
i8J+W+i4VchBQ8HTe8wUtERNva+YVVpeil4eJSet3eWAfaAJHbXPcZV35JcmoBni
+bRdrvR4umw2pPZ0iFRJf4UrPFLH4KfiJs1Sgu9M0FD/Id4Gvg6+LnZtAoGALUmF
7vMQVfa429/epbqYE3WilTtVPO5qW2kpq7UzwjH1/jsSTALOq9RR3m59ZmCjPY42
kus6BzWBOWy9Kr0VvSYbH/yyojgyUkWPTg9n8UCHkRQ7yr5hHpRl7+Lnk0U4vA0U
rcpoH8y/HIJzjdqcTGJ4EtuDGOGhb2oFTvq1UhkCgYEAsimQfUzM/HeQ/E+VMj66
dTKN9xjKSMdX/67Ht4HjpCQRC8J4LYThZ8+0xTufwipFVZni2MocpHfu3C9HXNDb
RtoCCao/Jge5jXbd4hFzvWyqXQTXlsKziiw+PZRWkSF6f2MR8tGgMlhhszkBJtjT
oDODdIyyUPbpLqOXlexQEbc=
-----END PRIVATE KEY-----
</key>


<tls-auth>
#
# 2048 bit OpenVPN static key
#
-----BEGIN OpenVPN Static key V1-----
03e4baef82a4c913a31ee00f5e30bd8e
929b8589aecf3c552430761df6f4ad00
2eed9c915395f1a48f940252d37eac73
83596029a38e8705b52961ea5bb4be1f
a70b9ec0e94350b0c806465c06ad1d3a
210559679ae2c488386328bf4cb22394
8b5a50f46b1f12ad812fddcf98967811
5a2abe30d62629a116451b28be45b666
434c7325c80e9435e6cdf4b677a87a71
3d100254620c4636e56f14353d73afb4
076dc0615be7614b5283dc9e1728e158
8d03acc817bd7796d7a62d7225cdd6dd
1e222f681380631384cc2a4590e59729
218a2eba420537fd03efd151cf30a114
fc6c41fc16fb66431193cbdfed84ff36
6e5a41bc30f39bf869f19068c3f29396
-----END OpenVPN Static key V1-----
</tls-auth>
```

- **<ca></ca>** — данными из файла /etc/openvpn/certs/ca.crt,
- **<cert></cert>** — данными из файла /etc/openvpn/easy-rsa/pki/issued/client-1.crt,
- **<key></key>** — данными из файла /etc/openvpn/easy-rsa/pki/private/client-1.key,
- **<tls-auth></tls-auth>** — данными из файла /etc/openvpn/certs/ta.key.

После этого сохраните изменения с помощью комбинации клавиш **Ctrl + O** и закройте файл сочетанием **Ctrl + X.**
