# SPA

## SPA란

- 서버로부터 완전한 페이지를 불러오지 않고, 현재 페이지를 동적으로 다시 작성하면서 사용자와 소통하는 웹 애플리케이션이나, 웹 사이트.

- 클릭, 스크롤을 통한 상호작용에서 최소한의 변경만 일어난다.

- 이때 페이지의 변경은 최초로 로드된 JS를 통해 미리 브라우저에 올라간 템플릿만 교체되는 것이다.

## SPA 라우팅

- HTML5의 **history API를 이용하여 server로 요청을 전달하기 전에 화면 이동을 유도**한다.

- history.state에 담아둔 정보로 ajax에 요청을 보내 화면을 갱신한다.

- 예시 (JS만 이용)

```js
// 각 페이지 js 파일 import
import Home from "./pages/home.js"
import Login from "./pages/login.js"

const $ = document;

// router 함수의 정의 (async)
const router = async () => {

  // routes 배열의 정의 : (경로 , view : 실제 위치)
  // view 설정을 통한 랜더링 진행
  const routes = [
    { path : "/" , view : Home },
    { path : "/home" , view : Home },
    { path : "/login", view : Login },
  ];

  const pageMatches = routes.map(route=>{
    return {
      route,
      isMatch : route.path === location.pathname
    };
  });

  let match = pageMatches.find(pageMatch => pageMatch.isMatch);
  const container = $.querySelector("#container");
  container.innerHTML=``;

  if(!match) {
    container.innerHTML=`<h1>404 Not Found</h1>`
  }
  else {
    const page = new match.route.view();
    // promise 객체로 전달된 요소들을 container에 appendchild
    const elements = await page.getPage();
    elements.forEach(element => {
      container.appendChild(element);
    });
  }

}

// 정의한 route 함수를 바탕으로 event를 구현한다.
// HTML 모두 load 되었을 때(DOMContentLoaded 사용)
// : click 들에 대한 이벤트 부과
// : router() 함수 호출
document.addEventListener("DOMContentLoaded", () => {
  document.body.addEventListener("click", event => {
    if (event.target.matches("[data-link]")) {
      event.preventDefault();
      history.pushState(null, null, event.target.getAttribute('href'));
      router();
    }
  });
  router();
})

// popstate 이벤트로 브라우저의 뒤로가기 이벤트 발생 시에 뒤로 가도록 설정
// 히스토리 엔트리 간의 이동이 발생할 때 popstate 이벤트 발생
window.addEventListener("popstate", ()=>{
  router();
});
```

## SPA framework

- Angular, React, Vue 

- Virtual DOM 개념을 사용하여 SPA를 구현한다.