# api-test-suite

![CI](https://github.com/rizvihiqrar/api-test-suite/actions/workflows/playwright.yml/badge.svg)

Playwright API test suite targeting the public [DummyJSON](https://dummyjson.com) REST API. Covers authentication, CRUD operations, pagination, and error handling. No browser is required.

---

## What It Tests

| File | Tags | Coverage |
|---|---|---|
| `auth.spec.js` | `@smoke` / `@regression` | Login with valid credentials, wrong password, missing password; `/auth/me` with and without a token |
| `users.spec.js` | `@smoke` / `@regression` | Paginated list and skip, single user schema, 404 for unknown IDs, create, PUT, PATCH, DELETE |

**13 tests across 2 files.**

---

## Project Structure

```
├── tests/
│   ├── auth.spec.js        # Login and token-protected endpoint tests
│   └── users.spec.js       # CRUD, pagination, and schema tests for /users
├── utils/
│   └── apiClient.js        # Request wrapper and schema assertion helper
├── fixtures/
│   └── testData.js         # Public DummyJSON test credentials and payloads
├── playwright.config.js    # API project, no browser
└── .github/workflows/      # GitHub Actions CI
```

---

## Quick Start

```bash
git clone https://github.com/rizvihiqrar/api-test-suite.git
cd api-test-suite
npm install

# All tests
npm test

# Smoke tests only
npm run smoke

# Full regression
npm run regression
```

No `.env` file is required. The suite defaults to `https://dummyjson.com`. Set `BASE_URL` to point it at another API.

---

## Key Patterns

**`ApiClient`** wraps Playwright's `request` context to keep test code short:
```js
const client = new ApiClient(request);
const res = await client.post('/auth/login', { username, password });
expect(res.status()).toBe(200);
```

**`assertSchema`** checks response shape without a third-party schema library:
```js
assertSchema(body, { id: 'number', username: 'string', email: 'string' });
```

---

## CI/CD

GitHub Actions runs the full suite on push. No browser is installed, so the job is quick. The badge above reflects the latest run.

---

## Tech Stack

- **Playwright**: API testing through the built-in `request` fixture
- **Node.js**: test runner
- **GitHub Actions**: CI
