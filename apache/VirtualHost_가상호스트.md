# VirtualHost

*참고 : [https://blog.jiniworld.me/27#a01](https://blog.jiniworld.me/27#a01), [https://araikuma.tistory.com/806?category=879108](https://araikuma.tistory.com/806?category=879108)*

한 서버에서 여러 웹사이트 서비스 가능, 이름 기반 Host를 주로 사용

예)

http://jiniworld.me:**9090** → http://ci.jiniworld.me

http://jiniworld.me:**8080** → http://api.jiniworld.me

기본적으로 루트 접근할 경우에는 홈 디렉토리 내의 **index.html** 가 열림

### 기본 속성

- ServerAdmin : 관리자 이메일 주소
- DocumentRoot : 홈페이지 디렉토리 위치
- ServerName : 도메인명 (DNS 연결 되어 있어야 함)
- ServerAlias : 도메인 별명
- ErrorLog :  에러로그 파일 위치
- CustomLog : 접속로그 파일 위치

```jsx
<VirtualHost *:80>
	 ServerName test.jiniworld.me
	 DocumentRoot /var/www/test
	 ErrorLog "/var/log/httpd/api/test-error_log"
	 CustomLog "/var/log/httpd/api/test-access_log" common
</VirtualHost>

# http://test.jiniworld.me로 접속할 경우, /var/www/test/index.html 페이지가 열림
```

**특정 ip에서만 접근할 수 있도록** 설정하고 싶다면, <VirtualHost> 태그 내에 아래 코드를 넣어주면 됨

```jsx
<Location />
   Require ip ip1 ip2 ip3 ..
</Location>
```