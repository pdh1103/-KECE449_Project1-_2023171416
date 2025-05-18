# -KECE449_Project1-_2023171416

# 박다현
# 2023171416 

---

## 📌 프로젝트 개요

이 프로젝트는 **Google OAuth 2.0 인증 시스템**과 **Gemini API**를 이용하여  
사용자가 프롬프트를 입력하고 AI의 응답을 받을 수 있는 간단한 챗봇 웹 애플리케이션입니다.

Node.js와 Express를 기반으로 구성되어 있으며, 사용자는 브라우저에서 Google 계정으로 로그인한 후,  
질문을 입력하면 Gemini AI로부터 텍스트 응답을 받아볼 수 있습니다.

---

## 🔁 프로그램 실행 흐름

1. **사용자 진입**  
   사용자가 웹 브라우저에서 `http://localhost:3000`에 접속하면,  
   `index.html` 화면이 열리고 `Login with Google` 버튼이 표시됩니다.

2. **Google 로그인 인증**  
   사용자가 버튼을 클릭하면 `/auth/google` 경로로 리다이렉트되며,  
   Google OAuth 인증 절차를 거쳐 로그인이 성공하면 `/prompt` 페이지로 이동합니다.

3. **프롬프트 입력**  
   로그인한 사용자는 `prompt.html` 페이지에서 질문을 입력할 수 있으며,  
   이 입력은 `/ask` 라우트를 통해 서버로 전송됩니다.

4. **Gemini API 호출**  
   서버(`server.js`)는 사용자의 질문을 Gemini API로 전달하고,  
   응답으로 받은 AI의 메시지를 클라이언트로 다시 전송합니다.

5. **결과 출력**  
   사용자는 `prompt.html` 화면에서 Gemini AI의 응답 결과를 실시간으로 확인할 수 있습니다.

6. **로그아웃**  
   `/logout` 경로를 통해 세션을 종료하고 로그아웃할 수 있습니다.



## 🧠 코드 구성 및 흐름 설명

프로젝트의 전체 로직은 `server.js`를 중심으로 구성되어 작동합니다.

1. **모듈 및 환경변수 로드**
   - `express`, `passport`, `axios` 등 필요한 Node.js 모듈을 불러오고,  
     `.env` 파일에서 OAuth 및 API 키 정보를 가져옵니다.

2. **Express 및 세션 설정**
   - `express-session`을 사용하여 로그인 세션을 관리하며,  
     `express.static`을 통해 `public` 폴더 내 index.html을 serving합니다.

3. **Passport를 통한 Google 로그인 전략 등록**
   - `passport-google-oauth20`을 사용하여 Google 로그인 기능을 구성하고,  
     로그인 성공 시 사용자 정보를 세션에 저장합니다.

4. **라우터 구성**
   - `/auth/google`: Google 로그인 시작 경로  
   - `/auth/google/callback`: 로그인 완료 후 리디렉션 처리  
   - `/prompt`: 로그인한 사용자만 접근 가능한 프롬프트 입력 페이지  
   - `/ask`: 사용자의 질문을 Gemini API에 전달하고 응답을 반환하는 REST API  
   - `/logout`: 세션 종료 및 로그아웃 처리

5. **Gemini API 연동**
   - 사용자가 입력한 프롬프트를 `axios`를 사용해 Gemini API에 POST 방식으로 전송하며,  
     응답으로 받은 AI 메시지를 그대로 클라이언트에 전달합니다.

6. **프론트엔드 로직**
   - `index.html`: Google 로그인 버튼만 표시됨  
   - `prompt.html`: 사용자의 입력을 받아 JavaScript `fetch()`를 통해 서버의 `/ask`에 전송하고,  
     결과를 `<div id="response">`에 출력합니다.

인증, 프롬프트 수신, API 호출이라는 3단계 구조로 구성되어있습니다.

