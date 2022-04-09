---
title:  "XSS Reflected at sketch.pixiv.net Via 'next_url' - $500 리뷰"
layout: single
date: 2022-03-28T18:00:00+09:00
last_modified_at: 2022-03-28
category: Hacking
---
 

# XSS Reflected at sketch.pixiv.net Via 'next_url'  
Reflected XSS(Reflected Cross Site Scripting)는 파라미터 등을 매개로 서버에 스크립트를 전달(request)하여 스크립트가 페이지 내에서 실행되도록 하는 행위이다.  
  
### 요약
https://sketch.pixiv.net의 URL에서 Reflected XSS를 발생시킬 수 있었다.  
  
```https://sketch.pixiv.net/resign_request/success?next_url=javascript%3Aalert%2F**%2F(document.domain)```  
next_url이라는 파라미터에 스크립트를 작성해서 넘겼고, 실제로 동작하는 것을 볼 수 있다.  
  
![now](/assets/img/2022-03-28-1503601-XSS-Reflected-at-sketch.pixiv.net-Via-'next_url'/1.jpg){: .center}  
  
이 취약점을 이용하면 다른 사용자의 세션을 탈취하여 사용자인척 사이트 내에서 행동할 수 있다.  
  
이에 Bounty로 $500이 지급되었다.  

### 현재
현재는 whitelist에 등록되지 않은 url이 next_url의 인자로 들어왔다면 버튼을 disable 시킨다.  
  
![now](/assets/img/2022-03-28-1503601-XSS-Reflected-at-sketch.pixiv.net-Via-'next_url'/2.png){: .center}  
  
참고  
<https://hackerone.com/reports/1503601>  
원본 링크 