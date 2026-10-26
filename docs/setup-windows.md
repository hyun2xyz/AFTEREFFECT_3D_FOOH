# Windows 준비

Windows 10/11 x64 기준입니다. [START-HERE](../START-HERE.md)대로 압축을 푼 폴더를
에이전트와 PowerShell의 작업 폴더로 선택합니다. Git은 필요하지 않습니다.
설치된 도구를 먼저 확인하고 빠진 것만 준비합니다.

```powershell
py -3.13 -B scripts/setup_environment.py --check
```

빠진 항목이 있으면 종료 코드 `1`과 이유를 출력하고 아무것도 바꾸지 않습니다.

## 1. Python

지원 버전은 **3.12 / 3.13**, 첫 설치 권장은 3.13입니다.
[Python 공식 Windows 설치기](https://www.python.org/downloads/windows/)의 3.13 x64 설치기를
쓰거나, WinGet으로 사용자 범위에 설치합니다.

```powershell
winget install --id Python.Python.3.13 --exact --source winget --scope user --architecture x64
```

## 2. FFmpeg

[FFmpeg 공식 Windows 링크](https://ffmpeg.org/download.html#build-windows)가 연결하는
[Gyan release essentials ZIP](https://www.gyan.dev/ffmpeg/builds/)을 받습니다.

```powershell
$ToolRoot = Join-Path $env:LOCALAPPDATA 'AFTEREFFECT_3D_FOOH\tools'
New-Item -ItemType Directory -Path $ToolRoot -Force | Out-Null
$Zip = Join-Path $ToolRoot 'ffmpeg-release-essentials.zip'
Invoke-WebRequest 'https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip' -OutFile $Zip
Invoke-WebRequest 'https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-essentials.zip.sha256' -OutFile "$Zip.sha256"
$Expected = [regex]::Match((Get-Content "$Zip.sha256" -Raw), '\b[a-fA-F0-9]{64}\b').Value
if (-not $Expected -or (Get-FileHash $Zip -Algorithm SHA256).Hash -ne $Expected) { throw 'FFmpeg SHA256 mismatch' }
Expand-Archive -Path $Zip -DestinationPath (Join-Path $ToolRoot 'FFmpeg')
$Ffmpeg = (Get-ChildItem (Join-Path $ToolRoot 'FFmpeg') -Filter ffmpeg.exe -Recurse | Select-Object -First 1).FullName
$Ffprobe = Join-Path (Split-Path $Ffmpeg) 'ffprobe.exe'
```

## 3. Blender LTS

[Blender 공식 LTS 다운로드](https://www.blender.org/download/lts/)에서 최신 LTS의 Windows
x64 설치기를 받아 설치합니다. 기준 사례는 Blender 5.2 LTS에서 작업했습니다.
기본 위치(`C:\Program Files\Blender Foundation\Blender <버전>\blender.exe`)는 자동으로
찾습니다. 다른 위치라면 `--blender`로 지정합니다.

## 4. 프로젝트 설정과 패키지

```powershell
py -3.13 -B scripts/setup_environment.py --configure --ffmpeg $Ffmpeg --ffprobe $Ffprobe
py -3.13 -B scripts/setup_environment.py --install
.\.venv\Scripts\python.exe -B scripts/setup_environment.py --check
```

`--configure`는 찾은 실행 파일의 절대 경로만 `tools.local.json`에 저장합니다.
`--install`은 이 폴더의 `.venv`에 NumPy, OpenCV(headless), Pillow만 설치합니다.
시스템 PATH와 전역 설정은 바꾸지 않습니다.

## 5. ComfyUI와 Comfy 계정

[ComfyUI 공식 다운로드](https://www.comfy.org/download)에서 데스크톱 앱을 설치합니다.
Tripo 같은 Partner(API) 노드는 [공식 안내](https://docs.comfy.org/tutorials/api-nodes/overview)대로
**Settings → User**에서 Comfy 계정으로 로그인하고, **Settings → Credits**에서 선불
크레딧을 충전해야 동작합니다. 무료 사용은 없고, 로그인은 로컬(127.0.0.1)에서 합니다.
계정·결제·약관 동의는 학생이 직접 합니다. ComfyUI를 켜면 `--check`가
`http://127.0.0.1:8188`에 연결되는지 보여 줍니다(포트가 다르면 앱 설정을 확인합니다).

## 6. After Effects

[Adobe 공식 설치 안내](https://helpx.adobe.com/download-install/using/download-install-new-computer.html)를
따라 학생 자신의 계정·라이선스로 설치합니다. 평면 추적 경로(C)를 쓸 때는 함께 설치되는
Mocha AE를 사용합니다. 기본 위치가 아니면 `--after-effects "<AfterFX.exe 경로>"`로 지정합니다.

## 7. COLMAP(경로 B를 고른 경우만)

[AFTEREFFECT_COLMAP](https://github.com/hyun2xyz/AFTEREFFECT_COLMAP)의 Windows 준비 문서를 따릅니다.
