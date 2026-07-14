# Pelada de Segunda

App para avaliar os jogadores da pelada e sortear os times, com dados compartilhados
em tempo real (todo mundo que acessar vê e vota nos mesmos jogadores).

## 1. Criar o projeto no Firebase (grátis)

1. Acesse https://console.firebase.google.com e clique em **Adicionar projeto**.
2. Dê um nome (ex: `pelada-segunda`) e siga o assistente (pode desativar o Google Analytics, não precisa).
3. No menu lateral, vá em **Firestore Database** → **Criar banco de dados**.
   - Escolha o modo **de produção** (vamos colar regras próprias no passo 3).
   - Escolha uma região próxima (ex: `southamerica-east1` — São Paulo).
4. Ainda no console, clique no ícone de engrenagem → **Configurações do projeto**.
5. Em **Seus apps**, clique no ícone `</>` (Web) para criar um app web.
   - Dê um apelido qualquer (ex: `pelada-web`) e clique em **Registrar app**.
   - Você vai ver um bloco `firebaseConfig = { apiKey: ..., authDomain: ..., ... }`.
   - **Copie esse bloco inteiro.**

## 2. Colar a configuração no app

Abra o arquivo `index.html` deste projeto, procure por:

```js
const firebaseConfig = {
  apiKey: "COLE_AQUI_SUA_API_KEY",
  ...
};
```

e substitua pelos valores que você copiou no passo 1.5.

## 3. Configurar as regras do Firestore

No console do Firebase: **Firestore Database** → aba **Regras**, cole o conteúdo do
arquivo `firestore.rules` (também incluso aqui) e clique em **Publicar**.

⚠️ Essas regras deixam o banco **aberto para leitura e escrita por qualquer pessoa que
tenha a URL do app** — não tem login/senha. Isso é aceitável pra um app de uso entre
amigos com link não-divulgado, mas não coloque dados sensíveis. Se quiser travar mais
depois, dá pra adicionar Firebase Authentication.

## 4. Publicar no GitHub Pages

1. Crie um repositório novo no GitHub (ex: `pelada-segunda`).
2. Suba os arquivos `index.html` deste projeto para a raiz do repositório (pelo GitHub
   Desktop, `git push`, ou upload manual pelo site do GitHub).
3. No repositório: **Settings** → **Pages** → em "Build and deployment", escolha
   **Deploy from a branch**, branch `main`, pasta `/root`. Salve.
4. Em alguns minutos o GitHub mostra a URL pública (algo como
   `https://seu-usuario.github.io/pelada-segunda/`). É esse link que você manda pro
   pessoal do grupo.

## 5. Testar com um amigo

Abra o link em dois navegadores/dispositivos diferentes (ou manda pro seu amigo).
Cada um escolhe um nome na primeira vez que abre. Ao avaliar um jogador, a nota,
média e contagem devem atualizar **na hora** na tela de quem também estiver com o
app aberto — é a sincronização em tempo real do Firestore.

## Estrutura dos dados no Firestore

- `players` (coleção): um documento por jogador — `{name, position, createdAt}`.
- `votes` (coleção): um documento por (jogador + quem votou) — id
  `{playerId}__{voterId}`, campos `{playerId, voterId, voterName, stars, ts}`.
- `meta/currentDraw` (documento): último sorteio salvo — `{teams, missingSlots, teamSize, ts}`.

## Limitações conhecidas do MVP

- Sem login: a identidade de quem vota é só um nome guardado no navegador
  (`localStorage`). Trocar de navegador/aparelho conta como "pessoa nova".
- Regras do Firestore abertas (ver passo 3) — ok para uso interno com link privado.
- Se quiser fechar mais o acesso, evoluir para Firebase Authentication (login por
  e-mail/Google) é o próximo passo natural.
