# AGENTS.md — Regras do Projeto (Audiobook-App)

Este arquivo define **regras e preferências obrigatórias** para qualquer agente/IA que trabalhe neste repositório.
O objetivo é manter o projeto estável, previsível e focado em gerar áudio para estudo.

---

## 1) Objetivo do projeto (escopo)

- O app **gera**:
  - arquivos **MP3** em `audiobook_app/audios/`
  - arquivo **manifest** em `audiobook_app/manifest.json`
- O app **não é um player**. A reprodução é feita no **Player do Windows**.
- O app deve suportar **PDF e DOCX** (PDF-first, DOCX fallback).

---

## 2) Estruturas e caminhos (não quebrar)

**Não alterar** sem justificativa muito forte e clara:
- pasta oficial de saída: `audiobook_app/audios/`
- manifest oficial: `audiobook_app/manifest.json`

Não criar novas “pastas de saída” paralelas que causem confusão (ex.: `./audios` na raiz).

---

## 3) Regras de processamento de conteúdo (TTS)

### 3.1 Rodapés/cabeçalhos e boilerplate
- O sistema deve **evitar narrar** rodapés/cabeçalhos e textos repetidos (boilerplate).
- Em DOCX, considerar que “rodapés” podem vir:
  - no footer/header real (fácil de ignorar), **ou**
  - **colados no corpo** (body) como parágrafos/tabelas repetidas (precisa filtro).
- Implementar filtragem **mínima e segura** (padrões óbvios + repetição) para não remover conteúdo legítimo.

### 3.2 Tabelas
- Preferência do projeto: **ignorar 100% tabelas** durante extração/narração.
- Se um arquivo possuir tabelas com conteúdo potencialmente relevante, ainda assim a prioridade é **consistência e não poluir o áudio**.

### 3.3 Comandos e símbolos técnicos
- Símbolos em comandos não podem “sumir” na fala.
- Aplicar normalização apenas quando for claro que o texto é um comando (ex.: linhas monoespaçadas / blocos de código / padrões de CLI).
  - Ex.: `-` → “traço”, `/` → “barra”, `|` → “pipe”, `>` → “maior que”, `<` → “menor que”
  - Operadores como `>>`, `2>`, `&&` devem ser preservados/explicados de forma consistente.
- Evitar “over-normalization” em texto comum.

---

## 4) Ordem e segmentação dos tópicos

- Os arquivos são segmentados por tópico/subtópico:
  - `meu_livro_101_1`, `meu_livro_101_2`, ... até `meu_livro_104_7`
- **Importante:** `104_4` **não existe** no material original (pulo de 104.3 para 104.5). O sistema deve lidar com arquivos faltantes com naturalidade.
- O “Tópico Mestre” está contido em cada `_101_1`, `_102_1`, `_103_1`, `_104_1` e deve poder gerar um **áudio curto separado**, sem duplicações.

---

## 5) Preview (modo de teste rápido)

- Deve existir (ou ser possível habilitar) um modo de **Preview** para economizar tempo:
  - gerar **apenas** `101_1.mp3` (e depois `102_1.mp3` quando solicitado)
  - não processar o lote completo em preview.
- O preview deve usar o **mesmo pipeline real** (limpeza, filtros, TTS), apenas limitando o escopo.

---

## 6) Versionamento / GitHub (não subir conteúdo sensível)

Este repositório deve conter **somente código e documentação**.

Não versionar:
- `venv/`, caches, temporários
- saídas: `audiobook_app/audios/`, `*.mp3`, `audiobook_app/manifest.json` (se for saída gerada)
- inputs do livro: `*.pdf`, `*.docx` (tamanho/direitos autorais)

Use `.gitignore` para garantir isso.

---

## 7) Critérios de qualidade (Definition of Done)

Uma mudança está “pronta” quando:
- Não altera a estrutura oficial de saída.
- Não reintroduz narração de rodapés/cabeçalhos/boilerplate.
- Não quebra geração do `manifest.json`.
- Mantém logs claros para depuração (especialmente em preview).
- Mudanças são pequenas e controladas (evitar refactors grandes sem necessidade).

---

## 8) Comunicação (como propor mudanças)

Ao propor alterações:
- Explique **por que** a mudança é necessária (qual bug/risco ela resolve).
- Liste impacto em:
  - PDF vs DOCX
  - Preview vs lote completo
  - Manifest e estrutura de saída
- Preferir patches pequenos e iterativos.
- `venv/`, caches
- `audiobook_app/audios/`, `*.mp3`
- `audiobook_app/manifest.json` (se for gerado)
- PDFs/DOCX do material (por tamanho/direitos autorais)

Use `.gitignore` para garantir isso.
