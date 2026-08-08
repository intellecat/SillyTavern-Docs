---
order: 10
label: Windows
route: /ko/installation/windows/
---
# Windows 설치

!!!warning
Windows가 제어하는 폴더(Program Files, System32 등)에 설치하지 마세요.

관리자 권한으로 START.BAT를 실행하지 마세요

Windows 7에서는 NodeJS 20을 실행할 수 없으므로 설치가 불가능합니다
!!!

## Git를 통한 설치

1. [NodeJS](https://nodejs.org/en) 설치 (최신 LTS 버전 권장)
2. [Git for Windows](https://gitforwindows.org/) 설치
3. Windows 탐색기 열기 (`Win+E`)
4. Windows가 제어하거나 모니터링하지 않는 폴더를 탐색하거나 생성합니다. (예: C:\MySpecialFolder\)
5. 상단의 '주소 표시줄'을 클릭하고 `cmd`를 입력한 다음 Enter를 눌러 해당 폴더 내에서 명령 프롬프트를 엽니다.
6. 검은 박스(명령 프롬프트)가 나타나면 다음 중 하나를 입력하고 Enter를 누릅니다:

   - Release 브랜치의 경우: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - Staging 브랜치의 경우: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. 모든 복제가 완료되면 `Start.bat`를 더블클릭하여 NodeJS가 필요한 요구 사항을 설치하도록 합니다.
8. 그러면 서버가 시작되고 SillyTavern이 브라우저에서 열립니다.

## SillyTavern Launcher를 통한 설치

1.  키보드에서: **`WINDOWS + R`**을 눌러 실행 대화 상자를 엽니다. 그런 다음 다음 명령을 실행하여 git을 설치합니다:
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. 키보드에서: **`WINDOWS + E`**를 눌러 파일 탐색기를 열고 런처를 설치할 폴더로 이동합니다. 원하는 폴더에 들어가면 주소 표시줄에 `cmd`를 입력하고 Enter를 누릅니다. 그런 다음 다음 명령을 실행합니다:
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## GitHub Desktop을 통한 설치
(이 방법은 GitHub Desktop에서**만** git 사용을 허용하며, 명령줄에서도 `git`을 사용하려면 [Git for Windows](https://gitforwindows.org/)도 설치해야 합니다)

1. [NodeJS](https://nodejs.org/en) 설치 (최신 LTS 버전 권장)
2. [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32) 설치
3. GitHub Desktop을 설치한 후 `Clone a repository from the internet....`을 클릭합니다 (참고: 이 단계에서 GitHub 계정을 만들 **필요는 없습니다**)

    ![image](/static/windows-1.png)

4. 메뉴에서 URL 탭을 클릭하고 이 URL `https://github.com/SillyTavern/SillyTavern`을 입력한 다음 Clone을 클릭합니다. Local path를 변경하여 SillyTavern이 다운로드될 위치를 변경할 수 있습니다.

    ![image](/static/windows-2.png)

5. SillyTavern을 열려면 Windows 탐색기를 사용하여 저장소를 복제한 폴더로 이동합니다. 기본적으로 저장소는 여기에 복제됩니다: `C:\Users\[Your Windows Username]\Documents\GitHub\SillyTavern`

6. `start.bat` 파일을 더블클릭합니다. (참고: 파일 이름의 `.bat` 부분이 OS에 의해 숨겨질 수 있으며, 이 경우 "`Start`"라는 파일로 보입니다. 이것을 더블클릭하여 SillyTavern을 실행합니다)

    ![image](/static/windows-3.png)

7. 더블클릭하면 큰 검은색 명령 콘솔 창이 열리고 SillyTavern이 작동에 필요한 것을 설치하기 시작합니다.

8. 설치 프로세스가 끝나고 모든 것이 작동하면 명령 콘솔 창은 다음과 같이 보이고 SillyTavern 탭이 브라우저에서 열려야 합니다:

    ![image](/static/windows-4.png)

9. [지원되는 API](/Usage/API_Connections/index.md)에 연결하고 채팅을 시작하세요!
