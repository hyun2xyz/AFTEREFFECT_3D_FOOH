# Mac 준비

[START-HERE](../START-HERE.md)대로 압축을 푼 폴더를 에이전트에 엽니다. Git은
필요하지 않습니다. 설치된 도구를 먼저 확인하고 빠진 것만 준비합니다.

```sh
python3.13 -B scripts/setup_environment.py --check
```

## 1. Python과 FFmpeg

[Homebrew 공식 설치 안내](https://brew.sh/)를 확인하고
[Python 3.13](https://formulae.brew.sh/formula/python@3.13)과
[FFmpeg](https://formulae.brew.sh/formula/ffmpeg)을 설치합니다.

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```sh
TASK_BREW="$(command -v brew 2>/dev/null || true)"
[ -x "$TASK_BREW" ] || TASK_BREW=/opt/homebrew/bin/brew
[ -x "$TASK_BREW" ] || TASK_BREW=/usr/local/bin/brew
"$TASK_BREW" install python@3.13 ffmpeg
TASK_PYTHON="$("$TASK_BREW" --prefix python@3.13)/bin/python3.13"
```

## 2. Blender LTS

[Blender 공식 LTS 다운로드](https://www.blender.org/download/lts/)에서 최신 LTS의 macOS
버전(Apple Silicon 또는 Intel)을 받아 `/Applications`에 설치합니다. 기준 사례는
Blender 5.2 LTS에서 작업했습니다. `/Applications/Blender.app`은 자동으로 찾습니다.

## 3. 프로젝트 설정과 패키지

```sh
"$TASK_PYTHON" -B scripts/setup_environment.py --configure
"$TASK_PYTHON" -B scripts/setup_environment.py --install
.venv/bin/python -B scripts/setup_environment.py --check
```

경로가 다르면 `--ffmpeg`, `--ffprobe`, `--blender`로 실제 파일을 지정합니다.
`--install`은 이 폴더의 `.venv`에 NumPy, OpenCV(headless), Pillow만 설치합니다.

## 4. ComfyUI와 Comfy 계정

[ComfyUI 공식 다운로드](https://www.comfy.org/download)에서 데스크톱 앱을 설치합니다.
Tripo 같은 Partner(API) 노드는 [공식 안내](https://docs.comfy.org/tutorials/api-nodes/overview)대로
**Settings → User**에서 Comfy 계정으로 로그인하고, **Settings → Credits**에서 선불
크레딧을 충전해야 동작합니다. 무료 사용은 없습니다. 계정·결제·약관 동의는 학생이 직접 합니다.

## 5. After Effects

[Adobe 공식 설치 안내](https://helpx.adobe.com/download-install/using/download-install-new-computer.html)를
따라 학생 자신의 계정·라이선스로 설치합니다. 평면 추적 경로(C)에는 함께 설치되는
Mocha AE를 씁니다.

## 6. COLMAP(경로 B를 고른 경우만)

[AFTEREFFECT_COLMAP](https://github.com/hyun2xyz/AFTEREFFECT_COLMAP)의 Mac 준비 문서를 따릅니다.
