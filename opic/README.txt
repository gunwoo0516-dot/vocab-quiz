OPIc Sentence Cards - Firebase Sync

1) Firebase Firestore Rules를 firestore.rules.txt 내용으로 업데이트
2) GitHub repo gunwoo0516-dot/vocab-quiz 에 opic 폴더 생성
3) 아래 5개 파일을 opic 폴더에 업로드
   - index.html
   - manifest.webmanifest
   - service-worker.js
   - icon-192.png
   - icon-512.png

배포 URL:
https://gunwoo0516-dot.github.io/vocab-quiz/opic/

기존 단어장과 같은 Firebase Email/Password 계정으로 로그인하면
문장과 즐겨찾기가 users/{uid}/sentences/{sentenceId}에 저장되어
PC와 iPhone에서 동기화됩니다.

Bulk 형식:
한국어 문장 | English sentence
