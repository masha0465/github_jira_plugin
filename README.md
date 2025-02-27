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
