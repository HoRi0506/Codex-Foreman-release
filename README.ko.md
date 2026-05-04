# Codex-Cli-Captain

<p align="center">
  <a href="./README.md">English</a> ·
  <a href="./README.ko.md">한국어</a> ·
  <a href="./README.ja.md">日本語</a>
</p>

<p align="center">
  <img src="./docs/assets/ccc-banner.png" alt="CCC Codex-Cli-Captain banner" width="100%">
</p>

<p align="center"><em>Codex CLI에서 긴 작업을 안정적으로 이어가고 싶을 때 CCC를 사용하세요.<br>
요청 앞에 <code>$cap</code>을 붙이면 CCC가 계획, task card, fan-in, status, restart handoff를 관리합니다.</em></p>

현재 공개 버전: `0.0.11-pre`.

## 사전 릴리즈 안내

`0.0.11-pre`는 공개 사전 릴리즈입니다. macOS가 기본 지원 경로이며, macOS arm64는 로컬에서 설치와 동작을 확인했습니다. Linux와 Windows asset도 제공하지만, 이 maintainer PC에서는 패키징까지만 가능하고 실행 검증은 할 수 없으므로 초기 테스트 경로로 봐야 합니다.

이전 사전 릴리즈가 필요하면 아래 값을 사용하세요.

```text
CCC_VERSION=v0.0.10-pre
```

## 설치

macOS 또는 Linux:

```text
Install Codex-Cli-Captain from https://github.com/HoRi0506/Codex-Cli-Captain-Release by running:
curl -fsSL https://raw.githubusercontent.com/HoRi0506/Codex-Cli-Captain-Release/main/install.sh | bash

After installation finishes, fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

Windows PowerShell:

```text
Install Codex-Cli-Captain from https://github.com/HoRi0506/Codex-Cli-Captain-Release by running:
iwr -UseB https://raw.githubusercontent.com/HoRi0506/Codex-Cli-Captain-Release/main/install.ps1 | iex

After installation finishes, fully exit Codex CLI.
Start a new Codex CLI session.
Then run:
ccc check-install
```

업데이트할 때도 같은 설치 명령을 다시 실행한 뒤 Codex CLI를 재시작하고 `ccc check-install`을 실행하세요.

## 기본 사용법

요청 앞에 `$cap`을 붙이면 CCC가 작업을 관리합니다.

```text
$cap 인증 흐름을 리팩터링하고 테스트가 통과하도록 유지해줘
```

CCC에서 `$cap`은 진입점입니다. LongWay, task card, checklist, fan-in, status, restart handoff는 CCC가 관리합니다.

## Plan And Goal 활용

`0.0.11-pre`에서는 Codex host가 제공하는 planning/goal 기능을 `$cap`과 함께 보조적으로 사용할 수 있습니다.

- 넓거나 위험하거나 애매한 작업은 `$cap` 전에 `/plan`을 사용해 요청을 먼저 정리하세요.
- `/goal`은 host가 지원할 때 장기 목표를 기억시키는 힌트로 사용할 수 있습니다. 다만 CCC의 LongWay, checklist, fan-in, status가 실제 작업의 기준입니다.
- 좁은 작업은 `$cap`만으로 충분합니다.

예시:

```text
/plan
/goal 릴리즈 문서를 사용자에게 보이는 수준으로 간결하게 유지한다
$cap 릴리즈 README를 정리하고 0.0.11-pre 후속 작업을 문서에 기록해줘
```

## 추천 역할 설정

CCC를 자주 사용한다면 ChatGPT Pro $100 요금제를 시작점으로 권장합니다. `$cap` workflow는 captain과 specialist handoff를 반복하면서 Codex 사용량을 더 많이 쓸 수 있기 때문입니다. Reasoning은 사용자의 작업 스타일과 작업 위험도에 맞춰 조정하세요. 넓은 계획, 위험한 코드 변경, 리뷰에는 높은 reasoning을 유지하고, 좁고 반복적이거나 위험이 낮은 작업에는 낮춰도 됩니다.

| CCC role | Agent | 추천 모델 | Reasoning | 용도 |
| --- | --- | --- | --- | --- |
| `orchestrator` | `captain` | `gpt-5.5` | `medium` | LongWay 관리와 최종 라우팅 판단 |
| `way` | `tactician` | `gpt-5.5` | `high` | 계획 수립과 다음 작업 선택 |
| `explorer` | `scout` | `gpt-5.4-mini` | `high` | 읽기 전용 repo 조사 |
| `code specialist` | `raider` | `gpt-5.5` | `high` | 코드/config 수정과 복구 |
| `documenter` | `scribe` | `gpt-5.4-mini` | `medium` | README, 릴리즈 노트, 사용자 문구 |
| `verifier` | `arbiter` | `gpt-5.5` | `high` | 리뷰, 리스크, 회귀 확인 |
| `companion_reader` | `companion_reader` | `gpt-5.4-mini` | `medium` | 저비용 filesystem/docs/web/git/gh 읽기 작업 |
| `companion_operator` | `companion_operator` | `gpt-5.4-mini` | `medium` | 저비용 git/gh 변경 및 좁은 도구 실행 |

`gpt-5.5`는 ChatGPT 인증 Codex에서 고가치 역할에 권장되는 모델입니다. 현재 계정이나 실행 경로에서 아직 사용할 수 없다면 해당 역할은 rollout이 도달할 때까지 `gpt-5.4`를 사용합니다.

## 0.0.11-pre 범위

이번 릴리즈는 planning, execution, task card, checklist/status/fan-in truth, restart handoff를 중심으로 CCC walking skeleton 흐름을 제공합니다. Full parity 또는 rebuild 완료 릴리즈는 아닙니다.

Intervention/review-retry-replan-reclaim parity, schema contract promotion, setup config surface 정리, full runtime parity는 후속 hardening 작업으로 남아 있습니다.

## Codex App And CLI 표시

CCC는 Codex CLI와 Codex app 모두에서 읽을 수 있는 status 출력을 제공합니다. `ccc status`, `ccc activity`, MCP status payload에는 LongWay 진행, 현재 task card, specialist/subagent lane 상태, fan-in readiness, blocker, 다음 captain action, context-health warning, restart handoff, compact cost-routing visibility가 포함됩니다. CCC MCP server가 Codex app sidebar를 직접 강제로 띄울 수는 없고, host app panel이 CCC status/activity payload를 렌더링하거나 polling해야 합니다.

## 선택적 Tolaria Mirror

CCC는 local-first로 동작합니다. Tolaria가 설치되고 설정되어 있으면 CCC code graph와 repo별 CCC memory를 Tolaria vault에 mirror해서 나중에 검색하거나 복구할 수 있습니다. Tolaria가 없거나 설정되지 않은 환경에서는 기존처럼 local `.ccc/graph/store.json`과 `.ccc/memory.json`을 사용합니다.
