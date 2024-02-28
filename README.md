# This is a minimal reproduction repository for a memory leak in @testing-library/jest-dom

## Issue description

Jest has a --detectLeaks flag, which causes tests to fail if there is a memory leak. Importing `@testing-library/jest-dom` causes a memory leak in NodeJS v20. However, in NodeJS v18 the memory leak does not occur.

## How to reproduce

1. Clone this repository
2. Switch to NodeJS v20
3. Run `npm install` and `npm run test`
4. The test should fail with the message "Your test suite is leaking memory. Please ensure all references are cleaned."
5. Remove `require("@testing-library/jest-dom");` from jest-setup.js
6. Rerun the test. It should pass, indicating that the memory leak is resolved.

## Important notes

1. The memory leak does not occur in Node.js v18.
2. When commenting out `require('aria-query');` `var matchers = require('./matchers-5ae87d41.js');` and `expect.extend(matchers.extensions);` from `./node_modules/@testing-library/jest-dom/dist/index.js` memory leak disappears. Both `var matchers` and `require('aria-query')` cause a memory leak.
