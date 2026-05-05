# Codex-Cli-Captain

<p align="center">
  <a href="./README.md">English</a> ·
  <a href="./README.ko.md">한국어</a> ·
  <a href="./README.ja.md">日本語</a>
</p>

<p align="center">
  <img src="./docs/assets/ccc-banner.png" alt="CCC Codex-Cli-Captain banner" width="100%">
</p>

<p align="center"><em>Codex CLI나 Codex App으로 end-to-end로 하고 싶으시다구요?<br>
그런데 고사양 모델로 end-to-end를 돌리는 건 걱정된다구요?<br>
그렇다면 CCC를 이용해보시는 건 어떨까요?<br>
여러분은 그저 하고자 하는 것 앞에 <code>$cap</code>만 붙이면 됩니다.<br>
그러면 놀라운 일이 펼쳐질 거예요!</em></p>

현재 공개 버전: `0.0.13-pre`.

지원 플랫폼: `darwin-arm64`, `darwin-x86_64`, `linux-arm64`, `linux-x86_64`, `windows-x86_64`.

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

## v0.0.13-pre 동작

- `$cap`은 public entrypoint입니다.
- 사용자에게 보이는 lifecycle 변경은 quiet `ran` 커맨드를 우선합니다: `ccc start --quiet --json-file`, `ccc orchestrate --quiet --json-file`, `ccc subagent-update --quiet --json-file`, `ccc memory --quiet --json-file`, `ccc checklist --quiet --json-file`, `ccc status --quiet --json-file`, `ccc status --text --json-file`.
- `ccc checklist`와 `ccc status`는 간결해야 합니다. LongWay row는 짧은 operator-facing summary입니다.
- 설정된 `ccc_*` custom agent가 기본 specialist 대상입니다. `worker`와 `explorer` 같은 generic label은 operator가 명시적으로 override하지 않으면 invalid입니다.
- `ccc memory`는 opt-in이며 기본값은 unconfigured입니다.
- SSL Skill Registry는 routing, planning, review용 bounded evidence로 제공되며 persisted run state를 대체하지 않습니다.
- mutation 완료는 specialist fan-in 뒤에 진행되며, review-sensitive 변경의 최종 gate는 arbiter review입니다.

## Planning

`$cap`이 CCC 진입점입니다. Host planning surface는 CCC orchestration path가 아닙니다.

- 넓거나 위험하거나 애매한 작업은 `$cap`으로 진입해 CCC가 `PLAN_SEQUENCE`를 만들고 설정된 Way agent에 planning을 맡기게 하세요.
- Host Plan Mode는 사용자가 입력할 내용을 정리하는 데 도움을 줄 수 있지만, CCC가 Way 안에서 background planning engine으로 트리거하지는 않습니다.
- CCC LongWay, task card, checklist, fan-in, status가 실제 작업의 기준입니다.
- 좁은 작업은 `$cap`만으로 충분합니다.

예시:

```text
$cap 릴리즈 README 수정을 계획하고, 막히는 Way 질문이 있으면 먼저 묻고, LongWay 승인 전에는 파일을 바꾸지 마
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
