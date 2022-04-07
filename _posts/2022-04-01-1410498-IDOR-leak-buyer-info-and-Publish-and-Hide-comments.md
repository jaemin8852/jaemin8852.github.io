---
title:  "IDOR: leak buyer info and Publish/Hide foreign comments at Judge.me - $1,250 리뷰"
layout: single
date: 2022-04-01T17:02:00+09:00
last_modified_at: 2022-04-01
category: Hacking
---

<https://hackerone.com/reports/1410498>  
원본 링크

# IDOR: leak buyer info & Publish/Hide foreign comments
### 요약
IDOR(Insecure Direct Object Reference)은 서버로 요청하는 메시지의 URL이나 파라미터를 변경하여 정상적이지 않은 기능을 수행하는 공격 기법이다.  
  
이 사례에서는 서버로 요청하는 댓글의 id 파라미터를 변경함으로써 댓글을 단 사람이 구매한 물품, 댓글 주인의 이름, 이메일을 확인할 수 있고, 그 댓글을 publish 하거나 숨겨버릴 수 있다.  
다른 shop에 지장을 줄 수 있는 취약점이고(댓글을 숨긴다거나), 구매정보와 구매자 정보의 유출로 인해 피싱이 이뤄질 수도 있기 때문에 위험하다.  
  
```Our apps help you collect and display reviews on Shopify```  
참고로 Judge.me는 이런 앱이다.  

### 재현
1. Shopify의 본인 store에 'Checkout Comments"를 설치한다.  
2. Free 상품을 만들고 주문을 넣은 다음 comment를 입력한다.  
![input order information](/assets/img/2022-04-01-1410498-IDOR-leak-buyer-info&Publish-and-Hide-comments/1.png){: .center}  
주문 정보 입력  
![comment](/assets/img/2022-04-01-1410498-IDOR-leak-buyer-info&Publish-and-Hide-comments/2.png){: .center}  
Comment 입력하고 send  
3. Shopify에서 /admin/apps/checkout-comments/extensions/checkout_comments/comments로 이동하고, Publish를 누를 때 Burp Suite를 사용하여 Intercept 하고 요청을 repeater로 보낸다.  
![publish](/assets/img/2022-04-01-1410498-IDOR-leak-buyer-info&Publish-and-Hide-comments/3.png){: .center}  
Intercept on 하고 publish  
![go to repeater](/assets/img/2022-04-01-1410498-IDOR-leak-buyer-info&Publish-and-Hide-comments/4.png){: .center}  
Intercept에 걸린 post를 repeater로 보냄  
4. repeater에서 comment_id를 바꿔서 요청을 보내면 응답에서 사용자의 정보가 노출된다.  
![leak](/assets/img/2022-04-01-1410498-IDOR-leak-buyer-info&Publish-and-Hide-comments/5.png){: .center}  
원래 요청(첫 이미지에서 구매 정보에 입력했던 정보)
![leak](/assets/img/2022-04-01-1410498-IDOR-leak-buyer-info&Publish-and-Hide-comments/6.png){: .center}  
comment_id 변경 후 요청(다른 누군가의 댓글 및 구매 정보)  
5. 뿐만 아니라 curated 매개변수를 spam으로 바꾸면 댓글을 숨길 수도 있다.  
![hide](/assets/img/2022-04-01-1410498-IDOR-leak-buyer-info&Publish-and-Hide-comments/7.png){: .center}  
  
   
### 생각해볼 점
이에 Bounty로 $1,250이 지급되었다.  
사용자의 구매 정보와 개인 정보가 있기 때문에 피싱 메일로 이어지면 많은 피해자가 발생할 수 있을 것이다.  
