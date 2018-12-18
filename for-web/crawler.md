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
pip install scrapy
```

### 사용 방법

1. Scrapy 프로젝트 만들기
2. 사이트를 크롤링하고 데이터를 추출하는 스파이더 작성
3. 명령 줄을 사용하여 스크랩 된 데이터 내보내기
4. 재귀 적으로 링크를 따라 가기 위해 스파이더 변경하기
5. 스파이더 인수 사용하기

크롤링은 start_urls 속성에 정의 된 URL을 요청하고,
기본 콜백 메소드 parse를 호출하여 응답 객체를 인수로 전달하여 시작됩니다.
구문 분석 콜백에서 CSS 선택기를 사용하여 인용 요소를 반복하고,
추출 된 인용 텍스트와 작성자로 파이썬 사전을 만들고,
다음 페이지에 대한 링크를 찾고 콜백과 동일한 구문 분석 메서드를 사용하여 다른 요청을 예약합니다.

### 예제 1. 일단 뭔가 실행해보기

명언이 적힌 사이트에서 정보를 긁어와봅시다!

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"
    start_urls = [
        'http://quotes.toscrape.com/tag/humor/',
    ]

    def parse(self, response):
        for quote in response.css('div.quote'):
            yield {
                'text': quote.css('span.text::text').extract_first(),
                'author': quote.xpath('span/small/text()').extract_first(),
            }

        next_page = response.css('li.next a::attr("href")').extract_first()
        if next_page is not None:
            yield response.follow(next_page, self.parse)
```

위와 같은 코드로 quotes_spider.py 파일을 생성하고, 아래 명령어로 실행합니다.

```bash
scrapy runspider quotes_spider.py -o quotes.json
```

그러면 http://quotes.toscrape.com/tag/humor/의 정보가 quotes.json에 저장된 것을 볼 수 있습니다.

### 예제 2. Scrapy Tutorial

이제는 scrapy 프로젝트를 생성하고 실행해봅시다.

```bash
scrapy startproject tutorial
```

위의 명령어로 tutorial이라는 프로젝트가 생성됩니다. 그 안에는 다양한 파일들이 있는데, 먼저 spiders 폴더로 들어가 quotes_spider.py 파일을 생성하고 아래의 코드를 작성합니다.

```python
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"

    def start_requests(self):
        urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/',
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse)

    def parse(self, response):
        page = response.url.split("/")[-2]
        filename = 'quotes-%s.html' % page
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log('Saved file %s' % filename)
```

보시다시피, QuotesSpider 클래스는 몇 가지 특성과 메서드를 정의합니다.
- name : 스파이더를 식별합니다. 프로젝트 내에서 고유해야합니다. 즉, 다른 스파이더에 대해 동일한 이름을 설정할 수 없습니다.
- start_requests () : Spider가 크롤링을 시작하는 요청의 반복 가능 (요청 목록을 반환하거나 생성기 함수를 작성할 수 있음)을 반환해야합니다. 후속 요청은 이러한 초기 요청에서 연속적으로 생성됩니다.
- parse () : 각각의 요청에 대해 다운로드 된 응답을 처리하기 위해 호출 될 메소드. response 매개 변수는 페이지 내용을 보유하고 처리하는 데 도움이되는 메소드가있는 TextResponse의 인스턴스입니다.
    parse () 메서드는 대개 응답을 파싱하고 crawl한 데이터를 dicts로 추출하고 따라가야할 새 URL을 찾고 새 요청 (Request)을 만듭니다.

그럼 이제 위의 코드를 실행해봅시다!
동일한 폴더(tutorial/spiders)에서 아래의 명령어를 실행하면 됩니다!

```bash
scrapy crawl quotes
```
그 결과 quotes-1.html, quotes-2.html이 생성된 것을 보실 수 있습니다.

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


reference
1. [awesome-crawler](https://github.com/BruceDone/awesome-crawler)
2. [Scrapy documentation](https://doc.scrapy.org/en/latest/intro/overview.html)
