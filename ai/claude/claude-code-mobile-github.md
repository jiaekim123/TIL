# Claude Code Mobile과 GitHub 연동

## 개요

Claude Code Mobile은 모바일 기기에서 Claude AI를 활용하여 코드 작성과 프로젝트 관리를 할 수 있는 도구입니다. GitHub와 연동하면 모바일에서도 저장소를 직접 관리하고, 이슈를 처리하며, 코드 리뷰 및 수정 작업을 수행할 수 있습니다.

### 주요 기능

- GitHub 저장소 직접 접근 및 브라우징
- 이슈 및 Pull Request 생성 및 관리
- AI 기반 코드 작성 및 수정
- 커밋 및 푸시 작업
- 브랜치 관리

## 사용법

### 1. GitHub 연동 설정

1. Claude Code Mobile 앱을 실행합니다
2. 설정(Settings) 메뉴로 이동합니다
3. "Connect to GitHub" 또는 "GitHub Integration" 옵션을 선택합니다
4. GitHub 계정으로 로그인하고 권한을 승인합니다
5. 연동할 저장소에 대한 접근 권한을 설정합니다

### 2. 저장소 선택 및 작업

1. 메인 화면에서 "Select Repository" 버튼을 탭합니다
2. 연동된 GitHub 계정의 저장소 목록에서 작업할 저장소를 선택합니다
3. 브랜치를 선택하거나 새로운 브랜치를 생성합니다

### 3. 코드 작업

1. 수정하고 싶은 파일을 선택합니다
2. Claude에게 자연어로 작업 내용을 설명합니다
   - 예: "이 함수에 에러 핸들링을 추가해줘"
   - 예: "새로운 API 엔드포인트를 만들어줘"
3. Claude가 제안한 코드를 리뷰하고 승인합니다

### 4. 변경사항 커밋 및 푸시

1. 작업이 완료되면 "Commit Changes" 버튼을 탭합니다
2. 커밋 메시지를 작성합니다 (Claude가 자동으로 제안할 수도 있습니다)
3. "Push to GitHub" 버튼을 탭하여 변경사항을 원격 저장소에 반영합니다

### 5. Pull Request 생성

1. "Create Pull Request" 옵션을 선택합니다
2. PR 제목과 설명을 작성합니다
3. 리뷰어를 지정하고 라벨을 추가합니다 (선택사항)
4. "Create" 버튼을 탭하여 PR을 생성합니다

## 활용 팁

- **이슈 기반 작업**: GitHub 이슈와 연동하여 작업하면 자동으로 이슈 번호가 커밋 메시지에 포함됩니다
- **자동 코드 리뷰**: Claude에게 코드 리뷰를 요청하여 개선점을 찾을 수 있습니다
- **빠른 버그 수정**: 모바일에서도 간단한 버그 수정을 빠르게 처리할 수 있습니다
- **문서 작성**: README나 기술 문서 작성에도 활용할 수 있습니다

## 주의사항

- 큰 규모의 리팩토링은 데스크톱 환경에서 진행하는 것을 권장합니다
- 모바일 데이터 사용 시 데이터 소모량에 주의하세요
- 중요한 변경사항은 반드시 테스트 후 푸시하세요
- GitHub Personal Access Token 관리에 주의하세요

## 참고 자료

- [Claude Code 공식 문서](https://docs.anthropic.com/claude/docs)
- [GitHub Mobile 문서](https://docs.github.com/en/get-started/using-github/github-mobile)
