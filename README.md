<h1>Tailwind CSS Blazor (.NET)</h1> 
Blazor é um framework .NET de código aberto para construir front-ends dinâmicos e interativos para seus aplicativos com modelos HTML, C# e Razor. O Blazor permite que você componha componentes diretamente para seu servidor ou para o lado do cliente. Essa flexibilidade permite que os desenvolvedores criem aplicativos móveis e web fullstack com uma estrutura de IU de página única.

Normalmente, a maioria dos frameworks frontend usa JavaScript por baixo dos panos, mas com o Blazor, você pode construir front-ends e back-ends com C#. Desenvolvedores que são bem versados ​​em C# podem facilmente construir aplicativos fullstack sem mudar para um framework diferente.

Mais empresas estão adotando o Blazor em seus fluxos de trabalho de desenvolvimento porque um desenvolvedor pode escrever lógica do lado do cliente e do lado do servidor com apenas C# e .NET. Alguns exemplos incluem GE Aviation, BurnRate, The Postage e Pernod Ricard.

O Blazor fornece todos os andaimes, abstrações, ferramentas e otimizações de que você precisa em um projeto.

<h3>Requisitos</h3>
Neste guia, você aprenderá como construir um novo Projeto Blazor e como integrar componentes da IU do Flowbite em seu aplicativo. Usaremos um componente modal para este exercício para demonstrar um caso de uso real.

Você precisará instalar e configurar o .NET SDK, Tailwind CSS, Blazor e Flowbite em seu aplicativo. Certifique-se de ter instalado o NPM e o Node.js em seu ambiente local. Vamos começar!

<h3>Crie um novo projeto Blazor</h3>
Comece baixando e instalando o .NET SDK. O SDK nos permite desenvolver aplicativos com frameworks .NET. O site do Blazor detecta qual versão você precisará para seu ambiente local.

Comece instalando o repositório de pacotes da Microsoft que contém a chave de assinatura do pacote:
```
donet new blazorwasm –o BlazorTailwindTodo;
```
Na raiz do projeto instale o tailwind
```
npm install -D tailwindcss;
```

<h3>Crie e configure o arquivo PostCSS</h3>
Create a tailwind.config.js file in the BlazorApp directory or your root directory and add these configurations:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./**/*.{html,js,razor}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
Execute este comando no seu diretório raiz para gerar um arquivo de configuração CSS do Tailwind:

```
npx tailwindcss init
```
Em seguida, crie um arquivo Theme.css na pasta wwwroot/ (dentro da pasta CSS que você não excluiu anteriormente). Adicione estas diretivas CSS ao app.css:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
Acesse seu terminal e execute o Tailwind CLI para gerar o CSS de saída e observar alterações em seu projeto:

```json
{
  "name": "blazorwindtodo",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "watch": "npx tailwindcss -i ./Themes/Theme.css -o ./wwwroot/css/theme.css --watch",
    "build": "npx tailwindcss -i ./Themes/Theme.css -o ./wwwroot/css/theme.mini.css --minify"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "devDependencies": {
    "tailwindcss": "^3.4.17"
  }
}
```
Configure sua index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorWindTodo</title>
    <base href="/" />
    <link rel="stylesheet" href="css/theme.css" />
    <link rel="stylesheet" href="css/app.css" />
    <link rel="icon" type="image/png" href="favicon.png" />
    <!-- <link href="BlazorWindTodo.styles.css" rel="stylesheet" /> -->
    <script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="h-full bg-gray-100">
    <div id="app">
        <svg class="loading-progress">
            <circle r="40%" cx="50%" cy="50%" />
            <circle r="40%" cx="50%" cy="50%" />
        </svg>
        <div class="loading-progress-text"></div>
    </div>

    <div id="blazor-error-ui">
        An unhandled error has occurred.
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>
    <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>
```
<h2>Install Flowbite</h2>
Flowbite é uma biblioteca de componentes de UI de código aberto que é construída com Tailwind CSS e vanilla JavaScript. Veja como você pode instalá-lo e configurá-lo seguindo estas etapas para fazê-lo funcionar com nosso projeto Blazor:
```
npm install flowbite
```
Require Flowbite in the Tailwind configuration file as a plugin:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./**/*.{html,js,razor}",
    "./node_modules/flowbite/**/*.js"
  ],
  theme: {
    extend: {},
  },
  plugins: [
    require('flowbite/plugin')
  ],
}
```

<h2>Integração WASM</h2>
Para usar o Flowbite com o Blazor WebAssembly (WASM), você precisará configurar as funções init do Flowbite usando uma camada de interoperabilidade que garante a renderização do DOM antes de aplicar os ouvintes de eventos por meio da API de atributos de dados.

Definar a função JS para inicializar o Flowbite dentro do arquivo index.html:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorWindTodo</title>
    <base href="/" />
    <link rel="stylesheet" href="css/theme.css" />
    <link rel="stylesheet" href="css/app.css" />
    <link rel="icon" type="image/png" href="favicon.png" />
    <!-- <link href="BlazorWindTodo.styles.css" rel="stylesheet" /> -->
    <script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="h-full bg-gray-100">
    <div id="app">
        <svg class="loading-progress">
            <circle r="40%" cx="50%" cy="50%" />
            <circle r="40%" cx="50%" cy="50%" />
        </svg>
        <div class="loading-progress-text"></div>
    </div>

    <div id="blazor-error-ui">
        An unhandled error has occurred.
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>
    <script src="_framework/blazor.webassembly.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/flowbite@2.5.2/dist/flowbite.min.js"></script>
</body>
<script>
    window.initializeFlowbite = () => {
        initFlowbite();
    }
</script>
</html>
```
