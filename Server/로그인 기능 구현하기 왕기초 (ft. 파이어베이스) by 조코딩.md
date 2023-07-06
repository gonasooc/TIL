[로그인 기능 구현하기 왕기초 (ft. 파이어베이스)](https://www.youtube.com/watch?v=tPqTE14DEUg)

## 로그인 기능의 이해

- 최초에 회원가입을 통해 이메일 혹은 아이디, 패스워드 등을 포함한 개인정보를 받고, 로그인 진행 시 이메일과 패스워드를 서비스 DB에 넘겨줬을 때 일치하는 정보가 있으면 개별 토큰값을 전해줌
- 로그인 이후에는 해당 토큰을 header에 넣은 상태로 요청하게 되고 그걸 통해 유저를 기억함 → 토큰 + 원하는 요청을 보내면 알맞은 응답을 보내주는 구조

## Firebase란?

- 쉽고 빠르게 서버(백엔드)를 구축할 수 있는 서비스
- 빌드 → Cloud Storage / Firebase Hosting / Firebase Cloud Messaging / Cloud Firestore / Firebase Authentication …

```jsx
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>login</title>
  </head>
  <body>
    <form action="">
      <h2>회원가입</h2>
      <div>
        <label for="">email: </label>
        <input type="email" id="signUpEmail" />
      </div>
      <div>
        <label for="">password: </label>
        <input type="password" id="signUpPassword" />
      </div>
      <button type="submit" id="signUpButton">회원가입하기</button>
    </form>
    <form action="">
      <h2>로그인</h2>
      <div>
        <label for="">email: </label>
        <input type="email" id="signInEmail" />
      </div>
      <div>
        <label for="">password: </label>
        <input type="password" id="signInPassword" />
      </div>
      <button type="submit" id="signInButton">로그인하기</button>
    </form>
    <script type="module">
      // Import the functions you need from the SDKs you need
      import { initializeApp } from "https://www.gstatic.com/firebasejs/9.23.0/firebase-app.js";
      // TODO: Add SDKs for Firebase products that you want to use
      // https://firebase.google.com/docs/web/setup#available-libraries

      // Your web app's Firebase configuration
      const firebaseConfig = {
        apiKey: "AIzaSyDo16bNzczL1jvysGkTjuhG0SLovsh9SVc",
        authDomain: "easylogin-98ad1.firebaseapp.com",
        projectId: "easylogin-98ad1",
        storageBucket: "easylogin-98ad1.appspot.com",
        messagingSenderId: "657356166900",
        appId: "1:657356166900:web:7e781b1bebf2895647f0a1",
      };

      // Initialize Firebase
      const app = initializeApp(firebaseConfig);

      import {
        getAuth,
        createUserWithEmailAndPassword,
        signInWithEmailAndPassword,
      } from "https://www.gstatic.com/firebasejs/9.23.0/firebase-auth.js";

      document
        .getElementById("signUpButton")
        .addEventListener("click", (event) => {
          event.preventDefault();
          const email = document.getElementById("signUpEmail").value;
          const password = document.getElementById("signUpPassword").value;
          const auth = getAuth();
          createUserWithEmailAndPassword(auth, email, password)
            .then((userCredential) => {
              console.log("userCredential", userCredential);
              // Signed in
              const user = userCredential.user;
              // ...
            })
            .catch((error) => {
              console.log(error);
              const errorCode = error.code;
              const errorMessage = error.message;
              // ..
            });
        });

      document
        .getElementById("signInButton")
        .addEventListener("click", (event) => {
          event.preventDefault();
          const email = document.getElementById("signInEmail").value;
          const password = document.getElementById("signInPassword").value;

          const auth = getAuth();
          signInWithEmailAndPassword(auth, email, password)
            .then((userCredential) => {
              console.log("userCredential", userCredential);
              // Signed in
              const user = userCredential.user;
              // ...
            })
            .catch((error) => {
              const errorCode = error.code;
              const errorMessage = error.message;
            });
        });
    </script>
  </body>
</html>
```
