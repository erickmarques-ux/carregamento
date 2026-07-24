# Carregamento — Torre de Controle (web/mobile)

Página pública, só leitura, para acompanhar o carregamento pelo celular ou tablet.

## Por que não usa o Apps Script

A política do Workspace do Mercado Livre só permite publicar web app para contas
`@mercadolivre.com`. No celular não dá para logar, então o Apps Script não serve
como endpoint público. A saída: a extensão grava o snapshot como JSON **neste
repositório**, que é público de verdade.

```
Torre (extensão, no PC)  --commit-->  data/SMG15.json  <--fetch--  celular
      tem a sessão do ML              repositório público          sem login
```

## Passo a passo

### 1. Criar o repositório
Crie um repositório **público** (ex.: `carregamento`) e suba `index.html` e `manifest.json`.

### 2. Criar o token
GitHub > Settings > Developer settings > **Personal access tokens** > *Fine-grained tokens* > Generate new token:
- **Repository access:** Only select repositories > escolha só este repositório
- **Permissions:** Repository permissions > **Contents: Read and write**
- Gere e copie o token (só aparece uma vez)

### 3. Configurar a extensão
No `host.js`, no topo:
```js
const GITHUB = {
  owner:  'seu-usuario',
  repo:   'carregamento',
  branch: 'main',
  token:  'github_pat_...',
  pasta:  'data'
};
```

### 4. Configurar a página
No `index.html`, o mesmo owner/repo:
```js
var REPO = { owner:'seu-usuario', repo:'carregamento', branch:'main', pasta:'data' };
```

### 5. Ligar o Pages
Settings > Pages > Source: `main` / raiz. O link sai como
`https://seu-usuario.github.io/carregamento/`.

## Sobre o token

O token fica dentro da extensão, então quem tiver a extensão consegue lê-lo.
Por isso ele deve ser **fine-grained, restrito a este único repositório, só Contents**.
No pior caso alguém escreve neste repositório — que só guarda snapshot de carregamento.
Nenhum dado de sessão do ML passa por aqui.

Se o token vazar: GitHub > Settings > Tokens > Revoke, gere outro e atualize o `host.js`.

## Instalar como app no celular

- **Android/Chrome:** menu ⋮ > "Adicionar à tela inicial"
- **iPhone/Safari:** compartilhar > "Adicionar à Tela de Início"
