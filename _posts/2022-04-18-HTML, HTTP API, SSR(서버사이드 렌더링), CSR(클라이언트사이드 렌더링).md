---
layout: post
title: HTML, HTTP API, SSR(서버사이드 렌더링), CSR(클라이언트 사이드 렌더링)
subtitle: HTML, HTTP API, SSR(서버사이드 렌더링), CSR(클라이언트 사이드 렌더링)
category: web
---

정적 리소스

- 고정된 HTML 파일 CSS, 이미지, 동영상 등을 제공
- 주로 웹페이지

HTML 페이지

- 동적으로 필요한 HTML파일을 생성해서 전달

HTTP API

- 주로 JSON 형태로 데이터 통신

UI 클라이언트 접점

- 앱 클라이언트(아이폰, 안드로이드, PC앱)
- 웹 브라우저에서 자바 스크립트를 통한 HTTP API 호출
- React, Vue.js 같은 웹 클라이언트

서버 to 서버

- 주문 서버 → 결제 서버
- 기업간 데이터 통신

서버 사이드 렌더링(SSR)

- 서버에서 최종 HTML을 생성하여 클라이언트에 전달
- 주로 정적인 화면에 사용
- 관련기술 : JSP, 타임리프 → 백엔드 개발자

![SSR(서버사이드 렌더링).png](/img/post/SSR(서버사이드 렌더링).png)

클라이언트 사이드 렌더링(CSR)

- HTML 결과를 자바 스크립트를 사용해 웹 브라우저에서 동적으로 생성하여 적용
- 주로 동적인 화면에 사용, 웹 환경을 마치 앱처럼 필요한 부분부분 변경할수 있음
- 구글지도 , Gmail, 구글 캘린더
- 관련기술 : React , Vue.js → 웹 프론트엔드 개발자

![클라이언트 사이드 렌더링(CSR).png](/img/post/클라이언트 사이드 렌더링(CSR).png)
