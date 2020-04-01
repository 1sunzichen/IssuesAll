设置vsCode
安装插件：ESList和Prettier-Code formatter
修改settings.json:
```
"editor.formatOnSave": true,
"eslint.validate": [
    "vue",
    "html",
    "javascript",
    "typescript",
    "javascriptreact",
    "typescriptreact"
],
"files.associations": {
    "*.js": "javascriptreact"
},
```
安装依赖
由于create-react-app的eslint依赖已经安装好了，我们这里只需要安装prettier
```
yarn add --dev prettier eslint-config-prettier eslint-plugin-prettier
```
修改配置
新建.eslintrc并在package.json里面删除eslintConfig
```
{
  "extends": ["react-app", "plugin:prettier/recommended"],
  "rules": {}
}
```
新建.prettierrc并添加
```
{
  "printWidth": 90,
  "bracketSpacing": false,
  "trailingComma": "es5"
}
```
添加格式化命令
在packgage.json里面添加script标签
```
"scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint --fix .",
    "format": "prettier --write \"**/*.+(js|jsx|json|css|md)\""
  },
```
添加commit命令
添加在commit之前自动化格式代码, 这样会在commit的时候速度比较慢，但是有利于保证代码仓库的质量。
添加依赖
```
yarn add --dev lint-staged husky
```
修改package.json
```
  "lint-staged": {
    "*.+(js|jsx)": [
      "eslint --fix",
      "git add"
    ],
    "*.+(json|css|md)": [
      "prettier --write",
      "git add"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
```
只使用eslint进行格式化
如果我们选择只使用eslnit,需要先disable这个插件Prettier-Code formatter，然后在settings.json里面加入
```
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
```
