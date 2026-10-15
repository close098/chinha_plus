# 친하 공식 사이트 보안·DB 업데이트 1.1.0

## 이번 업데이트

- 회원 로그인: IP별 로그인 실패 8회가 10분 창 안에 누적되면 10분 차단
- 총괄 관리자 로그인: IP별 실패 5회가 15분 창 안에 누적되면 15분 차단
- 회원가입 요청은 IP별 1시간에 8회, 계정 찾기 요청은 15분에 12회 제한
- 이메일 비밀번호 재설정 코드를 대입하는 시도는 IP별 실패 8회 이후 15분 차단
- HTTPS 운영 환경(`NODE_ENV=production`)에서는 회원 및 총괄 인증 쿠키에 `Secure`, `HttpOnly`, `SameSite=Lax` 적용
- `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`, `Permissions-Policy` 응답 헤더 추가
- DB는 서버 시작 시 변경 전 스냅샷을 저장하며, 이후 10회 변경 또는 10분마다 이전 디스크 DB를 백업하고 최대 20개 유지
- DB JSON 또는 핵심 컬렉션 구조가 손상되면 새 빈 DB로 대체하지 않고, 손상 복사본을 보관한 뒤 시작을 중단
- DB 저장 시 임시 파일과 `fsync` 사용, Windows에서 파일 교체 실패 시 이전 파일을 되돌리는 처리 추가
- 공개 Git 저장소에 DB, 백업, 초기 관리자 비밀번호, 로그, 손상 DB, 업로드 파일이 들어가지 않도록 `.gitignore` 보강
- 관리자가 아닌 역할 변경 API 호출은 거부되며, 총괄이 지정한 역할은 서버 DB에 기록
- 공식 YouTube/Discord URL은 고정값으로 유지

## 실행

1. 압축 파일을 풀고 `server` 폴더로 이동합니다.
2. PowerShell에서 `node server.js`를 실행합니다.
3. 콘솔에 출력되는 주소를 브라우저에서 엽니다.
4. 기존 DB가 있다면 교체 전에 반드시 복사본을 따로 보관합니다.

## HTTPS 배포

- HTTPS를 실제로 적용한 호스팅 환경에서 `NODE_ENV=production`을 설정합니다. 평문 HTTP 로컬/TV 연결에서 이 값을 사용하면 브라우저가 `Secure` 쿠키를 보내지 않을 수 있습니다.
- `ENABLE_HSTS=true`는 HTTPS가 정상 작동하는 경우에만 설정하세요.
- `TRUST_PROXY=true`는 신뢰할 수 있는 리버스 프록시를 사용하는 경우에만 설정하세요. 기본값은 프록시 헤더를 신뢰하지 않습니다.

## DB 복구

DB가 손상되면 서버가 새 DB로 자동 덮어쓰지 않고 종료됩니다. 먼저 `server/data/db.json`, `server/data/db.corrupt-*.json`, `server/data/backups/` 전체를 별도 위치에 복사하세요. 유효한 스냅샷을 확인한 다음 복구본을 `db.json`으로 되돌리고 다시 실행합니다.

## 유의 사항

- 로그인 제한은 현재 단일 Node.js 프로세스의 메모리 안에서 관리되어 서버 재시작 시 초기화됩니다. 대규모 공개 서비스로 발전시키려면 프록시/호스팅 수준의 공유 rate limit와 모니터링을 추가해야 합니다.
- 이 ZIP에 포함된 DB에는 회원 이메일과 비밀번호/복구 코드 해시가 들어 있을 수 있습니다. 공개 저장소나 타인에게 보내는 파일로 공유하지 마세요.
- 로컬 Node.js 서버는 인터넷에 자동 공개되지 않습니다. 다른 장소에서 접속하려면 HTTPS 호스팅 배포가 필요합니다.


## 1.1.1 Windows fsync fix

- Fixed Windows startup failure `EPERM: operation not permitted, fsync` when creating a DB safety backup.
- Backup flush now uses a writable file handle and treats known Windows fsync limitations as best-effort; database writes still use an atomic temporary-file replacement.


## Version 1.2.0: role-based staff panel
- The ordinary header no longer shows an administrator entry to guests or VIEWER accounts.
- FRIEND, THUMBNAILER, CHAT_ADMIN and TEACHER accounts see a shared role panel with scoped tools.
- The owner dashboard has a separate `/?owner=1` entry; the owner password/session is still required.
- The server continues to protect `/api/admin/*` independently, so staff roles do not become owners by seeing the new panel.
- FRIEND video add/delete uses existing creator endpoints with server-side ownership checks; THUMBNAILER can make a PNG locally; CHAT_ADMIN/TEACHER use their server-checked chat/notice tools.
