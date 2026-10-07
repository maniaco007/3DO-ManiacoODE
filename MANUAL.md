# Manual — 3DO ManiacoODE

**[English](MANUAL.en.md)** · **[Voltar ao início](README.md)**

## Conteúdo

1. [O que você precisa](#1-o-que-você-precisa)
2. [Primeira instalação da firmware](#2-primeira-instalação-da-firmware)
3. [Preparar o pendrive](#3-preparar-o-pendrive)
4. [Programa do PC — Gerenciador do pendrive](#4-programa-do-pc--gerenciador-do-pendrive)
5. [Usando o menu](#5-usando-o-menu)
6. [Opções](#6-opções)
7. [Gerenciador de saves](#7-gerenciador-de-saves)
8. [Memory Unit FZ-EM256](#8-memory-unit-fz-em256)
9. [Atualizar a firmware](#9-atualizar-a-firmware)
10. [Problemas comuns](#10-problemas-comuns)

## 1. O que você precisa

- Um 3DO com a **ODE SataTo3DO** (placa com RP2040) instalada. Testado num **Panasonic FZ-10**.
- Um pendrive em **FAT32** ou **exFAT**.
- Os arquivos da página de **[Releases](../../releases)**:
  - **`SataTo3DO.uf2`** — a firmware da ODE;
  - **`boot.iso`** — o menu;
  - **`3DO-ManiacoODE-Gerenciador.exe`** — o programa do PC que prepara o pendrive (opcional, para **Windows 10 ou 11**).

## 2. Primeira instalação da firmware

Só na primeira vez é preciso gravar a firmware pelo PC:

1. Com o console desligado, ligue o RP2040 da ODE ao PC pelo USB em **modo BOOTSEL** (segure o botão BOOTSEL da placa enquanto conecta).
2. O PC mostra uma unidade chamada **RPI-RP2**.
3. Copie o **`SataTo3DO.uf2`** para essa unidade. A placa grava e reinicia sozinha.

Depois disso, as atualizações são feitas pelo pendrive ([seção 9](#9-atualizar-a-firmware)).

## 3. Preparar o pendrive

O jeito mais fácil é o **programa do PC** ([seção 4](#4-programa-do-pc--gerenciador-do-pendrive)): ele instala o menu, organiza os jogos, gera as capas e cria a pasta de saves em poucos cliques.

Se preferir fazer à mão, organize o pendrive assim:

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
- As **capas** e os dados dos jogos (gênero, estúdio e ano) são gerados pelo programa do PC.

## 4. Programa do PC — Gerenciador do pendrive

O **3DO ManiacoODE — Gerenciador do pendrive** deixa o pendrive pronto para o 3DO: instala o menu, copia a firmware, organiza os jogos do jeito que a ODE lê, gera as capas e cria a pasta de saves.

**Instalação:** não há instalação. Baixe o `3DO-ManiacoODE-Gerenciador.exe` na página de [Releases](../../releases) e abra.

> Na primeira vez, o Windows pode mostrar "O Windows protegeu o computador" (SmartScreen), porque o programa é novo e não tem assinatura paga. Clique em **Mais informações → Executar assim mesmo**.

**Internet:** as capas e os dados dos jogos são baixados na hora de usar e ficam guardados no PC (`%LOCALAPPDATA%\3DO-ManiacoODE`). Da segunda vez em diante, o que já foi baixado funciona sem internet.

**Seu pendrive está seguro:** o programa **não apaga nada**. O `boot.iso` antigo é guardado, jogos repetidos vão para a pasta `_duplicados`, imagens trocadas vão para `covers/originais/_antigas`, e a organização dos jogos pode ser desfeita.

### 4.1 A janela

![Preparar pendrive](imagens/app-preparar.png)

- **Pendrive** (topo): escolha a letra do pendrive. O programa sugere primeiro os que já têm o `boot.iso`. Ao lado aparecem o sistema de arquivos, o espaço livre e se o menu está instalado. Use **Procurar…** se o pendrive não aparecer.
- **Abas:** Preparar pendrive, Organizar jogos e Capas.
- **Rodapé:** mostra a etapa atual, a porcentagem, o tempo que falta e o botão **Cancelar**.
- **Menu Arquivo:** abre o pendrive no Explorer.
- **Menu Tema:** muda as cores da janela (Noite, Menu ManiacoODE (azul e ouro) e Claro). A escolha fica salva.
- **Menu Idioma / Language:** mostra a janela em português ou em inglês. Na primeira vez, o programa usa o idioma do Windows.
- **Menu Ajuda:** blog Maniaco Game Room e Sobre.

### 4.2 Preparar pendrive

Marque o que quer fazer e clique em **Preparar pendrive**. Cada passo pode ser feito sozinho.

1. **Instalar o menu (`boot.iso`):** aponte o `boot.iso` baixado. O que já estava no pendrive vira `boot_anterior.iso`.
2. **Atualizar a firmware (`SataTo3DO.uf2`):** o programa confere se o arquivo é mesmo uma firmware do RP2040 e copia para a raiz do pendrive. Depois, no 3DO, abra **Opções → Atualizar firmware** ([seção 9](#9-atualizar-a-firmware)).
3. **Copiar os jogos da pasta do PC:** aponte a pasta com os seus jogos (`.iso`, `.cue` + `.bin`). Eles vão para o pendrive já organizados, conforme **Como organizar os jogos** ([seção 4.3](#43-organizar-jogos)). O programa confere se cabe antes de copiar e pula o que já está lá.
4. **Gerar as capas e os dados dos jogos:** usa as opções da aba Capas ([seção 4.4](#44-capas)).
5. **Criar a pasta `saves`:** necessária para o backup de saves no pendrive.

O programa só continua se o pendrive estiver em **FAT32** ou **exFAT**. Ao final, a caixa de texto mostra o **relatório** de tudo o que foi feito, inclusive os jogos que ficaram de fora e o motivo.

### 4.3 Organizar jogos

![Organizar jogos](imagens/app-organizar.png)

Arruma os jogos que **já estão no pendrive**, com as mesmas regras que a firmware usa para montar a lista.

1. Escolha as opções:
   - **Nomes limpos:** tira região e marcas do nome (`Need For Speed, The (USA) [!]` → `The Need For Speed`);
   - **Pastas:** uma pasta para cada jogo, por letra (A, B, C…) ou manter as pastas atuais;
   - **Mover versões repetidas para `_duplicados`:** deixa só uma cópia de cada jogo. Entre um `.iso` e um `.cue` + `.bin`, fica o `.cue` + `.bin`, que tem o áudio do CD.
2. Clique em **1) Analisar o pendrive**. A lista mostra o que vai acontecer com cada jogo:
   - **Já está certo** — nada muda;
   - **Mover / renomear** — vai para outra pasta e/ou ganha o nome limpo;
   - **Mover p/ `_duplicados`** — cópia repetida;
   - **Corrigir o `.cue`** — o `.cue` aponta para um arquivo que não existe ou tem nome de trilha longo demais para a firmware;
   - **Criar o `.cue`** — `.bin` sem `.cue`;
   - **Não entra na lista** — não é um disco de 3DO, ou o `.cue` não tem como ser corrigido.
3. Confira e clique em **2) Aplicar**.
4. Mudou de ideia? **Desfazer a última organização** devolve tudo como estava.

Arquivos que a ODE não lê (`.zip`, `.7z`, `.chd`…) aparecem num aviso embaixo da lista: extraia ou converta para `.iso` ou `.cue` + `.bin`. As imagens e os modos de capa que você escolheu acompanham o jogo quando ele é renomeado. Depois de aplicar, o programa oferece gerar as capas de novo, porque os nomes mudaram.

### 4.4 Capas

![Capas](imagens/app-capas.png)

A capa aparece ao lado da lista no menu, numa moldura de 106×116 pixels, com o gênero, o estúdio e o ano do jogo.

1. Clique em **1) Ver capas do pendrive**. A lista mostra cada jogo, de onde vem a capa e se ela já está no pendrive. Jogos **sem capa** aparecem primeiro, em vermelho; capas da **Wikipedia** e **telas de título** aparecem em amarelo, para você conferir.
2. Clique num jogo para ver a **prévia na TV**, já dentro da moldura do menu.
3. Clique em **2) Gerar capas no pendrive** para gravar tudo na pasta `covers`.

**Como encaixar a capa (padrão para todos os jogos):**

- **Preencher a moldura** (recomendado): estica a capa inteira até encher a moldura, sem cortar nada.
- **Manter a proporção:** a capa fica sem esticar, com faixas em volta.

**Esta capa:** no painel da prévia, escolha o modo só do jogo selecionado — **Padrão** (segue a opção acima), **Preencher** ou **Proporção**. Assim umas capas preenchem a moldura e outras mantêm a proporção. A lista mostra "· preenche a moldura" ou "· mantém a proporção" nos jogos com modo próprio.

**Onde procurar as capas:** o programa usa a primeira que encontrar, nesta ordem:

1. **Suas imagens** — escolhidas na aba Capas ou na **sua pasta de imagens** (opcional; o nome do arquivo é o nome do jogo, em PNG, JPG, WEBP, BMP ou GIF);
2. **libretro-thumbnails** — acervo de capas, com as versões americana, europeia e japonesa;
3. **ScreenScraper** e **TheGamesDB** — só com conta ([seção 4.5](#45-contas-do-screenscraper-e-do-thegamesdb));
4. **Wikipedia** — a imagem do artigo do jogo. **Confira:** às vezes é a caixa de outra plataforma;
5. **Tela de título** do jogo — último recurso, sempre com a proporção mantida.

As caixas **Usar a Wikipedia…** e **Usar a tela de título…** ligam ou desligam essas fontes na escolha automática. **Baixar da internet o que faltar** desligada usa só o que já foi baixado antes.

**Trocar a capa de um jogo** (botões do painel da prévia):

![Escolher entre as capas](imagens/app-escolher.png)

- **Escolher entre as capas…** — abre todas as capas encontradas para o jogo (todas as regiões, Wikipedia, tela de título…), já como ficam na TV. Clique na que quiser.
- **Escolher imagem…** — uma imagem do seu PC.
- **Colar imagem (Ctrl+V)** — uma imagem copiada, por exemplo do navegador ("Copiar imagem").
- **Procurar na internet** — abre a busca de imagens no navegador.
- **Voltar à capa automática** — desfaz a troca. A imagem que você tinha escolhido vai para `covers/originais/_antigas`.

A capa escolhida fica guardada no pendrive e é usada nas próximas vezes. Depois de trocar, clique em **2) Gerar capas no pendrive**.

### 4.5 Contas do ScreenScraper e do TheGamesDB

Opcional. Com estas contas, o programa também procura capas no **ScreenScraper** e no **TheGamesDB** quando o libretro não tiver. Abra pelo botão **Contas (ScreenScraper, TheGamesDB)…** da aba Capas.

- **ScreenScraper:** precisa de uma **credencial de desenvolvedor** (pedida à equipe do site) e, de preferência, da **sua conta** no site (usuário e senha), que dá mais acessos por dia.
- **TheGamesDB:** precisa de uma **chave de API**, pedida no fórum do site.
- **Testar** confere as contas sem fechar a janela; **Salvar** guarda.
- As senhas e a chave ficam **só neste computador**, protegidas pelo Windows.

Se a conta for recusada ou a cota do dia acabar, o programa para de usar aquela fonte até ser aberto de novo e segue com as outras.

### 4.6 O que o programa grava no pendrive

| No pendrive | O que é |
|---|---|
| `boot.iso` | o menu |
| `boot_anterior.iso` | o menu que estava antes |
| `SataTo3DO.uf2` | a firmware nova, até ser aplicada (vira `.ok` ou `.erro`) |
| `saves/` | backup de saves feito pelo menu |
| `covers/` | capas (`.cap`), dados dos jogos (`dados.cap`) e a lista das capas (`indice.cap`) |
| `covers/originais/` | as imagens que você escolheu e o modo de cada capa (`<jogo>.modo`) |
| `covers/originais/_antigas/` | imagens que você trocou |
| `_duplicados/` | versões repetidas tiradas da lista |
| `.maniacoode/` | o registro para **Desfazer a última organização** |

## 5. Usando o menu

Ao ligar o console, toca a **abertura em vídeo** (A, B, C, X ou Start pula) e o menu abre com **todos os jogos em ordem alfabética**, com a capa e os dados do jogo escolhido.

| Botão | Na lista de jogos |
|---|---|
| **▲ ▼** | sobe e desce na lista |
| **L / R** (ou **◄ ►**) | pula uma tela inteira de jogos |
| **A** (ou **Start**) | inicia o jogo |
| **X** | abre as **Opções** |

Quando você escolhe um jogo, o 3DO reinicia e carrega o disco. Para voltar ao menu, desligue e ligue o console.

## 6. Opções

![Opções](imagens/opcoes.png)

| Item | O que faz |
|---|---|
| **Tamanho da tela** | Diminui o menu de 100 % até 80 %, em passos de 5 %, para TVs que cortam as bordas. Ajuste com **◄ ►**. |
| **Idioma / Language** | Textos do menu em português ou em inglês. Troque com **◄ ►**. |
| **Gerenciar saves** | Abre o gerenciador de saves (**A**). |
| **Atualizar firmware** | Grava a `SataTo3DO.uf2` do pendrive ([seção 9](#9-atualizar-a-firmware)). |
| **Informações** | Versão do menu, versão da firmware da ODE, quantidade de jogos, de capas e de jogos com gênero/estúdio/ano. |

**▲ ▼** escolhem o item; **B** volta para a lista. O tamanho da tela e o idioma ficam salvos na memória do 3DO (NVRAM) ao voltar.

## 7. Gerenciador de saves

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

## 8. Memory Unit FZ-EM256

- **Conecte e desconecte a Memory Unit somente com o console desligado.** Com o console ligado, o 3DO trava.
- Com a unidade conectada, o console liga normalmente no menu e os jogos funcionam.
- A aba **Memory Unit** procura a unidade quando é aberta. Se ela não estiver conectada, aparece "Memory Unit não encontrado".
- O menu **não formata** a Memory Unit: só monta a unidade para ler e gravar saves.

## 9. Atualizar a firmware

Depois da primeira instalação, não é mais preciso abrir o console.

### Pelo menu (firmware v1.1 em diante)

1. Copie o **`SataTo3DO.uf2`** novo para a **raiz** do pendrive (ou use o passo 2 do programa do PC).
2. Ligue o console e abra **Opções → Atualizar firmware**. A ODE confere o arquivo ("Conferindo o arquivo...").
3. Aparece **"Atualizar para vX.Y.Z?"**. Aperte **A** para gravar ou **B** para cancelar.
4. Quando aparecer **"Firmware gravada. Desligue o console e ligue de novo."**, desligue e ligue o console. A nova versão aparece em **Opções → Informações**.

O arquivo é renomeado para **`SataTo3DO.uf2.ok`** depois de gravado. Outras respostas:

- **"Nenhuma atualização no pendrive"** — o `SataTo3DO.uf2` não está na raiz;
- **"A firmware já está nesta versão."** — nada é gravado (o arquivo vira `.ok`);
- **"Arquivo inválido (renomeado para .erro)."** — o arquivo está corrompido ou não é a firmware da ODE; a firmware atual continua funcionando.

> **Não desligue o console durante a gravação.**

### Vindo da v1.0

A firmware v1.0 não atualiza pelo menu (as Opções mostram "Firmware da ODE: original"). Para passar para a v1.1, copie o `SataTo3DO.uf2` para a raiz do pendrive e **ligue o console**: a v1.0 grava o arquivo sozinha ao ligar (o LED do Pico fica aceso alguns segundos) e a ODE reinicia. Daí em diante, use o menu.

### Socorro: se o menu não abrir

Copie a firmware para a raiz do pendrive com o nome **`SataTo3DO_forcar.uf2`** e ligue o console: esse arquivo é gravado ao ligar, sem passar pelo menu. O LED do Pico fica aceso enquanto grava; **5 piscadas devagar** = arquivo recusado. O arquivo vira `.ok` ou `.erro`, como no menu.

> Se faltar energia bem na gravação final (cerca de 2 segundos), regrave pelo PC em modo BOOTSEL ([seção 2](#2-primeira-instalação-da-firmware)).

## 10. Problemas comuns

| Problema | Solução |
|---|---|
| **As bordas do menu ficam cortadas na TV** | Opções → **Tamanho da tela** → diminua para 95 % ou 90 %. |
| **Um jogo não aparece na lista** | Use **Organizar jogos** no programa do PC: ele mostra o motivo e corrige `.cue` quebrado ou sem par. À mão: confira a extensão (`.iso`, `.cue`) e se o nome em `FILE "…"` do `.cue` é igual ao arquivo da pasta. |
| **Um jogo está sem capa** | No programa do PC, aba Capas: **Escolher entre as capas…**, **Escolher imagem…** ou **Colar imagem**, depois **Gerar capas no pendrive**. |
| **A capa veio de outra plataforma** | Acontece com a Wikipedia. Use **Escolher entre as capas…** ou uma imagem sua. |
| **A capa ficou esticada demais** | Em **Esta capa**, escolha **Proporção** para esse jogo e gere as capas de novo. |
| **"O Windows protegeu o computador"** | **Mais informações → Executar assim mesmo** (o programa não tem assinatura paga). |
| **A aba Pendrive avisa para criar a pasta** | Crie a pasta **`saves`** na raiz do pendrive (ou use o passo 5 do programa do PC). |
| **"Memory Unit não encontrado"** | Desligue o console, conecte a Memory Unit e ligue de novo. |
| **O arquivo virou `SataTo3DO.uf2.erro`** | O arquivo está corrompido ou não é a firmware da ODE. Baixe de novo na página de Releases. |
| **Quero voltar ao menu anterior** | Apague o `boot.iso` e renomeie o `boot_anterior.iso` (ou o antigo) de volta para `boot.iso`. |
