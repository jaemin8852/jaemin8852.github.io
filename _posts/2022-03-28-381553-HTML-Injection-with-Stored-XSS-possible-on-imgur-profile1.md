---
title:  "HTML Injection with Stored XSS possible on imgur profile (1) - $750 리뷰"
layout: single
date: 2022-03-28T21:10:00+09:00
last_modified_at: 2022-03-28
category: Hacking
---

<https://hackerone.com/reports/381553>
원본 링크

# HTML Injection with Stored XSS  

### 요약
프로필에서 앨범을 만들고 이름에 html 코드를 넣으면, 평문 그대로 나오는 것이 아니라 html이 실제로 동작한다. 이에 공격자가 원하는 Script를 넣어서 실행시킬 수 있다.   

### 재현
1. Imgur 계정에 로그인한다.  
2. 앨범을 만들고 이름을 아래와 같이 설정한다.  
```html
#"></div><a href=javaSCRIPT&colon;alert(/XSS/)>XSS</a>
```  
(&colon;은 :의 다른 표현이다)  
3. https://*username*.imgur.com으로 로그인 하면 Script가 실행된다.  
![script](/assets/img/2022-03-28-381553-HTML-Injection-with-Stored-XSS-possible-on-imgur-profile1/1.png){: .center}  
  
### 생각해볼 점
이에 Bounty로 $750이 지급되었다.  
링크에 수상한 script가 있는 것도 아니고 그냥 공격자의 프로필만 접속해도, 피해자가 로그인을 하고 있지 않아도 스크립트가 실행된다는 점에서 피해가 발생할 확률이 높을 것이다.  
사용자로부터 받은 text 필드를 평문으로 출력할 수 있도록 코딩해야 할 것이다. 다음 게시물에서 알 수 있듯, <>만 치환하는 필터링을 거쳤기에 이를 우회해서 또 XSS를 성공시켰으니 말이다.  
[다음 게시물 보러가기](https://jaemin8852.github.io/hacking/484434-HTML-Injection-with-Stored-XSS-possible-on-imgur-profile2/)