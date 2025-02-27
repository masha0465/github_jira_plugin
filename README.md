# github_jira_plugin

Jira와 GitHub을 연동하는 플러그인을 만들기 위해서는 Jira의 API와 GitHub API를 활용하여 두 시스템 간의 데이터를 주고받을 수 있도록 해야 합니다. GitHub 이슈가 생성되거나 업데이트되면 Jira의 태스크로 변환되거나 상태가 업데이트되도록 자동화하는 과정입니다. 다음은 플러그인 개발을 위한 상세한 절차입니다.

1. 개발 환경 준비
Jira 개발자 계정 만들기: Jira Developer Portal에서 개발자 계정을 만들고 Jira Cloud API를 사용할 수 있는 앱을 등록합니다.
GitHub API 설정: GitHub Developer 사이트에서 GitHub API를 사용하기 위한 OAuth 또는 Personal Access Token을 생성합니다.
2. 필요한 라이브러리 및 도구 설치
Node.js와 npm: JavaScript로 API를 호출하고, 서버를 구동할 수 있도록 Node.js와 npm을 설치합니다.
Jira API 라이브러리: Atlassian의 REST API를 활용하기 위해 axios나 node-fetch와 같은 HTTP 클라이언트 라이브러리를 설치합니다.
bash
복사
편집
npm install axios
GitHub API 라이브러리: GitHub API를 사용하기 위해 @octokit/rest 라이브러리를 설치합니다.
bash
복사
편집
npm install @octokit/rest
3. Jira와 GitHub API 연동
Jira API 호출 예시
Jira에서 이슈를 생성하거나 업데이트하려면 Jira의 REST API를 사용해야 합니다. 예를 들어, 이슈를 생성하는 코드 예시는 다음과 같습니다.

javascript
복사
편집
const axios = require('axios');

// Jira API 설정
const JIRA_URL = 'https://your-jira-instance.atlassian.net/rest/api/3/issue';
const JIRA_API_TOKEN = 'your-jira-api-token';
const JIRA_USER = 'your-email@example.com';

// Jira 이슈 생성 함수
async function createJiraIssue(issueData) {
  const auth = Buffer.from(`${JIRA_USER}:${JIRA_API_TOKEN}`).toString('base64');
  const config = {
    headers: {
      'Authorization': `Basic ${auth}`,
      'Content-Type': 'application/json',
    }
  };

  const response = await axios.post(JIRA_URL, issueData, config);
  return response.data;
}

// Jira 이슈 생성 예시
const newIssue = {
  fields: {
    project: {
      key: 'PROJ',
    },
    summary: 'New Issue from GitHub',
    description: 'This issue was created from GitHub integration.',
    issuetype: {
      name: 'Task',
    }
  }
};

createJiraIssue(newIssue).then(response => {
  console.log('Jira Issue Created:', response);
});
GitHub API 호출 예시
GitHub에서 이슈를 생성하거나 업데이트하려면 GitHub API를 사용해야 합니다. 예를 들어, GitHub에서 이슈가 업데이트될 때 Jira 태스크 상태를 변경하려면 다음과 같은 방법을 사용할 수 있습니다.

javascript
복사
편집
const { Octokit } = require("@octokit/rest");

// GitHub API 설정
const octokit = new Octokit({ auth: 'your-github-token' });

// GitHub 이슈 업데이트 함수
async function updateGitHubIssue(owner, repo, issueNumber, issueData) {
  const response = await octokit.issues.update({
    owner,
    repo,
    issue_number: issueNumber,
    ...issueData
  });
  return response.data;
}

// GitHub 이슈 업데이트 예시
updateGitHubIssue('your-github-username', 'your-repository', 1, {
  title: 'Updated Issue Title',
  body: 'Updated issue body description',
}).then(response => {
  console.log('GitHub Issue Updated:', response);
});
4. 플러그인 기능 설계
자동화 흐름 설계
GitHub 이슈가 생성될 때:
GitHub에서 이슈가 생성되면 Jira에 해당 이슈를 자동으로 생성하도록 합니다.
createJiraIssue() 함수는 GitHub의 이슈 데이터(예: 제목, 설명 등)를 Jira의 형식에 맞게 변환하여 이슈를 생성합니다.
GitHub 이슈 상태가 변경될 때:
GitHub 이슈의 상태가 변경될 때(예: open, closed), Jira의 해당 태스크 상태를 업데이트합니다.
updateJiraIssue() 함수에서 Jira의 상태를 업데이트하는 작업을 수행합니다.
예시 흐름
GitHub에서 이슈가 생성됨 → 이슈 내용이 Jira로 전송되어 새로운 Jira 태스크 생성
GitHub에서 이슈 상태 변경 → 해당 상태에 맞춰 Jira 태스크 상태 업데이트 (예: In Progress, Done 등)
Jira에서 이슈 상태 변경 → GitHub 이슈 상태를 자동으로 업데이트
5. GitHub Webhook 설정
GitHub에서 이벤트가 발생할 때마다 플러그인이 반응하도록 Webhook을 설정합니다. 예를 들어, 이슈가 생성되거나 업데이트될 때마다 webhook을 통해 특정 URL로 POST 요청을 보낼 수 있습니다.

GitHub 리포지토리 설정에서 Webhook을 추가하고, issue 이벤트를 선택하여 해당 이벤트가 발생할 때마다 플러그인이 실행되도록 합니다.
6. Jira에서 Webhook 설정
Jira에서 상태 변화가 있을 때, 해당 정보를 GitHub으로 전달하려면 Jira에서 Webhook을 설정합니다. 이를 통해 Jira 태스크가 변경될 때마다 GitHub의 관련 이슈를 업데이트할 수 있습니다.

7. 플러그인 배포
플러그인이 완성되면, 이를 서버에 배포하거나 AWS Lambda, Google Cloud Functions와 같은 서버리스 환경에 배포할 수 있습니다. 이를 통해 GitHub와 Jira 간의 통합을 클라우드 환경에서도 손쉽게 유지할 수 있습니다.

8. 테스트 및 개선
여러 리포지토리와 프로젝트에서 실제로 플러그인을 테스트하고, 오류나 기능 개선이 필요한 부분을 파악해 보완합니다.
예를 들어, 특정 이슈에 대한 자동화가 잘못 작동하는 경우, 디버깅을 통해 개선합니다.
이 플러그인은 GitHub와 Jira의 API를 활용하여 두 시스템 간의 통합을 실현하며, 팀의 작업 효율성을 크게 향상시킬 수 있습니다. 특히, 수동 작업을 줄여주고, 자동화된 태스크 관리가 가능하게 됩니다.
