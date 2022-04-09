---
title:  "Subdomain Takeover on fddkim.zomato.com - $350 리뷰"
layout: single
date: 2022-03-27T17:20:00+09:00
last_modified_at: 2022-03-27
category: Hacking
---


# Subdomain Takeover 
Subdomain Takeover는 간단히 말해서 fddkim.zomato.com 같은 zomato.com의 하위 도메인을 탈취하는 것이다.  
원래 fddkim.zomato.com이 어떤 유효한 페이지를 가리키고 있었는데, 가리키는 건 유지된 상태로 페이지가 사라졌을 때 **발생할 수 있다.**  
**사라진 페이지의 주소**와 같은 주소로 내 페이지를 배포할 수 있을 때, 저 하위 도메인은 내 페이지를 가리키게 되고 이를 STO(Subdomain Takeover)라고 한다.   
가리키는 게 비어있는 subdomain을 찾는 건 어렵지 않지만 실제로 저걸 takeover하기는 쉽지 않다.  

takeover에 성공한다면 서브 도메인에서 그대로 내 페이지가 열리기 때문에 [외부 도메인으로 연결되는 링크를 하이재킹](https://jaemin8852.github.io/hacking/1466889-Broken-link-hijacking/)하는 것과는 다르다고 볼 수 있다. (더 위험함)  
  
아래에서는 해커가 작성한 writeup을 요약 및 리뷰하도록 하겠다.
  
### 요약
보통 subdomain을 찾을 때, 대량의 subdomain을 검색해서 응답이 이상한 것들을 찾아내야 하기 때문에 프로그램을 직접 만들거나 github에 있는 코드를 이용한다.  
또한 해커는 subdomain이 takeover까지 이어질 수 있는지 빠르게 판단하기 위해서 <https://github.com/EdOverflow/can-i-take-over-xyz> 이런 곳에서 미리 찾아본다. writeup 작성한 해커도 평소에는 그랬는데 이번 프로젝트를 진행할 땐 왠지 모르게 들어가지 않았었다고 한다.  
  
해커가 찾은 subdomain은 freshdesk에서 호스팅되는 페이지를 가리켰고, 이는 다음과 같이 이상한 응답을 표시했다.  
![response](/assets/img/2022-03-27-1130376-subdomain-takeover-on-fddkim.zomato.com/1.png){: .center}  
  
이 사이트(freshdesk)는 위에 적어둔 링크인 https://github.com/EdOverflow/can-i-take-over-xyz에서 취약하지 않다고 나와있었지만, 해커는 이를 보지 않고 무작정 가입 후 takeover를 시도했다.  
  
![response](/assets/img/2022-03-27-1130376-subdomain-takeover-on-fddkim.zomato.com/2.png){: .center}  
  
이처럼 실제 도메인 소유자인지 확인하기 때문에 취약하지 않다고 되어있었을 것이다.  
하지만 해커는 요청과 응답을 조정하면서 계속해서 시도했고 결국 takeover까지 성공한다.  
![response](/assets/img/2022-03-27-1130376-subdomain-takeover-on-fddkim.zomato.com/3.png){: .center}  
  
이렇게 취약하지 않다고 생각했던 곳이 뚫린다면, 이를 사용하는 **발생할 수 있던** 모든 곳이 취약해진다. 해커는 이런 zero-day 취약점을 다량 제보하여 총 $5,000을 Bounty로 받게 되었다.  
  
### 생각해볼 점
물론 이 사례에서 운도 따랐겠지만(not vulnerable 하다는 것을 못 보고 시도할 수 있었던), 계속해서 취약한 subdomain들을 탐색하고 요청을 조작하고 응답을 살펴보면서 우회 방법을 찾았기에 성공할 수 있던 값진 노력의 결과라고 생각한다.  
  
참고  
<https://hackerone.com/reports/1130376>  
원본 링크  
https://medium.com/@moSec/how-i-hacked-thousand-of-subdomains-6aa43b92282c  
해커가 작성한 writeup 원문