# 구성요소

*참고 : 설치에서 트러블슈팅까지 웹로직의 모든 것 WebLogic Expert*

### **Domain**

웹로직의 관리 단위

하나의 어드민 서버가 필수, 추가로 매니지드 서버를 기동할 수 있다 

### **Admin**

도메인을 관리하기 위한 관리 서버, 도메인의 구성 및 설정을 할 수 있음

### **Managed**

웹로직 서버 인스턴스, 어드민 서버에서 설정한 구성과 환경으로 기동함, 실제 서비스는 매니지드 서버에서 담당

### **Cluster**

무중단 페일오버를 위함, 같은 도메인의 두 개 이상의 서버로 구성됨

# 설정파일 - config.xml

웹로직 구성 정보가 저장

(1 admin ↔ 1 config.xml, 도메인 당 1개)

어드민 콘솔에서 설정한 정보가 여기에 저장됨

(도메인 이름, 각 서버 인스턴스, 클러스터, 자원 등등 설정)

어드민 서버에서 config.xml을 참조하여 기동하고, 매니지드 서버는 어드민 서버를 참조하여 기동

(config.xml → admin → managed)

### **위치**

```bash
${DOMAIN_HOME}/config/**config.xml**
```

### **수정 방법**

1) 어드민 콘솔 (웹로직 서버 콘솔)

2) config.xml 파일 직접 수정

### **구성 요소**

- server의 자식 요소
    1. **name**
    2. **listen-port**
    3. **cluster**
    4. self-tuning-thread-pool-size-min
    5. graceful-shutdown-timeout : stop했을 때 실행하던거 마저 실행하라고 기다려주는 시간
    6. staging-mode : nostage/stage
        - **stage mode**
            - 기본 동작
                
                admin에 배포된 **application을 managed 서버 디렉토리로 copy**해서 배포
                
                stage 디렉토리에 애플리케이션을 복사
                
            - 특징 및 장점
                
                **서버가 startup될 때마다 managed 및 stage 디렉토리에 복사**하기 때문에 시간이 오래 걸림
                
                managed 서버가 많을 수록 시간이 오래 걸림
                
                application 사이즈가 작은 경우 적합
                
        - ⭐ **nostage mode**
            - 기본 동작
                
                **실제 애플리케이션의 절대 경로를 참조** 
                
                애플리케이션을 각 BOX 별로 copy  해놓고, 배포할 때 파일 경로만 지정하는 방식
                
            - 특징 및 장점
                
                admin, managed 모두 같은 application에 직접 접근
                
                **애플리케이션 변경 시 admin 서버가 자동으로 감지 & refresh**
                
                application 사이즈가 큰 경우 적합
                
            
    7. production-mode-enabled  : true/false
        - **개발 모드** (false) : class 파일을 바꾸면 자동으로 변경 사항이 갱신
        - ⭐️ **운영 모드** (true) : 서버 재기동해야 변경 사항이 갱신
