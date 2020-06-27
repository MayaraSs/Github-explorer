## Github explorer

# Iniciando o projeto

# A parte de configuração do projeto é opcional e cada desenvolvedor pode utilizar do jeito que achar melhor.

- [x] Template

  - Foi executado o comando para criar um template de projeto typescript.

  ```
    npx create-react-app project-react --template=typescript
  ```

  Foi excluída algumas pastas do projeto original, pois não será utilizadas no nomento.

- [x] Agora vamos configurar o ESLint

  ```
    yarn add eslint -D
  ```

  Exclui a configuração antiga do eslint do packge json

  ```
   - yarn eslint --init
  ```

  Como estou utilizando o yarn espero finalizar a execução do comando anterior e copio a última linha para instalar as dependências necessárias com o yarn.

  ```
    - yarn add eslint-plugin-react@^7.20.0 @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint-plugin-import@^2.21.2 eslint-plugin-jsx-a11y@^6.3.0 eslint-plugin-react-hooks@^4 || ^3 || ^2.3.0 @typescript-eslint/parser@latest -D

  ```

  Crio um arquivo chamado eslintignore para ignorar o eslint em alguns arquivos.

  ```
    \*_/_.js
    node_modules
    build
  ```

  No arquivo .eslintrc vou adicionar

  ```
    No extends: "plugin:@typescript-eslint/recommended"
    No plugins: "react-hooks",
    No rules: "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/jsx-filename-extension": [1, { "extensions": [".tsx"] }],
    "import/prefer-default-export": "off",
    "import/extensions": [
    "error",
    "ignorePackages",
     {
    "ts": "never",
    "tsx": "never"
     }
     ]

  ```

  Executo o comando abaixo, que irá permitir que o react entenda typescript nas importações.

  ```
    yarn add eslint-import-resolver-typescript -D

  ```

  No meu arquivo .eslintrc adiciono

  ```
    "settings": {
    "import/resolver": {
    "typescript": {}
    }
    }

  ```

- [x] Configurando o Prettier

  ```
    yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
  ```

  No arquivo .eslintrc adiciono:

  ```
    No extend: "prettier/@typescript-eslint",
    "plugin:prettier/recommended"
    No plugins: "prettier"
    No rules: "prettier/prettier": "error"
  ```

  Crio um arquivo chamado prettier.config.js para exportar algumas configurações

  ```
    module.exports = {
    singleQuote: true,
    trailingComma: "all",
    allowParens: "avoud",
    };
  ```

# Criando a aplicação

- [x] Criando as Rotas da aplicação.

  Como a aplicação terá duas telas a Home e User é necessário ter alguma biblioteca que lida com o repasse de uma tela para outra.

  ```
    yarn add react-router-dom
  ```

  Crio uma pasta chamada Routes e dentro desta pasta crio um arquivo chamado index.tsx.

  Como vou precisar navegar na aplicação vou criar rotas e páginas, para isso vou criar um arquivo chamado pages dentro da pasta src. E geralmente é criada uma pasta para cada página para organizar tudo que é relacionado a uma página ficará dentro daquele arquivo.

  Criei duas pastas (Dashboard e Repository) em cada pasta criei um arquivo index.jsx e criei um componente simples.

  Esqueci de add os tipos da biblioteca então executei o comando:

  ```
    yarn add @types/react-router-dom -D
  ```

  No arquivo de rotas importei as rotas. Criei um componente chamado Routes do tipo React.FC porque ele vai ser uma função que retorna alguma coisa. Eu vou ter um Switch por volta e a cada página da minha aplicação eu vou ter uma rota, eu passo um path indicando o caminho da minha rota e o componente que eu quero mostrar na tela.

  ```
    import React from 'react';
    import { Switch, Route } from 'react-router-dom';

    import Dashboard from '../pages/Dashboard';
    import Repository from '../pages/Repository';

    const Routes: React.FC = () => (
    <Switch>
    <Route path="/" component={Dashboard}></Route>
    </Switch>
    );

    export default Routes;
  ```

  O Switch basicamente permite o acesso a cada rota separada, eu consigo criar uma pagina para cada rota e acessar separadamente se eu tirar o Switch e apenas colocar um <> </> Aperacerá na tela todas as rotas descritas em apenas uma página. Então, o Switch garantirá que apenas uma rota seja acessada.

  Fui dar um yarn start e deu um erro que nunca tinha visto, fui atrás dos desenvolvedores para solucionar.

  ```
    - Tive que criar um arquivo .env
    - Inseri SKIP_PREFLIGHT_CHECK=true
  ```

  Agora consigo verificar a minha aplicação, mas nada mudou porque dentro do meu App eu preciso mostrar as rotas.

  Após importadas as rotas e realizada a chamada deu um erro:

  ```
    You should not use <Switch> outside a <Router>
  ```

  Deu esse erro porque dentro do router-dom existe alguns tipos de router. Neste caso iremos utilizar o BrowserRouter, eu coloco ele por volta das rotas e ele funciona como o endereço e só colocar o path que ele acessa a página.

  ```
    import React from 'react';
    import { BrowserRouter } from 'react-router-dom';

    import Routes from './routes';

    const App: React.FC = () => (
    <BrowserRouter>
    <Routes />
    </BrowserRouter>
    );

    export default App;
  ```

  Mas, se criar uma nova rota e colocar o path no endereço ainda vou continuar acessando a rota antiga, porque o react-router-dom ele não faz uma verificação de igualdade do path com o caminho que inseri no endereço, ele faz apenas uma verificação de inclusão, ou seja, ele apenas verifica se existi uma barra e cai sempre na primeira rota, para resolver esse problema eu preciso inserir na minha rota a propriedade exact que irá faz uma verificação de igualdade.

  ```
    <Route path="/" exact component={Dashboard}></Route>
  ```

- [x] Aprendendo a utilizar o pacote do react Styled Components, esse pacote basicamente isola o css em componentes não afetando o restante da aplicação.

  ```
    yarn add styled-components
    yarn add @types/styled-components -D
  ```

  Então, é criado um arquivo styles.tsx. E importado o arquivo styled. E começo a criar os meus componentes estilizado. Criei também um arquivo global para armazenar o css que será utilizado em todas págnas da minha aplicação e importei no mu arquivo App. Coloquei o <> por volta, porque não posso ter o <BrowserRouter> e o <GlobalStyle> um embaixo do outro sem nada por volta deles.

```
   <>
    <BrowserRouter>
      <Routes />
    </BrowserRouter>
    <GlobalStyle />
  </>
```

Criei uma pasta chamada assets para armazenar todas as imagens utilizadas na aplicação.

- [x] Estilizando Dashboard

  Usando Css com typescript.

  Entendendo o conceito de estado de componentes de encadeamento. Eu consigo colocar o input direto dentro do Form, isso significa que todo input dentro do Form vai ter esse stylo.

  ```
    export const Form = styled.form`
    margin-top: 40px;
    max-width: 700 px;

    display: flex;

    input {
      flex: 1;
      height: 70px;
      padding: 0 24px;
      border: 0;
      border-radius: 5px 0 0 5px;
    }
  ```

  Quando eu uso o & dentro do stylo eu estou m referindo ao próprio elemento.

  Adicionei a biblioteca polished. QUe permite trabalhar com cores para clarear, escurecer, etc.

  ```
  yarn add polished
  ```

  Para incluir js dentro do meu css sempre uso essa sintaxe \${ ...}.
