친하 공식 사이트 v1.3 - 별도 총괄 로그인 + 공개 배포 준비

- website/index.html: 일반 방문자용 사이트
- website/owner-login.html: 별도 총괄 로그인 화면 (/owner-login)
- server/server.js: 로그인, 세션, 권한, 채팅, 역할 관리 및 DB 서버
- server/data/db.json: 실제 실행 시 생성되는 개인 데이터 파일 (배포 ZIP에는 포함하지 않음)
- render.yaml: Render 공개 호스팅용 Blueprint

로컬 실행:
  server 폴더에서 `node server.js`
  브라우저는 터미널에 표시된 URL로 접속
  총괄 로그인은 URL 끝에 /owner-login을 붙여 접속

공개 배포:
  DEPLOY-PUBLIC-HOSTING-KO.md 읽기
  GitHub 저장소와 Render 계정을 소유자가 직접 연결해야 하며, 공개 호스팅은 이 ZIP만으로 자동 생성되지 않습니다.
  Render 설정은 파일 DB/업로드를 /var/data 영속 디스크에 저장합니다. 이 구성은 유료 플랜/디스크 비용이 발생할 수 있습니다.

개인정보 보호:
  실제 DB, 백업, .env, 관리자 초기 비밀번호 파일을 공개 저장소에 업로드하지 마세요.
