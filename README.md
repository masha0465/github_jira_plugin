# github_jira_plugin

# Jira와 GitHub 통합 플러그인 개발

이 프로젝트는 **Jira**와 **GitHub** 간의 통합을 위한 플러그인을 개발하는 방법을 설명합니다. 이 플러그인은 GitHub 이슈와 Jira 태스크를 자동으로 연동하여 팀의 협업을 더욱 효율적으로 만들어줍니다.

## 📋 개발 환경 준비

1. **Jira 개발자 계정 만들기**
   - [Jira Developer Portal](https://developer.atlassian.com/cloud/jira/platform/)에서 개발자 계정을 만들고, Jira Cloud API를 사용할 수 있는 앱을 등록합니다.
   
2. **GitHub API 설정**
   - [GitHub Developer](https://developer.github.com/) 사이트에서 **OAuth** 또는 **Personal Access Token**을 생성하여 GitHub API를 사용합니다.

## ⚙️ 필요한 라이브러리 및 도구 설치

1. **Node.js**와 **npm** 설치
   - JavaScript로 API를 호출하고 서버를 구동할 수 있도록 Node.js와 npm을 설치합니다.

2. **필수 라이브러리 설치**
   ```bash
   npm install axios @octokit/rest
🔧 Jira와 GitHub API 연동
Jira API 호출 예시

![image](https://github.com/user-attachments/assets/68126552-cfb6-4a7a-adf0-34ced3759a5b)


GitHub API 호출 예시

![image](https://github.com/user-attachments/assets/45e32629-a60d-41a3-a300-26b029da2fe6)

🚀 플러그인 기능 설계
1. GitHub 이슈가 생성될 때
GitHub에서 이슈가 생성되면 Jira에 해당 이슈를 자동으로 생성합니다.
2. GitHub 이슈 상태가 변경될 때
GitHub 이슈 상태가 변경되면, Jira의 해당 태스크 상태를 자동으로 업데이트합니다.
3. Jira에서 이슈 상태 변경 시
Jira에서 이슈 상태가 변경되면, 해당 GitHub 이슈의 상태도 자동으로 업데이트됩니다.
🔗 GitHub Webhook 설정
GitHub Webhook 설정:

GitHub 리포지토리 설정에서 Webhook을 추가하고, issue 이벤트를 선택하여 해당 이벤트가 발생할 때마다 플러그인이 실행되도록 합니다.
Jira Webhook 설정:

Jira에서 상태 변화가 있을 때, 해당 정보를 GitHub으로 전달하기 위해 Jira에서 Webhook을 설정합니다.
🌍 배포
플러그인이 완성되면, AWS Lambda나 Google Cloud Functions와 같은 서버리스 환경에 배포할 수 있습니다.
🧪 테스트 및 개선
여러 리포지토리와 프로젝트에서 실제로 플러그인을 테스트하고, 오류나 기능 개선이 필요한 부분을 파악하여 보완합니다.

Jira와 Confluence 간의 연동을 위한 API 기반 플러그인을 개발하는 방법에 대해 상세히 설명드릴게요. 두 툴 모두 REST API를 제공하므로, 이 API를 활용하여 데이터를 실시간으로 가져오고, Confluence에서 이를 대시보드 형식으로 시각화할 수 있습니다.

1. Jira와 Confluence API 이해하기
먼저 Jira API와 Confluence API의 기본적인 사용법을 이해해야 합니다. 두 시스템 모두 REST API를 제공하므로 HTTP 요청을 통해 데이터를 가져오거나 업데이트할 수 있습니다.

Jira REST API
기본 API URL: https://your-jira-instance.atlassian.net/rest/api/2/
주요 엔드포인트:
이슈 목록: /search (JQL 쿼리를 사용해 이슈 필터링)
이슈 상세: /issue/{issueKey} (이슈 상태, 우선순위, 담당자 등 정보)
스프린트 진행 상태: /board/{boardId}/sprint (스프린트 정보)
예시: 특정 이슈 상태 가져오기
bash
복사
편집
GET /rest/api/2/issue/{issueKey}
Confluence REST API
기본 API URL: https://your-confluence-instance.atlassian.net/wiki/rest/api/
주요 엔드포인트:
페이지 목록: /content (Confluence 페이지 조회)
페이지 작성: /content (새 페이지 생성)
페이지 업데이트: /content/{pageId} (기존 페이지 수정)
예시: Confluence 페이지에 데이터 삽입하기
bash
복사
편집
POST /rest/api/content
{
  "type": "page",
  "title": "Jira 대시보드",
  "space": { "key": "YOUR_SPACE_KEY" },
  "body": {
    "storage": {
      "value": "<h2>Jira 이슈 상태</h2><p>데이터 삽입</p>",
      "representation": "storage"
    }
  }
}
2. 플러그인 개발 환경 설정
플러그인 개발을 위해 Atlassian SDK를 사용하면, Jira와 Confluence에 대한 개발이 매우 용이해집니다. SDK를 설치하고 플러그인 개발을 위한 기본 환경을 구성합니다.

Atlassian SDK 설치: Atlassian SDK 설치 가이드
플러그인 프로젝트 생성:
bash
복사
편집
atlas-create-jira-plugin
또는
bash
복사
편집
atlas-create-confluence-plugin
이 명령어는 Jira 또는 Confluence용 플러그인 템플릿을 생성해줍니다.
3. Jira 데이터 가져오기
플러그인에서 Jira REST API를 사용하여 데이터를 가져오는 방법은 다음과 같습니다.

HTTP 클라이언트 설정: 플러그인 내에서 Jira API에 요청을 보내기 위해 HTTP 클라이언트를 설정합니다.

java
복사
편집
HttpClient client = HttpClientBuilder.create().build();
HttpGet request = new HttpGet("https://your-jira-instance.atlassian.net/rest/api/2/search?jql=project=MYPROJECT");
request.addHeader("Authorization", "Basic " + Base64.encodeBase64String("username:token".getBytes()));
HttpResponse response = client.execute(request);
String jsonResponse = EntityUtils.toString(response.getEntity());
데이터 파싱: Jira에서 반환된 JSON 데이터를 파싱하여 필요한 정보를 추출합니다.

java
복사
편집
JSONObject json = new JSONObject(jsonResponse);
JSONArray issues = json.getJSONArray("issues");
for (int i = 0; i < issues.length(); i++) {
    JSONObject issue = issues.getJSONObject(i);
    String key = issue.getString("key");
    String status = issue.getJSONObject("fields").getJSONObject("status").getString("name");
    // 필요한 데이터 추출
}
Jira 이슈 상태 및 우선순위 가져오기:

java
복사
편집
String issueKey = "MYPROJECT-123";
HttpGet request = new HttpGet("https://your-jira-instance.atlassian.net/rest/api/2/issue/" + issueKey);
// 처리...
4. Confluence 페이지에 데이터 표시
Jira에서 데이터를 가져왔다면 이를 Confluence 페이지에 삽입하는 방법은 다음과 같습니다.

Confluence 페이지 생성: 새로운 페이지를 생성하여, Jira 데이터로 생성한 대시보드를 표시합니다.

java
복사
편집
String url = "https://your-confluence-instance.atlassian.net/wiki/rest/api/content";
HttpPost post = new HttpPost(url);
post.addHeader("Authorization", "Basic " + Base64.encodeBase64String("username:token".getBytes()));

String jsonBody = "{"
    + "\"type\": \"page\","
    + "\"title\": \"Jira 대시보드\","
    + "\"space\": { \"key\": \"YOUR_SPACE_KEY\" },"
    + "\"body\": {"
    + "\"storage\": {"
    + "\"value\": \"<h2>진행 중인 Jira 이슈</h2><p>데이터 삽입</p>\","
    + "\"representation\": \"storage\""
    + "}}}";

StringEntity entity = new StringEntity(jsonBody);
post.setEntity(entity);
HttpResponse response = client.execute(post);
대시보드 삽입: Confluence 페이지 내에서 Jira 데이터를 시각적으로 표현할 수 있는 HTML이나 마크업을 삽입합니다.

5. 플러그인 실행 및 테스트
플러그인 빌드:
bash
복사
편집
atlas-package
Jira 또는 Confluence에 플러그인 설치: 플러그인을 Atlassian 제품에 설치하여, 실제 환경에서 동작하는지 테스트합니다.
bash
복사
편집
atlas-run
6. 고급 기능 추가 (선택 사항)
자동화된 데이터 갱신: 일정 시간 간격으로 Jira 데이터를 갱신하여 Confluence 대시보드를 업데이트할 수 있습니다.
권한 관리: Confluence 페이지에 대한 읽기/수정 권한을 설정하여, 필요한 팀원만 대시보드를 볼 수 있도록 할 수 있습니다.
7. 배포
완성된 플러그인은 Atlassian Marketplace에 배포하거나, 내부적으로 배포하여 사용자가 쉽게 설치하고 활용할 수 있도록 할 수 있습니다.

이와 같이, Jira와 Confluence의 REST API를 사용하여 플러그인을 개발하고, 실시간으로 데이터를 가져와 Confluence 내에서 대시보드를 동적으로 표시할 수 있습니다. 코드 예시나 플러그인 설정에 대해 추가적으로 궁금한 점이 있으면 알려주세요!
