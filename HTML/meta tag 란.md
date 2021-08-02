# meta tag 란?

웹 페이지의 보이지 않는 정보를 제공.

페이지에 대한 설명, 제작자, 크롤링 정책 등.

SEO는 search engine optimization으로 ,meta tag를 이용해 description, keywords, author, subject, classification등의 정보를 표기할 수 있으며, 검색엔진은 이런 정보를 적극적으로 활용한다.

name은 메타요소가 어떤 정보의 형태를 가지고 있는지 알려주고 content는 실제 메타데이터의 컨텐츠이다.

```html
<!DOCTYPE html>
<html lang="ko">
    lang속성 지정해야하는 이유(스크린리더가 지정된 언어를 인지해서 읽음)
    만약 lang속성이en이고 중간에 한글내용이 있다치면 span lang="ko"로 문자열태그 처리하면 한글로 읽어준다.
  <head>
    <meta charset="utf-8"> / 인코딩 지정집합 UTF-8(한국어 표현가능 유니코드 형식의 하나)
    <meta name="viewport" content="width=device-width, initial-scale=1">
      /내 화면에 보이는 여역. 스크롤로 봐야할 부분은 제외.기기의 넓이 화면배율1
    <meta name="keywords" content="apple banna">
      /해당 컨텐츠의 키워드. 구글을 제외한 검색엔진을 위함.(구글은 반영안해줌)
    <meta name="desciption" content="나의 블로그">
      /해당 페이지에 대한 설명. 검색했을 시 제목 아래 노출되는 부분.
    <meta name="generator" content="">
      /이 웹페이지를 무엇으로 만들었는지 기술하는 태그 다른사람이 코드를 알아볼때 참고할 수 있도록 하는 면이 있는데 요즘은 번들러를 통해 코드가 변환되는 경우가 있기에 기술할 필요성이 떨어졌다. 웹표준이나 SEO측면 불필요한 메타태그.
    <meta name="robot" content="noindex nofollow">
      / 인덱싱을 제어하는데 사용.(검색엔진 크롤러의 허용, 불허용)하지만 보통 robots 메타태그보다 robots.txt파일로 제어하는게 일반적이다. 사용가능한 content는 index, noindex, follow, nofollow가 있다. 앞의두개는 페이지 인덱스 여부를 말해주고 뒤에 두개는 페이지 링크를 크롤러가 따라갈 수 있는지 여부를 말해준다. no붙으면 불허한다고 보면 된다.
    <meta http-equiv="refresh" content="0; url=https://example.com/">
     / 지정한 시간 이후 이정한 URL으로 자동으로 페이지를 이동시킬 수 있다. content에 초 단위로 시간을 지정하고 0일 경우에는 즉시 이동한다. 하지만 해당 동작은 권장되지 않는다. 자동으로 URL 리다이렉션은 최근 웹에서는 지양하고 있으며 피싱 등의 위험으로 간주 될 수 있다. 또한 5초로 리다이렉트를 지정해놓았는데 그 전에 사용자가 뒤로가기를 하는 경우에 뒤로가기를 했는데도 5초 후 지정한 URL로 이동될 수 있으니 사용하지 않는 것이 좋다.
    <meta property="og:type" content="website">
    <meta property="og:title" content="페이지 제목">
    <meta property="og:description" content="페이지 설명">
    <meta property="og:image" content="http://www.mysite.com/myimage.jpg">
    <meta property="og:url" content="http://www.mysite.com">
      / 오픈그래프는 웹 페이지가 소셜 미디어 또는 오픈그래프를 활용한 사이트로 공유될때 사용되는정보이다.
      예를들어 페이스북에 링크 붙여놓기, 카카오톡 링크공유하기시 해당 게시물의 제목, 설명, 이미지가 간략하게 나타나는 기능을 제어하는태그이다. // 페이스북 개발.
      https://ogp.me/ 관련링크
```

> 검색엔진이 검색 키워드에 대한 검색결과를 사용자한테 제공하기 위해서는 크롤러가 끊임없이 온라인 상의 문서를 수집해야하고, 크롤러가 수집한 문서를 인덱서가 잘 정리해서 인덱스 서버에 색인해 둬야 한다.
> 크롤러는 사이트와 사이트 사이의 링크를 타고다니며 문서를 수집하는 역할만 하며, 인덱서는 수집된 문서를 검색엔진이 사용자에게 결과물을 빠르고 쉽게 재공할 수 있도록 색인 또는 인덱스 역할을 한다.

https://euncho.medium.com/google-%EA%B2%80%EC%83%89%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EA%B0%80-fbc0e421a97a

검색엔진은 어떻게 동작하는가?(구글 FE엔지니어 조은씨.)





https://webclub.tistory.com/354

추가 메타태그 정리. 재희님의 블로그.

