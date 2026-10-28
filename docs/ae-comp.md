# After Effects 컴프 만들기

원본 플레이트와 결과 시퀀스를 레이어로 쌓은 **편집 가능한 AE 컴프**를 만듭니다.
`scripts/ae_comp.py`(Python 표준 라이브러리)가 샷 값으로 JSX를 만들고, 학생이 AE에서 실행합니다.
AE MCP, 상시 브리지, 자동 저장·렌더는 쓰지 않습니다. 설정의 근거는 [rules](rules.md) 9–10장입니다.

## 만드는 컴프

| 레이어(위 → 아래) | 소스 | 해석 |
| --- | --- | --- |
| CG | `--cg` 최종 패스 RGBA 시퀀스(필수) | 알파 Straight |
| Shadow | `--shadow` Shadow Catcher RGBA 시퀀스(선택) | 알파 Straight |
| Plate | `work/<shot>/frames/` (`--plate`) | 알파 무시. 컴프 크기·길이·FPS의 기준 |

16 bpc, 작업 색공간 Rec.709 Gamma 2.4, linear blending을 켭니다(기준 사례 설정).

- 렌더가 원본 첫 프레임보다 늦게 시작하면 `--cg-start`, `--shadow-start`에 컴프 프레임을 줍니다.
  원본 첫 장이 0입니다. 렌더가 원본 길이를 넘으면 생성기가 멈춥니다.
- PNG 16bit·EXR 시퀀스를 받습니다. 블러·그레인·색 맞춤은 학생이 CG에만 따로 겁니다.

스크립트는 **빈 프로젝트에서만** 폴더 하나(샷 이름), 푸티지, 컴프 하나를 추가합니다. 저장·닫기·렌더는
하지 않습니다. 중간에 실패하면 이번 실행에서 추가한 항목을 지우고 바꾼 프로젝트 설정을 되돌립니다.

## 1. 소스 확인

- 시퀀스는 **폴더 하나에 시퀀스 하나**입니다. 같은 이름, 같은 자릿수(0 채움), 빈 번호 없는 연속 번호여야
  합니다. 다른 파일이 섞여 있거나 번호가 빠지면 생성기가 멈춥니다(숨김 파일은 무시).
- 영상 파일은 받지 않습니다. FFmpeg로 PNG 시퀀스를 먼저 뽑습니다([workflow](workflow.md) 1단계).
- 결과 시퀀스는 원본과 같은 해상도여야 합니다. AE가 가져온 크기가 다르면 스크립트가 멈춥니다.
- FPS는 `--fps 30000/1001` 또는 `work/<shot>/contract.json`의 `"fps": [30000, 1001]`입니다.
- 샷 이름은 공백 없이 글자·숫자·`_`·`-`·`.`로 씁니다. 한글도 됩니다.

## 2. JSX 만들기(에이전트)

```sh
.venv/bin/python -B scripts/ae_comp.py --shot shot01 --cg work/shot01/renders/final --shadow work/shot01/renders/shadow
```

```powershell
.\.venv\Scripts\python.exe -B scripts\ae_comp.py --shot shot01 --cg work\shot01\renders\final --shadow work\shot01\renders\shadow
```

`work/<shot>/ae/build_comp.jsx`가 생기고, 레이어별 프레임 수와 `job` 번호가 출력됩니다.
옵션 전체는 `--help`로 봅니다. 경로를 생략한 레이어는 위 표의 기본 폴더를 씁니다.

## 3. AE에서 실행(학생)

1. 열려 있는 작업을 저장합니다. **File > New > New Project**로 빈 프로젝트를 엽니다.
2. **File > Scripts > Run Script File**에서 `work/<shot>/ae/build_comp.jsx`를 고릅니다.
3. "컴프를 만들었습니다" 알림이 뜨고 컴프가 Viewer에 열립니다.
4. **File > Save As**로 `work/<shot>/ae/<shot>.aep`에 직접 저장합니다.

## 4. 결과 확인(에이전트)

스크립트는 `work/<shot>/ae/build_comp.result.json`을 남깁니다. 다음을 확인합니다.

- `job_id`가 2단계에서 출력된 번호와 같은지(예전 기록을 읽지 않도록).
- `status`가 `ok`인지. 아니면 `error`를 학생에게 그대로 전합니다.
- `project`의 bpc·작업 색공간·linear blending, `comp`의 크기·픽셀 종횡비·프레임 수.
- `layers`(아래 → 위)의 `in_frame`·`out_frame`, 결과 레이어의 `alpha`가 `straight`인지.

그다음 학생과 함께 시작·중간·끝 프레임을 봅니다. 최종 MP4는 전체 디코드하고 색 태그를 확인합니다([rules](rules.md) 10장).

| 멈춘 이유 | 뜻 |
| --- | --- |
| 프로젝트에 이미 항목이 있음 | 아무것도 바꾸지 않았습니다. 새 프로젝트에서 다시 실행합니다 |
| `has no alpha channel` | 결과 시퀀스에 알파가 없습니다. RGBA로 다시 렌더합니다 |
| `imported as N frames` | AE가 읽은 프레임 수가 다릅니다. 폴더 내용을 확인합니다 |
| `is WxH, plate is WxH` | 결과 해상도가 원본과 다릅니다 |
| 색 관리가 OCIO | Project Settings > Color를 Adobe로 둔 프로젝트에서 실행합니다 |
| 결과 기록을 쓰지 못함 | AE 설정 Scripting & Expressions의 'Allow Scripts to Write Files and Access Network'가 꺼져 있으면 컴프는 만들어지고 기록만 빠집니다. 학생이 알림 내용을 알려 주거나 직접 켭니다. 에이전트가 이 설정을 바꾸지 않습니다 |

## 검증 기록

2026-10-28, macOS, After Effects 26.3(26.3x87)에서 이 저장소의 `scripts/ae_comp.py`로 만든 JSX를
합성 시퀀스로 실행했습니다. 결과 기록은 [validation/ae-comp-macos-ae26.3-2026-10-28.json](validation/ae-comp-macos-ae26.3-2026-10-28.json)에 있습니다.
렌더 확인은 `saveFrameToPng`로 뽑은 8비트 PNG입니다.

| 확인 | 결과 |
| --- | --- |
| 프로젝트·컴프·레이어 읽기 | `16 bpc / Rec.709 Gamma 2.4 / linear on`, 640×360 PAR 1, 20프레임, 레이어 3개 |
| CG·그림자 시작 전 프레임(0, 1) | CG·그림자를 끈 컴프와 동일 |
| CG·그림자 밖 | 최대 1/255(8비트 반올림) |
| CG 영역(16bit PNG, 프레임 2·10·19) | 원본이 바뀌어도 동일(불투명) |
| 그림자(검정, 알파 50%) / 원본 밝기 | 0.753, 0.751, 0.750(linear blending) |
| 알파 0 아래의 초록 RGB | 0픽셀 |

함께 확인한 것(세 저장소가 같은 `ae_comp.jsx`를 쓰며 REMOVE 설정으로 시험): 항목이 있는 프로젝트에서는 아무것도 바꾸지 않고 멈춤, 알파 없는 결과로 실패하면
추가 항목이 지워지고 8 bpc·작업 색공간 없음·linear 끔 설정으로 되돌아감, 이 과정에서 AE 경고 창이 뜨지 않음.
실패 후 작업 색공간을 `"None"`으로 되돌리면 AE가 `Profile "None" is missing` 경고 창을 띄워 스크립트가 멈추던
문제를 찾아, 빈 문자열로 해제하도록 고쳤습니다.

아직 확인하지 않은 것: **Windows의 실제 AE**, 실제 학생 샷, 파일 쓰기 설정이 꺼진 AE, OCIO 프로젝트에서의
멈춤(읽기만 하는 검사), AE 26.3 이외 버전.
