# 목차

- [요약](#요약)
- [깃허브 이모지 API](#깃허브-이모지)

# 요약

개인적으로 기록할만한 웹 기술 관련 아이디어 및 API를 선정하고 기억하는 공간

**선정하기만 하면 발전은 없다! 공부해라!**

 - 어떤 기능을 제공하는 API인지 한 줄 설명을 작성해야 함
 - 왜 유용하다고 생각했는지, 그리고 유용하다고 생각했다면 어떨 때 또는 어디에 사용하면 좋을지도 작성할 것
 - 어떻게 사용해야 하는지 상세 스펙을 학습해서 작성할 것
 - 만약 별도의 documentation 페이지가 존재한다면, 정확한 명세를 기억할 수 있도록 링크를 남길 것
 - API에 대한 의문점이 있다면 그냥 넘어가지 말고 반드시 첨언하고 팩트체크를 할 것


# 깃허브 이모지

> 깃허브에서 호스팅하는 이모지의 "Key": "\*.png" URL **전체 쌍 목록**을 알려줌

**REQUEST:**

https://api.github.com/emojis

**RESPONSE:**
```json
{
    "+1": "https://github.githubassets.com/images/icons/emoji/unicode/1f44d.png?v8",
    "-1": "https://github.githubassets.com/images/icons/emoji/unicode/1f44e.png?v8",
    "100": "https://github.githubassets.com/images/icons/emoji/unicode/1f4af.png?v8",
    중략(정말...많다)
    "zombie_woman": "https://github.githubassets.com/images/icons/emoji/unicode/1f9df-2640.png?v8",
    "zzz": "https://github.githubassets.com/images/icons/emoji/unicode/1f4a4.png?v8"
}
```

**의문점:**
 - a부터 zzz까지 수백 개가 넘는 이모지 URL 쌍이 날아오는데 서버에 부담이 없을 리가 없는데. 별도의 정책이 있나?
 > 알아본 결과, 깃허브의 rate_limit 정책을 적용받고 있다. [GitHub REST API Rate Limit - 레퍼런스](https://docs.github.com/en/rest/overview/resources-in-the-rest-api#rate-limiting)
 - 그럼 남은 횟수는 어떻게 확인할 수 있나?
 > [Postman](https://www.postman.com/)을 사용하여 레퍼런스에서 알려준 URL로 Request(https://api.github.com/rate_limit)를 날려 보니 아래와 같이 응답이 날아왔다.
```
{
    "resources": {
        "core": {
            "limit": 60,
            "remaining": 55,
            "reset": 1625703387,
            "used": 5,
            "resource": "core"
        },
        "graphql": {
            "limit": 0,
            "remaining": 0,
            "reset": 1625705305,
            "used": 0,
            "resource": "graphql"
        },
        "integration_manifest": {
            "limit": 5000,
            "remaining": 5000,
            "reset": 1625705305,
            "used": 0,
            "resource": "integration_manifest"
        },
        "search": {
            "limit": 10,
            "remaining": 10,
            "reset": 1625701765,
            "used": 0,
            "resource": "search"
        }
    },
    "rate": {
        "limit": 60,
        "remaining": 55,
        "reset": 1625703387,
        "used": 5,
        "resource": "core"
    }
}
```
 - 이모지 목록 요청을 다섯 번 날렸는데, resources.core remaining이 55다. 레퍼런스에 따르면 Unauthorized 접근에 대해서는 시간 당 60회라는 제한이 있단다.
 > 단, 목록을 받아오는게 아닌. 이모지 이미지 리소스에 접근하는 것은 카운트가 감소하지 않았다. 즉, 처음 앱이나 페이지를 로드할 때 한번 호출하고 변수든 어디든 통으로 킵해놨다가 이미지 리소스에 접근하는 방식이 가장 현명하겠다.
