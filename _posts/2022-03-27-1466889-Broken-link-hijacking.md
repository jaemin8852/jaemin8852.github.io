---
title:  "Broken link hijacking in kubernetes - $100 리뷰"
layout: single
date: 2022-03-27T10:00:00+09:00
last_modified_at: 2022-03-27
category: Hacking
---

<https://hackerone.com/reports/1466889>
원본 링크

# Broken link hijacking
### 요약
웹사이트가 외부 서비스에 대한 pages, sources, links를 제공했는데 사용할 수 없게 된 경우, 공격자가 이를 탈취해서 웹사이트를 가장하여 악성행위를 할 수 있다.

### 재현
1. https://kubernetes-csi.github.io/docs/drivers.html?highlight=chubaofs#production-drivers에 방문한다.
2. ChubaoFS를 검색하고 아래 링크를 클릭한다.  
![find chubao](/assets/img/2022-03-27-1466889-Broken-link-hijacking/1.jpg){: .center}  
![link](/assets/img/2022-03-27-1466889-Broken-link-hijacking/2.png){: .center}  
 연결된 링크는 위와 같음
3.  들어가면 404 page가 뜬다.  
![404 page](/assets/img/2022-03-27-1466889-Broken-link-hijacking/3.jpg){: .center}  
두 번째 사진에 있는 링크를 보고 github의 username과 repository를 알아낼 수 있고, username을 chubaofs로 바꿀 수 있다면 위의 링크를 가로챌 수 있다.
4. username을 chubaofs로 변경하고, chubaofs-csi라는 repository를 만든다.  
![change username](/assets/img/2022-03-27-1466889-Broken-link-hijacking/4.jpg){: .center}  
![make repository](/assets/img/2022-03-27-1466889-Broken-link-hijacking/5.jpg){: .center}  
5. 이제 Chubao 링크를 타고 들어오면 공격자의 깃허브 repository로 이동하게 된다. 공격자는 악성 프로그램을 사용자의 PC에 심을 수 있게 된다.
  
### 생각해볼 점
이에 Bounty로 $100이 지급되었다.  
사이트 내에 링크가 걸려져 있을 경우, 특히나 github와 같이 사용자가 링크의 구조를 변경할 수 있는 서비스에 링크했을 경우에는 이에 대한 관리가 필요하다.  
사이트 사용자는 사이트를 믿고 쉽게 설치할 수 있기 때문에 위험할 수 있다.