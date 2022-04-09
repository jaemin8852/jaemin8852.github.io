---
title:  "Stored XSS at Shopify Dashboard - $1,000 리뷰"
layout: single
date: 2022-03-29T19:00:00+09:00
last_modified_at: 2022-03-29
category: Hacking
---


# Stored XSS at Shopify Dashboard

### 요약
setting에서 street address에 XSS 페이로드를 입력하면 dashboard 페이지를 열었을 때 Stored XSS가 발생한다. 웹방화벽은 ```<!-->```를 태그 앞에 사용해서 우회할 수 있었다.  
  
### 재현
1. Shopify의 본인 스토어 계정을 연다.  
2. https://username.myshopify.com/admin/settings/general에 들어간다.  
3. street address에 다음과 같은 XSS 페이로드를 입력하고 저장한다.  
```xss"><!--><svg/onload=alert(document.domain)>```  
  
4. https://username.myshopify.com/admin/dashboards/live에 접속하면 Script가 실행된다.  
![script](/assets/img/2022-03-29-415484-Stored-xss-at-shopify-dashboard/1.png){: .center}  
  
### 생각해볼 점
이에 Bounty로 $1,000이 지급되었다. 심각도도 High(7~8.9)로 평가가 되었다.  
사용자의 세션 탈취가 주 영향으로 거론되는 XSS 공격인데, 이 사례에서는 admin에서만 고칠 수 있고 admin에 있는 메뉴에서 실행된다는 점에서 다른 사람한테 적용시키기 어려울 것 같다는(혹은 다른 취약점과 동반되어야 사용 가능하다는) 생각이 들었다.🤔  
  
참고  
<https://hackerone.com/reports/415484>
원본 링크