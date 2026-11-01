# 우주공강 (Spring Boot)

Java 17, Spring Boot, Thymeleaf, Spring Data JPA, MySQL 기반 웹 프로젝트입니다.

## 요구 사항

- Java 17
- MySQL 8.x 또는 호환되는 MySQL 서버
- Maven (또는 프로젝트에 포함된 Maven Wrapper)

## 로컬 실행

1. MySQL에서 `teamjavaprogramming` 데이터베이스를 생성합니다.
2. 환경변수를 설정합니다. 실제 비밀번호는 소스 코드나 GitHub에 저장하지 마세요.

macOS/Linux 예시:

```bash
export DB_URL='jdbc:mysql://localhost:3306/teamjavaprogramming'
export DB_USERNAME='root'
export DB_PASSWORD='여기에_로컬_DB_비밀번호'
./mvnw spring-boot:run
```

Windows PowerShell 예시:

```powershell
$env:DB_URL='jdbc:mysql://localhost:3306/teamjavaprogramming'
$env:DB_USERNAME='root'
$env:DB_PASSWORD='여기에_로컬_DB_비밀번호'
.\mvnw.cmd spring-boot:run
```

브라우저에서 `http://localhost:8080`으로 접속합니다. 실제 시작 경로는 컨트롤러 매핑에 따라 달라질 수 있습니다.

## GitHub에 올리기

1. GitHub에서 새 저장소를 생성합니다.
2. 이 프로젝트 폴더의 내용을 저장소에 push합니다.
3. `.env`, 실제 비밀번호, API 키, 개인 데이터는 커밋하지 않습니다.

## Render에 Docker로 배포

1. 프로젝트를 GitHub에 push합니다.
2. Render에서 **New → Web Service**를 선택하고 GitHub 저장소를 연결합니다.
3. Runtime을 **Docker**로 선택하고 Dockerfile 경로를 `./Dockerfile`로 지정합니다.
4. 배포 환경변수를 등록합니다.

| 변수 | 값 |
|---|---|
| `DB_URL` | 클라우드 MySQL JDBC URL (예: `jdbc:mysql://호스트:3306/데이터베이스?…`) |
| `DB_USERNAME` | 운영 DB 사용자명 |
| `DB_PASSWORD` | 운영 DB 비밀번호 |
| `DDL_AUTO` | 초기 테스트는 `update` 가능. 운영 환경은 검토 후 `validate` 및 마이그레이션 사용 권장 |
| `SHOW_SQL` | `false` |

Render의 공개 URL로 접속할 수 있습니다. Render 웹 서비스만 배포한다고 MySQL 데이터베이스까지 자동으로 만들어지는 것은 아닙니다. 별도의 관리형 MySQL 또는 접근 가능한 외부 MySQL이 필요합니다. DB 방화벽/접속 허용 목록도 설정해야 합니다.

## 중요한 배포 전 확인

- 운영 DB에 필요한 스키마와 데이터가 준비되어 있는지 확인하세요. `ddl-auto=update`는 데이터베이스 이전이나 백업을 대신하지 않습니다.
- `application.properties`에는 실제 비밀번호를 넣지 않습니다. 배포 환경의 Environment 설정에 비밀값을 등록하세요.
- 이전 버전에서 DB 비밀번호를 소스 코드에 넣어 사용했다면, 해당 비밀번호를 즉시 변경하고 새 비밀번호를 환경변수로 설정하세요. Git 이력에 이미 커밋된 비밀값은 파일에서 지우는 것만으로 이력에서 사라지지 않습니다.
- 실제 로그인/회원가입 및 DB 연결을 다른 네트워크에서 테스트하세요.
