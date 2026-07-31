# Dinheiros

App pessoal de controle financeiro focado em **previsibilidade**: lance suas entradas e saídas (inclusive as fixas/recorrentes) e veja o saldo projetado no futuro.

Desenvolvido por **ALLTORAL**.

## Stack

- HTML puro + React via CDN (sem build, sem Node, sem `npm install`)
- Firebase (Auth com Google + Firestore) para sincronizar os dados entre dispositivos
- PWA: instalável na tela de início do celular, funciona em tela cheia como um app nativo

## Estrutura

```
index.html          → app inteiro (interface + lógica)
manifest.json        → configuração do PWA (nome, ícone, cores)
icon-192.png          → ícone do app (192x192)
icon-512.png          → ícone do app (512x512)
apple-touch-icon.png  → ícone para iPhone/iPad
```

## Rodando localmente

Não precisa de instalação. Basta abrir o `index.html` num navegador, ou servir a pasta com qualquer servidor estático:

```bash
python3 -m http.server 8000
```

e acessar `http://localhost:8000`.

## Deploy

O projeto é 100% estático, então funciona em qualquer host de arquivos estáticos:

- **Netlify** (recomendado): conecte este repositório em [app.netlify.com](https://app.netlify.com) → "Add new site" → "Import an existing project" → escolha este repo no GitHub. Todo `git push` na branch principal atualiza o site automaticamente.
- **GitHub Pages**: em Settings → Pages, escolha a branch `main` e a pasta raiz.
- **Vercel**: importe o repositório em vercel.com, sem configuração adicional necessária.

## Configuração do Firebase

O app usa Firebase Auth (login com Google) e Firestore (banco de dados) para sincronizar os lançamentos entre dispositivos. As credenciais em `index.html` (`firebaseConfig`) são públicas por natureza — quem protege os dados são as regras do Firestore, configuradas para que cada usuário só acesse os próprios dados:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /alltoral-fin/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

Se for clonar este projeto para outro uso, troque o `firebaseConfig` pelo de um novo projeto Firebase.
