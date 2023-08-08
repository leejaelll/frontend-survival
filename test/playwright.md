# Playwright

## 학습 키워드

* E2E(End to End) Test
* Headless Chrome
* Puppeteer
* Playwright
* CodeceptJS



* [Playwright](https://playwright.dev/)
* [Playwright Configuration](https://playwright.dev/docs/test-configuration)
* [Ashal의 Playwright](https://github.com/ahastudio/til/blob/main/test/playwright.md)

{% hint style="info" %}
### Playwright

웹 브라우저 기반 E2E 테스트 자동화 도구.

Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원한다.

_👉🏻 즉, 사용자가 실제로 소프트웨어를 사용했을때 플로우를 테스트하는 것_

* Playwright에는 페이지 요소가 표시될 때까지 자동으로 대기하고 테스트 실행의 스크린샷을 찍고 비디오를 녹화하기 위한 기본 제공 지원과 같이 강력하고 안정적인 브라우저 테스트를 쉽게 작성할 수 있는 기능도 포함되어 있다.
* 또한 눈에 보이는 브라우저 창 없이 테스트를 실행할 수 있는 헤드리스 모드와 개발자가 테스트 실행 중에 발생하는 문제를 디버그하고 해결하는 데 도움이 되는 슬로우 모션 모드가 포함되어 있다.
{% endhint %}

\


{% hint style="info" %}
### Headless Chrome

* 개발자가 UI(사용자 인터페이스) 없이 프로그래밍 방식으로 Chrome 브라우저 기능을 사용할 수 있게 해주는 도구입니다.
* 본질적으로 일반 Chrome과 동일하지만 그래픽 사용자 인터페이스가 없으므로 자동 테스트, 웹 스크래핑 및 기타 헤드리스 애플리케이션에 사용할 수 있다.
* 일반 브라우저처럼 웹 페이지와 상호 작용하고 JavaScript 코드를 실행하는 방법을 제공하지만 모두 백그라운드에서 수행한다.
{% endhint %}

\


Playwright 패키지 설치

```bash
npm i -D @playwright/test eslint-plugin-playwright
```

`playwright.config.ts` 파일

```jsx
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  testDir: './tests',
  retries: 0,
  use: {
    baseURL: 'http://localhost:8080',
    headless: !!process.env.CI,
    screenshot: 'only-on-failure',
  },
};

export default config;
```

`tests/.eslintrc.js` 파일

```jsx
module.exports = {
  env: {
    jest: false,
  },
  extends: ['plugin:playwright/playwright-test'],
  rules: {
    'import/no-extraneous-dependencies': 'off',
  },
};
```

`tests/home.spec.ts`

```jsx
import { test, expect } from '@playwright/test';

test('Filter products', async ({ page }) => {
  await page.goto('/');

  await expect(page.getByText('Apple')).toBeVisible();

  const searchInput = page.getByLabel('Search');

  await searchInput.fill('a');

  await expect(page.getByText('Apple')).toBeVisible();

  await searchInput.fill('aa');

  await expect(page.getByText('Apple')).toBeHidden();
});

test('Click the “Increase” button', async ({ page }) => {
  await page.goto('/');

  const count = 13;

  await Promise.all(
    [...Array(count)].map(async () => {
      await page.getByText('Increase').click();
    })
  );

  await expect(page.getByText(`${count}`)).toBeVisible();
});
```

테스트 실행

```bash
npx playwright test
```

`.gitignore` 파일에 에러 상황의 스크린샷 등이 담기는 `test-results` 디렉터리 추가.

```
/test-results/
```

인간 친화적인 E2E 테스팅 도구로 CodeceptJS가 있다.

* [CodeceptJS](https://codecept.io/)
* [CodeceptJS 3 시작하기](https://github.com/ahastudio/til/blob/main/test/20201207-codeceptjs.md)
* [CodeceptJS 사용](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-%EC%82%AC%EC%9A%A9)
