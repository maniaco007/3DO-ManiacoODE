# Manual — 3DO ManiacoODE

**[English](MANUAL.en.md)** · **[Voltar ao início](README.md)**

## Conteúdo

1. [O que você precisa](#1-o-que-você-precisa)
2. [Primeira instalação da firmware](#2-primeira-instalação-da-firmware)
3. [Preparar o pendrive](#3-preparar-o-pendrive)
4. [Usando o menu](#4-usando-o-menu)
5. [Opções](#5-opções)
6. [Gerenciador de saves](#6-gerenciador-de-saves)
7. [Memory Unit FZ-EM256](#7-memory-unit-fz-em256)
8. [Atualizar a firmware pelo pendrive](#8-atualizar-a-firmware-pelo-pendrive)
9. [Problemas comuns](#9-problemas-comuns)

## 1. O que você precisa

- Um 3DO com a **ODE SataTo3DO** (placa com RP2040) instalada. Testado num **Panasonic FZ-10**.
- Um pendrive em **FAT32** ou **exFAT**.
- Os dois arquivos da página de **[Releases](../../releases)**:
  - **`SataTo3DO.uf2`** — a firmware da ODE;
  - **`boot.iso`** — o menu.

## 2. Primeira instalação da firmware

Só na primeira vez é preciso gravar a firmware pelo PC:

1. Com o console desligado, ligue o RP2040 da ODE ao PC pelo USB em **modo BOOTSEL** (segure o botão BOOTSEL da placa enquanto conecta).
2. O PC mostra uma unidade chamada **RPI-RP2**.
3. Copie o **`SataTo3DO.uf2`** para essa unidade. A placa grava e reinicia sozinha.

Depois disso, as atualizações são feitas pelo pendrive ([seção 8](#8-atualizar-a-firmware-pelo-pendrive)).

## 3. Preparar o pendrive

Organize o pendrive assim:

```
boot.iso                 ← o menu
saves/                   ← pasta para o backup de saves (crie vazia)
Alone in the Dark/
    Alone in the Dark.iso
Gex/
    Gex.iso
Policenauts/
    Policenauts (Disc 1).iso
    Policenauts (Disc 2).iso
```

- **`boot.iso`** fica na raiz. Se já tinha outro menu, renomeie o antigo (ex.: `boot_antigo.iso`) para poder voltar.
- **Cada jogo na sua pasta.** Se a pasta tem um único jogo, o menu mostra o **nome da pasta**; se tem vários arquivos, mostra o nome de cada arquivo.
- Formatos aceitos: **`.iso`** (2048 ou 2352 bytes por setor) e **`.cue` + `.bin`**.
- Jogos com **vários discos**: deixe "Disc 1", "Disc 2"… no nome do arquivo e o menu mostra "(Disco 1)", "(Disco 2)".
- As pastas **`saves`** e **`covers`** não aparecem na lista de jogos.

## 4. Usando o menu

Ao ligar o console, toca a **abertura em vídeo** (A, B, C, X ou Start pula) e o menu abre com **todos os jogos em ordem alfabética**.

| Botão | Na lista de jogos |
|---|---|
| **▲ ▼** | sobe e desce na lista |
| **L / R** (ou **◄ ►**) | pula uma tela inteira de jogos |
| **A** (ou **Start**) | inicia o jogo |
| **X** | abre as **Opções** |

Quando você escolhe um jogo, o 3DO reinicia e carrega o disco. Para voltar ao menu, desligue e ligue o console.

## 5. Opções

![Opções](imagens/opcoes.png)

| Item | O que faz |
|---|---|
| **Tamanho da tela** | Diminui o menu de 100 % até 80 %, em passos de 5 %, para TVs que cortam as bordas. Ajuste com **◄ ►**; fica salvo na memória do 3DO (NVRAM) ao sair com **B**. |
| **Gerenciar saves** | Abre o gerenciador de saves (**A**). |
| **Informações** | Versão do menu e quantidade de jogos no pendrive. |

**▲ ▼** escolhem o item; **B** volta para a lista.

## 6. Gerenciador de saves

![Gerenciar saves](imagens/opcoes-saves.png)

O gerenciador tem três abas, trocadas com **L / R**:

| Aba | O que mostra |
|---|---|
| **Console** | os saves da memória interna do 3DO (NVRAM) e o espaço livre |
| **Memory Unit** | os saves da Memory Unit FZ-EM256, se estiver conectada |
| **Pendrive** | o backup de saves na pasta `saves` do pendrive |

Escolha um save e aperte **A** para ver as ações:

- **Copiar para o pendrive / para o Memory Unit / para o console**;
- **Restaurar no console / no Memory Unit** (na aba Pendrive);
- **Apagar** (ou **Tirar do backup**, na aba Pendrive);
- **Copiar todos para o pendrive** / **Restaurar todos no console**.

Apagar, substituir e restaurar todos pedem confirmação. **B** volta.

> A aba **Pendrive** só funciona se existir a pasta **`saves`** na raiz do pendrive. Se ela não existir, o menu avisa.

## 7. Memory Unit FZ-EM256

- **Conecte e desconecte a Memory Unit somente com o console desligado.** Com o console ligado, o 3DO trava.
- Com a unidade conectada, o console liga normalmente no menu e os jogos funcionam.
- A aba **Memory Unit** procura a unidade quando é aberta. Se ela não estiver conectada, aparece "Memory Unit não encontrado".
- O menu **não formata** a Memory Unit: só monta a unidade para ler e gravar saves.

## 8. Atualizar a firmware pelo pendrive

Depois da primeira instalação, não é mais preciso abrir o console:

1. Copie o **`SataTo3DO.uf2`** novo para a **raiz** do pendrive.
2. Coloque o pendrive na ODE e ligue o console.
3. O LED do Pico (na placa da ODE) fica aceso alguns segundos enquanto a firmware é conferida e gravada, e a ODE reinicia sozinha.
4. O arquivo é renomeado:
   - **`SataTo3DO.uf2.ok`** — atualização feita (ou a versão já era a mesma);
   - **`SataTo3DO.uf2.erro`** — arquivo recusado (o LED pisca 5 vezes devagar); a firmware atual continua funcionando.

> **Não desligue o console durante a atualização.** Se faltar energia bem na gravação final (cerca de 2 segundos), regrave pelo PC em modo BOOTSEL ([seção 2](#2-primeira-instalação-da-firmware)).

## 9. Problemas comuns

| Problema | Solução |
|---|---|
| **As bordas do menu ficam cortadas na TV** | Opções → **Tamanho da tela** → diminua para 95 % ou 90 %. |
| **Um jogo não aparece na lista** | Confira a extensão (`.iso`, `.cue`). Se o jogo tem `.cue`, abra o `.cue` num editor de texto e confira se o nome do arquivo em `FILE "…"` é igual ao arquivo da pasta. Se a pasta já tem o `.iso`, o `.cue` pode ser apagado. |
| **A aba Pendrive avisa para criar a pasta** | Crie a pasta **`saves`** na raiz do pendrive. |
| **"Memory Unit não encontrado"** | Desligue o console, conecte a Memory Unit e ligue de novo. |
| **O arquivo virou `SataTo3DO.uf2.erro`** | O arquivo está corrompido ou não é a firmware da ODE. Baixe de novo na página de Releases. |
| **Quero voltar ao menu anterior** | Apague o `boot.iso` e renomeie o antigo de volta para `boot.iso`. |
