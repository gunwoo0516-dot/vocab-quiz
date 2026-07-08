# Vocab Quiz App

영어 단어를 직접 입력하고, 랜덤 quiz로 반복 학습할 수 있는 단어장 web app입니다.

PC와 phone에서 같은 계정으로 login하면 Firebase를 통해 단어장이 sync됩니다.

## 주요 기능

* 영어 단어 / 뜻 직접 추가
* 여러 단어 bulk 추가
* `Day 1`, `Day 2`, `Day 3`처럼 Day별 단어 관리
* 전체 Day 또는 특정 Day만 선택해서 quiz 가능
* `영단어 → 뜻`, `뜻 → 영단어`, 랜덤 방향 quiz
* 단어별 출제 횟수 / 정답 횟수 / 오답 횟수 기록
* `n회 이상` 틀린 단어만 다시 quiz 가능
* Weak Words tab에서 자주 틀리는 단어 확인
* 단어 / 뜻 / Day 기준 search
* Firebase Authentication을 이용한 login
* Firestore를 이용한 cloud sync
* Export / Import backup 지원

## 사용 흐름

1. web page 접속
2. Email / Password로 sign up 또는 login
3. 단어와 뜻 입력
4. Day 선택
5. Quiz tab에서 학습할 Day 선택
6. Quiz 진행
7. Result 또는 Weak tab에서 오답 확인
8. n회 이상 틀린 단어만 다시 학습

## Tech Stack

* HTML
* CSS
* JavaScript
* Firebase Authentication
* Firebase Firestore
* GitHub Pages

## Firebase 설정 방법

### 1. Firebase project 생성

Firebase Console에서 새 project를 생성합니다.

### 2. Web app 등록

Firebase project 안에서 Web app을 등록한 뒤, `firebaseConfig` 값을 복사합니다.

`index.html` 안의 아래 부분을 본인 Firebase project 값으로 교체합니다.

```js
const firebaseConfig = {
  apiKey: "PASTE_API_KEY_HERE",
  authDomain: "PASTE_PROJECT_ID.firebaseapp.com",
  projectId: "PASTE_PROJECT_ID",
  storageBucket: "PASTE_PROJECT_ID.appspot.com",
  messagingSenderId: "PASTE_MESSAGING_SENDER_ID",
  appId: "PASTE_APP_ID"
};
```

### 3. Authentication 설정

Firebase Console에서 아래 설정을 켭니다.

```text
Authentication
→ 로그인 방법
→ Email/Password
→ 사용 설정
→ 저장
```

### 4. Firestore Database 생성

Firebase Console에서 Firestore Database를 생성합니다.

```text
Firestore
→ 데이터베이스 만들기
→ Production mode
→ region 선택
→ 만들기
```

### 5. Firestore Rules 설정

Firestore의 `규칙` tab에 아래 내용을 넣고 게시합니다.

```js
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/words/{wordId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

이 규칙은 login한 user가 자기 단어장만 읽고 쓸 수 있도록 제한합니다.

## GitHub Pages 배포 방법

1. GitHub repository 생성
2. `index.html` 업로드
3. Repository의 `Settings` 이동
4. `Pages` 선택
5. Source를 `Deploy from a branch`로 설정
6. Branch를 `main`, folder를 `/root`로 설정
7. Save
8. 생성된 GitHub Pages URL로 접속

예상 URL 형식:

```text
https://YOUR_GITHUB_ID.github.io/YOUR_REPOSITORY_NAME/
```

## Phone에서 사용하기

GitHub Pages URL을 phone browser에서 엽니다.

```text
https://YOUR_GITHUB_ID.github.io/YOUR_REPOSITORY_NAME/
```

PC에서 만든 것과 같은 Email / Password로 login하면 같은 단어장이 표시됩니다.

iPhone에서는 Safari에서 page를 연 뒤 아래 순서로 app처럼 사용할 수 있습니다.

```text
공유 버튼
→ 홈 화면에 추가
```

## 단어 입력 format

### 단어 하나 추가

```text
word: frugal
meaning: 절약하는
day: 1
```

### Bulk 추가

```text
frugal / 절약하는
cut down on / 줄이다
mortgage / 담보 대출
```

Day를 직접 지정할 수도 있습니다.

```text
Day 1: frugal / 절약하는
Day 2: recession / 불경기 / 경기 침체
Day 3: reimburse / 배상하다
```

## Data 구조

Firestore에는 아래 구조로 저장됩니다.

```text
users
  └── {userId}
        └── words
              └── {wordId}
                    ├── word
                    ├── meaning
                    ├── day
                    ├── askedTotal
                    ├── correctTotal
                    └── wrongTotal
```

## 주의사항

* `firebaseConfig`의 `apiKey`는 password가 아닙니다.
* 하지만 Firestore Rules는 반드시 설정해야 합니다.
* Rules를 잘못 설정하면 다른 사람이 data를 읽거나 쓸 수 있습니다.
* Firebase project는 처음에는 무료 plan인 Spark plan으로 사용하면 됩니다.
* OpenAI API 같은 secret key는 절대 `index.html` 안에 직접 넣으면 안 됩니다.

## 향후 추가 가능 기능

* AI 예문 생성
* 단어별 memo
* 4지선다 quiz
* 오답만 reset / 전체 기록 reset 분리
* spaced repetition 방식의 자동 복습 schedule
* CSV / Excel import
