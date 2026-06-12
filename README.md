# 임계점° — 영작 트레이너 설치 가이드

> 교재 『영어로 문장 만들기 훈련』(1·2·3차 임계점) 구조를 따르는 가족용 영작 훈련 앱입니다.
> 파일 3개: `index.html`(앱 전체), `firestore.rules`(보안 규칙), 이 문서.

---

## 0. 전체 흐름 한눈에

1. Firebase 프로젝트 만들기 (무료 Spark 플랜이면 충분)
2. Google 로그인 켜기 + Firestore 만들기
3. `index.html`에 Firebase 설정값 붙여넣기
4. **이메일 2곳 교체** (index.html + firestore.rules)
5. GitHub Pages에 올리기 + 승인 도메인 추가
6. 끝. 가족 각자 Google 계정으로 로그인하면 됩니다.

소요 시간: 처음 해보셔도 20~30분.

---

## 1. Firebase 프로젝트 만들기

1. https://console.firebase.google.com 접속 → **프로젝트 추가**
2. 이름은 자유 (예: `critical-point`). Google 애널리틱스는 꺼도 됩니다.
3. 프로젝트가 만들어지면 좌측 상단 ⚙️ → **프로젝트 설정** → 아래로 내려 **앱 추가 → 웹(</>)** 선택
4. 앱 닉네임 입력 후 등록하면 `firebaseConfig` 코드가 나옵니다. **이 값을 복사해두세요.**

```js
const firebaseConfig = {
  apiKey: "AIza....",
  authDomain: "xxx.firebaseapp.com",
  projectId: "xxx",
  ...
};
```

## 2. Google 로그인 켜기

1. 좌측 메뉴 **빌드 → Authentication** → 시작하기
2. **로그인 방법** 탭 → **Google** 선택 → 사용 설정 → 저장

## 3. Firestore 만들기

1. 좌측 메뉴 **빌드 → Firestore Database** → 데이터베이스 만들기
2. 위치는 `asia-northeast3 (서울)` 권장
3. **프로덕션 모드**로 시작 (규칙은 다음 단계에서 붙여넣습니다)
4. 만들어지면 **규칙** 탭 → 내용을 전부 지우고 `firestore.rules` 파일 내용을 붙여넣기 → **게시**

## 4. index.html 설정값 교체

`index.html`을 텍스트 편집기로 열고 **①설정** 블록(약 180~205줄 부근)을 찾습니다.

### 4-1. firebaseConfig 교체
1단계에서 복사한 값을 그대로 붙여넣습니다.

### 4-2. 이메일 교체 ★ 가장 중요
```js
const FAMILY = [
  { email: "child10@example.com", name: "첫째", grade: 10 },
  { email: "child8@example.com",  name: "둘째", grade: 8  },
  { email: "child6@example.com",  name: "셋째", grade: 6  },
];
const TEACHER_EMAILS = ["junkim@example.com"];
```
→ 자녀들이 실제로 쓸 **Google 계정 이메일**과 준킴쌤 이메일로 바꿉니다. `name`도 실제 이름으로 바꾸면 가족 보드에 그 이름이 표시됩니다.

> ⚠️ **반드시 2곳을 똑같이 수정하세요.**
> ① `index.html`의 FAMILY + TEACHER_EMAILS
> ② `firestore.rules`의 `isFamily()` 함수 안 이메일 목록
> 한 곳만 바꾸면 로그인은 되는데 저장이 안 되거나, 그 반대가 됩니다.
> rules를 수정한 뒤에는 Firebase 콘솔 → Firestore → 규칙 탭에서 다시 **게시**해야 적용됩니다.

## 5. GitHub Pages 배포

이미 Qum Educlesia 플랫폼을 GitHub Pages로 운영 중이시니 익숙한 방식 그대로입니다.

1. 저장소에 `index.html` 업로드 (예: `yeongjak/` 폴더 안)
2. Settings → Pages에서 배포 확인 (기존 저장소에 폴더로 넣으면 추가 설정 불필요)
3. **Firebase 승인 도메인 추가** ★ 빠뜨리기 쉬움
   - Firebase 콘솔 → Authentication → 설정 → **승인된 도메인** → `사용자이름.github.io` 추가
   - 이걸 안 하면 GitHub Pages에서 Google 로그인 팝업이 차단됩니다.

## 6. 첫 사용

- 각자 자기 Google 계정으로 로그인 → 화이트리스트에 있으면 **배치고사**(9문항, 빈칸으로 건너뛰기 가능)부터 시작
- 배치 기준: 레벨당 3문항 중 2개 이상 정답 → 다음 레벨 후보. 레벨1·2 모두 통과 시 레벨3에서 시작.
- 매일 홈 화면의 **오늘의 유닛**을 누르면 됨 (유닛당 3문장, 2개 이상 정답 시 통과)
- 한 레벨의 유닛을 모두 마치면 **임계점 시험**(12문항 무작위, 10개 이상 통과) → 다음 레벨로 승급
- 레벨3 돌파 시: 듀오링고(DET) 준비 1단계 → AP 연계 2단계 안내 화면이 나옵니다.
- 가족 보드: 홈 화면 하단에서 서로의 오늘 기록과 연속 일수(🔥)를 봅니다. 준킴쌤 계정은 상세 테이블까지 보입니다.

## 7. 자주 묻는 문제

| 증상 | 원인 | 해결 |
|---|---|---|
| "등록되지 않은 계정" | 로그인한 Google 이메일이 FAMILY에 없음 | index.html FAMILY 확인 (대소문자 무관, 오타 주의) |
| 로그인은 되는데 저장 안 됨 | rules의 이메일 목록 불일치 | firestore.rules 수정 후 **게시** |
| 로그인 팝업 차단/실패 | 승인 도메인 누락 | Authentication → 승인된 도메인에 github.io 주소 추가 |
| 문제(채점)가 작동 안 함 | 인터넷 연결 문제 | 채점은 앱 내부 로직이라 오프라인에서도 동작하나, 저장은 온라인 필요 |

## 8. 나중에 학교 전체로 확장할 때 (범위 3)

- FAMILY 배열 → Firestore의 `roster` 컬렉션으로 옮기고, rules도 컬렉션 조회 방식으로 변경
- 반/학년 그룹핑, 교사 대시보드 추가
- 지금 구조(users + logs)는 그대로 재사용 가능하도록 설계되어 있습니다.
