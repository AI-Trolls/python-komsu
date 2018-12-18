## 파이썬을 이용한 웹 크롤러(crawler) 개발!

웹 크롤링이란 웹 사이트들에서 원하는 정보를 추출하는 것입니다. 
단순히 한 페이지의 정보를 가져올 수도 있지만, 보통은 그 페이지 내의 Url 링크들을 모두 저장해두고, 방문하며 계속 반복하는-
지정된 도메인이나 위치/범위 내에서 정보를 긁어 모읍니다!
다양한 콘텐츠들이 다양한 형식으로 갖추어져 있는데, 그런 정보들을 쓸만하게 잘 파싱하는 것도 일이겠죠.

[awesome-crawler](https://github.com/BruceDone/awesome-crawler#python)
에서 유용해 보이는 3가지 크롤러를 사용해보고 간단한 사용법을 공유해보고자 합니다.

우선 몇 가지 크롤링 관련 용어를 정리하고 읽으시면 좋을 것 같습니다.
- world wide web의 개념에서 크롤링하는 녀석을 crawler, spider, bot 등으로 불립니다.
- scrape는 crawl의 정식 명칭입니다! 긁다, 긁어 모으다라는 뜻입니다. crawl는 사실 기어가다 라는 뜻이죠.

## 1. [Scrapy](https://github.com/scrapy/scrapy)

가장 유명한! 깃 커밋이나 컨트리뷰터만 봐도 가장 영향력있는 python crawler로 볼 수 있습니다.

Scrapy는 빠른 웹 크롤링 및 웹 스크래핑 프레임 워크로서 웹 사이트를 크롤링하고 해당 페이지에서 구조화된 데이터를 추출하는데 사용됩니다.
데이터 마이닝부터 모니터링 및 테스트에 이르기까지 광범위한 용도로 사용할 수 있습니다.

### 설치
```bash
pip install scarpy
```

### 사용 방법

1. Scrapy 프로젝트 만들기
2. 사이트를 크롤링하고 데이터를 추출하는 스파이더 작성
3. 명령 줄을 사용하여 스크랩 된 데이터 내보내기
4. 재귀 적으로 링크를 따라 가기 위해 스파이더 변경하기
5. 스파이더 인수 사용하기

### 예제

## 2. [Pyspider](https://github.com/binux/pyspider)

스크립트 편집기, 작업 모니터, 프로젝트 관리자 및 결과 뷰어로 강력한 WebUI를 갖고 있습니다.
MySQL, MongoDB, Redis, SQLite, Elasticsearch, SQLAlchemy와 PostgreSQ를 데이터베이스 백엔드로 가능하며,
메시지 큐로 RabbitMQ, Beanstalk, Redis 및 Kombu를 사용할 수 있고,
작업 우선 순위, 재시도, 정기적, 시기별 재 크롤링 등 다양한 스케쥴링 기능도 가지고 있습니다.

### 설치

### 사용 방법

웹으로부터 정보를 scrape 하는 방법은
1. 우리가 원하는 정보를 포함하고 있는 페이지의 URL들을 찾는다.
2. HTTP를 통해 페이지 가져 온다.
3. HTML에서 정보 추출한다.
4. 원하는 것을 포함하고있는 URL들을 더 많이 찾기 위해 2로 돌아간다.

## 3. [newspaper](https://github.com/codelucas/newspaper)

Python 3의 뉴스, 전체 텍스트 및 기사 메타 데이터 추출
