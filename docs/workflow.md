# 첫 영상부터 After Effects까지

한 번에 한 단계씩 진행합니다. 각 단계가 끝나면 `work/<shot>/next.md`에 만든 파일,
확인한 것, 남은 문제, 다음 단계 하나를 적습니다. 규칙의 근거는 [rules](rules.md)에 있습니다.

## 작업 폴더

```text
work/<shot>/
  source/          원본 링크 또는 복사본(원본은 수정하지 않음)
  frames/          작업 프레임 PNG
  contract.json    배치 영역·검수 기준·카메라 경로(카메라 계산 전에 작성)
  camera/          카메라 결과, 독립 검수 결과(JSON)
  comfy/           API 형식 워크플로 JSON, 제출 기록, 받은 GLB
  blender/         .blend, Blender Python 스크립트, 캐시
  renders/         회색·재질·조명·그림자·최종 RGBA 시퀀스
  ae/              생성한 JSX, 저장한 AEP
  review/          비교 이미지·영상, report.json
  next.md
```

`work/`는 Git에 올리지 않습니다. 영상·모델·캐시·AEP는 각자의 컴퓨터에만 둡니다.

## 1. 샷 고르기

- 첫 샷은 6–10초, 넓은 바닥이 보이고 카메라 움직임이 단순한 영상이 좋습니다.
- ffprobe로 해상도·유리수 FPS·프레임 수를 읽고, FFmpeg로 프레임을 뽑습니다.
- 샷을 보고 카메라 경로 A(고정)·B(COLMAP)·C(평면 추적) 중 하나를 학생과 고릅니다.

## 2. contract.json

```json
{
  "source": "work/<shot>/source/clip.mp4",
  "fps": [30000, 1001],
  "frames": [420, 659],
  "working_size": [1920, 1080],
  "camera_route": "A_static_lines | B_colmap | C_planar",
  "placement_region": "보도 위 화면 중앙, 바닥 평면",
  "holdout": "피팅에 쓰지 않을 직선·점 목록",
  "gate": {"p95_px": 1.5, "max_px": 3.0},
  "assumptions": {"camera_height_m": 1.65, "subject_height_m": 5.0}
}
```

## 3. 카메라와 독립 검수

- A: 에이전트가 직선 피팅 스크립트를 만들고, 학생과 함께 직선을 고릅니다.
- B: AFTEREFFECT_COLMAP 저장소의 절차로 카메라를 풉니다.
- C: 학생이 AE에서 Mocha로 바닥을 추적하거나, 에이전트가 고정 기준점 ECC를 실행합니다.

결과를 `camera/`에 저장하고 독립 검수 값을 `review/camera_check.json`에 남깁니다.
기준을 넘으면 다음 단계로 가지 않습니다.

## 4. 회색 부착 테스트

에이전트가 Blender 백그라운드 스크립트로 카메라와 고정 회색 상자를 만들어 렌더하고,
원본과 겹친 비교 영상을 만듭니다. 학생이 미끄러짐을 함께 확인합니다.

## 5. 모델 생성(유료 단계)

학생이 ComfyUI 데스크톱에 로그인하고 크레딧을 확인합니다. 에이전트가 API 형식
워크플로를 만들어 보여 주고, 예상 크레딧을 학생이 승인한 뒤 한 번만 제출합니다.

## 6. 모델 정리·애니메이션·물리

GLB 검사 → 구멍 메우기(발 정점 유지) → 샷 길이에 맞춘 애니메이션 → 1/4 프레임 접지
검사 → (선택) Bullet 시뮬레이션 굽기와 재생 검사.

## 7. 렌더 패스와 AE 합성

같은 카메라·시계로 회색·재질·조명·그림자·최종 패스를 렌더하고, `scripts/ae_comp.py`가
만든 JSX로 AE 프로젝트를 구성합니다([AE 컴프 만들기](ae-comp.md)). 학생이 렌더하고, 에이전트가 최종 파일을 전체 디코드·검수합니다.

긴 렌더·굽기는 에이전트의 백그라운드 실행 기능을 쓰고, 같은 작업을 겹쳐 실행하지 않습니다.

## 다음 확장

이번 샷에서 실제로 쓴 단계만 `scripts/`로 옮깁니다. 예: 직선 기반 카메라, 카메라 →
Blender 변환, 회색 부착 렌더, Comfy 제출(중복 방지 포함), 접지 검사.
