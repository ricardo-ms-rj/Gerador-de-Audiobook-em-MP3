# audiobook_app
Programa para converter documentos em PDF em audiobooks.
# LPIC1_APP — Audiobook Generator (MP3 + manifest)

Projeto para gerar **áudios MP3 (audiobook)** a partir de materiais de estudo do **LPIC-1**, com foco em estudo no PC.

A decisão de escopo é manter o app focado em **GERAR MP3 + MANIFEST**, usando o **Player do Windows** para reprodução (seek, teclas multimídia etc.).

## Objetivo

- Converter conteúdos de estudo em **MP3**, organizados por tópico/subtópico.
- Gerar um **`manifest.json`** com a estrutura dos áudios, para consumo pelo app e também pelo `quiz_generator`.
- Suportar entrada em **PDF** e **DOCX** (com estratégia PDF-first e fallback em DOCX).

## Ambiente (validado)

- Windows 11
- Python (venv): 3.12.0
- Flet: 0.28.3
- flet-audio: 0.1.0
- PyMuPDF: 1.27.1
- edge-tts: 7.2.7
- python-docx: OK

> Observação: o projeto foi validado neste ambiente. Outros ambientes podem exigir ajustes.

## Organização de pastas (estrutura oficial)

Raiz:
- `C:\Estudos\LPIC1_APP\`

Pasta oficial do app:
- `C:\Estudos\LPIC1_APP\audiobook_app\`

Código (exemplos principais):
- `main.py` — ponto de entrada do app
- `interface.py` — UI (Flet)
- `motor_audio.py` — motor de leitura/limpeza/TTS e geração de saídas

Saídas oficiais:
- MP3: `C:\Estudos\LPIC1_APP\audiobook_app\audios\`
- Manifest: `C:\Estudos\LPIC1_APP\audiobook_app\manifest.json`

Itens antigos arquivados (não usar):
- `C:\Estudos\LPIC1_APP\audios` → `audios__VAZIA_NAO_USAR`
- `C:\Estudos\LPIC1_APP\manifest.json` → `manifest__OLD_2026-02-16.json`

## Conteúdos de estudo (entradas)

PDF:
- PDF completo original:
  - `C:\Estudos\LPIC1_APP\pdf\meu_livro.pdf`
- PDFs fragmentados (por subtópico):
  - `C:\Estudos\LPIC1_APP\pdf\meu_livro_101_1.pdf ... meu_livro_104_7.pdf`

DOCX (fallback / testes):
- Não existe `meu_livro.docx` (conversão feita diretamente a partir dos PDFs fragmentados).
- DOCX fragmentados:
  - `C:\Estudos\LPIC1_APP\docs\meu_livro_101_1.docx ... meu_livro_104_7.docx`
  - **Observação importante:** `104_4` não existe no material original (pulo do 104.3 para 104.5).

## Como usar (visão geral)

1. Garanta que o ambiente Python e as dependências do `requirements.txt` estejam instalados.
2. Inicie o app pelo `main.py` usando o ambiente virtual configurado.
3. Selecione o conteúdo de entrada (PDF/DOCX) conforme o fluxo do app.
4. Gere os MP3.
5. Os arquivos finais serão gravados em:
   - `audiobook_app\audios\`
   - `audiobook_app\manifest.json`

Reprodução:
- O estudo/reprodução deve ser feito pelo **Player do Windows** (o app não é o player).

## Saídas recentes (teste DOCX)

MP3 gerados (temporários, ainda não 100% validados para estudo):
- `101.mp3`
- `101_1.mp3`
- `102.mp3`
- `102_1.mp3`

Manifest oficial:
- `audiobook_app\manifest.json`

## Problemas conhecidos (áudio)

- Duplicação do “Tópico Mestre” no início do `101_1` (não bloqueante).
- Símbolos em comandos às vezes são “engolidos” na fala (`-`, `/`, `<`, `>` etc.).
- Tabelas: leitura inconsistente (pula ou repete). Preferência: **ignorar 100% tabelas**.
- Pronúncia de termos técnicos: desejável, mas manter ajustes mínimos para não atrasar os estudos.

## Problemas conhecidos (UI/Player)

- Player embutido apresentou UX inconsistente (play/seek).
- Decisão: manter reprodução no **Player do Windows** e app como **gerador**.

## Próximos passos (planejados)

Prioridade: estabilizar o motor com mudanças minimalistas e baixo risco:

A) Normalização de comandos/símbolos em contexto de comando:
- `-` → “traço”
- `/` → “barra”
- `<` / `>` → “menor/maior que”
- `|` → “pipe”
- `>>`, `2>` etc.

B) Ignorar 100% tabelas.

C) Filtragem mínima de cabeçalho/rodapé:
- padrões óbvios + repetição (assinatura por documento se necessário)

D) Manter PDF-first e DOCX fallback.

## Integração com Quiz (plano)

Existe um gerador de quiz em:
- `C:\Estudos\LPIC1_APP\quiz_generator\` (`gerar_quiz.py`)

Planejado:
- O `quiz_generator` deve ler o manifest oficial:
  - `audiobook_app\manifest.json`
- Fluxo desejado:
  - “Abrir Texto” (PDF/DOCX) e “Abrir Áudio” (MP3 no Player do Windows)
  - “Fazer Quiz do Tópico/Subtópico”

## Notas importantes para versionamento (GitHub)

Este repositório deve conter **apenas o código e documentação**.

Recomendado NÃO versionar:
- `venv/`, caches, arquivos temporários
- `audios/`, `*.mp3`
- `manifest.json` gerado (se for saída)
- PDFs/DOCX do material (por tamanho/direitos autorais)

Use um `.gitignore` adequado para manter o repositório limpo.

## Documentação de progresso

O histórico detalhado e decisões do projeto podem ficar em:
- `docs/LOG_PROGRESSO.txt`
