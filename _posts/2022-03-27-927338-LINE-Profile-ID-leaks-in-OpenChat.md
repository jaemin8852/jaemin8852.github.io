---
title:  "LINE Profile ID leaks in OpenChat - $3,000 리뷰"
layout: single
date: 2022-03-27T18:30:00+09:00
last_modified_at: 2022-03-27
category: Hacking
---

<https://hackerone.com/reports/927338>  
원본 링크  

# LINE Profile ID leaks in OpenChat 
  
### 요약
```Users can participate in OpenChat using a new OpenChat profile that is distinct from the LINE profile. However, when the victim attaches an image in a post in OpenChat's Note, the ID of LINE Profile was stored together in the image's metadata. From this information, it is possible to determine the LINE user profile of OpenChat participants.```
  
유저들은 라인 프로필과 다른 새로운 오픈챗 프로필로 오픈챗을 이용할 수 있다. 하지만 피해자가 이미지를 올리면 기존 라인 프로필의 ID까지 이미지의 메타데이터에 저장되었다. 이에 따라 오픈챗 참가자의 라인 프로필을 확인할 수 있다.  
  
이는 LINE의 object storage service가 공통된 특징을 가지고 있었기 때문인데, 그로 인해 해커가 익명성이 보장되는 오픈챗에 적용해보고 익명성이 깨진다는 것을 알아낼 수 있었다.  
  
### 생각해볼 점
이에 Bounty로 $3,000이 지급되었다.  
새로운 프로필을 만들고 익명성을 보장받는 오픈챗이었기 때문에 이미지를 통해서 user의 기존 라인 프로필 정보를 가져오는 건 큰 문제가 될 수 있다. 그렇기 때문에 심각성을 High(7~8.9)로 평가하고 큰 바운티를 지급한 것을 볼 수 있다.  
기존에 사용하던 object storage service를 그대로 가져와서 사용했기 때문에 일어난 일인데, 하나하나 다 체크하기가 쉬운 일은 아닐 것 같다.