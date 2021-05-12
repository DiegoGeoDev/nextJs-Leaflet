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


6 - Configurar Git Hooks com Husky (Opcional)

Precisa criar um repositótio no github para o projeto
https://typicode.github.io/husky/#/
Instalar o husky e criar o pre-commit
npx husky-init && yarn

Instalar o lint staged
https://github.com/okonet/lint-staged

Configurar o lint staged no package.json
```json
  "lint-staged": {
    "src/**/*":[
      "yarn lint --fix"
    ]
  }
```

Configurar o lint staged no pre-commit

```batch
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install lint-staged
```

Desta forma ao tentar subir o código no repositório somente será permitido se não existir erros

Ao tentar fazer
git add .
git commit -m "text"
Vai verificar se existem erros e somente vai permitir fazer commits se não existir


7 - Configurar o Typescript com Jest

https://jestjs.io/pt-BR/


```batch
yarn add --dev jest @babel/preset-typescript @types/jest
```

Criar o arquivo jest.config.js

```javascript
module.exports = {
  testEnvironment: 'jsdom',
  testPathIgnorePatterns: ['/node_modules/', '/.next/'],
  collectCoverage: true,
  collectCoverageFrom: ['src/**/*.ts(x)'],
  setupFilesAfterEnv: ['<rootDir>/.jest/setup.ts']
}
```

Atualizar o arquivo .eslintrc.json

```json
  "env": {
    "browser": true,
    "es2020": true,
    "jest": true,
    "node": true
  },
```

Configurar o babel criando o arquivo .babelrc

```json
{
  "presets": ["next/babel", "@babel/preset-typescript"]
}
```

Criar a pasta .jest e o arquivo setup.ts


8 - Instalar React Testing Library o Testing Library jest dom

https://testing-library.com/docs/react-testing-library/intro/
https://github.com/testing-library/jest-dom

```batch
yarn add --dev @testing-library/react @testing-library/jest-dom
```

Adicionar o import no arquivo .jest/setup.ts

import '@testing-library/jest-dom'

Atualizar o package.json

```json
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint src --max-warnings=0",
    "postinstall": "husky install",
    "test": "jest",
    "test:watch": "yarn test --watch"
  },
  "lint-staged": {
    "src/**/*": [
      "yarn lint --fix", "yarn test --bail"
    ]
  },
```

9 - Usando findRelatedTests para o hook

Utilizado para indicar ao jest para rodar testes para os arquivos que foram modificados

```json
  "lint-staged": {
    "src/**/*": [
      "yarn lint --fix",
      "yarn test --findRelatedTests --bail"
    ]
  },
```

10 - Instalando o Style Components e configurando SSR

https://styled-components.com/

instalar plugin babel e types

```bash
yarn add --dev babel-plugin-styled-components @types/styled-components
```

Atualizar o babel

```json
  "plugins": [
    [
      "babel-plugin-styled-components",
      {
        "ssr": false
      }
    ]
  ]
```

instalar styled components

```bash
yarn add styled-components
```

Atualizar o arquivo _document do Next (criar o mesmo em pages)


```javascript
import Document, {
  Html,
  Head,
  Main,
  NextScript,
  DocumentContext
} from 'next/document'
import { ServerStyleSheet } from 'styled-components'

export default class MyDocument extends Document {
  static async getInitialProps(ctx: DocumentContext) {
    const sheet = new ServerStyleSheet()
    const originalRenderPage = ctx.renderPage

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App) => (props) =>
            sheet.collectStyles(<App {...props} />)
        })

      const initialProps = await Document.getInitialProps(ctx)
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>
        )
      }
    } finally {
      sheet.seal()
    }
  }

  render() {
    return (
      <Html lang="pt-BR">
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    )
  }
}
```


11 - Criando estilos Globais com createGlobalStyle

Atualizar o tsconfig.json para absolute imports

"baseUrl": "src"

Criar o arquivo globl.ts em styles

Criar o arquivo _app.tsx em pages


12 - Melhorar snapshot com jest style components

```batch
yarn add --dev jest-styled-components
```


13 - NEXT PWA

https://github.com/shadowwalker/next-pwa

```batch
yarn add next-pwa
yarn add webpack@4
```
