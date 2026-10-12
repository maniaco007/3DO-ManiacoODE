# 3DO ManiacoODE

**[English](README.en.md)**

**Firmware nova e menu novo para a ODE SataTo3DO do 3DO: menu em português (ou inglês) com abertura em vídeo, capa e dados de cada jogo, todos os jogos do pendrive em ordem alfabética, gerenciador de saves (console, Memory Unit e pendrive), compatibilidade com a Memory Unit FZ-EM256 e atualização da firmware pelo próprio menu — e um programa do PC que deixa o pendrive pronto em poucos cliques.**

Um projeto do blog **[Maniaco Game Room](https://www.maniacogameroom.com.br/)**.

![Abertura do menu](imagens/abertura.png)

## O que o 3DO ManiacoODE traz

| | |
|---|---|
| ![Opções](imagens/opcoes.png) | ![Gerenciar saves](imagens/opcoes-saves.png) |

### Menu (`boot.iso`)

- **Menu inteiro em português**, com acentos (á, ã, ç, é…), ou **em inglês** (Opções → Idioma / Language).
- **Abertura em vídeo** ao ligar o console, escolhida nas Opções: quatro aberturas, aleatória ou desligada. A, B, C, X ou Start pula.
- **Prévia em vídeo:** parado num jogo, um clipe do jogo toca no lugar da capa (vídeos gravados pelo programa do PC).
- **Todos os jogos do pendrive numa lista só, em ordem alfabética**, sem precisar entrar em pastas. **L/R** pula uma tela inteira de jogos.
- **Capa de cada jogo** ao lado da lista, com **gênero, estúdio e ano**.
- Jogos com **vários discos** aparecem identificados: "Título (Disco 1)", "Título (Disco 2)".
- **Tamanho da tela** de 80 % a 100 %, para TVs que cortam as bordas. Fica salvo na memória do 3DO.
- **Gerenciador de saves** com três abas — **Console**, **Memory Unit** e **Pendrive**: copiar, restaurar, apagar e copiar todos de uma vez. Faça backup dos saves do console no pendrive.
- **Atualizar firmware** pelas Opções, com a versão da firmware da ODE nas Informações.

### Firmware (`SataTo3DO.uf2`)

- **Lista em ordem alfabética** de todos os jogos, em qualquer pasta do pendrive.
- **Atualização pelo pendrive:** depois da primeira gravação, nunca mais precisa abrir o console nem ligar a ODE no PC. Copie o `SataTo3DO.uf2` para o pendrive e use **Opções → Atualizar firmware**.
- **Memory Unit FZ-EM256:** o console liga com a unidade conectada e o menu e os jogos funcionam normalmente.
- **Saves no pendrive:** leitura e gravação de arquivos pelo menu.

## O programa do PC: Gerenciador do pendrive

![Programa do PC](imagens/app-preparar.png)

Com o pendrive no PC, poucos cliques:

1. instala o **menu** (`boot.iso`) e copia a **firmware** (`SataTo3DO.uf2`);
2. mostra os **jogos da sua pasta** e quais **já estão no pendrive**, e copia só os que você marcar, já organizados do jeito que a ODE lê, com **nomes limpos** e uma pasta para cada jogo;
3. **converte jogos `.chd`** para `.cue` + `.bin` na hora de copiar;
4. **corrige `.cue`** quebrado, cria o `.cue` que falta e tira `.cue` antigo que sobrou;
5. gera as **capas** e os dados dos jogos (gênero, estúdio e ano), que você também pode **preencher à mão**;
6. grava os **vídeos de prévia** dos jogos (do ScreenScraper, sem conta, ou um vídeo seu);
7. cria a pasta **`saves`**.

As capas vêm do **libretro-thumbnails**, da **Wikipedia** e, com conta, do **ScreenScraper** e do **TheGamesDB**. Você vê a **prévia de como fica na TV**, escolhe entre todas as capas encontradas ou usa uma imagem sua, e decide **capa por capa** se ela preenche a moldura ou mantém a proporção.

O programa funciona em **português ou inglês** (menu Idioma / Language) e também organiza um pendrive que já está pronto — com a opção de **não alterar** um jogo que já tem o nome certo — e **renomeia jogos** levando junto a capa e as informações. **Nada é apagado:** o que é substituído é guardado, e a organização pode ser desfeita.

| | |
|---|---|
| ![Organizar jogos](imagens/app-organizar.png) | ![Capas](imagens/app-capas.png) |

## Como usar

1. Baixe o **`SataTo3DO.uf2`**, o **`boot.iso`** e o **`3DO-ManiacoODE-Gerenciador.exe`** na página de **[Releases](../../releases)**.
2. **Primeira vez:** grave o `SataTo3DO.uf2` no RP2040 da ODE pelo PC (modo BOOTSEL). Das próximas vezes, a atualização é pelo menu.
3. Abra o programa, escolha o pendrive e clique em **Preparar pendrive**. (Prefere fazer à mão? Veja o [Manual](MANUAL.md#3-preparar-o-pendrive).)
4. Coloque o pendrive na ODE, ligue o console e escolha o jogo.

Passo a passo completo, com todas as opções: **[Manual](MANUAL.md)**.

## Requisitos

- 3DO com a **ODE SataTo3DO** (placa com RP2040). Testado num **Panasonic FZ-10**.
- Pendrive em **FAT32** ou **exFAT**.
- Jogos em **`.iso`** ou **`.cue` + `.bin`**.
- Programa do PC: **Windows 10 ou 11**, com internet na primeira vez (capas e dados dos jogos são baixados e ficam guardados no PC).

## Aviso

**Nunca conecte nem desconecte a Memory Unit com o console ligado** — o barramento do 3DO não aceita isso e o console trava. Conecte com ele desligado. Jogos e BIOS não são fornecidos.

## Licença

**Freeware — uso gratuito. © 2026 Maniaco Game Room. Todos os direitos reservados.** Veja a [licença](LICENCA.md) e os [componentes de terceiros](TERCEIROS.md).

Encontrou um problema? Abra uma [Issue](../../issues).
