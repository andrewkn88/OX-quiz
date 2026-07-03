# 실시간 OX 퀴즈 교실 v6

Vercel + Firebase Realtime Database로 배포하는 실시간 OX 퀴즈 웹앱입니다.
강사 화면과 학생 화면이 같은 페이지에 함께 있습니다.

## 포함 기능

- 강사/학생 한 화면 구성
- 학생 닉네임 입력
- 표 형식 문제 일괄 붙여넣기
- API 없이 주제별 OX 문제 자동 생성
  - 기본 내장 주제: 과학, 인공지능, 역사
  - 그 외 주제는 일반 교육용 OX 패턴으로 생성
- QR 코드 자동 생성
- 실시간 O/X 응답 현황
- 정답률 그래프
- 순위표
- 효과음 및 정답 애니메이션
- 문제별 결과 저장
- CSV 다운로드
- Vercel 배포용 vercel.json 포함

## GitHub에 올릴 파일

```text
index.html
vercel.json
README.md
```

## Firebase 설정

`index.html` 안의 `firebaseConfig` 값을 본인 Firebase 프로젝트 값으로 바꾸세요.

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

## Realtime Database Rules

수업 테스트용 최소 규칙입니다.

```json
{
  "rules": {
    "oxQuiz": {
      ".read": true,
      ".write": true
    }
  }
}
```

운영 보안을 강화하려면 Firebase Authentication을 추가하는 것이 좋습니다.

## 문제 입력 형식

엑셀/구글시트에서 아래 형식으로 복사해서 붙여넣으면 됩니다.

```text
문제	정답	해설
태양은 스스로 빛을 내는 별이다.	O	태양은 항성입니다.
지구는 태양보다 크다.	X	태양이 지구보다 훨씬 큽니다.
```


## API 없는 자동 생성 기능

강사 화면에서 주제와 문항 수를 입력한 뒤 **OX 문제 자동 생성**을 누르면 `문제 / 정답 / 해설` 형식으로 자동 입력됩니다.

확인된 내장 주제는 `과학`, `인공지능`, `역사`입니다. 그 외 주제는 일반 학습용 문항 패턴으로 생성됩니다.

주의: API를 사용하지 않기 때문에 진짜 AI 생성은 아니며, 내장된 문제은행과 문장 패턴을 조합하는 방식입니다. 강의 전에 문제와 해설을 한 번 확인한 뒤 사용하세요.
