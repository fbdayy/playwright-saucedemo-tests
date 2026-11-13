\# Playwright Sauce Demo Tests



\[!\[Playwright Tests](https://github.com/fbdayy/playwright-saucedemo-tests/actions/workflows/playwright.yml/badge.svg)](https://github.com/fbdayy/playwright-saucedemo-tests/actions/workflows/playwright.yml)



E2E test suite for \[saucedemo.com](https://www.saucedemo.com) built with Playwright and TypeScript. Demonstrates a production-style framework: Page Object Model, fixtures, isolated tests, and CI/CD.



\## 🚀 Quick start



```bash

npm ci

npx playwright install --with-deps

npm test

```



Open the HTML report:



```bash

npm run report

```



\## 🛠️ Tech stack \& architecture



\- \*\*Playwright\*\* — E2E framework.

\- \*\*TypeScript\*\* — type safety.

\- \*\*Page Object Model\*\* — locators and actions encapsulated in `pages/`.

\- \*\*Fixtures\*\* — inject page objects into tests without boilerplate.

\- \*\*Test isolation\*\* — each test gets a fresh page via `beforeEach` inside `test.describe`.

\- \*\*ESLint 10 + Prettier\*\* — consistent code style, linted in CI.

\- \*\*GitHub Actions\*\* — CI pipeline runs on every push and pull request.



\## 📁 Structure



```

.

├── pages/              # Page objects

│   ├── LoginPage.ts

│   └── InventoryPage.ts

├── fixtures/           # Playwright fixtures

│   └── app.fixture.ts

├── tests/              # Test specs

│   ├── login.spec.ts

│   └── inventory.spec.ts

├── .github/workflows/  # CI pipeline

│   └── playwright.yml

├── playwright.config.ts

├── tsconfig.json

├── eslint.config.ts

└── README.md

```



\## 🎯 Covered scenarios



\### Login (`tests/login.spec.ts`)



\- Standard user logs in successfully and lands on the products page.

\- Locked-out user sees the corresponding error message.

\- Empty fields trigger client-side validation error.



\### Inventory (`tests/inventory.spec.ts`)



\- Product catalog displays exactly 6 items.

\- Adding an item to the cart updates the cart badge.



\## 🧪 Test design notes



\- \*\*Isolation over speed.\*\* Every test starts from a clean state thanks to `test.beforeEach` inside `test.describe`, so a failure in one test never pollutes another. Parallel execution (4 workers by default) still keeps the suite at `4s`.

\- \*\*Locator priority.\*\* Role-based locators (`getByRole`, `getByPlaceholder`) are preferred over CSS where possible; `data-test` attributes are used where the DOM is stable enough to rely on them.

\- \*\*Web-first assertions.\*\* All assertions use Playwright's auto-retrying `expect` matchers (`toHaveText`, `toHaveCount`), so there are no hard sleeps or manual waits.

\- \*\*Grouped by feature, not by URL.\*\* Each `test.describe` block covers one functional area and owns its own navigation setup.



\## ⚖️ Trade-offs \& roadmap



\- \*\*No visual regression yet.\*\* Planned via `toHaveScreenshot()` once the UI stabilizes.

\- \*\*No API-level tests yet.\*\* Next step: cover the same scenarios via Playwright's `request` context to get faster, more granular feedback.

\- \*\*Single browser in CI.\*\* Chromium only for speed; Firefox and WebKit can be added as separate projects when needed.

\- \*\*One worker in CI.\*\* Set to `1` for deterministic runs on the free GitHub-hosted runners; sharding can be introduced as the suite grows.

\- \*\*Dependency hygiene.\*\* Transitive `minimatch` vulnerability is pinned to a safe version via `overrides` in `package.json`; `npm audit` reports 0 vulnerabilities.



\## 🤖 CI/CD



The workflow at `.github/workflows/playwright.yml` runs on every push and pull request to `main`:



1\. Checkout and set up Node from `.nvmrc`.

2\. Install dependencies with `npm ci` (lock-file enforced).

3\. Cache Playwright browsers between runs.

4\. Run `npm run lint`.

5\. Run `npx playwright test`.

6\. Upload the HTML report as a build artifact (retained for 14 days).



The `main` branch is protected: changes land only through pull requests that pass the CI check, with squash-merge enabled to keep history linear.



\## 📄 License



MIT — see \[LICENSE](LICENSE).

