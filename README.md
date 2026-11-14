# Time de Bots â€” seus bots no computador e na VPS, pela sua assinatura

[![Time de Bots](guia/assets/banner.jpg)](https://inematds.github.io/time-de-bots/guia/)

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## O que Ã©

O Time de Bots Ã© um kit gratuito do INEMA que instala um painel de bots no seu computador Linux ou Mac, ou
numa VPS. Cada bot tem nome, avatar, papel, memÃ³ria e tarefas agendadas, e vocÃªs conversam sozinhos ou em
grupo (`@bot`). As respostas vÃªm da **sua assinatura** do Claude ou do Codex, pelo login normal do programa
oficial, sem chave de API e sem cobranÃ§a por uso. VocÃª pode ter vÃ¡rios times separados na mesma mÃ¡quina.
Precisa de Node.js 22+ e de uma conta Claude ou ChatGPT com Codex.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/time-de-bots/guia/**

## O que tem aqui

| Pasta | O que Ã© |
|---|---|
| `app/` | O app de bots (Electron, licenÃ§a MIT, ver `app/LICENSE`). SÃ³ trocamos o nome visÃ­vel: [`kit/MUDANCAS-NO-APP.md`](kit/MUDANCAS-NO-APP.md) |
| `kit/instalar.sh` | Confere Node/npm, instala dependÃªncias, baixa os arquivos do app e roda os 193 testes |
| `kit/iniciar.sh` | Abre o app; `--instancia <nome>` cria outro time separado; tira qualquer chave de API do ambiente |
| `kit/scripts/conferir-resposta.mjs` | Prova que a resposta Ã© real pela assinatura (nÃ£o modo demonstraÃ§Ã£o) e grava em `kit/verificacao/` |
| `kit/vps/` | Modo servidor: tela virtual + acesso pelo navegador **sÃ³ por tÃºnel SSH**, serviÃ§o que sobe sozinho, vÃ¡rias instÃ¢ncias |
| `kit/prompts/` | Pedidos prontos para o Claude Code ou o Codex instalar e conferir por vocÃª |

## TrÃªs jeitos de usar

| Onde | Para quem | Como abre |
|---|---|---|
| **No seu computador** (Linux ou Mac) | quem quer os bots Ã  mÃ£o | `kit/iniciar.sh` |
| **Outro time na mesma mÃ¡quina** | separar pessoal e trabalho, ou um time por cliente | `kit/iniciar.sh --instancia trabalho` |
| **Numa VPS** | bots e tarefas agendadas ligados 24 h, acessados de qualquer lugar | `kit/vps/nova-instancia.sh principal 1` + tÃºnel SSH |

## O processo, do inÃ­cio ao fim

Quem faz: ðŸ§  vocÃª numa sessÃ£o com o assistente Â· âš™ï¸ script do kit Â· ðŸ¤– agente/modelo.

| # | Passo | Comando / arquivo | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/inematds/time-de-bots.git && cd time-de-bots` | ðŸ§  |
| 2 | Instalar e testar (193 testes) | `kit/instalar.sh` | âš™ï¸ |
| 3 | Entrar na sua conta (uma vez, no terminal) | `claude` ou `codex login` | ðŸ§  |
| 4 | Conferir uma resposta real pela assinatura | `node kit/scripts/conferir-resposta.mjs` | âš™ï¸ |
| 5 | Abrir o app | `kit/iniciar.sh` | âš™ï¸ |
| 6 | Em Settings â†’ AI subscription, escolher Claude ou Codex e criar o primeiro bot | no app | ðŸ§  |
| 7 | (Opcional) outro time separado | `kit/iniciar.sh --instancia trabalho` | âš™ï¸ |
| 8 | (VPS) preparar o servidor, com usuÃ¡rio comum que tenha sudo | `kit/vps/instalar-vps.sh` | âš™ï¸ |
| 9 | (VPS) instalar o Claude Code ou o Codex no servidor e entrar na conta pelo SSH | `claude` ou `codex login --device-auth` | ðŸ§  |
| 10 | (VPS) ligar uma instÃ¢ncia | `kit/vps/nova-instancia.sh principal 1` | âš™ï¸ |
| 11 | (VPS) abrir pelo navegador via tÃºnel | `ssh -N -L 6081:127.0.0.1:6081 usuario@servidor` â†’ `http://localhost:6081/vnc.html` | ðŸ§  |
| 12 | Preferir que o assistente faÃ§a 2â€“11 por vocÃª | [`kit/prompts/`](kit/prompts/) | ðŸ¤– |

## O que foi testado (09/11/2026)

| Teste | Resultado |
|---|---|
| Testes do app (`npm test`) no Linux ARM64 | 193 de 193 |
| Testes do app numa VPS Ubuntu 24.04 x86_64 (4 vCPU, 15 GB) | 193 de 193 |
| Resposta real pela assinatura Claude (modelo haiku) | recebida em 3,8 s, login `claude.ai` |
| Duas instÃ¢ncias ao mesmo tempo no computador | abertas, dados separados |
| Duas instÃ¢ncias como serviÃ§o na VPS | ativas, portas sÃ³ em 127.0.0.1 |
| Resposta real na VPS, Codex, voz, navegador na nuvem, Composio | **nÃ£o testados** |

## Cuidados

- **SÃ³ assinatura.** O app sÃ³ aceita o login `claude.ai` do Claude Code ou o login do Codex. O `kit/iniciar.sh`
  remove `ANTHROPIC_API_KEY`/`OPENAI_API_KEY` do ambiente para nada ir para cobranÃ§a por API sem vocÃª ver.
- **Limites continuam.** A assinatura tem limites de uso e termos do provedor; o painel nÃ£o aumenta isso.
- **Login Ã© seu.** Nunca cole token ou senha no chat do assistente; o login Ã© feito por vocÃª no terminal.
- **VPS fechada.** Nada fica aberto para a internet: VNC e navegador ouvem sÃ³ em 127.0.0.1, vocÃª entra por tÃºnel SSH.
- **Software experimental**, sem garantia. Teste primeiro com uma instÃ¢ncia descartÃ¡vel e faÃ§a backup de
  `~/.local/share/time-de-bots/` antes de atualizar.
- No Linux, o sandbox do Electron precisa de um ajuste com sudo: [`kit/COMO-LIGAR-SANDBOX.md`](kit/COMO-LIGAR-SANDBOX.md).

## DÃºvidas comuns

- **Composio e navegador na nuvem:** Composio liga os bots a outros apps seus (e-mail, agenda, planilhas), com conta e chave da prÃ³pria Composio; o navegador na nuvem Ã© um computador separado, hospedado por vocÃª, onde o bot navega enquanto vocÃª assiste. Os dois vÃªm desligados.
- **Outra porta SSH ou chave prÃ³pria:** `ssh -i ~/.ssh/minha-chave -p 2222 -N -L 6081:127.0.0.1:6081 usuario@servidor`. Porta 6081 ocupada no seu computador: `-L 7081:127.0.0.1:6081` e abra `http://localhost:7081/vnc.html`.
- **"Login do Claude: NÃƒO" no `conferir-resposta.mjs`:** rode `claude` no mesmo usuÃ¡rio que abre o app e entre com a conta Claude (nÃ£o chave de API); confira com `claude auth status`. Na VPS, depois do login: `systemctl --user restart time-de-bots@principal`.

## LicenÃ§a

Kit, guia e prompts do INEMA: [MIT](LICENSE). O app em `app/` tem a prÃ³pria licenÃ§a MIT e avisos de terceiros
(`app/LICENSE`, `app/THIRD_PARTY_NOTICES.md`), mantidos como a licenÃ§a pede.

---

Feito pelo [INEMA.CLUB](https://inema.club).

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe