---
order: -20
icon: file-added
templating: false
route: /ko/for-contributors/writing-extensions/
label: UI 익스텐션
---

# UI 익스텐션

UI 익스텐션은 SillyTavern의 이벤트와 API에 연결하여 기능을 확장합니다. 브라우저 컨텍스트에서 실행되며 DOM, JavaScript API 및 SillyTavern 컨텍스트에 대한 거의 무제한의 액세스 권한이 있습니다. 익스텐션은 UI를 수정하고, 내부 API를 호출하며, 채팅 데이터와 상호 작용할 수 있습니다. 이 가이드는 자신만의 익스텐션을 만드는 방법을 설명합니다(JavaScript 지식이 필요합니다).

!!!tip 그냥 익스텐션을 설치하고 싶으신가요?
여기로 가세요: [익스텐션](../extensions/index.md).
!!!

Node.js 서버의 기능을 확장하려면 [서버 플러그인](./Server-Plugins.md) 페이지를 참조하세요.

**JavaScript를 작성할 수 없나요?**

* 본격적인 익스텐션을 작성하는 대신 더 간단한 대안으로 [STscript](./st-script.md)를 고려하세요.
* [MDN 과정](https://developer.mozilla.org/en-US/docs/Learn/JavaScript)을 진행하고 완료되면 돌아오세요.

## 익스텐션 제출

[공식 콘텐츠 저장소](https://github.com/SillyTavern/SillyTavern-Content)에 익스텐션을 기여하고 싶으신가요? 연락주세요!

모든 익스텐션이 안전하고 사용하기 쉽도록 하기 위해 몇 가지 요구 사항이 있습니다:

1. 익스텐션은 오픈 소스여야 하며 자유 라이선스가 있어야 합니다([라이선스 선택](https://choosealicense.com/licenses/) 참조). 확실하지 않은 경우 AGPLv3가 좋은 선택입니다.
2. 익스텐션은 최신 릴리스 버전의 SillyTavern과 호환되어야 합니다. 핵심에 변경 사항이 있으면 익스텐션을 업데이트할 준비를 하세요.
3. 익스텐션은 잘 문서화되어야 합니다. 여기에는 설치 지침, 사용 예제 및 기능 목록이 포함된 README 파일이 포함됩니다.
4. 기능하기 위해 서버 플러그인이 필요한 익스텐션은 허용되지 않습니다.

## 예제

간단한 SillyTavern 익스텐션의 라이브 예제를 참조하세요:

* <https://github.com/city-unit/st-extension-example> - 기본 익스텐션 템플릿. 매니페스트 생성, 로컬 스크립트 가져오기, 설정 UI 패널 추가 및 영구 익스텐션 설정 사용을 보여줍니다.
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - GitHub의 모든 공식 SillyTavern 익스텐션 목록.

## 번들링

익스텐션은 번들링을 활용하여 나머지 모듈과 격리하고 Vue, React 등의 UI 프레임워크를 포함한 NPM의 모든 종속성을 사용할 수도 있습니다.

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - TypeScript와 Webpack을 사용하는 익스텐션의 템플릿 저장소(React 없음).
* <https://github.com/SillyTavern/Extension-ReactTemplate> - React와 Webpack을 사용하는 기본 익스텐션의 템플릿 저장소.

번들에서 상대 가져오기를 사용하려면 가져오기 래퍼를 만들어야 할 수 있습니다. 다음은 Webpack의 예입니다:

```js
/**
 * Import a member from a module by URL, bypassing webpack.
 * @param {string} url URL to import from
 * @param {string} what Name of the member to import
 * @param {any} defaultValue Fallback value
 * @returns {Promise<any>} Imported member
 */
export async function importFromUrl(url, what, defaultValue = null) {
    try {
        const module = await import(/* webpackIgnore: true */ url);
        if (!Object.hasOwn(module, what)) {
            throw new Error(`No ${what} in module`);
        }
        return module[what];
    } catch (error) {
        console.error(`Failed to import ${what} from ${url}: ${error}`);
        return defaultValue;
     }
}

// 'script.js' 모듈에서 함수 가져오기
const generateRaw = await importFromUrl('/script.js', 'generateRaw');
```

## manifest.json

모든 익스텐션에는 `data/<user-handle>/extensions`에 폴더가 있어야 하며 익스텐션의 진입점인 JS 스크립트 파일의 경로와 메타데이터가 포함된 `manifest.json` 파일이 있어야 합니다.

다운로드 가능한 익스텐션은 HTTP를 통해 제공될 때 `/scripts/extensions/third-party` 폴더에 마운트되므로 이를 기반으로 상대 가져오기를 사용해야 합니다. 로컬 개발을 쉽게 하려면 `/scripts/extensions/third-party` 폴더에 익스텐션 저장소를 배치하는 것을 고려하세요("모든 사용자에게 설치" 옵션).

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "dependencies": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/extension",
    "auto_update": true,
    "minimum_client_version": "1.0.0",
    "i18n": {
        "de-de": "i18n/de-de.json"
    },
    "hooks": {
        "install": "onInstall",
        "update": "onUpdate",
        "delete": "onDelete",
        "enable": "onEnable",
        "disable": "onDisable",
        "activate": "onActivate"
    }
}
```

### 매니페스트 필드

* `display_name`은 필수입니다. "익스텐션 관리" 메뉴에 표시됩니다.
* `loading_order`는 선택 사항입니다. 숫자가 높을수록 나중에 로드됩니다.
* `js`는 메인 JS 파일 참조이며 필수입니다.
* `css`는 선택적 스타일 파일 참조입니다.
* `author`는 필수입니다. 작성자의 이름 또는 연락처 정보를 포함해야 합니다.
* `auto_update`는 ST 패키지 버전이 변경될 때 익스텐션이 자동 업데이트되어야 하는 경우 `true`로 설정됩니다.
* `i18n`은 지원되는 로케일과 해당 JSON 파일을 지정하는 선택적 객체입니다(아래 참조).
* `dependencies`는 이 익스텐션이 의존하는 다른 **익스텐션**을 지정하는 선택적 문자열 배열입니다.
* `generate_interceptor`는 텍스트 생성 요청에서 호출되는 전역 함수의 이름을 지정하는 선택적 문자열입니다.
* `minimum_client_version`은 이 익스텐션이 작동하는 데 필요한 최소 SillyTavern 버전을 지정하는 선택적 문자열입니다.
* `hooks`는 JS 진입점 모듈에서 내보낸 [라이프사이클 훅](#lifecycle-hooks) 함수 이름을 지정하는 선택적 객체입니다.

### 종속성

익스텐션은 다른 SillyTavern 익스텐션에 의존할 수도 있습니다. 이러한 종속성이 누락되거나 비활성화된 경우 익스텐션이 로드되지 않습니다.

종속성은 `public/extensions` 디렉토리에 나타나는 **폴더 이름**으로 지정됩니다.

예제:

* 내장 익스텐션: `"vectors"`, `"caption"`
* 서드파티 익스텐션: `"third-party/Extension-WebLLM"`, `"third-party/Extension-Mermaid"`

### 더 이상 사용되지 않는 필드

* `requires`는 필수 **Extras 모듈**을 지정하는 선택적 문자열 배열입니다. 연결된 Extras API가 나열된 모든 모듈을 제공하지 않으면 익스텐션이 로드되지 않습니다.
* `optional`은 선택적 **Extras 모듈**을 지정하는 선택적 문자열 배열입니다. 이들이 누락되어도 익스텐션은 여전히 로드되며 익스텐션은 이들의 부재를 우아하게 처리해야 합니다.

연결된 Extras API에서 현재 제공하는 모듈을 확인하려면 `scripts/extensions.js`에서 `modules` 배열을 가져옵니다.


## 스크립팅

### 익스텐션 초기화를 위한 모범 사례

* 차단 로더가 활성화된 SillyTavern의 로딩 단계 중에 실행되어야 하는 동기 설정에는 `activate` 훅을 사용하세요.
* 모든 익스텐션과 UI 요소가 로드 및 설정된 후, 로더가 여전히 차단 중일 때 실행되어야 하는 설정에는 `APP_INITIALIZED` 이벤트를 사용하세요.
* SillyTavern이 사용 준비를 마치는 것을 차단할 필요가 없는 비동기 설정에는 `APP_READY` 이벤트를 사용하세요. 이벤트 핸들러는 대기(await)되므로 타이머나 유사한 메커니즘을 사용하여 처리를 지연시켜야 합니다.

### getContext 사용

`SillyTavern` 전역 객체의 `getContext()` 함수는 모든 메인 앱 상태 객체, 유용한 함수 및 유틸리티의 모음인 SillyTavern 컨텍스트에 대한 액세스를 제공합니다.

```js
const context = SillyTavern.getContext();
context.chat; // 채팅 로그 - MUTABLE
context.characters; // 캐릭터 목록
context.characterId; // 현재 캐릭터의 인덱스
context.groups; // 그룹 목록
context.groupId; // 현재 그룹의 ID
// 그 외 다수...
```

[SillyTavern 소스 코드](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js)에서 사용 가능한 속성 및 함수의 전체 목록을 찾을 수 있습니다.

!!!
`getContext`에 함수/속성이 누락된 경우 개발자에게 연락하거나 풀 리퀘스트를 보내주세요!
!!!

### 공유 라이브러리

SillyTavern 프론트엔드에서 내부적으로 사용하는 대부분의 npm 라이브러리는 `SillyTavern` 전역 객체의 `libs` 속성에서 공유됩니다.

* `lodash` - 유틸리티 라이브러리. [문서](https://lodash.com/).
* `Fuse` - 퍼지 검색 라이브러리. [문서](https://www.fusejs.io/).
* `DOMPurify` - HTML 새니타이제이션 라이브러리. [문서](https://github.com/cure53/DOMPurify).
* `hljs` - 구문 강조 라이브러리. [문서](https://highlightjs.org/).
* `localforage` - 브라우저 스토리지 라이브러리(IndexedDB/localStorage 추상화). [문서](https://localforage.github.io/localForage/).
* `Handlebars` - 템플릿 라이브러리. [문서](https://handlebarsjs.com/).
* `css` - CSS 파싱/문자열화 도구. [문서](https://github.com/nicolo-ribaudo/css-tools).
* `Bowser` - 브라우저/플랫폼 감지 라이브러리. [문서](https://github.com/bowser-js/bowser).
* `DiffMatchPatch` - 텍스트 diff, match, patch 라이브러리. [문서](https://github.com/google/diff-match-patch).
* `Readability` / `isProbablyReaderable` - Mozilla의 기사 추출 라이브러리. [문서](https://github.com/mozilla/readability).
* `SVGInject` - 인라인 SVG 주입 라이브러리. [문서](https://github.com/nicolo-ribaudo/svg-inject).
* `showdown` - Markdown 변환 라이브러리. [문서](https://showdownjs.com/).
* `moment` - 날짜/시간 조작 라이브러리. [문서](http://momentjs.com/).
* `seedrandom` - 시드 기반 난수 생성기. [문서](https://github.com/davidbau/seedrandom).
* `Popper` - 툴팁/팝오버 위치 지정 엔진. [문서](https://popper.js.org/).
* `droll` - 주사위 굴림 라이브러리. [문서](https://github.com/thebinarypenguin/droll).
* `morphdom` - 빠른 DOM 비교/패치 라이브러리. [문서](https://github.com/patrick-steele-iber/morphdom).
* `slideToggle` - 순수 JS 슬라이드 토글 애니메이션. [문서](https://github.com/nicolo-ribaudo/slidetoggle).
* `chalk` - 터미널 문자열 스타일링(브라우저에서는 제한적으로 사용). [문서](https://github.com/chalk/chalk).
* `yaml` - YAML 파서 및 문자열화 도구. [문서](https://eemeli.org/yaml/).
* `chevrotain` - 파서 구축 툴킷. [문서](https://chevrotain.io/).
* `gzipSync` / `gzip` - fflate의 빠른 압축 유틸리티. [문서](https://github.com/101arrowz/fflate).

[SillyTavern 소스 코드](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js)에서 내보낸 라이브러리의 전체 목록을 찾을 수 있습니다.

**예제:** DOMPurify 라이브러리 사용.

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### TypeScript 참고 사항

`getContext()` 및 `libs`를 포함하여 `SillyTavern` 전역 객체의 모든 메서드에 대한 자동 완성에 액세스하려면(아마 원하실 겁니다) TypeScript `.d.ts` 모듈 선언을 추가해야 합니다. 이 선언은 익스텐션의 위치에 따라 SillyTavern의 소스에서 전역 타입을 가져와야 합니다. 다음은 "모든 사용자" 및 "현재 사용자" 두 가지 설치 유형 모두에서 작동하는 예입니다.

**global.d.ts** - 이 파일을 익스텐션 디렉토리의 루트(`manifest.json` 옆)에 배치합니다:

```ts
export {};

// 1. 사용자 범위 익스텐션용 가져오기
import '../../../../public/global';
// 2. 서버 범위 익스텐션용 가져오기
import '../../../../global';

// 필요한 경우 추가 타입 정의...
declare global {
    // 여기에 전역 타입 선언 추가
}
```

### HTML 템플릿

익스텐션은 Handlebars HTML 템플릿을 사용하여 UI를 구축할 수 있습니다. `.html` 템플릿 파일을 익스텐션 디렉토리에 배치하고 `getContext()`의 `renderExtensionTemplateAsync()` 함수를 사용하여 렌더링합니다.

이 함수는 익스텐션의 폴더 이름, 템플릿 파일 이름(`.html` 제외), 그리고 Handlebars 템플릿 변수를 위한 선택적 데이터 객체를 인수로 받습니다. 반환된 HTML은 자동으로 DOMPurify로 새니타이즈되고 `data-i18n` 속성으로 로컬라이즈됩니다.

```js
const { renderExtensionTemplateAsync } = SillyTavern.getContext();

// 주어진 데이터로 'third-party/my-extension/settings.html'을 렌더링합니다
const settingsHtml = await renderExtensionTemplateAsync(
    'third-party/my-extension',
    'settings',
    { title: 'My Extension', version: '1.0', defaultValue: 'test' }
);

// 익스텐션 설정 패널에 추가
$('#extensions_settings2').append(settingsHtml);
```

**템플릿 파일 예제** (`settings.html`):

```html
<div class="my-extension-settings">
    <div class="inline-drawer">
        <div class="inline-drawer-toggle inline-drawer-header">
            <b data-i18n="{{title}}">{{title}}</b>
            <div class="inline-drawer-icon fa-solid fa-circle-chevron-down down"></div>
        </div>
        <div class="inline-drawer-content">
            <label for="my_ext_option">
                <span data-i18n="Option">Option</span>
            </label>
            <input id="my_ext_option" type="text" value="{{defaultValue}}" />
        </div>
    </div>
</div>
```

!!!warning
`renderExtensionTemplate()`(동기 버전)은 더 이상 사용되지 않습니다. 항상 `renderExtensionTemplateAsync()`를 대신 사용하세요.
!!!

### 다른 파일에서 가져오기

!!!warning
SillyTavern 코드에서 가져오기를 사용하는 것은 신뢰할 수 없으며 ST 모듈의 내부 구조가 변경되면 언제든지 중단될 수 있습니다. `getContext`는 보다 안정적인 API를 제공합니다.
!!!

번들 익스텐션을 빌드하지 않는 한 다른 JS 파일에서 변수와 함수를 가져올 수 있습니다.

예를 들어, 이 코드 스니펫은 백그라운드에서 현재 선택된 API의 응답을 생성합니다:

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt({ quietPrompt: text });
    // ...
}
```

## 상태 관리

### 영구 설정

익스텐션이 상태를 유지해야 하는 경우 `getContext()` 함수의 `extensionSettings` 객체를 사용하여 데이터를 저장하고 검색할 수 있습니다. 익스텐션은 설정 객체에 JSON 직렬화 가능한 데이터를 저장할 수 있으며 다른 익스텐션과의 충돌을 피하기 위해 고유한 키를 사용해야 합니다.

설정을 유지하려면 `saveSettingsDebounced()` 함수를 사용하여 설정을 서버에 저장합니다.

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// 익스텐션의 고유 식별자 정의
const MODULE_NAME = 'my_extension';

// 기본 설정 정의
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// 설정을 가져오거나 초기화하는 함수 정의
function getSettings() {
    // 설정이 없으면 초기화
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // 모든 기본 키가 존재하는지 확인(업데이트 후 유용)
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// 설정 사용
const settings = getSettings();
settings.option1 = 'new value';

// 설정 저장
saveSettingsDebounced();
```

### 채팅 메타데이터

특정 채팅에 일부 데이터를 바인딩하려면 `getContext()` 함수의 `chatMetadata` 객체를 사용할 수 있습니다. 이 객체를 사용하면 채팅과 관련된 임의의 데이터를 저장할 수 있으며, 이는 익스텐션별 상태를 저장하는 데 유용할 수 있습니다.

메타데이터를 유지하려면 `saveMetadata()` 함수를 사용하여 메타데이터를 서버에 저장합니다.

!!!warning
채팅이 전환될 때 참조가 변경되므로 `chatMetadata`에 대한 참조를 수명이 긴 변수에 저장하지 마세요. 현재 채팅 메타데이터에 액세스하려면 항상 `SillyTavern.getContext().chatMetadata`를 사용하세요.
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// 현재 채팅에 대한 메타데이터 설정
chatMetadata['my_key'] = 'my_value';

// 현재 채팅에 대한 메타데이터 가져오기
const value = chatMetadata['my_key'];

// 메타데이터를 서버에 저장
await saveMetadata();
```

!!!tip
채팅이 전환되면 `CHAT_CHANGED` 이벤트가 발생하므로 이 이벤트를 수신하여 익스텐션의 상태를 적절히 업데이트할 수 있습니다. [이벤트 수신](#listening-to-events) 섹션에서 자세히 참조하세요.
!!!

### 캐릭터 카드

SillyTavern은 캐릭터 카드 JSON 데이터에 임의의 데이터를 저장할 수 있는 [캐릭터 카드 V2 사양](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md)을 완전히 지원합니다.

이는 캐릭터와 관련된 추가 데이터를 저장하고 캐릭터 카드를 내보낼 때 공유할 수 있도록 하려는 익스텐션에 유용합니다.

캐릭터 카드 [익스텐션](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) 데이터 필드에 데이터를 쓰려면 `getContext()` 함수의 `writeExtensionField` 함수를 사용합니다. 이 함수는 캐릭터 ID, 문자열 키 및 쓸 값을 사용합니다. 값은 JSON 직렬화 가능해야 합니다.

!!!warning 이상한 점
`characterId`라고 불리지만 "실제" 고유 식별자가 아니라 `characters` 배열의 캐릭터 인덱스입니다.

현재 캐릭터의 인덱스는 컨텍스트의 `characterId` 속성으로 제공됩니다. 현재 선택한 캐릭터에 데이터를 쓰려면 `SillyTavern.getContext().characterId`를 사용합니다. 다른 캐릭터의 데이터를 저장해야 하는 경우 `characters` 배열에서 캐릭터를 검색하여 인덱스를 찾습니다.

**주의: 그룹 채팅이나 캐릭터가 선택되지 않은 경우 `characterId`는 `undefined`입니다!**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// 캐릭터 카드에 데이터 쓰기
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// 캐릭터 카드에서 데이터 읽기
const character = SillyTavern.getContext().characters[characterId];
// 데이터는 캐릭터 데이터의 `extensions` 객체에 저장됩니다
const myData = character.data?.extensions?.my_extension_key;
```

### 설정 프리셋

임의의 JSON 데이터는 메인 API 타입의 설정 프리셋에 저장할 수 있습니다. 프리셋 JSON과 함께 내보내고 가져오므로 프리셋에 대한 익스텐션별 설정을 저장하는 데 사용할 수 있습니다. 다음 API 타입은 설정 프리셋에서 데이터 익스텐션을 지원합니다:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

데이터를 읽거나 쓰려면 먼저 컨텍스트에서 PresetManager 인스턴스를 가져와야 합니다:

```js
const { getPresetManager } = SillyTavern.getContext();

// 현재 API 타입의 프리셋 매니저 가져오기
const pm = getPresetManager();

// 프리셋 익스텐션 필드에 데이터 쓰기:
// - path: 프리셋 데이터의 필드 경로
// - value: 쓸 값
// - name (선택 사항): 쓸 프리셋의 이름, 기본값은 현재 선택된 프리셋
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// 프리셋 익스텐션 필드에서 데이터 읽기:
// - path: 프리셋 데이터의 필드 경로
// - name (선택 사항): 읽을 프리셋의 이름, 기본값은 현재 선택된 프리셋
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
프리셋이 변경되거나 메인 API가 전환되면 `PRESET_CHANGED` 및 `MAIN_API_CHANGED` 이벤트가 발생하므로 이러한 이벤트를 수신하여 익스텐션의 상태를 적절히 업데이트할 수 있습니다. [이벤트 수신](#listening-to-events) 섹션에서 자세히 참조하세요.
!!!

## 국제화

!!!
번역 제공에 대한 일반 정보는 [국제화](/For_Contributors/i18n.md) 페이지를 참조하세요.
!!!

익스텐션은 HTML 템플릿의 `t`, `translate` 함수 및 `data-i18n` 속성과 함께 사용할 추가 로컬라이즈된 문자열을 제공할 수 있습니다.

여기에서 지원되는 로케일 목록을 참조하세요(`lang` 키): <https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### 직접 `addLocaleData` 호출

로케일 코드와 번역이 포함된 객체를 `addLocaleData` 함수에 전달합니다. 기존 키의 재정의는 *허용되지 않습니다*. 전달된 로케일 코드가 현재 선택한 로케일이 아닌 경우 데이터는 자동으로 무시됩니다.

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### 익스텐션 매니페스트를 통해

지원되는 로케일과 해당 JSON 파일 경로(익스텐션 디렉토리 기준 상대 경로) 목록이 포함된 i18n 객체를 매니페스트에 추가합니다.

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // 나머지 필드
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## 슬래시 명령 등록 (새로운 방법)

`registerSlashCommand`는 이전 버전과의 호환성을 위해 여전히 존재하지만, 새로운 슬래시 명령은 이제 `SlashCommandParser.addCommandObject()`를 통해 등록하여 명령과 매개변수에 대한 확장된 세부 정보를 파서(그리고 자동 완성 및 명령 도움말)에 제공해야 합니다.

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

등록된 모든 명령은 [STscript](/For_Contributors/st-script.md)에서 가능한 모든 방식으로 사용할 수 있습니다.

## 이벤트

### 이벤트 수신

`eventSource.on(eventType, eventHandler)`를 사용하여 이벤트를 수신합니다:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // 메시지 처리
}
```

주요 이벤트 타입:

**앱 라이프사이클:**

* `APP_INITIALIZED`: 앱이 초기화되고 거의 준비되었지만 로더는 여전히 표시되어 있습니다. 여기서 UI 수정을 수행할 수 있습니다. 앱이 초기화된 후 새 리스너가 연결될 때마다 자동으로 발생합니다.
* `APP_READY`: 앱이 완전히 로드되고 사용할 준비가 되었습니다. 앱이 준비된 후 새 리스너가 연결될 때마다 자동으로 발생합니다.

**메시지:**

* `MESSAGE_SENT`: 메시지가 사용자에 의해 전송되고 `chat` 객체에 기록되었지만 아직 UI에 렌더링되지 않았습니다.
* `MESSAGE_RECEIVED`: LLM 메시지가 생성되고 `chat` 객체에 기록되었지만 아직 UI에 렌더링되지 않았습니다.
* `USER_MESSAGE_RENDERED`: 사용자가 보낸 메시지가 UI에 렌더링되었습니다.
* `CHARACTER_MESSAGE_RENDERED`: 생성된 LLM 메시지가 UI에 렌더링되었습니다.
* `MESSAGE_EDITED`: 메시지가 사용자에 의해 편집되었습니다.
* `MESSAGE_DELETED`: 메시지가 삭제되었습니다.
* `MESSAGE_SWIPED`: 메시지 스와이프가 트리거되었습니다.
* `STREAM_TOKEN_RECEIVED`: 스트리밍 생성 중 새 토큰을 받았습니다.

**생성:**

* `GENERATION_AFTER_COMMANDS`: 슬래시 명령 처리 후 생성이 시작되려고 합니다.
* `GENERATION_STARTED`: 생성이 시작되었습니다.
* `GENERATION_STOPPED`: 사용자가 생성을 중지했습니다.
* `GENERATION_ENDED`: 생성이 완료되었거나 오류가 발생했습니다.

**채팅:**

* `CHAT_CHANGED`: 채팅이 전환되었습니다(예: 다른 캐릭터로 전환하거나 다른 채팅이 로드됨).
* `CHAT_CREATED`: 새 채팅이 생성되었습니다.
* `CHAT_DELETED`: 채팅이 삭제되었습니다.

**캐릭터:**

* `CHARACTER_EDITED`: 캐릭터의 데이터가 변경되었습니다.
* `CHARACTER_DELETED`: 캐릭터가 삭제되었습니다.
* `CHARACTER_DUPLICATED`: 캐릭터가 복제되었습니다.

**페르소나:**

* `PERSONA_CHANGED`: 활성 페르소나가 변경되었습니다.
* `PERSONA_CREATED`: 새 페르소나가 생성되었습니다.
* `PERSONA_UPDATED`: 페르소나가 업데이트되었습니다.
* `PERSONA_RENAMED`: 페르소나의 이름이 변경되었습니다.
* `PERSONA_DELETED`: 페르소나가 삭제되었습니다.

**설정 및 프리셋:**

* `SETTINGS_UPDATED`: 애플리케이션 설정이 업데이트되었습니다.
* `PRESET_CHANGED`: 활성 프리셋이 변경되었습니다.
* `MAIN_API_CHANGED`: 메인 API 타입이 전환되었습니다.
* `CHATCOMPLETION_SOURCE_CHANGED`: chat completion 소스가 변경되었습니다.
* `CHATCOMPLETION_MODEL_CHANGED`: chat completion 모델이 변경되었습니다.
* `CONNECTION_PROFILE_LOADED`: 연결 프로필이 로드되었습니다.

**World Info:**

* `WORLDINFO_UPDATED`: world info 데이터가 업데이트되었습니다.
* `WORLDINFO_SETTINGS_UPDATED`: world info 설정이 변경되었습니다.

**도구 호출:**

* `TOOL_CALLS_PERFORMED`: 도구 호출이 실행되었습니다.
* `TOOL_CALLS_RENDERED`: 도구 호출 결과가 채팅에 렌더링되었습니다.

**텍스트 음성 변환(TTS):**

* `TTS_JOB_STARTED`: TTS 작업이 시작되었습니다.
* `TTS_AUDIO_READY`: TTS 오디오 데이터가 재생 준비되었습니다.
* `TTS_JOB_COMPLETE`: TTS 작업이 완료되었습니다.

전체 이벤트 타입 목록은 [소스에서](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js) 찾을 수 있습니다.

!!!info 이벤트 데이터
각 이벤트가 리스너에게 데이터를 전달하는 방식은 균일하지 않습니다. 일부 이벤트는 데이터를 내보내지 않습니다. 일부는 객체 또는 원시 값을 전달합니다. 이벤트가 전달하는 데이터를 확인하려면 이벤트가 발생하는 소스 코드를 참조하거나 디버거로 확인하세요.
!!!

### 이벤트 발생

익스텐션에서 `eventSource.emit(eventType, ...eventData)`를 호출하여 사용자 정의 이벤트를 포함한 애플리케이션 이벤트를 생성할 수 있습니다:

```js
const { eventSource } = SillyTavern.getContext();

// 내장 event_types 필드 또는 모든 문자열일 수 있습니다.
const eventType = 'myCustomEvent';

// 실행을 계속하기 전에 모든 이벤트 핸들러가 완료되도록 하려면 `await`를 사용합니다.
await eventSource.emit(eventType, { data: 'custom event data' });
```

## 프롬프트 인터셉터

프롬프트 인터셉터는 익스텐션이 텍스트 생성 요청이 이루어지기 전에 채팅 데이터 수정, 주입 추가 또는 생성 중단과 같은 활동을 수행할 수 있는 방법을 제공합니다.

다른 익스텐션의 인터셉터는 순차적으로 실행됩니다. 순서는 각각의 `manifest.json` 파일의 `loading_order` 필드에 의해 결정됩니다. `loading_order` 값이 낮은 익스텐션이 먼저 실행됩니다. `loading_order`가 지정되지 않은 경우 `display_name`이 대체로 사용됩니다. 둘 다 지정되지 않은 경우 순서는 정의되지 않습니다.

### 인터셉터 등록

프롬프트 인터셉터를 정의하려면 익스텐션의 `manifest.json` 파일에 `generate_interceptor` 필드를 추가합니다. 값은 SillyTavern에서 호출할 전역 함수의 이름이어야 합니다.

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // 실행 순서에 영향
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... 기타 매니페스트 속성
}
```

### 인터셉터 함수

`generate_interceptor` 함수는 드라이 런이 아닌 생성 요청 시 호출되는 전역 함수입니다. 전역 범위에서 정의되어야 하며(예: `globalThis.myCustomInterceptorFunction = async function(...) { ... }`) 비동기 작업을 수행해야 하는 경우 `Promise`를 반환할 수 있습니다.

인터셉터 함수는 다음 인수를 받습니다:

* `chat`: 프롬프트 빌드에 사용될 채팅 기록을 나타내는 메시지 객체 배열입니다. 이 배열을 직접 수정할 수 있습니다(예: 메시지 추가, 제거 또는 변경). 메시지는 변경 가능하므로 배열에 대한 변경 사항은 실제 채팅 기록에 반영됩니다. 변경 사항을 임시로 유지하려면 `structuredClone`을 사용하여 메시지 객체의 딥 카피를 만드세요.
* `contextSize`: 다가오는 생성을 위해 계산된 현재 컨텍스트 크기(토큰 단위)를 나타내는 숫자입니다.
* `abort`: 호출되면 텍스트 생성이 진행되지 않도록 신호를 보내는 함수입니다. `true`인 경우 후속 인터셉터가 실행되지 않도록 하는 부울 매개변수를 허용합니다.
* `type`: 생성의 타입 또는 트리거를 나타내는 문자열입니다(예: `'quiet'`, `'regenerate'`, `'impersonate'`, `'swipe'` 등). 이를 통해 인터셉터는 생성이 시작된 방법에 따라 조건부로 논리를 적용할 수 있습니다.

**예제 구현:**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // 예제: 마지막 사용자 메시지 앞에 시스템 노트 추가
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // 마지막 메시지 앞에 삽입
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## 라이프사이클 훅

익스텐션은 익스텐션의 라이프사이클에서 특정 시점에 호출되는 라이프사이클 훅을 `manifest.json`에서 정의할 수 있습니다. 각 훅은 익스텐션의 JS 진입점 모듈(`js` 필드에 지정된 파일)에서 **내보낸 함수**에 매핑됩니다.

모든 훅은 선택 사항입니다. 훅 함수는 대기(await)될 `Promise`를 반환할 수 있습니다(5초 타임아웃 있음). 훅이 타임아웃을 초과하면 경고가 기록되고 실행은 계속됩니다. 훅에서 발생하는 오류는 캐치되어 기록되며 작업을 차단하지 않습니다.

### 사용 가능한 훅

| 훅 | 호출 시점 |
|------|-----------------|
| `activate` | 페이지 로드 중 익스텐션이 성공적으로 활성화될 때 |
| `install` | 익스텐션이 설치되고 설정이 로드된 후 |
| `update` | 익스텐션 업데이트 성공 후(다시 로드 토스트 전) |
| `delete` | 익스텐션이 서버에서 삭제되기 전 |
| `enable` | 익스텐션이 활성화되고 설정이 저장되기 전 |
| `disable` | 익스텐션이 비활성화되고 설정이 저장되기 전 |
| `clean` | 사용자가 익스텐션 관리자에서 "익스텐션 데이터 지우기" 버튼을 클릭하거나, 익스텐션 삭제 시 정리 옵션을 선택할 때 |

### 매니페스트 구성

훅 이름을 내보낸 함수 이름에 매핑하는 `hooks` 객체를 `manifest.json`에 추가합니다:

```json
{
    "display_name": "My Extension",
    "js": "index.js",
    // 다른 필드들...
    "hooks": {
        "install": "onInstall",
        "update": "onUpdate",
        "delete": "onDelete",
        "enable": "onEnable",
        "disable": "onDisable",
        "activate": "onActivate",
        "clean": "onClean"
    }
}
```

이름은 유효한 JS 함수 이름인 한 자유롭게 선택할 수 있습니다.
원하는 만큼의 훅을 구성할 수 있으며, 모두 구현할 필요는 없습니다.

### 구현

메인 JS 진입점에서 훅 함수를 내보냅니다. 각 함수는 인수를 받지 않으며 선택적으로 `Promise`를 반환할 수 있습니다:

```js
// index.js - 익스텐션의 진입점

export async function onInstall() {
    console.log('Extension installed! Performing first-time setup...');
    // 예: 기본 데이터 초기화, 저장소 항목 생성
}

export async function onActivate() {
    console.log('Extension activated during page load');
}

export async function onUpdate() {
    console.log('Extension updated! Running migrations...');
    // 예: 이전 형식에서 새 형식으로 데이터 마이그레이션
}

export async function onDelete() {
    console.log('Extension about to be deleted. Cleaning up...');
    // 예: 저장된 데이터 제거, localStorage 정리
    const { localforage } = SillyTavern.libs;
    await localforage.removeItem('my_extension_data');
}

export function onEnable() {
    console.log('Extension enabled');
}

export function onDisable() {
    console.log('Extension disabled');
}

export async function onClean() {
    console.log('Extension data cleaned');
    // 예: 여기서 익스텐션의 데이터를 정리
}
```

!!!warning
훅 함수에는 **5초 타임아웃**이 있습니다. 훅이 더 오래 걸리면 실행이 계속되고 경고가 기록됩니다. 훅 로직은 빠르고 가볍게 유지하세요.
!!!

## 텍스트 생성

SillyTavern은 현재 선택한 LLM API를 사용하여 다양한 컨텍스트에서 텍스트를 생성하는 여러 함수를 제공합니다. 이러한 함수를 사용하면 채팅 컨텍스트에서 텍스트를 생성하거나, 컨텍스트 없이 원시 생성을 하거나, 구조화된 출력을 사용할 수 있습니다.

### 채팅 컨텍스트 내에서

`generateQuietPrompt()` 함수는 백그라운드에서 추가된 "quiet" 프롬프트(히스토리 이후 지시)와 함께 채팅 컨텍스트에서 텍스트를 생성하는 데 사용됩니다(출력은 UI에 렌더링되지 않음). 이는 요약 또는 이미지 프롬프트 생성과 같이 관련 채팅 및 캐릭터 데이터를 그대로 유지하면서 사용자 경험을 방해하지 않고 텍스트를 생성하는 데 유용합니다.

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### 원시 생성

`generateRaw()` 함수는 채팅 컨텍스트 없이 텍스트를 생성하는 데 사용됩니다. 프롬프트 빌드 프로세스를 완전히 제어하려는 경우에 유용합니다.

Text Completion 문자열 또는 Chat Completion 객체 배열로 `prompt`를 허용하며, 선택한 API 타입에 따라 적절한 형식으로 요청을 구성합니다(예: 채팅/텍스트 모드 간 변환, 지시 형식 적용 등). 생성 프로세스를 더욱 제어하기 위해 함수에 추가 `systemPrompt` 및 `prefill`을 전달할 수도 있습니다.

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
Chat Completion 모드에서는 다음과 같은 프롬프트를 생성합니다:
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
Text Completion 모드(지시 없음)에서는 다음과 같은 프롬프트를 생성합니다:
"You are a helpful assistant.\nGenerate a story about a brave knight.\nOnce upon a time,"
*/

const result = await generateRaw({
    systemPrompt,
    prompt,
    prefill,
});
```

### 구조화된 출력

!!!info
현재 Chat Completion API에서만 지원됩니다. 가용성은 선택한 소스 및 모델에 따라 다릅니다. 선택한 모델이 구조화된 출력을 지원하지 않으면 생성이 실패하거나 빈 객체(`'{}'`)를 반환합니다. 구조화된 출력이 지원되는지 확인하려면 사용 중인 특정 API의 문서를 확인하세요.
!!!

구조화된 출력 기능을 사용하여 모델이 제공된 [JSON 스키마](https://json-schema.org/learn)를 준수하는 유효한 JSON 객체를 생성하도록 할 수 있습니다. 이는 상태 추적, 데이터 분류 등과 같은 구조화된 데이터가 필요한 익스텐션에 유용합니다.

구조화된 출력을 사용하려면 `generateRaw()` 또는 `generateQuietPrompt()`에 JSON 스키마 객체를 전달해야 합니다. 그러면 모델이 스키마와 일치하는 응답을 생성하고 문자열화된 JSON 객체로 반환됩니다.

!!!warning
출력은 스키마에 대해 검증되지 않으므로 생성된 출력의 파싱 및 검증을 직접 처리해야 합니다. 모델이 유효한 JSON 객체를 생성하지 못하면 함수는 빈 객체(`'{}'`)를 반환합니다.

[Zod](https://zod.dev/json-schema)는 JSON 스키마를 생성하고 검증하는 인기 있는 라이브러리입니다. 여기서는 사용법을 다루지 않습니다.
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// 예상 출력에 대한 JSON 스키마 정의
const jsonSchema = {
    // 필수: 스키마의 이름
    name: 'StoryStateModel',
    // 선택 사항: 스키마 설명
    description: 'A schema for a story state with location, plans, and memories.',
    // 선택 사항: 스키마가 엄격 모드에서 사용됨을 의미하며, 스키마에 정의된 필드만 허용됩니다
    strict: true,
    // 필수: 스키마의 정의
    value: {
        '$schema': 'http://json-schema.org/draft-04/schema#',
        'type': 'object',
        'properties': {
            'location': {
                'type': 'string'
            },
            'plans': {
                'type': 'string'
            },
            'memories': {
                'type': 'string'
            }
        },
        'required': [
            'location',
            'plans',
            'memories'
        ],
    },
};

const prompt = 'Generate a story state with location, plans, and memories. Output as a JSON object.';

const rawResult = await generateRaw({
    prompt,
    jsonSchema,
});

const quietResult = await generateQuietPrompt({
    quietPrompt: prompt,
    jsonSchema,
});
```

## 사용자 정의 매크로 등록

캐릭터 카드 필드, STscript 명령, 프롬프트 템플릿 등과 같이 매크로 대체가 지원되는 모든 곳에서 사용할 수 있는 사용자 정의 매크로를 등록할 수 있습니다.

### 새로운 매크로 시스템

매크로를 등록하는 권장 방법은 `SillyTavern.getContext()`를 통해 사용 가능한 `macros.register()` 함수를 사용하는 것입니다. 이 시스템은 인수, 카테고리, 설명 및 풍부한 문서 메타데이터를 지원합니다.

```js
const { macros } = SillyTavern.getContext();

// 핸들러 함수가 있는 간단한 매크로
macros.register('tomorrow', {
    description: 'Returns tomorrow\'s date',
    handler: () => {
        return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
    },
});

// 명명되지 않은 인수와 카테고리가 있는 매크로
macros.register('greet', {
    description: 'Generates a greeting for the given name',
    category: macros.category.UTILITY,
    unnamedArgs: [
        { name: 'name', description: 'The name to greet' },
    ],
    handler: ({ unnamedArgs }) => {
        const [name] = unnamedArgs;
        return `Hello, ${name}!`;
    },
});
```

`handler` 함수는 다음을 포함하는 [MacroExecutionContext](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/macros/engine/MacroRegistry.js) 객체를 받습니다:

* `args` - 매크로에 전달된 모든 명명되지 않은 인수.
* `unnamedArgs` - 정의된 인수 목록과 일치하는 위치 인수.
* `list` - 목록 인수(명명되지 않은 인수 이후), 목록이 활성화되지 않은 경우 `null`.
* `env` - 캐릭터 데이터, 채팅 상태 등에 액세스할 수 있는 매크로 환경.
* `resolve(text)` - 텍스트 내 중첩된 매크로를 해결하는 함수(`delayArgResolution`이 `true`인 경우).

이 외에도 더 많은 속성이 있습니다.

핸들러는 동기적으로 실행되므로 `Promise`를 반환하거나 비동기 작업을 동기적으로 호출할 수 없습니다.

매크로를 등록 해제하려면:

```js
const { macros } = SillyTavern.getContext();

macros.registry.unregisterMacro('greet');
```

기존 매크로에 대한 별칭도 등록할 수 있습니다:

```js
const { macros } = SillyTavern.getContext();

macros.registerAlias('greet', 'hello', { visible: true });
```

### 레거시 매크로 시스템 (사용 중단됨)

!!!warning
`getContext()`의 `registerMacro()` 및 `unregisterMacro()`는 **더 이상 사용되지 않습니다(deprecated)**. 대신 `macros.register()` 및 `macros.registry.unregisterMacro()`를 사용하세요.
!!!

레거시 API는 하위 호환성을 위해 여전히 사용할 수 있지만 향후 릴리스에서 제거될 예정입니다:

```js
const { registerMacro, unregisterMacro } = SillyTavern.getContext();

// 간단한 문자열 매크로
registerMacro('fizz', 'buzz');
// 함수 매크로 (동기여야 함)
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});

// 등록 해제
unregisterMacro('fizz');
```

## 메시지 형식 지정 훅

!!!warning Staging 기능
이것은 현재 SillyTavern의 `staging` 브랜치에서만 사용 가능하며 최신 릴리스에는 포함되어 있지 않습니다.
!!!

익스텐션은 메시지 형식 지정 파이프라인에 연결하여 텍스트가 DOM에 도달하기 전에 메시지 텍스트를 변환할 수 있습니다. 이는 주석(루비 태그, 툴팁) 추가, 강조 표시 또는 사용자 정의 텍스트 변환에 유용합니다.

!!!warning
훅은 동기적으로 실행되며 **반드시 문자열을 반환해야 합니다**. 비동기 함수와 문자열이 아닌 반환값은 등록 시점에 `TypeError`를 발생시키거나, 런타임에 콘솔 경고와 함께 조용히 무시됩니다. 이러한 훅에서 비용이 많이 드는 작업을 수행하지 마세요 — 모든 메시지 렌더링 시마다 실행됩니다.
!!!

### 파이프라인 단계

훅은 세 가지 파이프라인 단계에 등록할 수 있습니다. 모든 단계는 DOMPurify 새니타이제이션 **이전**에 실행되므로 출력은 항상 안전합니다:

| 단계 | 실행 시점 | 텍스트 형식 |
|-------|--------------|-------------|
| `beforeRegex` | 프롬프트 편향 제거 후, 사용자 정의 정규식 규칙 이전 | 일반 텍스트 |
| `afterRegex` | 사용자 정의 정규식 규칙 이후, Markdown 변환 이전 | 일반 텍스트 |
| `afterMarkdown` | Markdown-HTML 변환(showdown) 이후, DOMPurify 이전 | HTML 문자열 |

`afterMarkdown` 단계는 렌더링된 HTML에 주석을 달고자 하는 익스텐션에 있어 기본이자 가장 일반적인 삽입 지점입니다.

### 훅 등록

`getContext()`에서 `messageFormatter`에 액세스합니다:

```js
const { messageFormatter } = SillyTavern.getContext();

// 간단한 훅 - 메시지 텍스트 변환
messageFormatter.addHook((mes, ctx) => {
    // 사용자 메시지는 건너뜁니다
    if (ctx.isUser) return mes;

    // 일본어 텍스트에 후리가나 추가
    return addFurigana(mes);
});

// 명시적 단계와 순서가 있는 훅
messageFormatter.addHook((mes, ctx) => {
    // Markdown 변환 후, 새니타이제이션 전에 변환
    return mes.replace(/\*\*(.+?)\*\*/g, '<mark>$1</mark>');
}, {
    stage: messageFormatter.stage.AFTER_MARKDOWN,
    order: messageFormatter.order.EARLY,
});
```

### 훅 컨텍스트

훅은 메시지 메타데이터가 포함된 불변 컨텍스트 객체를 받습니다:

| 속성 | 타입 | 설명 |
|----------|------|-------------|
| `characterName` | `string` | 메시지와 연결된 캐릭터 이름 |
| `isSystem` | `boolean` | 시스템 메시지인지 여부 |
| `isUser` | `boolean` | 사용자가 보낸 메시지인지 여부 |
| `messageId` | `number` | 채팅 배열 내 메시지 인덱스, 임시 메시지(스트리밍 미리보기)의 경우 `-1` |
| `isReasoning` | `boolean` | 추론/사고 출력 메시지인지 여부 |
| `stage` | `string` | 현재 실행 중인 파이프라인 단계 |

컨텍스트 객체는 `Object.freeze()`로 동결되어 있어 — 수정을 시도해도 효과가 없습니다.

### 훅 순서

한 단계 내의 훅은 오름차순으로 실행됩니다. `order` 옵션을 사용하여 실행 순서를 제어합니다:

```js
const { hook_order } = messageFormatter;

// 미리 정의된 상수
hook_order.EARLIEST;  // 0
hook_order.EARLY;     // 10
hook_order.NORMAL;    // 50 (기본값)
hook_order.LATE;      // 90
hook_order.LATEST;    // 100

// 사용자 정의 숫자 값
messageFormatter.addHook(myHook, { order: 25 });
```

숫자가 낮을수록 먼저 실행됩니다. 이는 여러 익스텐션이 동일한 텍스트를 변환할 때 유용합니다 — 예를 들어 한 익스텐션이 데이터를 먼저 추출하고 다른 익스텐션이 나중에 형식을 지정할 수 있습니다.

### 오류 처리

훅 실행은 try/catch로 감싸져 있습니다. 훅이 오류를 발생시키면 건너뛰고 콘솔 오류가 기록됩니다 — 파이프라인은 나머지 훅들과 함께 계속 진행됩니다.

훅이 문자열이 아닌 값(`undefined` 또는 `Promise` 포함)을 반환하면 콘솔 경고가 발생하고 반환 값은 무시됩니다. 파이프라인은 이전 텍스트를 변경하지 않고 계속됩니다.

### 전체 파이프라인 순서

참고로, 전체 메시지 형식 지정 파이프라인은 다음과 같습니다:

1. 프롬프트 편향 제거(메시지 0에만 해당)
2. 주석/숨겨진 메시지 정규화
3. `beforeRegex` 익스텐션 훅
4. 사용자 정의 정규식 규칙(`getRegexedString`)
5. `afterRegex` 익스텐션 훅
6. Markdown 자동 수정(`fixMarkdown`)
7. HTML 태그 인코딩(`encode_tags`)
8. Showdown Markdown → HTML 변환
9. `afterMarkdown` 익스텐션 훅
10. 이름 접두사 제거(`allow_name2_display`)
11. DOMPurify 새니타이제이션

모든 익스텐션 훅(3, 5, 9단계)은 DOMPurify **이전에** 실행되므로 출력은 항상 새니타이즈됩니다.

## 함수 도구 호출

익스텐션은 chat completion 중에 LLM이 호출할 수 있는 사용자 정의 함수 도구를 등록할 수 있습니다. 이를 통해 익스텐션이 모델의 구조화된 데이터에 반응할 수 있습니다 — 예를 들어 API 쿼리, 계산 수행 또는 익스텐션 기능 트리거 등입니다.

전제 조건, 지원되는 API, 등록 필드, 팁을 포함한 전체 가이드는 전용 [함수 호출](./Function-Calling.md) 페이지를 참조하세요.

**간단한 예제:**

```js
const { registerFunctionTool } = SillyTavern.getContext();

registerFunctionTool({
    name: 'get_weather',
    displayName: 'Get Weather',
    description: 'Get the current weather for a given location',
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            location: { type: 'string', description: 'City name' },
        },
        required: ['location'],
    },
    action: async ({ location }) => {
        const data = await fetchWeatherData(location);
        return JSON.stringify(data);
    },
});
```

## 액션 로더

액션 로더는 장시간 실행되는 작업을 위한 로딩 오버레이 및 토스트 알림 시스템을 제공합니다. 더 이상 사용되지 않는 `showLoader()` / `hideLoader()` 함수를 대체합니다.

`getContext()`의 `loader`를 통해 액세스합니다:

```js
const { loader } = SillyTavern.getContext();

// 중지 가능한 토스트가 있는 기본 차단 로더
const handle = loader.show({ message: 'Processing data...' });
try {
    const result = await someExpensiveOperation();
} finally {
    await handle.hide();
}
```

### 옵션

| 옵션 | 기본값 | 설명 |
|--------|---------|-------------|
| `blocking` | `true` | 상호 작용을 차단하는 전체 화면 오버레이 표시 |
| `message` | `'Generating...'` | 토스트 알림에 표시되는 메시지 |
| `title` | `''` | 토스트의 선택적 제목 |
| `toastMode` | `'stoppable'` | `'stoppable'`(중지 버튼 포함), `'static'`(버튼 없음), 또는 `'none'`(토스트 없음) |
| `stopTooltip` | `'Stop'` | 중지 버튼의 툴팁 텍스트 |
| `onStop` | `null` | 사용자 정의 중지 핸들러. 기본값은 `stopGeneration()` |
| `onHide` | `null` | 로더가 숨겨질 때(중지가 아님) 호출됨 |
| `overlayContent` | `null` | 기본 스피너를 대체하는 사용자 정의 HTML 요소 또는 문자열 |

### 로더 스태킹

여러 로더가 동시에 활성화될 수 있습니다. 오버레이는 적어도 하나의 차단 로더가 활성 상태인 한 표시된 상태로 유지됩니다:

```js
const { loader } = SillyTavern.getContext();

const loader1 = loader.show({ message: 'Task 1...' });
const loader2 = loader.show({ message: 'Task 2...' });
await loader1.hide(); // 오버레이 유지 — loader2가 여전히 활성 상태
await loader2.hide(); // 이제 오버레이가 숨겨짐
```

### 비차단 로더

UI를 차단하지 않아야 하는 백그라운드 작업의 경우:

```js
const { loader } = SillyTavern.getContext();

const handle = loader.show({
    blocking: false,
    message: 'Downloading in background...',
    onStop: () => abortDownload(),
});
```

## 팝업 및 사용자 피드백

### 팝업 헬퍼

SillyTavern은 `getContext()`의 `Popup.show`를 통해 편리한 팝업 헬퍼를 제공합니다:

```js
const { Popup } = SillyTavern.getContext();

// 확인 대화 상자 — POPUP_RESULT.AFFIRMATIVE 또는 POPUP_RESULT.NEGATIVE를 반환
const confirmed = await Popup.show.confirm('Confirm Action', 'Are you sure you want to proceed?');

// 텍스트 입력 대화 상자 — 입력된 문자열, 취소 시 null을 반환
const userInput = await Popup.show.input('Enter Name', 'Please provide a name:', 'default value');

// 정보 표시 — 클릭된 버튼 결과를 반환
await Popup.show.text('Info', 'Operation completed successfully.');
```

### 사용자 정의 팝업

더 복잡한 팝업의 경우, 전체 옵션과 함께 `Popup`을 직접 인스턴스화합니다:

```js
const { Popup, POPUP_TYPE, POPUP_RESULT } = SillyTavern.getContext();

const popup = new Popup(
    '<div>Custom HTML content here</div>',
    POPUP_TYPE.TEXT,
    '',
    {
        wide: true,              // 와이드 표시 모드
        okButton: 'Save',       // 사용자 정의 OK 버튼 텍스트
        cancelButton: 'Discard', // 사용자 정의 Cancel 버튼 텍스트
        customButtons: [
            {
                text: 'Export',
                icon: 'fa-download',
                result: POPUP_RESULT.CUSTOM1,
            },
        ],
        customInputs: [
            {
                id: 'my_checkbox',
                label: 'Enable feature',
                type: 'checkbox',
                defaultState: false,
            },
        ],
        allowVerticalScrolling: true,
    }
);

const result = await popup.show();

if (result === POPUP_RESULT.AFFIRMATIVE) {
    // OK가 클릭됨
} else if (result === POPUP_RESULT.CUSTOM1) {
    // Export 버튼이 클릭됨
}

// 사용자 정의 입력 값 읽기
const checkboxValue = popup.inputResults?.get('my_checkbox');
```

### 팝업 타입

| 타입 | 설명 |
|------|-------------|
| `POPUP_TYPE.TEXT` | 버튼이 있는 일반 콘텐츠 팝업 |
| `POPUP_TYPE.CONFIRM` | 예/아니오 확인 대화 상자 |
| `POPUP_TYPE.INPUT` | 텍스트 입력 필드가 있는 팝업 |
| `POPUP_TYPE.DISPLAY` | 닫기 버튼만 있는 콘텐츠 전용 팝업 |
| `POPUP_TYPE.CROP` | 이미지 자르기 팝업 |

### 토스트 알림

가벼운 피드백에는 (전역적으로 사용 가능한) `toastr`을 사용하세요:

```js
toastr.success('Data saved successfully');
toastr.error('Failed to connect to API');
toastr.warning('This feature is experimental');
toastr.info('Processing...');
```

## 데이터 뱅크 스크레이퍼

익스텐션은 데이터 뱅크 기능을 위한 사용자 정의 데이터 스크레이퍼를 등록할 수 있습니다. 스크레이퍼는 사용자 정의 소스(예: 웹 페이지, API, 파일 형식)에서 데이터를 가져오는 방법을 제공합니다:

```js
const { registerDataBankScraper } = SillyTavern.getContext();

await registerDataBankScraper({
    id: 'my_scraper',
    name: 'My Data Source',
    description: 'Import data from My Data Source',
    iconClass: 'fa-solid fa-database',
    iconAvailable: true,
    isAvailable: async () => true,
    scrape: async () => {
        // File 객체의 배열을 반환
        const content = await fetchDataFromSource();
        return [new File([content], 'data.txt', { type: 'text/plain' })];
    },
});
```

## 디버그 함수

익스텐션은 (파워 유저 설정을 통해 액세스 가능한) 디버그 메뉴에 나타나는 사용자 정의 디버그 함수를 등록할 수 있습니다. 이는 진단 도구, 캐시/정리 기능 또는 개발 중 수동 트리거를 노출하는 데 유용합니다:

```js
const { registerDebugFunction } = SillyTavern.getContext();

registerDebugFunction(
    'my_ext_clear_cache',        // 고유 함수 ID
    'Clear My Extension Cache',   // 표시 이름
    'Clears all cached data for My Extension', // 설명
    async () => {
        const { localforage } = SillyTavern.libs;
        await localforage.removeItem('my_extension_cache');
        toastr.success('Cache cleared');
    }
);
```

## Extras 요청 수행

!!!warning
Extras API는 더 이상 사용되지 않습니다. 새 익스텐션에서 사용하는 것은 권장되지 않습니다.
!!!

`doExtrasFetch()` 함수를 사용하면 SillyTavern Extras API 서버에 요청할 수 있습니다.

예를 들어 `/api/summarize` 엔드포인트를 호출하려면:

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // 요청 본문
    })
});
```

`getApiUrl()`은 Extras 서버의 기본 URL을 반환합니다.

`doExtrasFetch()` 함수:

* `Authorization` 및 `Bypass-Tunnel-Reminder` 헤더를 추가합니다
* 결과를 페칭하는 것을 처리합니다
* 결과(응답 객체)를 반환합니다

이를 통해 익스텐션에서 Extras API를 쉽게 호출할 수 있습니다.

다음을 지정할 수 있습니다:

* 요청 메서드: GET, POST 등
* 추가 헤더
* POST 요청의 본문
* 기타 fetch 옵션

## 모범 사례

### 보안

**`extensionSettings`에 API 키나 비밀 정보를 저장하지 마세요**

익스텐션 설정은 다른 모든 익스텐션이 액세스할 수 있으며 평문으로 저장됩니다. 민감한 데이터를 클라이언트 측에 저장하지 마세요:

```js
// 나쁜 예 - 이렇게 하지 마세요!
extensionSettings[MODULE_NAME].apiKey = 'secret_key_123';

// 참고: 클라이언트 측 익스텐션에 비밀 정보를 안전하게 저장할 방법은 없습니다.
// 민감한 데이터를 다뤄야 한다면 대신 서버 플러그인을 사용하세요.
// 참조: https://docs.sillytavern.app/for-contributors/server-plugins/
```

**사용자 입력을 새니타이즈하세요**

명령, API 호출, DOM 조작에 사용하기 전에 항상 사용자 입력의 데이터를 검증하고 새니타이즈하세요:

```js
// 먼저 입력 타입을 검증
if (typeof userInput !== 'string') {
    toastr.error('Invalid input type');
    return;
}
// DOMPurify를 사용하여 HTML 입력을 새니타이즈
const { DOMPurify } = SillyTavern.libs;
const cleanInput = DOMPurify.sanitize(userInput);
```

**`eval()` 또는 `Function()` 생성자 사용을 피하세요**

이들은 임의의 코드를 실행할 수 있어 보안 위험을 초래합니다. 동적 평가가 필요한 경우 더 안전한 대안을 사용하거나 입력을 신중하게 제한하세요.

### 성능

**`extensionSettings`에 대용량 데이터를 저장하지 마세요**

익스텐션 설정은 메모리에 로드되며 자주 저장됩니다. 대용량 데이터는 성능 문제를 일으킬 수 있습니다:

```js
// 나쁜 예 - 대용량 데이터를 저장하지 마세요
extensionSettings[MODULE_NAME].largeDataset = { /* 수 메가바이트의 데이터 */ };

// 좋은 예 - localforage 사용(IndexedDB/localStorage 추상화)
const { localforage } = SillyTavern.libs;
await localforage.setItem(`${MODULE_NAME}_data`, largeData);

// 또는 더 작은 데이터에는 localStorage 사용
localStorage.setItem(`${MODULE_NAME}_data`, JSON.stringify(smallData));
```

**이벤트 리스너를 정리하세요**

메모리 누수를 방지하기 위해 더 이상 필요하지 않은 이벤트 리스너를 제거하세요:

```js
function cleanup() {
    eventSource.removeListener(event_types.MESSAGE_RECEIVED, handleMessage);
    document.getElementById('myElement').removeEventListener('click', handleClick);
}
```

**UI 스레드를 차단하지 마세요**

무거운 작업에는 async/await 또는 웹 워커를 사용하세요:

```js
// I/O 작업에는 async 사용
async function processData() {
    const result = await fetch('/api/process');
    return result.json();
}

// 무거운 계산을 나누어 처리
async function heavyComputation(data) {
    for (let i = 0; i < data.length; i++) {
        // 청크 처리
        if (i % 1000 === 0) {
            await new Promise(resolve => setTimeout(resolve, 0)); // UI에 양보
        }
    }
}
```

### 호환성

**직접 가져오기보다 `getContext()`를 선호하세요**

컨텍스트 API가 더 안정적이며 SillyTavern 업데이트로 인해 손상될 가능성이 적습니다:

```js
// 좋은 예 - 안정적인 API
const { chat, characters, saveSettingsDebounced } = SillyTavern.getContext();

// 피해야 할 예 - 내부 변경으로 손상될 수 있음
import { chat, characters } from '../../../../script.js';
```

**고유한 모듈 이름을 사용하세요**

설명적이고 고유한 모듈 이름을 사용하여 다른 익스텐션과의 충돌을 방지하세요:

```js
// 좋은 예 - 구체적이고 고유함
const MODULE_NAME = 'my_extension_name';

// 나쁜 예 - 너무 일반적이라 충돌 가능성이 높음
const MODULE_NAME = 'settings';
```

### 사용자 경험

**명확한 피드백을 제공하세요**

가벼운 알림에는 `toastr`을, 중요한 사용자 상호 작용에는 `Popup`을 사용하세요. 자세한 내용은 [팝업 및 사용자 피드백](#popups-and-user-feedback) 섹션을 참조하세요.

장시간 실행되는 작업에는 UI를 조용히 차단하는 대신 [액션 로더](#action-loader)를 사용하세요.

**유용한 콘솔 메시지를 제공하세요**

콘솔 로그에는 일관된 접두사를 사용하세요. 하지만 프로덕션에서 콘솔에 과도한 로그를 남발하지 마세요:

```js
const MODULE_NAME = 'MyExtension';

console.log(`[${MODULE_NAME}] Extension loaded`);
console.debug(`[${MODULE_NAME}] Processing data:`, data);
console.error(`[${MODULE_NAME}] Error occurred:`, error);
```

### 코드 품질

**`lib.js`의 번들 라이브러리를 사용하세요**

새 종속성을 추가하기 전에 [공유 라이브러리](#shared-libraries) 섹션을 확인하세요 — SillyTavern은 `SillyTavern.libs`를 통해 사용 가능한 많은 일반적인 라이브러리(lodash, Fuse, DOMPurify, moment, yaml 등)를 번들로 제공합니다.

**설정을 올바르게 초기화하세요**

항상 기본값을 제공하고 누락된 키를 처리하세요:

```js
function loadSettings() {
    // 업데이트 후 새 키를 처리하기 위해 기본값과 병합하고, 존재하지 않으면 초기화합니다.
    extensionSettings[MODULE_NAME] = SillyTavern.libs.lodash.merge(
        structuredClone(defaultSettings),
        extensionSettings[MODULE_NAME]
    );
}
```
