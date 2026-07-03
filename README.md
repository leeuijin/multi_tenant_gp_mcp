<img width="972" height="534" alt="스크린샷 2026-07-03 12 57 55" src="https://github.com/user-attachments/assets/79336bdb-a970-41a4-b3eb-669469d57b8c" />

# 중앙집중형 멀티테넌트 gp_mcp Architecture

- gp-mcp-server 프로세스 1개, 포트 1개(8083)가 모든 계정의 요청을 공통으로 처리
- 실제 격리(isolation)는 애플리케이션 로직이 아니라 GPDB 자체의 권한 시스템(permission denied for database)이 담당
- 테넌트(계정) 구분은 claude_desktop_config.json에 미리 등록된 3개의 커넥터(greenplum-gpadmin/greenplum-sales1/greenplum-marketinguser1)로 고정되어 있음 
— 각각 Basic Auth 헤더가 하드코딩된 정적 설정 (ID:PASSWORD 를 base64로 변환하여 claude_desktop_config.json Authorization 항목에 작성)
- 대화에서 "sales1 계정으로"라고 하면, Claude가 대화 맥락에서 알아서 크레덴셜을 만들어내는 게 아니라 미리 준비된 3개 커넥터 중 하나를 선택

<img width="975" height="536" alt="스크린샷 2026-07-03 12 58 49" src="https://github.com/user-attachments/assets/5e74c2cb-8d0a-4db5-92ea-c367026b12c3" />

# Centralized multitenant gp_mcp Architecture

- 1 gp-mcp-server process, 1 port (8083) handles requests from all accounts in common
- The GPDB's own authority system (permission denied for database) is responsible for the actual isolation, not application logic
- The tenant (account) classification is fixed with three pre-registered connectors (greenplum-gpadmin/greenplum-sales1/greenplum-marketinguser1) in the cluster_desktop_config.json 
— Static settings with hard-coded BasicAuthorization headers (Convert ID: PASSWORD to base64 and write it in the category claude_desktop_config.json Authorization)
- When you say "with sales1 account" in the conversation, Claud does not create credentials on his own in the context of the conversation, but rather chooses one of the three pre-prepared connectors
