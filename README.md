# mywebsite
sineweb-00

배포 URL: https://sineweb-00-future.cloud

## 기술 스택
HTML/CSS:	브라우저에 보이는 화면 구조와 디자인 담당. 로그인/회원가입/프로필 화면을 만듦.

Python:	백엔드 개발에 사용한 프로그래밍 언어

Flask:	브라우저 요청을 받아서 DB 조회 후 HTML을 돌려주는 웹 프레임워크

SQLite:	유저 정보(아이디, 비밀번호, 가입날짜)를 저장하는 데이터베이스. 파일 하나로 동작함.

SQLAlchemy:	Python 코드로 SQLite를 다룰 수 있게 해주는 라이브러리. SQL 직접 안 써도 됨.

Gunicorn:	Flask를 실제 서비스용으로 실행하는 WSGI 서버. 여러 요청을 동시에 처리함.

Nginx:	외부 요청을 받아서 Gunicorn으로 전달하는 리버스 프록시. HTTPS도 처리함.

Ubuntu 22.04:	Oracle Cloud 서버에 설치된 운영체제. 명령어로 조작하는 Linux 계열 OS

Oracle Cloud:	코드가 실제로 돌아가는 서버 컴퓨터. 24시간 켜져있는 클라우드 서비스

Let's Encrypt + Certbot:	HTTPS 인증서를 무료로 발급해주고 자동 갱신해주는 도구

가비아:	도메인을 구입한 도메인 등록 서비스

Git:	코드 변경 이력을 관리하는 도구. 언제든지 이전 상태로 되돌릴 수 있음.

## 전체 구조
[브라우저]

    ↓ HTTPS 요청
    
[Nginx] - 리버스 프록시, HTTPS 처리

    ↓ HTTP 전달
    
[Gunicorn] - WSGI 서버, 동시 요청 처리

    ↓
    
[Flask] - 백엔드 로직 처리

    ↓ 조회/저장

[SQLite] - 유저 데이터 저장

    ↓
    
[Flask] - HTML 생성

    ↑
[Gunicorn]

    ↑
[Nginx]

    ↑ HTTPS 응답
    
[브라우저] - 화면 표시

## 배포 과정


## 막혔던 부분과 해결 방법
대부분 클로드 코드가 추천하는 프로그램을 썼는데 DigitalOcean이 유료길래 무료인 Oracle Cloud Free Tier에서 서버 만듦.

근데 Oracle cloud Free Tier 쓰니까 사용자가 많은지 A1(ARM) 인스턴스는 서울/오사카 둘 다 용량이 꽉 차서 못 만들었고 AMD으로 만듦.
