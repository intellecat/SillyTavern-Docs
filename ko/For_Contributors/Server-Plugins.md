---
order: -30
icon: server
route: /ko/for-contributors/server-plugins/
label: 서버 플러그인
---

# 서버 플러그인

이러한 플러그인을 사용하면 새 API 엔드포인트 생성 또는 브라우저 환경에서 사용할 수 없는 Node.JS 패키지 사용과 같이 UI 익스텐션만으로는 달성할 수 없는 기능을 추가할 수 있습니다.

플러그인은 SillyTavern의 `plugins` 디렉토리에 포함되어 있으며 서버 시작 시 로드되지만, `config.yaml` 파일에서 `enableServerPlugins`가 `true`로 설정된 경우에*만* 로드됩니다.

!!!warning 경고
**서버 플러그인은 샌드박스 처리되지 않습니다. 이는 잠재적으로 전체 파일 시스템에 대한 액세스 권한을 얻거나 일반 UI 익스텐션이 할 수 없는 방식으로 광범위한 보안 취약점을 도입할 수 있음을 의미합니다. 신뢰할 수 있는 개발자의 서버 플러그인만 설치하세요!**
!!!

모든 공식 서버 플러그인 목록은 GitHub 조직 목록을 참조하세요: <https://github.com/search?q=topic%3Aplugin+org%3ASillyTavern&type=Repositories>

## 플러그인 타입

### 파일

`init` 함수를 내보내는 모듈이 포함된 ".js"(CommonJS 모듈용) 또는 ".mjs"(ES 모듈용) 확장자를 가진 실행 가능한 JavaScript 파일입니다. 이 함수는 Express 라우터(플러그인을 위해 특별히 생성됨)를 인수로 받아들이고 Promise를 반환합니다.

모듈은 또한 플러그인에 대한 정보(`id`, `name`, `description` 문자열)를 포함하는 `info` 객체를 내보내야 합니다. 이것은 로더에 플러그인에 대한 정보를 제공합니다.

라우터를 통해 `/api/plugins/{id}/{route}` 경로 아래에 등록될 라우트를 등록할 수 있습니다. 예를 들어, `example` 플러그인의 `router.get('/foo')`는 다음과 같은 라우트를 생성합니다: `/api/plugins/example/foo`.

플러그인은 *선택적으로* 서버를 종료할 때 정리를 수행하는 `exit` 함수를 내보낼 수도 있습니다. 인수가 없어야 하며 Promise를 반환해야 합니다.

플러그인 내보내기에 대한 TypeScript 계약:

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

아래에서 "Hello world!" 플러그인 예제를 참조하세요:

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // 여기서 초기화...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // 여기서 정리...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### 디렉토리

다음 방법 중 하나로 `plugins` 디렉토리의 하위 디렉토리에서 플러그인을 로드할 수 있습니다(우선 순위 순):

1. "main" 필드에 실행 가능한 파일의 경로를 포함하는 `package.json` 파일.
2. CommonJS 모듈용 `index.js` 파일.
3. ES 모듈용 `index.mjs` 파일.

결과 파일은 개별 파일과 동일한 요구 사항으로 `init` 함수와 `info` 객체를 내보내야 합니다.

디렉토리 플러그인 예제(`index.js` 파일 포함): <https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### 번들링

모든 요구 사항을 하나의 파일로 패키징하는 번들러(예: Webpack 또는 Browserify)를 사용하는 것이 좋습니다. "Node"를 빌드 대상으로 설정해야 합니다.

Webpack과 TypeScript를 사용하는 플러그인용 템플릿 저장소: <https://github.com/SillyTavern/Plugin-WebpackTemplate>
