---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
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
* `localforage` - 브라우저 스토리지 라이브러리. [문서](https://localforage.github.io/localForage/).
* `Fuse` - 퍼지 검색 라이브러리. [문서](https://www.fusejs.io/).
* `DOMPurify` - HTML 새니타이제이션 라이브러리. [문서](https://github.com/cure53/DOMPurify).
* `Handlebars` - 템플릿 라이브러리. [문서](https://handlebarsjs.com/).
* `moment` - 날짜/시간 조작 라이브러리. [문서](http://momentjs.com/).
* `showdown` - Markdown 변환 라이브러리. [문서](https://showdownjs.com/).

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

* `APP_READY`: 앱이 완전히 로드되고 사용할 준비가 되었습니다. 앱이 준비된 후 새 리스너가 연결될 때마다 자동으로 발생합니다.
* `MESSAGE_RECEIVED`: LLM 메시지가 생성되고 `chat` 객체에 기록되었지만 아직 UI에 렌더링되지 않았습니다.
* `MESSAGE_SENT`: 메시지가 사용자에 의해 전송되고 `chat` 객체에 기록되었지만 아직 UI에 렌더링되지 않았습니다.
* `USER_MESSAGE_RENDERED`: 사용자가 보낸 메시지가 UI에 렌더링되었습니다.
* `CHARACTER_MESSAGE_RENDERED`: 생성된 LLM 메시지가 UI에 렌더링되었습니다.
* `CHAT_CHANGED`: 채팅이 전환되었습니다(예: 다른 캐릭터로 전환하거나 다른 채팅이 로드됨).
* `GENERATION_AFTER_COMMANDS`: 슬래시 명령 처리 후 생성이 시작되려고 합니다.
* `GENERATION_STOPPED`: 사용자가 생성을 중지했습니다.
* `GENERATION_ENDED`: 생성이 완료되었거나 오류가 발생했습니다.
* `SETTINGS_UPDATED`: 애플리케이션 설정이 업데이트되었습니다.

나머지는 [소스에서](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js) 찾을 수 있습니다.

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

매크로를 등록하려면 `SillyTavern.getContext()` 객체의 `registerMacro()` 함수를 사용합니다. 이 함수는 고유한 문자열이어야 하는 매크로 이름과 문자열 또는 문자열을 반환하는 함수를 허용합니다. 함수는 각 `substituteParams` 호출 간에 다른 고유한 `nonce` 문자열과 함께 호출됩니다.

```js
const { registerMacro } = SillyTavern.getContext();

// 간단한 문자열 매크로
registerMacro('fizz', 'buzz');
// 함수 매크로
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

사용자 정의 매크로가 더 이상 필요하지 않으면 `unregisterMacro()` 함수를 사용하여 제거합니다:

```js
const { unregisterMacro } = SillyTavern.getContext();

// 'fizz' 매크로 등록 해제
unregisterMacro('fizz');
```

**사용자 정의 매크로에 관한 중요한 세부 사항 및 알려진 제한 사항:**

1. 현재는 간단한 문자열 대체 매크로만 지원됩니다. 향후 더 복잡한 매크로에 대한 지원을 추가하기 위해 작업 중입니다.
2. 값을 제공하기 위해 함수를 사용하는 매크로는 *동기*여야 합니다. `Promise`를 반환하는 것은 작동하지 않습니다.
3. 매크로를 등록할 때 매크로 이름을 이중 중괄호(`{{ }}`)로 감쌀 필요가 없습니다. SillyTavern이 자동으로 처리합니다.
4. 매크로는 일반 정규 표현식 대체이므로 많은 매크로를 등록하면 성능 문제가 발생하므로 신중하게 사용하세요.

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
