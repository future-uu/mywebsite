# mywebsite
sineweb-00

배포 URL: https://sineweb-00-future.cloud

## 기술 스택
| 이름 | 역할 |
|------|------|
| HTML/CSS | 브라우저에 보이는 화면 구조와 디자인 담당. 로그인/회원가입/프로필 화면을 만듦 |
| Python | 백엔드 개발에 사용한 프로그래밍 언어 |
| Flask | 브라우저 요청을 받아서 DB 조회 후 HTML을 돌려주는 웹 프레임워크 |
| SQLite | 유저 정보(아이디, 비밀번호, 가입날짜)를 저장하는 데이터베이스. 파일 하나로 동작함 |
| SQLAlchemy | Python 코드로 SQLite를 다룰 수 있게 해주는 라이브러리. SQL 직접 안 써도 됨 |
| Gunicorn | Flask를 실제 서비스용으로 실행하는 WSGI 서버. 여러 요청을 동시에 처리함 |
| Nginx | 외부 요청을 받아서 Gunicorn으로 전달하는 리버스 프록시. HTTPS도 처리함 |
| Ubuntu 22.04 | Oracle Cloud 서버에 설치된 운영체제. 명령어로 조작하는 Linux 계열 OS |
| Oracle Cloud | 코드가 실제로 돌아가는 서버 컴퓨터. 24시간 켜져있는 클라우드 서비스 |
| Let's Encrypt + Certbot | HTTPS 인증서를 무료로 발급해주고 자동 갱신해주는 도구 |
| 가비아 | 도메인을 구입한 도메인 등록 서비스 |


## 전체 구조
```
브라우저
   ↕ HTTPS
  Nginx (리버스 프록시)
   ↕ HTTP
  Gunicorn (WSGI 서버)
   ↕
  Flask (백엔드)
   ↕
  SQLite (DB)
```


## 배포 과정
1. 서버 준비

    Oracle Cloud Free Tier 가입

    인스턴스 생성 (Ubuntu 22.04, AMD)

    VCN(가상 네트워크) 생성 후 인스턴스에 연결

    Oracle 콘솔 Security List에서 80, 443 포트 오픈

    서버 내부 iptables로 80, 443 포트 추가 오픈

2. 도메인 연결

    가비아에서 sineweb-00-future.cloud 도메인 구입

    DNS A 레코드에 서버 IP(168.138.41.99) 등록

3. 서버 환경 세팅
   
    SSH로 서버 접속 후:

    패키지 업데이트 (apt update && apt upgrade)

    Python, pip, venv, Nginx 설치

    프로젝트 파일 업로드 (scp 명령어)

    가상환경(venv) 생성 후 Flask, Gunicorn 등 패키지 설치

4. Gunicorn 서비스 등록

    /etc/systemd/system/mywebsite.service 파일 생성

    서버 재시작해도 자동으로 Flask 앱이 켜지도록 등록

    systemctl enable mywebsite 로 자동 시작 설정

5. Nginx 리버스 프록시 설정

    /etc/nginx/sites-available/mywebsite 설정 파일 작성

    80번 포트로 들어오는 요청을 Gunicorn(5000번 포트)으로 전달하도록 설정

    sites-enabled에 링크 연결 후 Nginx 재시작

6. HTTPS 적용

    Certbot 설치

    Let's Encrypt에서 sineweb-00-future.cloud 인증서 발급

    Nginx 설정 자동 업데이트


## 막혔던 부분과 해결 방법
대부분 클로드 코드가 추천하는 프로그램을 썼는데 DigitalOcean이 유료길래 무료인 Oracle Cloud Free Tier에서 서버 만듦.

근데 Oracle cloud Free Tier 쓰니까 사용자가 많은지 A1(ARM) 인스턴스는 서울/오사카 둘 다 용량이 꽉 차서 못 만들었고 AMD으로 만듦.
