# 1. Eleição do Tema da Feira de Projetos 2026

**Equipe:** _(nome da equipe)_ · **Integrantes:** _(nome – papel)_ ×4 · **Curso:** Técnico em Desenvolvimento de Sistemas · **Prof.:** Leandro Gaudio Rosa

## 2. Objetivo e público
Eleição **simulada** para escolher o tema da Feira de Projetos da escola (4 candidaturas). Público: estudantes. Nenhum voto é real e nenhum dado pessoal é coletado. Pergunta-guia: *o que continua funcionando quando a internet cai?* → **tudo**: abrir o app, ver candidatos e propostas, votar e ver o voto como pendente até sincronizar.

## 3. Aplicação publicada
_(cole aqui o link do GitHub Pages / Netlify / Vercel)_

## 4. Tecnologias
HTML5, CSS3, JavaScript puro, Service Worker, Cache API, IndexedDB, Background Sync, Notifications/Push, Web App Manifest. Sem frameworks e sem dependências.

## 5. Funcionalidades
| Requisito | Onde |
|---|---|
| Candidatos e propostas | `index.html` + `data/candidatos.json` |
| Votação simulada, sem duplicidade | `app.js` (`votar`, chave `meuVoto` no IndexedDB) |
| Apuração | `resultado.html` |
| Offline | `sw.js` (precache + fallback `offline.html`) |
| Sincronização | `sw.js` (`sync` / `sincronizarPendencias`) |
| Notificação | painel da home + `push`/`notificationclick` no `sw.js` |

## 6. Cache e funcionamento offline
Resumo (detalhes em [`docs/DOCUMENTACAO-TECNICA.md`](docs/DOCUMENTACAO-TECNICA.md)): precache do app shell na instalação; **Network First** para páginas e `candidatos.json`; **Cache First** para CSS/JS/ícones; **Stale While Revalidate** para fontes externas; `offline.html` como fallback.

## 7. Background Sync
Voto vai para a fila (`fila` no IndexedDB) → `sync.register('sincronizar-votos')` → SW envia ao "servidor" simulado (store `votos`) quando houver rede → avisa a página com `postMessage`. Sem `SyncManager` (Safari/Firefox) usa o evento `online`.

## 8. Push Notifications
**Simulação didática controlada**: a página pede permissão e envia uma mensagem ao SW, que chama `showNotification`. O handler `push` real também existe (teste pelo botão *Push* do DevTools). Clique na notificação abre a tela relevante. Veja o que faltaria em produção na documentação técnica.

## 9. Preload, Prefetch e DNS-prefetch
- `preload` de `style.css`: bloqueia a renderização, é necessário já no primeiro paint.
- `prefetch` de `resultado.html`: próxima navegação provável após votar.
- `dns-prefetch` de `fonts.googleapis.com` e `fonts.gstatic.com`: domínios externos realmente usados (fonte Bricolage Grotesque).

## 10. Testes e evidências
Roteiro: [`docs/ROTEIRO-TESTES.md`](docs/ROTEIRO-TESTES.md). Prints em [`/screenshots`](screenshots/) (nomes sugeridos no roteiro).

## 11. Problemas encontrados e soluções
_(preencham com a experiência real da equipe; ex.: cache antigo servindo CSS velho → versionamento `VERSAO` e limpeza no `activate`)_

## 12. Limitações e melhorias
- Não há servidor: o "envio" é simulado dentro do próprio navegador, então a apuração é local a cada aparelho.
- Duplicidade controlada por navegador/sessão (limpar dados do site libera novo voto).
- Push real exige servidor, chaves VAPID e `PushManager.subscribe`.
- Melhorias: backend real, autenticação por matrícula, Periodic Background Sync.

## 13. Vídeo de demonstração
_(cole o link, máx. 7 min, com teste offline)_

---
## Como executar localmente
Service Worker exige `localhost` ou HTTPS:
```bash
python3 -m http.server 8080     # ou: npx serve .
```
Abra `http://localhost:8080`.
