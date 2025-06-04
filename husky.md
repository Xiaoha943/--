##### husky

配置项：

package.json：

```json
 "scripts": {
     "precommit": "lint-staged",
 },

"lint-staged": {
    "**/*.less": "stylelint --syntax less",
    "**/*.{js,jsx,ts,tsx}": "npm run lint-staged:js",
    "**/*.{js,jsx,tsx,ts,less,md,json}": [
      "prettier --write"
    ]
  },
 "husky": {
    "hooks": {
      "pre-commit": "npm run lint",
      "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
    }
  },
 "devDependencies": {
     "husky": "^7.0.4",
 },
```

创建快捷执行precommit ：

`pnpm precommit`

7.0.4版本手动执行：

`npx husky install`

生成自动precommit：

`npx husky add .husky/pre-commit "pnpm lint-staged"`

![image-20241219160145194](D:\PrivateFiles\Note\复习笔记\img\image-20241219160145194.png)