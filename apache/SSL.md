# SSL

*참고 : [https://blog.jiniworld.me/96#a02-1](https://blog.jiniworld.me/96#a02-1)*

## 1. openssl

네트워크 상의 보안 통신을 위한 암호화 라이브러리, 암호화 보안 프로토콜인 TLS/SSL 프로토콜 구현을 제공

**mod_ssl**은 openssl을 이용하여 SSL/TLS 프로토콜 암호화를 해주는 Apache 모듈

```jsx
$ openssl version             # 버전 확인
$ sudo yum install openssl    # 라이브러리 다운
$ httpd -l                    # apache 웹서버에서 컴파일한 정적모듈 조회
```

## 2. 인증서 설정

Apache 웹서버의 가상 호스트에 SSL/TLS 인증서를 설정하기 위해서는 개인키와 2가지(또는 3가지) 인증서에 대한 설정이 필요

```jsx
<VirtualHost *:443>
  ...
  SSLCertificateKeyFile --개인키
  SSLCertificateFile --서버인증서
  SSLCACertificateFile --rootCA인증서
  ...
</VirtualHost>
```

## 3. 설정파일 (httpd-ssl.conf)

- **Listen 443**
- **LoadModule** module이름 modules/**mod_ssl.so < 필수***
- **SSLEngine (on/off)**
- **SSLCertificateFile**
    
    서버 인증서의 경로, 일반적으로 `.crt`의 확장자를 가진다.
    

```jsx
SSLCertificateFile /app/apache/conf/ssl/*.crt
```

- **SSLCertificateKeyFile**
    
    Private Key 파일의 경로, 일반적으로 `.key` 확장자를 가진다.
    

```jsx
SSLCertificateKeyFile /app/apache/conf/ssl/*.key, pem
```

- **SSLCertificateChainFile**
    
    체인 인증서, 브라우저나 앱에 CA List가 없을 경우 해당 CA의 유효성을 판단하기 위해 등록하기 여러 개의 `.crt`가 합쳐진 (Bundling 된) 파일 
    

```jsx
SSLCertificateChainFile /app/apache/conf/ssl/*.com_cert.pem
```

- **SSLProtocol**
    
    사용 가능한 SSL/TLS 프로토콜 버전을 설정 (SSLv2/3, TLSv1.1/2/3, all)
    

```jsx
# SSLv2, SSLv3를 제외한 모든 프로토콜 지원
SSLProtocol all -SSLv2 -SSLv3

# TLS 1.2 및 TLS 1.3을 제외한 모든 프로토콜은 제외
SSLProtocol -all +TLSv1.3 +TLSv1.2
```

대부분의 최신 브라우저는 이전 프로토콜을 사용하는 웹서버를 만날 때 사용자 환경이 저하될 수 있다.(예 : URL 표시 줄의 자물쇠 또는 보안 경고). 이러한 이유로 서버 구성에서 SSL 2.0 및 3.0 (SSLv2, SSLv3) 을 비활성화 해야 하며 TLS 프로토콜만 사용하도록 설정해야 함. 

출처 : [https://smartits.tistory.com/209](https://smartits.tistory.com/209)

- **SSLCipherSuite**  : 암호화 알고리즘

```jsx
SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
```

- **암호화 알고리즘 확인**

```jsx
$ openssl ciphers -v 
```

가상 호스트 설정을 변경했으면, 웹서버 재시작해야 함

만일, 가상 호스트 설정을 완료하고 웹서버 재시작도 원활히 되었음에도 사이트가 열리지 않고 아래와 같은 창이 뜬다면, 서버의 방화벽에 **https** 서비스가 개방되어있는지 확인해봐야 함