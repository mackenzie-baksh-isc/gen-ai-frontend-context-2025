### Cursor Prompt: Initialize React + Vite + Mantine project

Follow these steps exactly. Create files with the provided contents verbatim.

1. Scaffold Vite React + TS and install dependencies

Create a folder for the repo specified by the user, if the path/name not specified ask the user, if no details provided the name will be app-repo/ in the root directory

```bash
# Scaffold Vite React + TS
npm create vite@latest . -- --template react-ts

# Core UI libs
npm i @mantine/hooks @mantine/core @mantine/notifications @tabler/icons-react react-router-dom

# Tooling
npm i -D typescript vite vitest @vitest/coverage-v8 jsdom \
  eslint prettier eslint-config-prettier eslint-plugin-prettier \
  eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-import \
  @typescript-eslint/eslint-plugin @typescript-eslint/parser \
  vite-plugin-checker unplugin-fonts \
  husky @commitlint/cli @commitlint/config-conventional \
  sass

# Initialize Husky
npm pkg set scripts.prepare="husky install"
npm run prepare
```

2. Add Husky hooks

- Create `.husky/commit-msg` with:

```sh
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx --no -- commitlint --edit ${1}
```

- Create `.husky/pre-push` with:

```sh
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run lint
```

3. Add commitlint config

- Create `commitlint.config.cjs` with:

```js
module.exports = { extends: ["@commitlint/config-conventional"] };
```

4. Ensure folder structure under `src`

```bash
mkdir -p src/{common,components,pages,store,test}
```

5. Update package.json scripts (merge/overwrite these keys)

```json
{
  "scripts": {
    "dev": "vite",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "coverage": "vitest run --coverage",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "start": "vite preview",
    "lint": "eslint \"./src/*.{ts,tsx}\" \"cypress/**/*.{ts,tsx}\"  --max-warnings=0",
    "lint:fix": "eslint --fix './src/**/*.{ts,tsx}'",
    "prettier": "prettier './src/**/*.{ts,tsx,scss}'",
    "prettier:fix": "prettier --write './src/**/*.{ts,tsx,scss}'",
    "prepare": "husky install"
  }
}
```

6. Mantine Provider, Notifications, CSS, Router, and Example Page

- Overwrite `src/main.tsx` with:

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { MantineProvider } from "@mantine/core";
import { Notifications } from "@mantine/notifications";
import "@mantine/core/styles.css";
import "@mantine/notifications/styles.css";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <MantineProvider>
      <Notifications position="top-right" />
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </MantineProvider>
  </React.StrictMode>
);
```

- Overwrite `src/App.tsx` with:

```tsx
import { Routes, Route } from "react-router-dom";
import HomePage from "./pages/HomePage";

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />} />
    </Routes>
  );
}
```

- Create `src/pages/HomePage.module.scss` with:

```scss
.container {
  padding: 1rem;
}
```

- Create `src/pages/HomePage.tsx` with:

```tsx
import { useDisclosure } from "@mantine/hooks";
import { Button, Group, Modal, Title } from "@mantine/core";
import { CopyButton } from "@mantine/core";
import { notifications } from "@mantine/notifications";
import { IconCheck } from "@tabler/icons-react";
import styles from "./HomePage.module.scss";

export default function HomePage() {
  const [opened, { open, close }] = useDisclosure(false);

  return (
    <div className={styles.container}>
      <Title order={1}>Template</Title>
      <Group mt="md">
        <CopyButton
          value="template"
          timeout={1000}
          onCopy={() =>
            notifications.show({
              message: 'Copied "template"',
              color: "green",
              icon: <IconCheck />,
            })
          }
        >
          {({ copy, copied }) => (
            <Button onClick={copy} color={copied ? "teal" : "blue"}>
              {copied ? "Copied" : 'Copy "template"'}
            </Button>
          )}
        </CopyButton>

        <Button variant="outline" onClick={open}>
          Open modal
        </Button>
      </Group>

      <Modal opened={opened} onClose={close} title="Example modal">
        This modal is controlled with useDisclosure.
      </Modal>
    </div>
  );
}
```

7. Prettier config

- Create/overwrite `.prettierrc` with:

```json
{
  "parser": "typescript",
  "printWidth": 120,
  "singleQuote": true,
  "trailingComma": "all",
  "jsxSingleQuote": true,
  "tabWidth": 4,
  "useTabs": false,
  "semi": true,
  "arrowParens": "always",
  "endOfLine": "auto",
  "importOrder": [
    "^react",
    "<THIRD_PARTY_MODULES>",
    "^components/(.*)$",
    "^[./]"
  ],
  "importOrderSeparation": true,
  "importOrderSortSpecifiers": true,
  "plugins": ["@trivago/prettier-plugin-sort-imports"],
  "overrides": [
    {
      "files": "*.json",
      "options": { "tabWidth": 2 }
    },
    {
      "files": "*.yml",
      "options": { "tabWidth": 2 }
    },
    {
      "files": "*.yaml",
      "options": { "tabWidth": 2 }
    },
    {
      "files": "*.scss",
      "options": {
        "parser": "scss",
        "bracketSameLine": false,
        "bracketSpacing": false,
        "embeddedLanguageFormatting": "auto",
        "htmlWhitespaceSensitivity": "css",
        "insertPragma": false,
        "proseWrap": "preserve",
        "quoteProps": "as-needed",
        "requirePragma": false,
        "singleAttributePerLine": false,
        "singleQuote": true
      }
    }
  ]
}
```

8. TypeScript configs

- Overwrite `tsconfig.json` with:

```jsonc
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "allowJs": true,
    "skipLibCheck": true,
    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",
    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "esModuleInterop": true,
    "incremental": true,
    "sourceMap": true,
    "allowSyntheticDefaultImports": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "references": [
    {
      "path": "./tsconfig.node.json"
    }
  ],
  "include": ["src", "**/*.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules", "cypress", "vite.config.ts"],
  "types": ["vitest/globals", "@types/jest"]
}
```

- Overwrite `tsconfig.node.json` with:

```jsonc
{
  "compilerOptions": {
    "composite": true,
    "skipLibCheck": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true
  },
  "include": ["vite.config.ts"]
}
```

9. EditorConfig

- Create/overwrite `.editorconfig` with:

```ini
# refer https://editorconfig.org/ for more details

root = true

[*]
indent_style = space
indent_size = 4
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
```

10. Env example

- Create empty `.env.example` file.

11. Git ignore

- Create/overwrite `.gitignore` with:

```
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.
# dependencies
/node_modules
/.pnp
.pnp.js
# testing
/coverage
# production
/build
# misc
.DS_Store
*.pem
# debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*
# local env files
.env*.local
# typescript
*.tsbuildinfo
node_modules/
.yalc/
# VS code files
.vscode
.vscode/launch.json
build/
dist/
.next/
#npmrc file
.npmrc
._.DS_Store
*.log*
.env
.env.local
.env.development.local
.env.test.local
.env.production.local
yalc.lock
.idea
node_modules
dist
dist-ssr
*.local
# Editor directories and files
.vscode/*
!.vscode/extensions.json
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

# Cypress
cypress/screenshots
cypress/snapshot/diff
cypress.env.json
cypress/downloads
```

12. Vite config

- Overwrite `vite.config.ts` with:

```ts
/// <reference types="vitest" />
/// <reference types="vite/client" />
import react from "@vitejs/plugin-react";
import Unfonts from "unplugin-fonts/vite";
import { URL, fileURLToPath } from "url";
import { splitVendorChunkPlugin } from "vite";
import { defineConfig } from "vitest/config";
import checker from "vite-plugin-checker";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react(),
    Unfonts({
      // Google Fonts API V2
      google: {
        injectTo: "head-prepend",
        families: [
          // Montserrat as an object with specific styles and options
          {
            name: "Montserrat",
            styles: "wght@300;400;500;600;700",
            defer: true,
          },
        ],
      },
    }),
    checker({
      eslint: {
        // for example, lint .ts and .tsx
        lintCommand: 'eslint "./src/**/*.{ts,tsx}"',
      },
      typescript: true,
    }),
    splitVendorChunkPlugin(),
  ],
  resolve: {
    alias: [
      {
        find: "@",
        replacement: fileURLToPath(new URL("./src", import.meta.url)),
      },
    ],
  },
  server: {
    port: 3000,
  },
  css: {
    preprocessorOptions: {
      scss: {
        silenceDeprecations: [
          "legacy-js-api",
          "import",
          "global-builtin",
          "mixed-decls",
        ],
      },
    },
  },
  test: {
    globals: true,
    environment: "jsdom",
    setupFiles: "./src/test/setupTest.ts",
    css: {
      modules: {
        classNameStrategy: "scoped",
      },
    },
    coverage: {
      provider: "v8",
      all: true,
      include: ["src", "**/*.ts", "**/*.tsx"],
      exclude: [
        "src/config.ts",
        "dist/",
        "src/test",
        "src/store",
        "coverage/",
        "node_modules/",
        "src/vite-env.d.ts",
        "src/**/index.{ts,tsx}",
        "src/**/*.test.{ts,tsx}",
      ],
    },
  },
});
```

13. ESLint config

- Create/overwrite `.eslintrc.js` with:

```js
module.exports = {
  env: {
    browser: true,
    es6: true,
    node: true,
  },
  extends: [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier",
    "plugin:prettier/recommended",
  ],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 2023,
    sourceType: "module",
  },
  plugins: [
    "react",
    "react-hooks",
    "@typescript-eslint",
    "prettier",
    "import",
    "unused-imports",
  ],
  rules: {
    "react/self-closing-comp": ["error", { component: true, html: true }],
    "@typescript-eslint/no-unused-vars": "off",
    "unused-imports/no-unused-imports": "warn",
    "unused-imports/no-unused-vars": [
      "warn",
      {
        vars: "all",
        varsIgnorePattern: "^_",
        args: "after-used",
        argsIgnorePattern: "^_",
      },
    ],
    "@typescript-eslint/explicit-member-accessibility": "off",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/interface-name-prefix": "off",
    "@typescript-eslint/no-explicit-any": "off",
    "arrow-parens": ["off", "as-needed"],
    "spaced-comment": ["warn", "always"],
    "id-blacklist": [
      "error",
      "any",
      "Number",
      "number",
      "String",
      "string",
      "Boolean",
      "boolean",
      "Undefined",
    ],
    "id-match": "error",
    "no-duplicate-imports": "warn",
    "no-eval": "error",
    "object-shorthand": "warn",
    "prefer-template": "error",
    "@typescript-eslint/no-non-null-assertion": "warn",
    "dot-notation": ["off", { allowKeywords: true }],
    "no-nested-ternary": "warn", // Changed from 'error' to 'warn' to allow nested ternaries with warnings
    "no-console": ["warn", { allow: ["warn", "error"] }],
    "react-hooks/exhaustive-deps": "warn",
    "react/no-unknown-property": "error", // disallows unknown props or properties from being pass into a React Component or HTML Element
    "react/prop-types": "off",
    "no-fallthrough": ["error", { allowEmptyCase: true }],
    quotes: [
      "error",
      "single",
      { avoidEscape: true, allowTemplateLiterals: true },
    ],
    camelcase: "error",
    eqeqeq: ["error", "smart"],
    radix: ["warn", "always"],
  },
  settings: {
    react: {
      version: "detect",
    },
  },
  ignorePatterns: [
    ".eslintrc.js",
    "setup-jest.js",
    "coverage",
    "node_modules",
    "build",
    "dist",
    "public",
  ],
};
```

14. Vitest setup file

- Create `src/test/setupTest.ts` (empty or put any shared test setup you need):

```ts
// setup for vitest; jsdom environment is configured in vite.config.ts
```

15. Finalize

```bash
npm install
npm run lint
npm run build

# Initialize git and make the first commit using conventional commits
git init
git add .
git commit -m "feat: initialize repo"
```

### Notes and fixes

- **Node/Vite compatibility**: If using Node < 20.19.0, either upgrade Node to 20.19+ or pin Vite to a compatible version.
  - Option used: set `vite` to `^6.0.0` and `@vitejs/plugin-react` to `^4.3.0`, and remove `splitVendorChunkPlugin()` from `vite.config.ts`.
- **ESLint config format under ESM package**: With `"type": "module"` and ESLint v8, `.eslintrc.js` must be CommonJS.
  - Option used: rename to `.eslintrc.cjs` and pin `eslint` to `^8.57.0`.
  - Alternative: keep ESLint v9+ and migrate to `eslint.config.js` (flat config).
- **ESLint dependencies**: Install `eslint-plugin-unused-imports` if referenced by the rules.
- **Lint script glob**: Remove `"cypress/**/*.{ts,tsx}"` from the `lint` script if Cypress is not present, or add a placeholder file.
- **React in JSX scope rule**: With `eslint-plugin-react`'s `react/react-in-jsx-scope`, add `import React from 'react';` to JSX files (e.g., `src/App.tsx`, `src/pages/HomePage.tsx`).
- **Non-null assertions**: Replace `document.getElementById('root')!` with a runtime check in `src/main.tsx` to satisfy `@typescript-eslint/no-non-null-assertion` when running with zero warnings.
- **Mantine CopyButton API**: `onCopy` prop is not supported. Trigger `notifications.show(...)` inside the button `onClick` after calling `copy()`.
- **Vitest setup file formatting**: Avoid trailing blank lines in `src/test/setupTest.ts`; adding `export {};` keeps it a module and satisfies formatters.
- **Husky setup**: Run `git init` before `npm run prepare` so Husky can install. Husky v9 prints a deprecation note about sourcing `husky.sh`; hooks provided here are fine for v9 but should be updated when moving to v10.
- **TypeScript types**: If `"types": ["vitest/globals", "@types/jest"]` is present in `tsconfig.json`, install `@types/jest` as a dev dependency.
- **ESLint ignores**: Consider ignoring `src/vite-env.d.ts` in ESLint to avoid style warnings from auto-generated comments.
- **ESLint nested ternary expressions**: The `no-nested-ternary` rule is set to 'warn' instead of 'error' to allow nested ternary expressions with warnings rather than blocking errors.
- **Prettier import sorting**: When using Prettier v3.x with import sorting, add `@trivago/prettier-plugin-sort-imports` to dependencies and explicitly specify it in the plugins array in `.prettierrc`.

### 16. 404 (Not Found) route support

- Add a `NotFound` page at `src/pages/NotFound.tsx`:

```tsx
import React from "react";
import { Button, Center, Group, Stack, Text, Title } from "@mantine/core";
import { Link } from "react-router-dom";

export default function NotFound() {
  return (
    <Center style={{ minHeight: "60vh" }}>
      <Stack align="center" gap="sm">
        <Title order={2}>404 - Page not found</Title>
        <Text c="dimmed">The page you are looking for does not exist.</Text>
        <Group>
          <Button component={Link} to="/" variant="filled">
            Go home
          </Button>
        </Group>
      </Stack>
    </Center>
  );
}
```

- Register a catch-all route in `src/App.tsx` so only `/` is supported and all others show 404:

```tsx
import { Routes, Route } from "react-router-dom";
import HomePage from "./pages/HomePage";
import NotFound from "./pages/NotFound";

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```
