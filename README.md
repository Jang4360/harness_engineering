# AI Harness 시작점

하네스의 canonical 문서는 `.ai/README.md`에 있습니다.

## 초기 설정

1. `./.ai/scripts/install-root-entrypoints.sh` 실행
2. `./.ai/scripts/bootstrap-template.sh "프로젝트 이름"` 실행
3. `./.ai/scripts/sync-adapters.sh` 실행
4. `./.ai/scripts/verify.sh` 실행

canonical `.ai/` 패키지 기준으로 이 루트 `README.md`를 다시 생성하려면 `./.ai/scripts/install-root-entrypoints.sh --write-readme --force`를 실행하면 됩니다.
