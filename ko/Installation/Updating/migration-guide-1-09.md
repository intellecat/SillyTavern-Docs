---
order: 109
route: /ko/installation/updating/migration-guide-1-09/
---

# 1.9.0 마이그레이션 가이드

## main/dev를 사용하는 경우 새 브랜치로 마이그레이션하는 방법은?

_**새로 설치하는 것이 권장됩니다.**_ 그러나 기존 SillyTavern 복사본을 사용하려면 아래 지침을 따르세요.

**중요!** 무엇이든 하기 전에 설치의 *완전한 백업*을 만드세요. 프로세스에서 *데이터를 잃을 수 있으므로* 이 경고를 무시하지 마세요.

백업할 파일을 잘 모르시나요? 여기에서 목록을 확인하세요: [SillyTavern 업데이트 방법](/Installation/Updating/index.md#updating-from-1120-to-1120)

### git 설치

1. SillyTavern 설치 폴더에서 터미널 프롬프트(cmd, PowerShell, Termux 등)를 엽니다.
2. `git fetch`를 입력한 다음 `git pull`을 입력하여 업데이트를 가져옵니다.
3. 설정이 손실될 수 있습니다. 백업을 만들었나요? `git switch release` 또는 `git switch staging`은 각각 브랜치를 변경합니다
4. 오류가 없으면 다음 항목으로 건너뜁니다. 다음과 같은 것이 있을 수 있습니다:
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   영향을 받는 파일 목록이 표시됩니다. 해당 설정 파일이 교체되는 것을 신경 쓰지 않는다면 `git switch -f release` 또는 `git switch -f staging`이 브랜치를 설정합니다.
   해당 변경 사항을 저장하고 싶다면 백업에서 복원하세요.

5. `npm install`을 입력한 다음 `npm run start`를 입력하여 모든 것이 올바르게 작동하는지 테스트합니다.
6. 즐기세요! 필요한 경우 백업에서 데이터를 복원하세요.

### fatal: invalid reference: release

이것은 오래된 원격(조직 저장소로 마이그레이션하기 전)에서 단일 브랜치만 복제한 경우 발생할 수 있습니다. 이 문제를 해결하려면 새 원격에서 브랜치를 추가하고 가져와야 합니다:

```
git remote add st https://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

그런 다음 5단계부터 진행하세요.

### ZIP 설치

아무것도 변경되지 않습니다. 평소처럼 브랜치/릴리스 ZIP을 다운로드하기만 하면 됩니다.
