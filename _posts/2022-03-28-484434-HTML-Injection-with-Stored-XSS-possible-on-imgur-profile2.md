---
title:  "HTML Injection with Stored XSS possible on imgur profile (2) - $650 리뷰"
layout: single
date: 2022-03-28T21:30:00+09:00
last_modified_at: 2022-03-28
category: Hacking
---
 
  
[이전 게시물(HTML Injection with Stored XSS possible on imgur profile (1) - $750 리뷰) 보러가기](https://jaemin8852.github.io/hacking/381553-HTML-Injection-with-Stored-XSS-possible-on-imgur-profile1/)  
  
# HTML Injection with Stored XSS

### 요약
프로필에서 앨범을 만들고 이름에 html 코드를 넣으면, 평문 그대로 나오는 것이 아니라 html이 실제로 동작한다. 이에 공격자가 원하는 Script를 넣어서 실행시킬 수 있다.  
  
이전 제보에서 취약점을 수정했지만, 이번 제보에서는 수정된 페이지에서 <> 필터링을 우회하여 XSS 공격을 성공했다.  
  
### 재현
1. Imgur 계정에 로그인한다.  
2. 앨범을 만들고 이름을 아래와 같이 설정한다.  
```html
"/>&lt;script>alert(1)&lt;/script>"/>
```  
(```&lt;```은 <의 다른 표현이다)  
```HTML entities for the alternation of <```  
3. https://*username*.imgur.com으로 로그인 하면 Script가 실행된다.  
![script](/assets/img/2022-03-28-484434-HTML-Injection-with-Stored-XSS-possible-on-imgur-profile2/1.png){: .center}  
  
### 생각해볼 점
이에 Bounty로 $650이 지급되었다.  
링크에 수상한 script가 있는 것도 아니고 그냥 공격자의 프로필만 접속해도, 피해자가 로그인을 하고 있지 않아도 스크립트가 실행된다는 점에서 피해가 발생할 확률이 높을 것이다.  
사용자로부터 받은 text 필드를 평문으로 출력할 수 있도록 코딩해야 할 것이다. 필터링을 확실하게 하거나!😄  
  
참고  
<https://hackerone.com/reports/484434>
원본 링크 