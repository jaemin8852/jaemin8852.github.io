---
title:  "Stored XSS at https://linkpop.com - $1,600 리뷰"
layout: single
date: 2022-03-27
last_modified_at: 2022-03-27
category: Hacking
---

<https://hackerone.com/reports/1441988>  
원본 링크

# Stored XSS at https://linkpop.com  
  
### 요약
```There is Stored XSS vulnerability at https://linkpop.com/dashboard/admin that can later be delivered through unique linkpop link. This is due to lack of sanitizaiton and relying on client side protections when inserting urls to our applications.```
  

클라이언트측에서만 필터링을 거쳤기 때문에 쉽게 우회가 가능했고, 이는 Stored XSS 취약점으로 연결되었다.  

### 재현
1. <www.linkpop.com> 에 방문한다.  
2. 로그인 후 새로운 템플릿을 만든다.  
  
![screen shot](/assets/img/2022-03-27-1441988-Stored-XSS-at-linkpop.com/1.png){: .center}  
  
이처럼 link를 만들어주는 서비스다.  
  
![client](/assets/img/2022-03-27-1441988-Stored-XSS-at-linkpop.com/2.png){: .center}  
  
스크립트를 그대로 URL에 넣으면 클라이언트측에서 막는다.  
  
3. Burp Suite Proxy Intercept를 켜주고 Publish를 눌렀을 때 요청을 캡쳐한다.  
여기서 URL을 수정하고 Forward한다.  
  
![capture](/assets/img/2022-03-27-1441988-Stored-XSS-at-linkpop.com/3.png){: .center}  
  
4. 링크를 클릭하면 스크립트가 실행되어 도메인을 표시한다.  
  
![scripting](/assets/img/2022-03-27-1441988-Stored-XSS-at-linkpop.com/4.png){: .center}  
   
  
### 현재 상태
지금은 Burp에서 URL 값을 변경해도 유효한지 필터링하기 때문에 스크립트 삽입이 불가능하다.  
  
![now](/assets/img/2022-03-27-1441988-Stored-XSS-at-linkpop.com/5.png){: .center}  
  
### 생각해볼 점
이에 Bounty로 $1,600이 지급되었다.  
기억하기론 당시 만들어진지 얼마 안 된 서비스였다. 서비스 자체 목적이 링크를 클릭하게 만드는 것인 만큼 Stored XSS는 위험한 취약점으로 작용할 수 있었기에 큰 바운티가 지급된 것 같다. Shopify에서 제공하는 서비스기 때문에 쿠키를 탈취했을 때에도 사용 범위가 넓을 것이라 생각된다.