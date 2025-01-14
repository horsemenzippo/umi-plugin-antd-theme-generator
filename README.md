<!-- @format -->

# umi-plugin-antd-theme-generator

[![NPM version](https://img.shields.io/npm/v/umi-plugin-antd-theme-generator.svg?style=flat)](https://npmjs.org/package/umi-plugin-antd-theme-generator) [![NPM downloads](http://img.shields.io/npm/dm/umi-plugin-antd-theme-generator.svg?style=flat)](https://npmjs.org/package/umi-plugin-antd-theme-generator)


Generate theme css file by theme config.

Theme css are generated by following steps.
1. use less plugin post processor to collect less files in use.
2. convert less file to css for each theme.

## Configure

```js
export default {
  antdThemeGenerator: {
    theme: [
      {
        key: 'dust',
        modifyVars: {
          '@primary-color': '#F5222D',
          '@border-radius-base': '5px',
        },
      },
      {
        key: 'volcano',
        modifyVars: {
          '@primary-color': '#FA541C',
        },
      },
    ],
    // compress css
    min: process.env.NODE_ENV === 'production',
    // css module
    generateScopedName：(filePath: string, className: string) => {
      const match = filePath.match(/src(.*)/);
      if (match && match[1]) {
        const basePath = match[1].replace('.less', '');
        const arr = winPath(basePath)
          .split('/')
          .map((a: string) => a.replace(/([A-Z])/g, '-$1'))
          .map((a: string) => a.toLowerCase());
        return `${arr.join('-')}-${className}`.replace(/--/g, '-');
      }
      return className;
    }
  },
}
```

### theme

Themes that will generate css file.

### min

Compress css or not. Default is false.

### generateScopedName

Used by css module to generate css className. Default is as the example above.

### varFile

Variable file containing ant design specific and your own custom variables. Default is `/node_modules/antd/lib/style/themes/default.less`.

## How to change theme

To change theme, you should invoke `changeTheme` with the `key` of theme.

```js
import { changeTheme } from 'umi';

changeTheme("dust");
```

## LICENSE

MIT
