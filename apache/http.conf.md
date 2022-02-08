# httpd.conf - 설정파일

- **ServerRoot**
    
    apache 설치 경로 (apache Home)
    

```jsx
# ServerRoot "/app/apache"
```

- **Listen**
    
    수신할 포트 번호 설정
    

```jsx
Listen 80 : http
Listen 443 : https
Listen 8080  
```

- **LoadModule**
    
    apache와 호환되는 모듈 지시 (apache 설치 후 추가 모듈)
    

```jsx
LoadModule modname "*"(파일경로) 
LoadModule ssl_module modules/mod_ssl.so 
```

- **ServerName**

```jsx
ServerName ip/ip:port/hostname

# hostname은 바뀔수도 있기 때문에 ip:port를 선호함
```

- **DocumentRoot**
    
    웹페이지 root 지정
    

```jsx
DocumentRoot "~/webcontent"
--> http://~~/${DocumentRoot} 

ex) DocumentRoot "~/webcontent/test/a.html" == http://~/webcontent/test/a.html
```

- User, Group
    
    Apache 자식 프로세스들의 ”실행소유자”와 “소유그룹”을 각각 어떤 계정으로 부여할 것인지 결정
    

```jsx
User nobody
Group nobody
```

- **Directory**
    
    각 디렉토리에 고유한 설정을 적용하는 블록
    
    root 디렉토리 권한
    

```jsx
  # 서버 자체 실제 디렉토리의 절대경로

<Directory "${SRVROOT}/htdocs">
    Options FollowSymLinks 
    AllowOverride None
    Require all granted -- 접근 허용
		Order allow, deny
		Allow from all
</Directory>
```

- **Options** : 특정 디렉토리에서 사용할 수 있는 서버 기능 제어
    - **FollowSymLinks**
        
        심볼릭링크 허용, 웹브라우저에서 링크 파일의 경로까지도 확인 가능
        
        *심볼릭링크 : 바로가기 아이콘과 비슷한 것 
        
        ```jsx
        $ ln -s [대상 경로] [링크 경로] 
        ```
        
    - **Indexes**
        
        클라이언트가 요청한 디렉토리 경로에 DirectoryIndex에 지시한 파일(index.html등)이 없을 경우, 디렉토리 파일목록을 화면에 표시한다.
        
        - Indexes가 설정된 상태에서 DocumentRoot 디렉토리에 지정된 파일 (예 :  index.html) 이 없다면 디렉토리 리스트가 노출됨
        - **DirectoryIndex**
            
            : 클라이언트가 파일이 아닌 디렉토리 경로를 요청했을 경우 제공할 파일 목록을 설정
            
            ```jsx
            # DirectoryIndex index.html 
            ```
            
    - **Includes**
        
        SSI 허용
        
        - SSI : 웹 페이지 내에서 CGI를 사용하지 않고서도 방문자 카운터, 로고 수정 등을 간단히 사용할 수 있게 해주는 기능
    - **IncludesNOEXEC**
        
        SSI는 허용되지만 #exec 사용과 #include는 허용되지 않는다. 즉 SSI를 사용하면서 시스템에 위험한 SSI의 실행 태그는 허용하지 않겠다는 설정
        
    - **ExecCGI**
        
        mod_cgi를 사용하는 CGI 스크립트 실행을 허용
        
        - CGI : WAS가 없었을 때 웹서버에서 동적 컨텐츠를 처리하기 위해서 사용했었음
    - **MultiViews**
        
        웹브라우저의 요청에 따라 적절한 페이지로 보여준다. 웹브라우저의 종류나 웹문서의 종류에 따라서 가장 적합한 페이지를 보여줄 수 있도록 하는 설정
        
    - **All** : 모든 Options가 MultiViews에 허용된다.
    - **None** : 어떠한 옵션도 적용하지 않는다.
- **AllowOverride** : .htaccess 파일 사용 여부 결정 ( All / None )
    - 디렉토리의 설정 내용을 외부 파일(.htaccess)에서 재설정할 수 있는지 여부 결정
    - httpd.conf 설정 대신 외부 파일의 내용이 적용됨
- 접근 제한
    - **Require** : 해당 디렉토리의 접근 허용 여부
        - all denied : 모든 접근 거부
        - all granted : 모든 접근 허용
        - ip [xxx.xxx.xxx.xxx](http://xxx.xxx.xxx.xxx) : 특정 ip 접근 허용
        - not ip xxx.xxx.xxx.xxx
        - host [example.com](http://example.com) : 특정 호스트 접근 허용
        - not host example.com
    - **Allow** : 접근 허용 ex) Allow from all
    - **Deny** : 접근 거부 ex) Deny from all
        
        ![Untitled](httpd%20conf%20-%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20d13d5920e022434da76f9a9754e4f6ba/Untitled.png)
        
- **Order** : 접근을 통제하는 순서
    - deny, allow – deny 지시자 부터 검사하고 allow 지시자를 검사
    - allow, deny – allow 지시자 부터 검사하고 deny 지시자를 검사
    - mutual-failure – allow 목록에 없는 모든 host에게 접속을 거부

- **Location (URL)**

```jsx
# "http://ip/server-status" 로 접근하면 apache의 작동 상태를 모니터링 할 수 있음

<Location /server-status>
    SetHandler server-status 
    Order deny, allow
		Allow from all
</Location>
```

- **ErrorLog** : 에러 로그 생성 경로 지정
- **LogFormat** : 커스텀 로그에 사용되는 로그 형식 설정
- **CustomLog** : Access 로그 파일이 저장되는 위치와 포맷 설정
- **DefaultType**
- **AddType** : apache에서 사용할 media-type과 확장자를 매핑하여 추가
- **Include :** 필요한 설정 파일만 로드할 수 있음
    
    ```jsx
    # Include conf/httpd-ssl.conf -- SSL을 지원하기 위한 설정파일 포함
    ```
    

<참고 : [https://itwarehouses.tistory.com/6](https://itwarehouses.tistory.com/6), [https://opentutorials.org/course/3647/23838](https://opentutorials.org/course/3647/23838)>

- **섹션 컨테이너** : 접근을 허용하는 적용 범위를 제한 (구성 적용 범위를 제한)
    1. Directory : 디렉토리 단위
    2. Files : 파일 단위 (주로 접근 권한 설정)
    3. Location : URL 경로 단위
        
        
    - 우선순위 : Directory  > Files  >  Location  순서로 설정 적용됨