# 처음 시작하기

이 폴더는 **Codex 또는 Claude Code와 함께** 실사 영상에 3D 캐릭터·오브젝트를 넣는
수업용 starter입니다. Python·FFmpeg·Blender가 없는 컴퓨터도 설치 단계부터 시작할 수
있습니다. After Effects와 Comfy 계정(선불 크레딧)은 학생이 별도로 준비하며, 앱과
모델은 이 폴더에 포함하지 않습니다.

## 폴더를 연 뒤 이 문장을 붙여 넣기

ZIP을 완전히 풀고 `AGENTS.md`, `scripts`, `docs`, `.agents`가 있는 폴더를 에이전트의
작업 폴더로 선택합니다. Git이나 전역 스킬 설치는 필요하지 않습니다.

> 이 폴더의 AGENTS.md와 .agents/skills/aftereffect-3d-fooh/SKILL.md를 읽고 시작해 주세요.
> 실제 OS와 설치된 도구를 확인하고, OS setup 문서의 공식 출처로 빠진 것만 준비해 주세요.
> Python 3.13(또는 3.12), 프로젝트 .venv와 tools.local.json을 사용해 주세요.
> 제가 주는 짧은 영상에 맞는 카메라 경로를 골라 검수하고, 회색 상자 테스트를 통과한
> 뒤에 캐릭터를 만들어 합성하는 과정을 docs/rules.md대로 한 단계씩 진행해 주세요.
> Adobe·Comfy 로그인과 결제·약관 동의는 제가 직접 합니다. 유료 생성은 예상 크레딧을
> 보여 주고 승인을 받은 뒤 한 번만 실행해 주세요. 원본은 보존하고, 기준 통과 여부와
> 남은 가정(초점·축척)을 구분해 알려 주세요. 실패를 성공으로 보고하지 마세요.

## 진행 순서

1. 현재 OS의 [Windows setup](docs/setup-windows.md) 또는 [Mac setup](docs/setup-macos.md)만
   읽고 도구를 준비합니다. `setup_environment.py --check`는 표준 라이브러리만으로
   검사하고, `--configure`는 경로만 저장하며, `--install`을 요청할 때만 `.venv`에
   패키지를 설치합니다.
2. [workflow](docs/workflow.md)대로 내 영상과 `work/<shot>` 폴더를 정하고, 카메라 계산 전에
   `contract.json`에 배치 영역과 기준을 적습니다.
3. 카메라 → 회색 부착 → 모델 생성 → 정리·접지·물리 → 렌더 패스 → AE 순서로 진행합니다.
   각 단계의 규칙은 [rules](docs/rules.md)에 있습니다.

첫 샷은 6–10초, 넓은 바닥이 보이고 카메라 움직임이 단순한 영상으로 시작합니다.
