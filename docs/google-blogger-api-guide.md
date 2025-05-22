# Blogger API 가이드: API 사용

Blogger API를 사용하면 REST API를 통해 Blogger 콘텐츠를 애플리케이션과 통합할 수 있습니다. 시작하기 전에 승인을 설정해야 합니다.

## 1. 소개

이 문서는 Blogger API와 상호작용할 수 있는 애플리케이션을 작성하려는 개발자를 위해 작성되었습니다. Blogger는 지속적으로 의견을 게시할 수 있는 웹사이트를 만드는 도구입니다.

Blogger 개념에 익숙하지 않은 경우, 코딩을 시작하기 전에 [Blogger 시작하기](https://support.google.com/blogger/answer/1623800) 문서를 읽어보는 것이 좋습니다.

## 2. 요청 승인 및 애플리케이션 식별

애플리케이션에서 Blogger API로 전송하는 모든 요청은 Google에 애플리케이션을 식별해야 합니다. 애플리케이션을 식별하는 방법에는 두 가지가 있습니다:

1.  **OAuth 2.0 토큰 사용:** 이 토큰은 요청을 승인하는 역할도 합니다.
2.  **애플리케이션의 API 키 사용:**

사용할 옵션은 다음과 같이 결정합니다:

*   **승인이 필요한 요청 (예: 개인의 비공개 데이터 요청):**
    *   애플리케이션은 요청과 함께 OAuth 2.0 토큰을 **반드시** 제공해야 합니다.
    *   API 키도 함께 제공할 수 있지만 필수는 아닙니다.
*   **승인이 필요 없는 요청 (예: 공개 데이터 요청):**
    *   애플리케이션은 API 키 또는 OAuth 2.0 토큰 중 하나 또는 둘 다를 편의에 따라 제공해야 합니다.

### 2.1. 승인 프로토콜 정보

요청을 승인하려면 애플리케이션에서 **OAuth 2.0**을 사용해야 합니다. 다른 승인 프로토콜은 지원되지 않습니다. 애플리케이션에서 "Google 계정으로 로그인" 기능을 사용하는 경우, 승인의 일부 절차는 자동으로 처리됩니다.

### 2.2. OAuth 2.0을 사용하여 요청 승인하기

Blogger API에 대한 비공개 사용자 데이터 요청은 인증된 사용자의 승인을 받아야 합니다. 이 프로세스는 OAuth 클라이언트 ID를 사용하여 진행됩니다.

#### OAuth 클라이언트 ID 가져오기

[Google Cloud Console 사용자 인증 정보 페이지](https://console.cloud.google.com/apis/credentials)에서 OAuth 클라이언트 ID를 생성하거나 기존 ID를 사용합니다.

#### OAuth 2.0 승인 절차 (흐름)

OAuth 2.0의 세부적인 승인 절차는 개발 중인 애플리케이션 종류에 따라 약간씩 다릅니다. 다음은 모든 애플리케이션 유형에 적용되는 일반적인 과정입니다:

1.  애플리케이션이 사용자 데이터에 액세스해야 하는 경우, Google에 특정 액세스 **범위(Scope)**를 요청합니다.
2.  Google은 사용자에게 "애플리케이션이 일부 데이터를 요청하도록 승인할 것인지" 묻는 **동의 화면**을 표시합니다.
3.  사용자가 승인하면, Google은 애플리케이션에 **제한 시간이 있는 액세스 토큰**을 제공합니다.
4.  애플리케이션은 이 **액세스 토큰을 첨부**하여 사용자 데이터를 요청합니다.
5.  Google이 요청과 토큰이 유효하다고 판단하면, 요청된 데이터를 반환합니다.

일부 흐름에는 새로운 액세스 토큰을 얻기 위해 **갱신 토큰(Refresh Token)**을 사용하는 등의 추가 단계가 포함될 수 있습니다. 다양한 유형의 애플리케이션에 적용되는 흐름을 자세히 알아보려면 [Google의 OAuth 2.0 문서](https://developers.google.com/identity/protocols/oauth2)를 참조하세요.

#### Blogger API OAuth 2.0 범위

*   `https://www.googleapis.com/auth/blogger`: Blogger 계정 관리

OAuth 2.0을 사용하여 액세스를 요청하려면, 애플리케이션에 위 범위 정보와 함께 애플리케이션 등록 시 Google에서 제공하는 정보(예: 클라이언트 ID, 클라이언트 보안 비밀)가 필요합니다.

> **팁:** Google API 클라이언트 라이브러리는 사용자를 대신하여 일부 승인 과정을 처리할 수 있습니다. 여러 프로그래밍 언어로 제공되므로, 자세한 내용은 [라이브러리 및 샘플 페이지](https://developers.google.com/blogger/docs/3.0/libraries)를 참조하세요.

### 2.3. API 키 획득 및 사용

Blogger API에 대한 공개 데이터 요청에는 식별자(API 키 또는 액세스 토큰)가 수반되어야 합니다.

#### API 키 가져오기

[Google Cloud Console 사용자 인증 정보 페이지](https://console.cloud.google.com/apis/credentials)에서 API 키를 생성하거나 기존 키를 사용합니다.

API 키가 있으면, 애플리케이션은 모든 요청 URL에 쿼리 매개변수 `key=YOUR_API_KEY`를 추가할 수 있습니다. API 키는 URL에 안전하게 포함될 수 있으며, 별도의 인코딩이 필요하지 않습니다.

## 3. 블로그 관리하기

### 3.1. 블로그 검색 (ID 사용)

특정 블로그의 정보를 검색하려면 해당 블로그 URI에 HTTP GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/{blogId}?key=YOUR_API_KEY
    ```
*   **예시 요청 (공개 블로그):**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/2399953?key=YOUR_API_KEY
    ```
    *   공개 블로그 검색 시에는 사용자 인증(Authorization 헤더)이 필요 없지만, API 키는 제공해야 합니다.
    *   비공개 블로그는 인증이 필요합니다.

*   **성공 응답 (HTTP 200 OK):**
    ```json
    {
      "kind": "blogger#blog",
      "id": "2399953",
      "name": "Blogger Buzz",
      "description": "The Official Buzz from Blogger at Google",
      "published": "2007-04-23T22:17:29.261Z",
      "updated": "2011-08-02T06:01:15.941Z",
      "url": "http://buzz.blogger.com/",
      "selfLink": "https://www.googleapis.com/blogger/v3/blogs/2399953",
      "posts": {
        "totalItems": 494,
        "selfLink": "https://www.googleapis.com/blogger/v3/blogs/2399953/posts"
      },
      "pages": {
        "totalItems": 2,
        "selfLink": "https://www.googleapis.com/blogger/v3/blogs/2399953/pages"
      },
      "locale": {
        "language": "en",
        "country": "",
        "variant": ""
      }
    }
    ```

### 3.2. 블로그 검색 (URL 사용)

URL을 사용하여 블로그를 검색하려면 `byurl` 엔드포인트에 HTTP GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/byurl?url={blog-url}&key=YOUR_API_KEY
    ```
*   **예시 요청:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/byurl?url=http://code.blogger.com/&key=YOUR_API_KEY
    ```
*   **성공 응답 (HTTP 200 OK):**
    ```json
    {
     "kind": "blogger#blog",
     "id": "3213900",
     "name": "Blogger Developers Network",
     // ... (나머지 필드는 위와 유사)
    }
    ```

### 3.3. 사용자 블로그 목록 검색

특정 사용자의 블로그 목록을 검색하려면 다음 URI에 HTTP GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/users/{userId}/blogs
    ```
    *   `userId`는 사용자의 ID이거나, 현재 인증된 사용자를 지칭하는 `self`일 수 있습니다.
*   **예시 요청 (인증 필요):**
    ```
    GET https://www.googleapis.com/blogger/v3/users/self/blogs
    Authorization: Bearer YOUR_OAUTH2_TOKEN
    ```
    *   **참고:** 사용자가 자신의 블로그를 나열하려면 인증을 받아야 하므로, `Authorization` HTTP 헤더를 GET 요청과 함께 제공해야 합니다.

*   **성공 응답 (HTTP 200 OK):**
    ```json
    {
      "kind": "blogger#blogList",
      "items": [
        {
          "kind": "blogger#blog",
          "id": "4967929378133675647",
          "name": "Brett's Test Blawg",
          // ... (나머지 필드는 위와 유사)
        }
      ]
    }
    ```

## 4. 게시물 작업

### 4.1. 블로그에서 게시물 목록 검색

특정 블로그의 게시물 목록을 검색하려면 다음 URI에 GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts?key=YOUR_API_KEY
    ```
*   **예시 요청 (공개 블로그):**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/2399953/posts?key=YOUR_API_KEY
    ```
    *   공개 블로그 검색 시 인증은 필요 없지만 API 키는 제공해야 합니다. 비공개 블로그는 인증이 필요합니다.

*   **성공 응답 (HTTP 200 OK):**
    ```json
    {
      "kind": "blogger#postList",
      "nextPageToken": "CgkIChiAkceVjiYQ0b2SAQ",
      "items": [
        {
          "kind": "blogger#post",
          "id": "7706273476706534553",
          "blog": { "id": "2399953" },
          "published": "2011-08-01T19:58:00.000Z",
          "updated": "2011-08-01T19:58:51.947Z",
          "url": "http://buzz.blogger.com/2011/08/latest-updates-august-1st.html",
          "selfLink": "https://www.googleapis.com/blogger/v3/blogs/2399953/posts/7706273476706534553",
          "title": "Latest updates, August 1st",
          "content": "elided for readability",
          "author": { /* ... */ },
          "replies": { /* ... */ }
        },
        // ... (다른 게시물)
      ]
    }
    ```

### 4.2. 특정 게시물 검색

블로그에서 특정 게시물을 검색하려면 다음 URI에 GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/{postId}?key=YOUR_API_KEY
    ```
*   **예시 요청 (공개 블로그):**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/2399953/posts/7706273476706534553?key=YOUR_API_KEY
    ```
*   **성공 응답 (HTTP 200 OK):** 위 게시물 목록의 개별 항목과 유사한 구조.

### 4.3. 게시물 검색 (키워드)

블로그 내에서 특정 키워드로 게시물을 검색하려면 다음 URI에 GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/search?q={query_terms}&key=YOUR_API_KEY
    ```
*   **예시 요청 (공개 블로그):**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/3213900/posts/search?q=documentation&key=YOUR-API_KEY
    ```
*   **성공 응답 (HTTP 200 OK):** 게시물 목록 형식 (`blogger#postList`).

### 4.4. 게시물 추가 (인증 필요)

블로그에 새 게시물을 추가하려면 다음 URI에 POST 요청을 전송합니다.

*   **요청 형식:**
    ```
    POST https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/
    Authorization: Bearer YOUR_OAUTH2_TOKEN
    Content-Type: application/json

    {
      "kind": "blogger#post",
      "blog": {
        "id": "{blogId}"
      },
      "title": "A new post",
      "content": "With <b>exciting</b> content..."
    }
    ```
*   **성공 응답 (HTTP 200 OK 또는 201 Created):** 생성된 게시물 정보.

### 4.5. 게시물 삭제 (인증 필요)

블로그 게시물을 삭제하려면 다음 URI에 DELETE 요청을 전송합니다.

*   **요청 형식:**
    ```
    DELETE https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/{postId}
    Authorization: Bearer YOUR_OAUTH2_TOKEN
    ```
*   **성공 응답 (HTTP 200 OK 또는 204 No Content).**

### 4.6. 경로로 게시물 검색

게시물의 경로(path)를 사용하여 검색하려면 다음 URI에 GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/bypath?path={post-path}&key=YOUR_API_KEY
    ```
*   **예시 요청 (공개 블로그):**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/2399953/posts/bypath?path=/2011/08/latest-updates-august-1st.html&key=YOUR-API-KEY
    ```
*   **성공 응답 (HTTP 200 OK):** 특정 게시물 정보.

### 4.7. 게시물 업데이트 (PUT, 인증 필요)

기존 게시물을 업데이트하려면 다음 URI에 PUT 요청을 전송합니다. PUT 요청은 전체 리소스를 대체합니다.

*   **요청 형식:**
    ```
    PUT https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/{postId}
    Authorization: Bearer YOUR_OAUTH2_TOKEN
    Content-Type: application/json

    {
     "kind": "blogger#post",
     "id": "{postId}",
     "blog": { "id": "{blogId}" },
     // ... (업데이트할 모든 필드 포함)
     "title": "An updated post",
     "content": "With really <b>exciting</b> content..."
    }
    ```
*   **성공 응답 (HTTP 200 OK):** 업데이트된 게시물 정보.

### 4.8. 게시물 부분 업데이트 (PATCH, 인증 필요)

게시물의 특정 필드만 업데이트하려면 다음 URI에 PATCH 요청을 전송합니다.

*   **요청 형식:**
    ```
    PATCH https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/{postId}
    Authorization: Bearer YOUR_OAUTH2_TOKEN
    Content-Type: application/json

    {
     "content": "With absolutely <b>fabulous</b> content..." // 변경할 필드만 포함
    }
    ```
*   **성공 응답 (HTTP 200 OK):** 업데이트된 게시물 정보.

## 5. 댓글 사용하기

### 5.1. 게시물의 댓글 목록 검색

특정 게시물의 댓글 목록을 검색하려면 다음 URI에 GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/{postId}/comments?key=YOUR_API_KEY
    ```
*   **예시 요청 (공개 블로그):**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/2399953/posts/6069922188027612413/comments?key=YOUR-API-KEY
    ```
*   **성공 응답 (HTTP 200 OK):**
    ```json
    {
      "kind": "blogger#commentList",
      "nextPageToken": "...",
      "items": [
        {
           "kind": "blogger#comment",
           "id": "9200761938824362519",
           // ... (댓글 상세 정보)
        }
      ]
    }
    ```

### 5.2. 특정 댓글 검색

게시물에서 특정 댓글을 검색하려면 다음 URI에 GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/posts/{postId}/comments/{commentId}?key=YOUR_API_KEY
    ```
*   **성공 응답 (HTTP 200 OK):** 특정 댓글 정보.

## 6. 페이지 작업

(게시물 작업과 유사하며, 엔드포인트가 `/pages`로 변경됩니다.)

### 6.1. 블로그 페이지 목록 검색
*   **요청 형식:** `GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/pages?key=YOUR_API_KEY`

### 6.2. 특정 페이지 검색
*   **요청 형식:** `GET https://www.googleapis.com/blogger/v3/blogs/{blogId}/pages/{pageId}?key=YOUR_API_KEY`

## 7. 사용자 작업

### 7.1. 사용자 정보 검색 (인증 필요)

사용자 정보를 검색하려면 다음 URI에 HTTP GET 요청을 전송합니다.

*   **요청 형식:**
    ```
    GET https://www.googleapis.com/blogger/v3/users/{userId}
    Authorization: Bearer YOUR_OAUTH2_TOKEN
    ```
    *   `userId`는 사용자의 ID이거나, 현재 인증된 사용자를 지칭하는 `self`일 수 있습니다.
*   **예시 요청:**
    ```
    GET https://www.googleapis.com/blogger/v3/users/self
    Authorization: Bearer YOUR_OAUTH2_TOKEN
    ```
*   **성공 응답 (HTTP 200 OK):**
    ```json
    {
      "kind": "blogger#user",
      "id": "901569848744",
      "selfLink": "https://www.googleapis.com/blogger/v3/users/901569848744",
      "blogs": {
        "selfLink": "https://www.googleapis.com/blogger/v3/users/901569848744/blogs"
      }
      // ... (기타 사용자 정보)
    }
    ```

## 8. 표준 쿼리 매개변수

Blogger API의 모든 메서드 및 리소스와 함께 사용할 수 있는 표준 쿼리 매개변수에 대한 자세한 내용은 [시스템 매개변수 문서](https://developers.google.com/blogger/docs/3.0/reference/standard-parameters)를 참조하세요.
