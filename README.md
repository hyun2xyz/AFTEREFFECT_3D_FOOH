# AFTEREFFECT_3D_FOOH

**VFX 학생이 LLM 에이전트와 함께 시작하는 3D FOOH 스킬의 출발점.**
실사 영상의 카메라를 맞추고, ComfyUI의 Tripo API 노드로 캐릭터를 만들고, Blender에서
접지·물리·렌더 패스를 만든 뒤 After Effects에서 합성합니다. Blender와 AE는 Python
스크립트와 Python이 만든 JSX로 다룹니다. AE MCP나 별도 서버는 쓰지 않습니다.

## 학생은 이렇게 시작합니다

공통강의의 Git 세팅을 마친 컴퓨터에서, 이 저장소를 내 GitHub 계정에 **포크**해서 시작합니다.

1. Codex 또는 Claude Code에 붙여 넣습니다.

   > https://github.com/hyun2xyz/AFTEREFFECT_3D_FOOH 를 제 GitHub 계정으로 포크해서 문서(Documents) 폴더에
   > 받아 주세요. 끝나면 origin과 upstream 주소를 보여 주세요.

2. 받은 `AFTEREFFECT_3D_FOOH` 폴더를 Codex에서 다시 열고 [START-HERE.md](START-HERE.md)의 문장이나
   다음처럼 요청합니다.

> START-HERE.md, AGENTS.md와 공유 스킬을 읽고 제 컴퓨터부터 확인해 주세요.
> 필요한 것만 설치 안내하고, 제가 주는 짧은 영상에 3D 캐릭터를 세워 합성하는 과정을
> 한 단계씩 진행해 주세요. 유료 생성은 예상 크레딧을 보여 주고 제 승인을 받은 뒤
> 한 번만 실행해 주세요. 카메라 기준을 통과했는지 숫자로 알려 주세요.

- [포크로 시작하고 업데이트 받기](docs/fork-and-update.md)
- [아무것도 없는 Windows 준비](docs/setup-windows.md)
- [아무것도 없는 Mac 준비](docs/setup-macos.md)
- [3D FOOH 규칙](docs/rules.md)
- [첫 영상부터 AE까지](docs/workflow.md)
- [AE 컴프 만들기](docs/ae-comp.md)

## 첫 버전의 범위

1. 샷에 맞는 카메라 경로 하나(직선 기반 고정 카메라, COLMAP, 평면 추적)를 고르고,
   배치 영역을 계산에 쓰지 않은 관측으로 검수합니다.
2. 고정 회색 상자로 전체 구간의 부착을 확인합니다.
3. ComfyUI Tripo API 노드로 모델·자동 리깅·애니메이션을 한 번 생성합니다.
4. Blender에서 모델을 정리하고 발 접지를 1/4 프레임 단위로 확인합니다. 필요하면
   Bullet 강체 시뮬레이션을 굽고 재생 검사를 합니다.
5. 같은 카메라·시계로 렌더 패스를 만들고, 원본과 CG를 분리한 AE 프로젝트로 합성합니다.

이 범위는 배치 영역의 독립 검수에서 기준(1920×1080에서 p95 1.5 px, 최대 3 px)을 통과한
세 사례(벽에서 쏟아지는 공, 거리의 마스코트, 드론 농구장의 캐릭터)를 바탕으로 정했습니다.
천·불 같은 유체 시뮬레이션, 손에 든 물체 추적, 키잉은 이 출발점에 포함하지 않습니다.

## 에이전트/개발자

공유 스킬은 [.agents/skills/aftereffect-3d-fooh/SKILL.md](.agents/skills/aftereffect-3d-fooh/SKILL.md)입니다.
Codex는 저장소 스킬을 쓰고, Claude Code는 `CLAUDE.md`에서 같은 지침을 읽습니다.

```powershell
py -3.13 -B scripts/setup_environment.py --check
.\.venv\Scripts\python.exe -B -m unittest discover -s tests -v
```

```sh
python3.13 -B scripts/setup_environment.py --check
.venv/bin/python -B -m unittest discover -s tests -v
```

`scripts/`에는 설치 점검 도구와 AE 컴프 JSX 생성기(`ae_comp.py`, [사용법](docs/ae-comp.md))가 있습니다.
카메라·Blender·Comfy 제출 스크립트는 에이전트가 샷마다 `work/<shot>/`에 만들고, 실제 샷에서 통과한 것만 `scripts/`로 옮겨
각자의 스킬로 키웁니다. 실행 파일·Adobe 앱·생성 모델·학생 영상·AEP는 재배포하지 않습니다.
[출처·라이선스](THIRD_PARTY_NOTICES.md)를 확인하세요.
