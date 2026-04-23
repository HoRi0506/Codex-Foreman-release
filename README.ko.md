# Codex-Cli-Captain-Release

[English](./README.md) | [한국어](./README.ko.md) | [日本語](./README.ja.md)

Codex CLI용 Codex-Cli-Captain 설치 저장소입니다.

현재 공개 버전: `0.0.3`.

`$cap`은 Codex가 captain 역할을 하면서 `ccc-config.toml`의 모델과 reasoning 설정에 따라 적절한 에이전트로 작업을 라우팅하게 합니다.

## 설치

Codex CLI에 아래 문구를 붙여넣으세요.

```text
Install Codex-Cli-Captain from https://github.com/HoRi0506/Codex-Cli-Captain-Release by running:
curl -fsSL https://raw.githubusercontent.com/HoRi0506/Codex-Cli-Captain-Release/main/install.sh | bash

After installation finishes, fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

## 설정 변경 반영

`~/.config/foreman/ccc-config.toml`을 수정한 뒤 Codex CLI에 아래 문구를 붙여넣으세요.

```text
Run:
ccc setup

Then fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

## 추천 역할 설정

| CCC role | Agent | 추천 모델 | Reasoning | 용도 |
| --- | --- | --- | --- | --- |
| `orchestrator` | `captain` | `gpt-5.4` | `high` | LongWay 관리와 최종 라우팅 판단 |
| `way` | `tactician` | `gpt-5.4` | `medium` | 계획 수립과 다음 작업 선택 |
| `explorer` | `scout` | `gpt-5.4-mini` | `medium` | 읽기 전용 repo 조사 |
| `code specialist` | `raider` | `gpt-5.3-codex` | `high` | 코드/config 수정과 복구 |
| `documenter` | `scribe` | `gpt-5.4-mini` | `medium` | README, 릴리즈 노트, 사용자 문구 |
| `verifier` | `arbiter` | `gpt-5.4` | `medium` | 리뷰, 리스크, 회귀 확인 |
| `companion_reader` | `companion_reader` | `gpt-5.4-mini` | `medium` | 저비용 filesystem/docs/web/git/gh 읽기 작업 |
| `companion_operator` | `companion_operator` | `gpt-5.4-mini` | `medium` | 저비용 git/gh 변경 및 좁은 도구 실행 |

## 포함 내용

- `ccc` CLI와 MCP entrypoint
- compact `$cap` skill
- `~/.config/foreman/ccc-config.toml`
- `ccc-config.toml` 기준으로 동기화되는 CCC-managed custom agents
- 한국어, 영어, 일본어, 중국어 요청 신호에 대한 라우팅 점검
- 가벼운 filesystem/docs/fetch/git/gh 작업을 `gpt-5.4-mini` companion 역할로 라우팅
- 강화된 raider 모듈화 원칙
- compact prompt와 `--text`, `--quiet`, `--json-file` 저소음 CLI surface
- raw usage event가 있을 때 token 총량과 agent별 게이지 출력, 없을 때 명확한 unavailable 이유 출력
- 최소 release repo, stripped binary, 민감 문자열 검사 기반 릴리즈 위생

## 정상 확인

```text
CCC install check: status=ok version=0.0.3 entry=$cap registration=matching_registration config=present skill=matching_install
```
