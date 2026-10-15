# 친하 공식 사이트 공개 호스팅 안내 (Render)

이 프로젝트는 별도 Node.js 서버와 HTML 화면을 사용합니다. `render.yaml`은 Render Web Service 배포 설정이며, 파일 기반 DB와 업로드가 재배포 후 보존되도록 `/var/data` 영속 디스크를 사용하도록 구성했습니다.

## 먼저 꼭 알아둘 것

- 이 설정은 로컬 실행이 아니라 인터넷 공개 배포를 위한 것입니다.
- Render의 영속 디스크는 유료 웹 서비스가 필요할 수 있습니다. 결제 또는 유료 플랜을 선택하기 전에 요금과 약관을 확인하고 보호자와 상의하세요.
- ChatGPT에서 호스팅 계정에 대신 로그인하거나 계정을 연결할 수는 없습니다. 소유자가 직접 계정을 연결하고 배포 버튼을 눌러야 합니다. 계정 비밀번호나 비밀값을 채팅에 보내지 마세요.
- `server/data/db.json`, `server/data/backups`, `.env`는 개인정보/인증 정보가 들어갈 수 있으므로 GitHub에 올리면 안 됩니다. 이 패키지는 실제 DB 파일을 포함하지 않습니다. 새 호스팅 DB는 첫 실행 때 빈 데이터로 초기화됩니다.

## GitHub 저장소 준비

1. ZIP 압축을 풉니다.
2. 안쪽 `chinha-official-site-v1.3` 폴더의 **내용 전체**를 새 GitHub 저장소 루트에 업로드합니다. 바깥 폴더 자체를 한 단계 더 감싸서 올리지 않습니다.
3. `server/data/db.json`, `server/data/admin-initial-password.txt`, `.env` 또는 백업 ZIP을 커밋하지 마세요. `.gitignore`가 있지만 이미 추적된 파일은 별도 확인이 필요합니다.

## Render에 연결

1. Render 계정에 로그인합니다.
2. Dashboard에서 New → Blueprint를 선택하고 GitHub 저장소를 연결합니다.
3. `render.yaml`에서 Web Service 설정을 확인합니다.
4. `CHINHA_ADMIN_PASSWORD` 값을 충분히 길고 고유한 비밀번호로 설정합니다. 이 값은 공개 저장소나 소스 코드에 넣지 마세요.
5. 디스크가 `/var/data`에 연결되는지 확인하고 생성/배포합니다. 디스크/플랜 비용은 Render의 현재 안내에서 확인하세요.
6. Deploy가 Live가 된 후 Render가 발급한 `https://...onrender.com` 주소를 엽니다.
7. 총괄 로그인은 해당 주소 끝에 `/owner-login`을 붙입니다. 예: `https://서비스이름.onrender.com/owner-login`.
8. 로그인 후 관리자 화면에서 초기 설정을 확인하세요. `CHINHA_ADMIN_PASSWORD`는 새 DB의 초기 총괄 비밀번호로 쓰입니다. 이후 관리자 보안 메뉴에서 비밀번호를 바꾸면 환경 변수와 DB 비밀번호가 서로 달라질 수 있으므로 비밀번호 변경 후에는 DB 상태를 유지하고, 환경 변수는 새 관리자 비밀번호와 일치하도록 맞추거나 설정 지침을 따라 주세요.

## 데이터 경로

- 로컬: `server/data/db.json`, `server/uploads/`
- Render: `/var/data/db.json`, `/var/data/backups/`, `/var/data/uploads/`

로컬 DB가 자동으로 호스팅 DB로 복사되지는 않습니다. 기존 회원/채팅을 가져와야 하면 비공개로 안전하게 마이그레이션해야 합니다. 실제 DB를 공개 GitHub에 올려서 옮기지 마세요.
