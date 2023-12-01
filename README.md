<!-- markdownlint-configure-file {
  "MD033": false,
  "MD041": false,
} -->

<div align="center">
  <img src="./public/logo.svg" alt="PMJS logo" width="500">
</div>

# PMJS <!-- omit from toc -->

![npm](https://img.shields.io/npm/v/pmjs?style=for-the-badge&logo=npm&logoColor=white&labelColor=CB3837&color=CB3837)
![Static Badge](<https://img.shields.io/badge/Coverage%20(istanbul)-100%25-green?style=for-the-badge&logo=vitest&logoColor=black&labelColor=298D46&color=298D46>)

Asynchronous library with built-in caching to identify available package managers.

## Table of contents <!-- omit from toc -->

- [📡 Features](#-features)
- [🕹️ Install](#️-install)
- [🏄‍♀️ Usage](#️-usage)
  - [Example of defining a package manager for the current project](#example-of-defining-a-package-manager-for-the-current-project)
  - [Example of detecting all globally installed package managers](#example-of-detecting-all-globally-installed-package-managers)
  - [Clearing PMJS cache](#clearing-pmjs-cache)
- [📜 API](#-api)
  - [defineManager()](#definemanager)
  - [defineGlobalManagers()](#defineglobalmanagers)
  - [clearCache()](#clearcache)

## 📡 Features

- [x] Defines the package manager that manages your application.
- [x] Identifies all available globally installed package managers and their versions.
- [x] Can detect package managers such as: **`npm`**, **`yarn`**, **`pnpm`** and **`bun`**.
- [x] **Uses cache** at all stages, which can be forcibly cleared.
- [x] ESM and CommonJS available.

<!-- markdownlint-disable -->
<div align="right">...</div>
<!-- markdownlint-restore -->

## 🕹️ Install

```bash
npm install pmjs
```

PMJS offers several import options - default import, or named import.

```js
// default import
import pmjs from 'pmjs';

// or named import. Recommended!
import { defineManager, defineGlobalManagers } from 'pmjs';
```

Or if you want to use CommonJS:

```js
const pmjs = require('pmjs');
// or
const { defineManager, defineGlobalManagers } = require('pmjs');
```

<!-- markdownlint-disable -->
<div align="right">...</div>
<!-- markdownlint-restore -->

## 🏄‍♀️ Usage

### Example of defining a package manager for the current project

```js
import { defineManager } from 'pmjs';

async function demoFunction() {
  const packageManager = await defineManager('/path/to/project');

  console.log(packageManager); // npm | yarn | pnpm | bun
}

demoFunction();
```

### Example of detecting all globally installed package managers

```js
import { defineGlobalManagers } from 'pmjs';

async function demoFunction() {
  const globalManagers = await defineGlobalManagers();

  console.log(globalManagers);
  // [
  //   {
  //     manager: 'pnpm';
  //     version: '8.11.0';
  //   },
  //   {
  //     manager: 'bun';
  //     version: '1.0.14';
  //   },
  //   ...
  // ]
}

demoFunction();
```

### Clearing PMJS cache

Since all PMJS operations are cached in order to optimize speed, sometimes you may need to clear the cache manually.

```js
import { clearCache } from 'pmjs';

clearCache();
```

<!-- markdownlint-disable -->
<div align="right">...</div>
<!-- markdownlint-restore -->

## 📜 API

### defineManager()

Defines the package manager used in a given directory.  
Checks for the presence of characteristic files (e.g., package-lock.json for npm) for each known package manager.

Supported package managers: **`npm`**, **`yarn`**, **`pnpm`**, **`bun`**

```js
type PackageManager = "npm" | "yarn" | "pnpm" | "bun"

defineManager: (path?: string) => Promise<PackageManager | null>
```

| Argument | Type     | Default value                                                     | Description                                                                                     |
| -------- | -------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `path`   | `string` | [`process.cwd()`](https://nodejs.org/api/process.html#processcwd) | The path to the directory to check. If not provided, defaults to the current working directory. |

**Return**: A promise that resolves to the type of package manager used or `null` if no package manager can be defined.

### defineGlobalManagers()

```js
interface IGlobalManagerData {
 manager: PackageManager;
 version: string;
}

defineGlobalManagers: () => Promise<IGlobalManagerData[] | null>
```

**Return:** A promise that resolves to an array of global manager data, including the name and version of each installed package manager, or `null` if none is installed.

### clearCache()

Clears the entire cache.  
This function removes all cached data related to package manager checks.
Useful for resetting the state to ensure fresh data is fetched.

```js
clearCache: () => void
```
