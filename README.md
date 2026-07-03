# 실시간 OX 퀴즈 웹앱

Vercel + Firebase Realtime Database 조합으로 배포하는 실시간 OX 퀴즈 앱입니다.

## 화면 구성

- 학생 화면: `/`
- 강사 화면: `/admin`

학생은 비밀번호나 방코드 없이 기본 링크로 바로 입장합니다.
강사는 기본 링크 뒤에 `/admin`을 붙여 입장합니다.

## 주요 기능

- Firebase Realtime Database 실시간 연동
- 강사 화면과 학생 화면 분리
- O/X 퀴즈 진행
- 표 형식 문제 일괄 붙여넣기
- 정답 공개
- 해설 표시
- 학생 응답 실시간 집계
- 입장/선택/정답/오답 효과음
- 모바일 최적화

## GitHub에 올릴 파일

아래 파일만 올리면 됩니다.

```text
index.html
README.md
.gitignore
```

## Firebase 설정 방법

### 1. Firebase 프로젝트 생성

Firebase Console에서 새 프로젝트를 만듭니다.

### 2. 웹 앱 추가

프로젝트 설정에서 웹 앱을 추가한 뒤 아래와 같은 설정값을 복사합니다.

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  databaseURL: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

### 3. index.html에 설정값 입력

`index.html` 파일에서 아래 부분을 찾습니다.

```js
const firebaseConfig = {
  apiKey: "",
  authDomain: "",
  databaseURL: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: ""
};
```

여기에 Firebase Console에서 복사한 값을 붙여넣습니다.

### 4. Realtime Database 생성

Firebase Console에서 Realtime Database를 생성합니다.

처음에는 잠금 모드로 만든 뒤, Rules에 아래 규칙을 넣습니다.

```json
{
  "rules": {
    "rooms": {
      "main": {
        ".read": true,
        "state": {
          ".read": true,
          ".write": true
        },
        "students": {
          ".read": true,
          "$studentId": {
            ".write": true
          }
        }
      }
    }
  }
}
```

주의: 이 규칙은 수업용 간단 버전입니다. 학생도 데이터 읽기/쓰기가 가능하므로 공개 서비스용 보안 규칙은 아닙니다.
실제 서비스에서는 Firebase Authentication 또는 관리자 비밀번호 구조를 추가하는 것이 좋습니다.

## 문제 입력 형식

강사 화면에서 아래처럼 붙여넣습니다.

```text
문제	정답	해설
태양은 별이다	O	태양은 스스로 빛을 내는 항성입니다.
지구는 태양보다 크다	X	태양보다 지구가 훨씬 작습니다.
```

엑셀이나 구글시트에서 3개 열을 복사해서 붙여넣으면 됩니다.

| 문제 | 정답 | 해설 |
|---|---|---|
| 태양은 별이다 | O | 태양은 스스로 빛을 내는 항성입니다. |
| 지구는 태양보다 크다 | X | 태양보다 지구가 훨씬 작습니다. |

## Vercel 배포 방법

1. GitHub에 이 프로젝트 파일을 업로드합니다.
2. Vercel에서 `New Project`를 누릅니다.
3. GitHub 저장소를 선택합니다.
4. Framework Preset은 `Other` 또는 자동 감지 상태로 둡니다.
5. Deploy를 누릅니다.
6. 배포 완료 후 기본 링크는 학생용입니다.
7. 강사용은 `https://배포주소.vercel.app/admin` 입니다.

## 사용 순서

1. 강사가 `/admin`으로 접속합니다.
2. 문제를 표 형식으로 붙여넣습니다.
3. `문제 불러오기`를 누릅니다.
4. 학생들은 기본 링크 `/`로 접속합니다.
5. 강사가 `게임 시작`을 누릅니다.
6. 학생들이 O/X를 선택합니다.
7. 강사는 응답 현황을 봅니다.
8. 강사가 `정답 공개`를 누릅니다.
9. `다음 문제`로 진행합니다.

## 현재 버전의 한계

- 학생 이름 입력 기능은 없습니다.
- 관리자 인증 기능은 없습니다.
- 수업용 간단 버전이므로 Firebase 규칙을 엄격하게 설정하지 않았습니다.
- 브라우저 정책상 효과음은 사용자가 화면을 한 번 터치하거나 버튼을 눌러야 안정적으로 재생됩니다.

## 다음 개선 아이디어

- 학생 닉네임 입력
- QR 코드 자동 생성
- 순위표
- 정답률 그래프
- 관리자 비밀번호
- 여러 방 지원
- 문제별 결과 저장 및 CSV 다운로드
