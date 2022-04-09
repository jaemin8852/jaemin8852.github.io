---
title:  "Business Logic Flaw in the subscription of the Dragon app - $250 리뷰"
layout: single
date: 2022-03-27T12:00:00+09:00
last_modified_at: 2022-03-27
category: Hacking
---


# Business Logic Flaw 논리 결함
### 요약
```Business logic vulnerabilities are flaws in the design and implementation of an application that allow an attacker to elicit unintended behavior. This potentially enables attackers to manipulate legitimate functionality to achieve a malicious goal.```  
  
설계 및 구현에서의 논리 결함은 공격자가 악의적인 행동을 할 수 있게 가능성을 만든다.  
물론 이 사례에서는 백엔드 필터링으로 인해 app의 구독 코드를 성공적으로 얻지는 못했지만, 공격자가 원하는 방향대로 반영되었기 때문에 처리를 해주는 게 좋겠다.

### 재현
1. https://getdragon.ch/checkout에 방문한다.
2. Burp Suite Proxy Intercept를 켜주고 payment를 누른다.  
3. POST 요청에서 구매 정보 중 **price와 quantity를 음수**로 바꾼다.  
![proxy intercept](/assets/img/2022-03-27-1505189-Business-Logic-Flaw-in-the-subscription-of-the-Dragon-app/proxy-intercept.png){: .center}  
  
4. Forward 해주면 결제 성공 페이지가 뜨고 앱을 다운로드 받을 수 있다.  
![payment successful](/assets/img/2022-03-27-1505189-Business-Logic-Flaw-in-the-subscription-of-the-Dragon-app/payment-successful.png){: .center}   
  
  
### 관리자 답변
확인할 수 있는 문제이지만 심각도는 낮다.  
앱을 다운로드 받는다고 사용할 수 있는 것도 아니고, 다운로드 경로는 누구에게나 열려있다.  
**구독 코드**가 중요한데, 이는 백엔드에서 지불이 수신되지 않았기에 제공되지 않는다.  
  
![normal](/assets/img/2022-03-27-1505189-Business-Logic-Flaw-in-the-subscription-of-the-Dragon-app/shot.png){: .center}  
(정상)  
  
![no code](/assets/img/2022-03-27-1505189-Business-Logic-Flaw-in-the-subscription-of-the-Dragon-app/no.png){: .center}  
(구독 코드 제공 안됨)
  
그래서 이 문제 때문에 경제적으로 타격을 입지는 않는다.  
하지만 이는 여전히 문제이긴 하기 때문에 bounty를 지급하겠다.  
  
### 현재 상태
지금은 Burp에서 quantity 값을 음수로 변경하지 못하고, 가격은 변경한다고 해도 quantity를 기반으로 결정되기 때문에 제 값이 나온다.  
![screenshot](/assets/img/2022-03-27-1505189-Business-Logic-Flaw-in-the-subscription-of-the-Dragon-app/1.png){: .center}  
위 사진은 quantity 값을 음수로 변경하려 했을 때 응답이다.


### 생각해볼 점
이에 Bounty로 $250이 지급되었다.  
전 페이지에서 넘어온 값을 그대로 사용하지 않고 **필터링**을 거치면 예상하지 못한 값이 들어왔을 때도 잘 처리될 수 있다.  
  
참고
<https://hackerone.com/reports/1505189>  
원본 링크