robots.txt

robots.txt는 검색로봇에게 사이트 및 웹페이지를 수집할 수 있도록 허용하거나 제한하는 [국제 권고안](https://www.ietf.org/id/draft-koster-rep-00.txt)입니다. robots.txt 파일은 항상 사이트의 루트 디렉터리에 위치해야 하며 [로봇 배제 표준](https://ko.wikipedia.org/wiki/%EB%A1%9C%EB%B4%87_%EB%B0%B0%EC%A0%9C_%ED%91%9C%EC%A4%80)을 따르는 일반 텍스트 파일로 작성해야 합니다. 네이버 검색로봇은 robots.txt에 작성된 규칙을 준수하며, 만약 사이트의 루트 디렉터리에 robots.txt 파일이 없다면 모든 콘텐츠를 수집할 수 있도록 간주합니다.

간혹 특정 목적을 위하여 개발된 웹 스크랩퍼를 포함하여 일부 불완전한 검색로봇은 robots.txt 내의 규칙을 준수하지 않을 수 있습니다. 그러므로 개인 정보를 포함하여 외부에 노출되면 안 되는 콘텐츠의 경우 로그인 기능을 통하여 보호하거나 다른 차단 방법을 사용해야 합니다.

쇼핑 robots.txt

[https://search.shopping.naver.com/robots.txt](https://search.shopping.naver.com/robots.txt)

```
User-agent: * # 모든 로봇(robot)들에 적용합니다
Disallow: / # 모든 페이지들의 색인(indexing)을 금지합니다

User-agent: Yeti  # 접근을 허용하지 않을 로봇 이름을 설정합니다
Disallow: /best/api
Disallow: /best/graphql
Allow: /best/
Disallow: /
```

**Disallow:** # 허용하지 않을 항목에 대해 설정합니다

단 "Disallow"를 빈 값으로 설정할 경우, 모든 하위 경로에 대한 접근이 가능합니다
그리고 robots.txt 파일에는 최소한 한 개의 "Disallow" 필드가 존재해야만 합니다

```
Disallow: /help   # /help.html 과 /help/index.html 둘 다 허용 안됩니다
Disallow: /help/  # /help/index.html는 허용 안하나, /hepl.html은 허용됩니다
```

만약 모든 로봇에게 문서 접근을 허락하려면, robots.txt에 다음과 같이 입력하면 된다.

```
User-agent: *
Allow: /
```

모든 로봇을 차단하려면, robots.txt에 다음과 같이 입력하면 된다.

```
User-agent: *
Disallow: /
```

모든 로봇에 세 디렉터리 접근을 차단하려면, robots.txt에 다음과 같이 입력하면 된다.

```
User-agent: *
Disallow: /cgi-bin/
Disallow: /tmp/
Disallow: /junk/

```

모든 로봇에 특정 파일 접근을 차단하려면, robots.txt에 다음과 같이 입력하면 된다.

```
User-agent: *
Disallow: /directory/file.html
```

BadBot 로봇에 모든 파일 접근을 차단하려면, robots.txt에 다음과 같이 입력하면 된다.

```
User-agent: BadBot
Disallow: /
```

BadBot 과 Googlebot 로봇에 특정 디렉터리 접근을 차단하려면, robots.txt에 다음과 같이 입력하면 된다.

```
User-agent: BadBot
User-agent: Googlebot
Disallow: /private/
```

다양하게 조합하여 사용할 수 있다.

```
User-agent: googlebot        # googlebot 로봇만 적용
Disallow: /private/          # 이 디렉토리를 접근 차단한다.

User-agent: googlebot-news   # googlebot-news 로봇만 적용
Disallow: /                  # 모든 디렉토리를 접근 차단한다.

User-agent: *                # 모든 로봇 적용
Disallow: /something/        # 이 디렉토리를 접근 차단한다.
```
