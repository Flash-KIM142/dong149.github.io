---
title: 리그 오브 레전드(lol) riot api 를 이용해서 나만의 서비스 만들기 1탄
layout: post
part: project
categories:
- 개발
- 서비스
- 롤
- lol
- deathnote
---

오늘은 riot api를 이용해서 만든 lol service인 deathnote.gg 서비스를 소개하려합니다.


## deathnote.gg 

---
[서비스 바로가기 클릭 클릭!](https://league-of-legend-service.herokuapp.com/)
![데스노트 홈](https://github.com/dong149/image_resources/blob/master/patrick_blog/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-05-01%20%EC%98%A4%ED%9B%84%204.00.28.png?raw=true)



deathnote.gg 서비스는 제가 실제로 롤을 하다가 리폿이 성에 안차서 화가 잔뜩난 상태에서 
개발한 서비스입니다ㅋㅋㅋㅋ
실제로 리폿을 작성하긴하는데, 다른 사람들한테 저 자식이 개트롤이니까 닷지하라고 말을 하고싶은데,
그러질 못하다보니, 특정 유저를 상대로 리뷰를 남길 수 있는 서비스가 있으면 좋겠다라는 생각으로 개발을 하게 되었습니다.

개발하다보니, 단순히 리뷰만을 작성하게 하는 것은 매력이 없는 것 같았습니다. 그래서 개발하게 된 것이 트롤 분석기 기능입니다.
유저의 이름을 검색하면, 해당 유저의 이전 랭크 경기를 분석해서 100점 만점 기준으로 점수를 환산한 다음 기준에 따라 트롤 여부를
사용자에게 알려주는 기능입니다.



기회가 되면, 개발하는 과정을 초보자분들도 쉽게 따라하실 수 있게 정리해놓으려합니다!!


[deathnote.gg github 바로가기 https://github.com/dong149/lol-service](https://github.com/dong149/lol-service)
