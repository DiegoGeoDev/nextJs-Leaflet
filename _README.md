https://nextjs.org/


1 - criar o projeto

```batch
npx create-next-app
REM OU
yarn create next-app
```


2 - Integrar com TypeScript

Criar o arquivo tsconfig.json na raiz do projeto

```batch
yarn add --dev typescript @types/react @types/node
```

mudar strict para true no tsconfig.json

Criar a pasta src e colocar pages dentro da mesma


3 - Criar o arquivo .editorconfig


4 - Instalar o eslint

```batch
"lint": "eslint src --fix"
```

- To check syntax and find problems
- JavaScript modules (import/export)
- React
- Yes
- Browser
- JSON

Instalar as dependências

```batch
yarn add --dev eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest eslint@latest
```

Instalar o plugin para react hooks
https://www.npmjs.com/package/eslint-plugin-react-hooks

```
yarn add --dev eslint-plugin-react-hooks
```

Atualizar o arquivo .eslintrc.json com as configurações do plugin react hooks

```json
  "plugins": [
    "react",
    "react-hooks",
    "@typescript-eslint"
  ],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/prop-types": "off",
    "react/react-in-jsx-scope": "off",
    "@typescript-eslint/explicit-module-boundary-types": "off"
  }
```

Adicionar a configuração para detectar a versão do react

```json
"settings": {
  "react": {
    "version": "detect"
  }
}
```

O plugin do eslint para vscode ajuda na identificação de erros
porém é interessante criar o comando de análise do eslint no package.json
também para verificar itens de determinadas pastas neste casos como todo
o projeto fica em source o mesmo é utilizado nesta pasta

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "eslint src"
}
```


5 - Configurar o Prettier com eslint

https://prettier.io/

Instalar o prettier

```batch
yarn add --dev --exact prettier
```

Criar o arquivo .prettierrc

```json
{
  "trailingComma": "none",
  "semi": false,
  "singleQuote": true
}
```

Instalar o plugin do preetier para eslint

https://github.com/prettier/eslint-plugin-prettier

```batch
yarn add --dev eslint-plugin-prettier eslint-config-prettier
```

Caso o vscode não tenha os plugins de preetier e eslint é necessário criar a pasta
.vscode com o arquivo settings.json

```json
{
  "editor.formatOnSave": false,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```


6 - Configurar Git Hooks com Husky

https://typicode.github.io/husky/#/

Precisa criar um repositótio no github para o projeto

yarn add --dev husky


