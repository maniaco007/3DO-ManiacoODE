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

> ⚠️ **Antes de gravar a nossa firmware, salve a firmware original da sua ODE.** Se a ManiacoODE não funcionar na sua placa, é o backup que deixa você **voltar à firmware original**. Sem ele, não há como voltar. O programa do PC faz o backup em um clique ([seção 4.8](#48-firmware-da-ode-backup-e-volta-à-original)); guarde uma cópia do arquivo também fora do PC (nuvem ou pendrive).

Só na primeira vez é preciso gravar a firmware pelo PC. O jeito mais fácil é a tela **Firmware** do programa do PC ([seção 4.8](#48-firmware-da-ode-backup-e-volta-à-original)), que faz os três passos abaixo com avisos. À mão:

1. **Modo BOOTSEL.** Com o console **desligado**, ligue o RP2040 da ODE ao PC pelo USB segurando o botão BOOTSEL da placa enquanto conecta. O PC mostra uma unidade chamada **RPI-RP2**.
2. **Backup da original (não pule).** Com a ODE ainda em BOOTSEL, salve a flash inteira com o `picotool` da Raspberry Pi: `picotool save -a firmware-original.uf2`. Confira o arquivo (o `.uf2` tem o dobro da flash: 4 MB numa ODE com 2 MB de flash) e guarde-o em dois lugares. Na tela Firmware do programa, o mesmo passo é o botão **Salvar o backup**.
3. **Gravar a ManiacoODE.** Copie o **`SataTo3DO.uf2`** para a unidade **RPI-RP2**. A placa grava e reinicia sozinha.

Depois disso, as atualizações são feitas pelo pendrive ([seção 9](#9-atualizar-a-firmware)).

**Se a nossa não funcionar na sua placa:** coloque a ODE em modo BOOTSEL de novo e copie o seu `firmware-original-….uf2` (o backup do passo 2) para a unidade **RPI-RP2**. A placa grava e reinicia com a firmware original. Na tela Firmware do programa: **Voltar à original**.

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
- **Cada jogo na sua pasta.** Se a pasta tem um único jogo, o menu mostra o **nome da pasta**; se tem vários arquivos, mostra o nome de cada arquivo. Por isso, numa pasta de letra (ou de região) com um jogo só solto dentro, como `E\Escape from Monster Manor (USA).iso`, o menu mostra "E": deixe cada jogo na sua própria pasta (o **Organizar jogos** do programa do PC faz isso).
- Formatos aceitos: **`.iso`** (2048 ou 2352 bytes por setor) e **`.cue` + `.bin`** (um `.bin` só por `.cue`). Jogos em **`.chd`** são convertidos pelo programa do PC ao copiar: viram **`.iso`** (ou `.cue` + `.bin`, se o CD tiver trilhas de áudio) ([seção 4.2](#42-preparar-pendrive)).
- Jogos com **vários discos**: deixe "Disc 1", "Disc 2"… no nome do arquivo e o menu mostra "(Disco 1)", "(Disco 2)".
- **Pastas por região** (`USA`, `JP`, `EU`…) funcionam: a ODE lista os jogos de todas as pastas (até 3 níveis), mas o menu **não navega por pastas** — mostra tudo numa lista só. Use o nome com a região (`Gex (USA)`, `Gex (Japan)`) para duas versões do mesmo jogo não aparecerem com o mesmo nome. O programa do PC faz isso em **Organizar jogos → Pastas → Por região** ([seção 4.3](#43-organizar-jogos)).
- As pastas **`saves`** e **`covers`** não aparecem na lista de jogos.
- As **capas** e os dados dos jogos (gênero, estúdio e ano) são gerados pelo programa do PC.
- **Para mudar o nome de um jogo**, use **Renomear jogo…** na aba Capas ou **Editar nome…** na aba Organizar do programa do PC ([seção 4.3](#43-organizar-jogos) e [seção 4.4](#44-capas)). Renomeando a pasta pelo Windows, o jogo continua funcionando (e os saves não se perdem), mas a capa, as informações e a imagem escolhida ficam com o nome antigo até você gerar as capas de novo.
- **Evite renomear ou mover muitas pastas de uma vez na raiz do pendrive.** Na FAT32, cada nome trocado deixa uma entrada apagada no diretório, e a ODE monta a lista de jogos percorrendo esse diretório. Com a firmware 1.1.2 isso só deixa a lista mais lenta (o menu mostra **Lendo a lista de jogos...**); com as firmwares anteriores, que montavam a lista antes de avisar o 3DO que havia disco, o 3DO podia desistir e mostrar a tela dele em vez do menu ([seção 10](#10-problemas-comuns)).

## 4. Programa do PC — Gerenciador do pendrive

O **3DO ManiacoODE — Gerenciador do pendrive** deixa o pendrive pronto para o 3DO: instala o menu, copia a firmware, organiza os jogos do jeito que a ODE lê, gera as capas, grava os vídeos de prévia e cria a pasta de saves.

**Instalação:** não há instalação. Baixe o `3DO-ManiacoODE-Gerenciador.exe` na página de [Releases](../../releases) e abra.

> Na primeira vez, o Windows pode mostrar "O Windows protegeu o computador" (SmartScreen), porque o programa é novo e não tem assinatura paga. Clique em **Mais informações → Executar assim mesmo**.

**Internet:** as capas, os dados e os vídeos dos jogos são baixados na hora de usar e ficam guardados no PC (`%LOCALAPPDATA%\3DO-ManiacoODE`). Da segunda vez em diante, o que já foi baixado funciona sem internet.

**Seu pendrive está seguro:** o programa **não apaga nada**. O `boot.iso` antigo é guardado, jogos repetidos vão para a pasta `_duplicados` (que **você** esvazia, se quiser: veja [4.3](#43-organizar-jogos)), imagens trocadas vão para `covers/originais/_antigas`, e a organização dos jogos pode ser desfeita.

### 4.1 A janela

![Preparar pendrive](imagens/app-preparar.png)

- **Barra lateral** (esquerda): as cinco telas — **Preparar pendrive**, **Organizar jogos**, **Capas**, **Vídeos** e **Firmware** (backup da firmware original, [seção 4.8](#48-firmware-da-ode-backup-e-volta-à-original)); a que está aberta fica em destaque. Embaixo aparece a situação do pendrive: sistema de arquivos, espaço livre e se o menu está instalado.
- **Pendrive** (topo): escolha a letra do pendrive. O programa sugere primeiro os que já têm o `boot.iso`. Use **Procurar…** se o pendrive não aparecer.
  - **Uma leitura só:** ao abrir, ao trocar de pendrive ou ao clicar em **Atualizar**, o programa lê os jogos do pendrive **uma vez**, em segundo plano (o rótulo ao lado mostra "Lendo o pendrive…" e depois "N jogos lidos às HH:MM"). Organizar jogos, Capas e Vídeos já aparecem preenchidos com essa leitura, sem você clicar de novo. Ela é refeita sozinha depois de organizar, desfazer, renomear ou preparar o pendrive; clique em **Atualizar** se você mexeu nos arquivos fora do programa.
- **Cada tela** tem o título e a **ação principal** (em verde) no canto superior direito. As opções avançadas ficam **recolhidas** atrás de um botão com **▸**: clique para abrir. Nada que grave ou processe arquivos vem **marcado** ao abrir o programa.
- **Rodapé:** mostra a etapa atual, a porcentagem, o tempo que falta e o botão **Cancelar**.
- **Menu Arquivo:** abre o pendrive no Explorer.
- **Menu Tema:** muda as cores da janela (Noite, Menu ManiacoODE (azul e ouro) e Claro). A escolha fica salva.
- **Menu Idioma / Language:** mostra a janela em português ou em inglês. Na primeira vez, o programa usa o idioma do Windows.
- **Menu Ajuda:** **Verificar atualizações…**, **Verificar ao abrir o programa**, blog Maniaco Game Room e Sobre.

**Atualizações do programa:** em **Ajuda → Verificar atualizações…** o programa consulta a última release no GitHub e mostra as novidades. Havendo versão nova, **Baixar e atualizar** baixa o programa, confere o **SHA-256** e, com o seu aval, fecha e troca o arquivo; o antigo fica ao lado como `3DO-ManiacoODE-Gerenciador.exe.anterior`, para você voltar se precisar. **Verificar ao abrir o programa** vem ligado: quando sai versão nova, aparece um aviso amarelo na barra lateral, embaixo (**⚠ Nova versão … disponível**); clique nele para ver as novidades e atualizar. Para não consultar o GitHub ao abrir, desmarque a opção no menu Ajuda. Precisa de internet. Só o programa do PC se atualiza sozinho: o `boot.iso` e a firmware você copia (a firmware também pelas Opções do menu).

### 4.2 Preparar pendrive

Marque o que quer fazer e clique em **Preparar pendrive** (botão verde, no canto superior direito). **Nenhum passo vem marcado** ao abrir o programa: escolher o arquivo ou a pasta de um passo já o marca. Cada passo pode ser feito sozinho. As opções de organização dos jogos ficam em **Como organizar os jogos ▸**.

1. **Instalar o menu (`boot.iso`):** aponte o `boot.iso` baixado. O que já estava no pendrive vira `boot_anterior.iso`.
2. **Atualizar a firmware (`SataTo3DO.uf2`):** o programa confere se o arquivo é mesmo uma firmware do RP2040 e copia para a raiz do pendrive. Depois, no 3DO, abra **Opções → Atualizar firmware** ([seção 9](#9-atualizar-a-firmware)).
3. **Copiar os jogos da pasta do PC:** aponte a pasta com os seus jogos (`.iso`, `.cue` + `.bin`, `.chd`). Eles vão para o pendrive já organizados, conforme **Como organizar os jogos** ([seção 4.3](#43-organizar-jogos)). O programa confere se cabe antes de copiar e pula o que já está lá.
4. **Gerar as capas e os dados dos jogos:** usa as opções da aba Capas ([seção 4.4](#44-capas)).
5. **Criar a pasta `saves`:** necessária para o backup de saves no pendrive.

**Jogos da pasta do PC:** assim que você escolhe a pasta, o programa lista os jogos dela e confere o pendrive. Para cada jogo aparecem o **nome no menu**, o arquivo, o tamanho e a coluna **No pendrive**:

- **Já está** — o jogo já está no pendrive, com o mesmo tamanho;
- **Já está (com outro nome)** — o mesmo jogo (e disco) está no pendrive com outro nome ou em outra pasta;
- **Não** — ainda não está no pendrive.

Na coluna **Enviar**, clique para marcar (☑) ou desmarcar (☐). Já vêm marcados só os que faltam; os botões **Só os que faltam**, **Todos** e **Nenhum** ajudam, e o resumo mostra quantos jogos e quantos GB vão. **Preparar pendrive** envia só os marcados.

**Jogos em `.chd`:** são convertidos direto no pendrive, com o **chdman** do MAME, que vem junto com o programa. A coluna **Arquivo na pasta** mostra o que sai de cada um:

- **`→ .iso`** — o CD tem **uma trilha de dados só** (é o caso de quase todos os jogos de 3DO). O programa tira o cabeçalho e a correção de erros de cada setor e confere **todos os setores** antes de aceitar o `.iso`; o arquivo no pendrive fica menor (2048 de 2352 bytes por setor). A conversão passa por uma pasta temporária do PC, então precisa de alguns GB livres no disco do Windows;
- **`→ .cue + .bin (com áudio)`** — o CD tem trilhas de áudio (que um `.iso` não guarda): sai um `.cue` + **um `.bin` só**, o formato que a ODE lê. O mesmo vale se algum setor não estiver no padrão ou se faltar espaço no PC para o `.iso`: o jogo nunca perde conteúdo, e o relatório avisa "ficou .cue + .bin".

O rodapé mostra "Jogo N de M · nome · %". O jogo convertido é conferido antes de ir para a pasta; um `.chd` estragado ou que não é de 3DO aparece no relatório e não deixa arquivo no pendrive. No fim, o programa pergunta se os `.chd` originais do PC vão para a **Lixeira** (dá para recuperar).

O programa só continua se o pendrive estiver em **FAT32** ou **exFAT**. Ao final, a caixa de texto mostra o **relatório** de tudo o que foi feito, inclusive os jogos que ficaram de fora e o motivo.

### 4.3 Organizar jogos

![Organizar jogos](imagens/app-organizar.png)

Arruma os jogos que **já estão no pendrive**, com as mesmas regras que a firmware usa para montar a lista.

1. Clique em **Opções ▸** e escolha (a lista se refaz sozinha a cada mudança):
   - **Nomes limpos:** tira região e marcas do nome (`Need For Speed, The (USA) [!]` → `The Need For Speed`);
   - **Pastas:** uma pasta para cada jogo (o padrão), por letra (A, B, C…), **por região** ou manter as pastas atuais. **Por região** cria as pastas `USA`, `JP`, `EU` e `Outras` (Coreia, Brasil, Ásia, mundo e jogos sem região no nome) e mantém a região no nome do jogo (`Gex (USA)`, `Gex (Japan)`), porque o menu mostra todos os jogos numa lista só. Nesse modo, versões de regiões diferentes **não** contam como repetidas; só é repetido o que tem a mesma região e o mesmo disco. Funciona melhor com os nomes originais (com a região entre parênteses): em jogos já com o nome limpo, sem região, tudo iria para `Outras`;
   - **Separar as versões repetidas do mesmo jogo:** deixa só uma cópia de cada jogo na lista e põe as outras na pasta `_duplicados`. Entre um `.iso` e um `.cue` + `.bin`, fica o `.cue` + `.bin`, que guarda as trilhas de áudio. Vem **desmarcada** ao abrir.
   > **Atenção:** o programa **nunca apaga nada**. Abra a pasta `_duplicados` de vez em quando e exclua você mesmo o que não quer guardar, para não ocupar espaço no pendrive. Só você decide o que apagar. A ODE lista os jogos de todas as pastas, e a pasta `_duplicados` não é escondida dela: se uma versão repetida aparecer no menu, tire-a de lá (ainda a confirmar no 3DO).
2. A **análise aparece sozinha** (a leitura do pendrive é a mesma das outras telas); **1) Atualizar a análise** lê o pendrive de novo. A lista mostra o que vai acontecer com cada jogo, com o **nome atual no menu** e o **nome final no menu**:
   - **Já está certo** — nada muda;
   - **Mover / renomear** — vai para outra pasta e/ou ganha o nome limpo;
   - **Mover p/ `_duplicados`** — cópia repetida (entre cópias iguais, fica a que já está com o nome e a pasta certos);
   - **Tirar .cue antigo** — `.cue` que aponta para um `.iso` de nome antigo, com uma trilha só de dados, cujo jogo está no pendrive: o `.iso` sozinho já é o disco inteiro, então o `.cue` vai para `_duplicados`;
   - **Corrigir o `.cue`** — o `.cue` aponta para um arquivo que não existe ou tem nome de trilha longo demais para a firmware;
   - **Criar o `.cue`** — `.bin` sem `.cue`;
   - **Não entra na lista** — não é um disco de 3DO, ou o `.cue` não tem como ser corrigido.
3. **Quer outro nome para um jogo?** Escolha-o e clique em **Editar nome…** (ou dê dois cliques nele). A janela mostra o **arquivo atual**, o **nome atual no menu** e o campo **Nome final no menu**; o nome vale **exatamente como você digitar** (sem limpeza e sem a região). Os nomes escolhidos aparecem com ✎ na lista, e o programa avisa se o nome é inválido ou se já existe outro jogo com ele. Ao aplicar, a pasta e o arquivo ganham o nome, e a **capa, a imagem escolhida, o modo, as informações e o vídeo** que o jogo já tinha vão junto (o que for novo precisa de **Gerar capas**, que o programa oferece no fim). O jogo fica marcado como **Não alterar**, para o nome escolhido não ser "limpo" de novo. **Voltar ao nome automático** desfaz.
4. **Algum jogo já está com o nome certo** (um homebrew, por exemplo)? Escolha-o e clique em **Não alterar / voltar a organizar**: ele fica onde está, com o nome atual, também nas próximas análises. Clique de novo para voltar a organizar.
5. Confira e clique em **2) Aplicar**.
6. Mudou de ideia? **Desfazer a última organização** devolve tudo como estava.

Arquivos que a ODE não lê (`.zip`, `.7z`…) aparecem num aviso embaixo da lista: extraia ou converta para `.iso` ou `.cue` + `.bin`. Jogos em `.chd` no pendrive também aparecem nesse aviso: copie-os pela aba **Preparar pendrive**, que converte. As imagens, os modos de capa, as informações e os vídeos que você escolheu acompanham o jogo quando ele é renomeado. Depois de aplicar, o programa oferece gerar as capas de novo, porque os nomes mudaram.

### 4.4 Capas

![Capas](imagens/app-capas.png)

A capa aparece ao lado da lista no menu, numa moldura de 106×116 pixels, com o gênero, o estúdio e o ano do jogo.

1. A grade **aparece sozinha** ao abrir a tela (usa a leitura do pendrive); **1) Atualizar a lista** lê o pendrive de novo. Cada jogo vira um **cartão** com a miniatura da capa, o nome e a situação: **capa ok**, **a conferir** (amarelo: capa da Wikipedia, tela de título ou captura de tela, para você conferir) ou **sem capa** (vermelho; esses aparecem primeiro). O cartão também diz se a capa já está no pendrive e de onde vêm as informações (**internet**, **você** ou **falta**). As miniaturas aparecem aos poucos.
2. **Buscar:** digite parte do nome. Os botões **Todos**, **Sem capa**, **A conferir**, **Falta gerar** e **Sem informações** filtram a grade, e o contador mostra "N de M jogos". **Sem informações** mostra os jogos que não têm gênero, estúdio e ano nem seus nem da internet (não é o mesmo que **Sem capa**). As **setas do teclado** andam pela grade.
3. Clique num cartão para ver, à direita, a **prévia na TV** (já dentro da moldura do menu), o **gênero, o estúdio e o lançamento** do jogo e as ações.
4. Clique em **2) Gerar capas no pendrive** para gravar tudo na pasta `covers`.

As opções desta tela ficam recolhidas em **Opções ▸**: como encaixar a capa, a sua pasta de imagens e onde procurar.

**Como encaixar a capa (padrão para todos os jogos):**

- **Preencher a moldura** (recomendado): estica a capa inteira até encher a moldura, sem cortar nada.
- **Manter a proporção:** a capa fica sem esticar, com faixas em volta.

**Esta capa:** no painel da prévia, escolha o modo só do jogo selecionado — **Padrão** (segue a opção acima), **Preencher** ou **Proporção**. Assim umas capas preenchem a moldura e outras mantêm a proporção. O modo escolhido fica guardado no pendrive (`<jogo>.modo`).

**Onde procurar as capas:** o programa usa a primeira que encontrar, nesta ordem:

1. **Suas imagens** — escolhidas na aba Capas ou na **sua pasta de imagens** (opcional; o nome do arquivo é o nome do jogo, em PNG, JPG, WEBP, BMP ou GIF);
2. **libretro-thumbnails** — acervo de capas, com as versões americana, europeia e japonesa;
3. **ScreenScraper** e **TheGamesDB** — só com conta ([seção 4.6](#46-contas-do-screenscraper-e-do-thegamesdb));
4. **Wikipedia** — a imagem do artigo do jogo. **Confira:** às vezes é a caixa de outra plataforma;
5. **Tela de título** do jogo — sempre com a proporção mantida;
6. **Captura de tela do jogo** (libretro) — último recurso, também com a proporção mantida.

As caixas **Usar a Wikipedia…** e **Usar a tela de título ou uma captura do jogo…** ligam ou desligam essas fontes na escolha automática. **Baixar da internet o que faltar** vem **desligada** ao abrir o programa e então usa só o que já foi baixado antes; marque-a para procurar na internet.

**Trocar a capa de um jogo** (botões do painel da prévia):

![Escolher entre as capas](imagens/app-escolher.png)

- **Escolher entre as capas…** — abre todas as capas encontradas para o jogo (todas as regiões, Wikipedia, tela de título…), já como ficam na TV. Clique na que quiser.
- **Escolher imagem…** — uma imagem do seu PC.
- **Colar imagem (Ctrl+V)** — uma imagem copiada, por exemplo do navegador ("Copiar imagem").
- **Procurar na internet** — abre a busca de imagens no navegador.
- **Voltar à capa automática** — desfaz a troca. A imagem que você tinha escolhido vai para `covers/originais/_antigas`.

A capa escolhida fica guardada no pendrive e é usada nas próximas vezes. Depois de trocar, clique em **2) Gerar capas no pendrive**.

**Importar capas de uma pasta:** o botão **Importar capas…** pega as imagens de uma pasta do PC (por exemplo, uma pasta do EmuMovies, com subpastas) e casa cada uma com um jogo do pendrive pelo **nome**: nome igual, mesmo título sem a região ou nome parecido; entre várias regiões ganha a do jogo, e o "Disco 2" não pega a imagem do "Disco 1". O programa mostra quantos arquivos achou, quantos jogos **já têm capa sua** (ficam como estão) e quantos ficaram sem arquivo, e **só importa se você confirmar**. As imagens vão para `covers/originais`; depois clique em **2) Gerar capas no pendrive**.

**Informações do jogo (gênero, estúdio e lançamento):** o cartão e o painel mostram de onde elas vêm — **internet**, **você** ou **falta**. Para preencher ou corrigir, escolha o jogo e clique em **Informações do jogo…** (ou dê dois cliques no cartão):

- campos **Gênero**, **Gênero em inglês** (usado com o menu em inglês), **Estúdio** e **Lançamento**;
- embaixo, as linhas **como o menu vai mostrar**, com a mesma fonte do 3DO: avisa quando o texto vai ser cortado com "…" e quando há caracteres que o menu não tem (aparecem como "?");
- **Salvar no pendrive** grava na hora, sem precisar gerar as capas de novo. O que você preenche vale mais que a internet; campo deixado vazio usa o da internet;
- **Usar o da internet** volta às informações automáticas (as suas vão para `covers/originais/_antigas`).

**Renomear jogo…:** muda o nome que o menu mostra. O programa renomeia a pasta do jogo (ou o arquivo, se o nome vem do arquivo) e leva junto a capa, a imagem escolhida, o modo da capa e as informações. O jogo renomeado fica marcado como **Não alterar** no Organizar, para o nome escolhido não ser trocado depois.

### 4.5 Vídeos

![Vídeos](imagens/app-videos.png)

Parado num jogo, o menu toca um clipe curto no lugar da capa, **com som** (menu v1.3 ou mais novo; o menu v1.2 toca o clipe sem som, e o v1.1 ignora os vídeos). No 3DO, dá para desligar a imagem em **Opções → Prévia em vídeo** e o som em **Opções → Som da prévia**.

1. A grade **aparece sozinha** ao abrir a tela; **1) Atualizar a lista** lê o pendrive de novo. Cada jogo vira um **cartão** com um quadro do vídeo que já está no menu e a situação: **vídeo ok**, **para baixar** (o mesmo nome existe no **ScreenScraper**), **sem resultado** ou **sem vídeo** (ainda não consultado). Os botões **Todos os jogos**, **Com vídeo**, **Para baixar** e **Sem vídeo** e o campo **Buscar** filtram a grade. **Sobre os vídeos ▸** mostra a licença.
2. Clique em **2) Baixar do ScreenScraper os que faltam**: o programa consulta a lista de jogos do site (não precisa de conta), mostra quantos vídeos vai baixar e grava no pendrive os dos jogos sem vídeo que têm o mesmo nome no site. Os **parecidos** ficam para você escolher. Se o site não responder, o programa tenta de novo sozinho (3 vezes); se continuar fora do ar, ele avisa quais jogos ficaram para depois: clique de novo no botão mais tarde, e só esses serão baixados.
3. Clique num cartão para ver, à direita, a **prévia animada**, do jeito que o menu mostra, dentro da moldura, e o que o ScreenScraper achou para o jogo.

**Um jogo de cada vez** (botões do painel da prévia):

- **Procurar no ScreenScraper…** — mostra os jogos do site parecidos com o nome; dá para procurar com outro nome. Escolha um e clique em **Usar este**.
- **Usar vídeo do computador…** — qualquer vídeo do PC (`.mp4`, `.mkv`, `.avi`, `.webm`, `.mov`…), por exemplo uma gravação do seu console.
- **Começar o clipe em (s)** e **Ver prévia** — escolha em que segundo do vídeo o clipe começa e veja como fica antes de gravar; **Aplicar com este início** grava. Os vídeos do ScreenScraper começam em 3 s, para pular a entrada.
- **Tirar o vídeo do menu** — o vídeo sai do menu; o original vai para `covers/originais/_antigas` (não é apagado).

Todo vídeo é ajustado sozinho pelo programa para o que o menu aguenta (clipe curto e leve). O vídeo original fica guardado em `covers/originais` (de 1 a 10 MB cada), para dar para mudar o início depois.

**Importar vídeos de uma pasta:** **Importar vídeos…** faz o mesmo que as capas: pega os vídeos de uma pasta do PC (por exemplo, clipes do EmuMovies), casa pelo nome com os jogos do pendrive, mostra o resumo e, com o seu aval, converte cada um para o menu (alguns segundos por jogo). Quem já tem vídeo fica como está.

**Som:** o programa grava o som do clipe junto do vídeo (mono, 11025 Hz, cerca de 88 KB a mais). A caixa **Gravar o som nos vídeos novos** do painel vem **ligada**; desligue se usa o menu v1.2, que não toca som (os bytes a mais só deixam o vídeo um pouco mais lento para carregar). Os vídeos gravados antes do som existir ficam **mudos**: **Adicionar som aos vídeos que já estão no pendrive…** refaz só esses, do original guardado ou baixando de novo do ScreenScraper (muda os `.vid`, não os jogos; vale a pena fazer um backup da pasta `covers` antes).

> Os vídeos do ScreenScraper têm licença Creative Commons BY-NC-SA: só para uso pessoal, sem fins comerciais.

### 4.6 Contas do ScreenScraper e do TheGamesDB

Opcional. Com estas contas, o programa também procura capas no **ScreenScraper** e no **TheGamesDB** quando o libretro não tiver. Abra pelo botão **Contas (ScreenScraper, TheGamesDB)…** da aba Capas.

- **ScreenScraper:** precisa de uma **credencial de desenvolvedor** (pedida à equipe do site) e, de preferência, da **sua conta** no site (usuário e senha), que dá mais acessos por dia.
- **TheGamesDB:** precisa de uma **chave de API**, pedida no fórum do site.
- **Testar** confere as contas sem fechar a janela e mostra **uma linha por serviço**: **funcionando**, **sem conexão** (internet ou serviço fora do ar), **credenciais recusadas**, **cota diária esgotada** ou **não testado** (falta preencher: no ScreenScraper, só o seu usuário e senha **não bastam** para procurar capas, é preciso a credencial de desenvolvedor); **Salvar** guarda.
- As senhas e a chave ficam **só neste computador**, protegidas pelo Windows.

Se a conta for recusada ou a cota do dia acabar, o programa para de usar aquela fonte até ser aberto de novo e segue com as outras.

### 4.7 O que o programa grava no pendrive

| No pendrive | O que é |
|---|---|
| `boot.iso` | o menu |
| `boot_anterior.iso` | o menu que estava antes |
| `SataTo3DO.uf2` | a firmware nova, até ser aplicada (vira `.ok` ou `.erro`) |
| `saves/` | backup de saves feito pelo menu |
| `covers/` | capas (`.cap`), vídeos de prévia (`.vid`, com o som), dados dos jogos (`dados.cap`) e a lista das capas e dos vídeos (`indice.cap`) |
| `covers/originais/` | as imagens que você escolheu, o modo de cada capa (`<jogo>.modo`), as informações preenchidas à mão (`<jogo>.info`), os vídeos originais e o início de cada clipe (`<jogo>.inicio`) |
| `covers/originais/_antigas/` | imagens e vídeos que você trocou ou tirou |
| `_duplicados/` | versões repetidas separadas da lista (você decide quando apagar; a ODE pode listá-las, veja [4.3](#43-organizar-jogos)) |
| `.maniacoode/` | o registro para **Desfazer a última organização** e os jogos marcados **Não alterar** (`nao_alterar.json`) |

### 4.8 Firmware da ODE: backup e volta à original

![Tela Firmware](imagens/app-firmware.png)

> ⚠️ **Salve a firmware original antes de gravar a nossa.** É o backup que deixa você voltar à firmware da ODE se a ManiacoODE não funcionar na sua placa. O programa avisa se você tentar gravar sem ele.

A tela **Firmware** (última da barra lateral) trabalha com a ODE ligada ao PC em **modo BOOTSEL** (a unidade **RPI-RP2** aparece). O aviso no alto da tela mostra se o programa a enxergou (**● ODE em modo BOOTSEL**) e se já há um backup (**✔ Backup da original**).

1. **Salvar a firmware original (backup).** **Salvar o backup** lê a flash inteira da ODE (só lê: nada é gravado nela) e salva `firmware-original-AAAAMMDD-HHMMSS.uf2`, mais um `.txt` com os dados da placa e o **SHA-256**, na pasta que você escolher (padrão: **Documentos\3DO-ManiacoODE\Backup da firmware**). O programa confere a leitura, marca os arquivos como somente leitura e nunca sobrescreve um backup anterior. **Guarde uma cópia fora do PC.** Se a ODE já estiver com a firmware ManiacoODE, o programa avisa: o arquivo salvo **não** é a original e não conta como backup.
2. **Gravar a firmware ManiacoODE.** Escolha o `SataTo3DO.uf2` e clique em **Gravar na ODE**. Se ainda não houver backup, o programa pergunta antes (resposta padrão: não). A ODE reinicia sozinha ao terminar; depois, desconecte o USB do PC. Nas próximas vezes, atualize pelo menu do 3DO ([seção 9](#9-atualizar-a-firmware)).
3. **Voltar à firmware original.** Se a nossa não funcionar, coloque a ODE em BOOTSEL de novo, escolha o backup (o programa já sugere o último) e clique em **Voltar à original**. O programa só aceita um backup de flash inteira que não seja a firmware ManiacoODE.

O botão **Como colocar a ODE em modo BOOTSEL…** mostra o passo a passo, e **Instalar o driver USB…** cuida do driver (abaixo). Uma linha da tela diz se o driver já está instalado: **✔ Driver USB instalado** ou **✖ Falta o driver USB**.

**Driver (só para o backup):** para salvar o backup, o programa fala direto com a placa pelo USB (usa o `picotool` da Raspberry Pi, que vai junto no `.exe`), e o Windows precisa do driver **WinUSB** para essa porta. Com a ODE em BOOTSEL, clique em **Instalar o driver USB…**:

1. O programa confere se o driver já está instalado; se estiver, só avisa. Se a ODE não estiver em BOOTSEL, pede para você colocá-la.
2. Se faltar, ele explica o que vai acontecer e, com o seu **"sim"**, abre o **Zadig** (que vai junto no `.exe`, sem você precisar baixar nada). O Windows pede a **permissão de administrador** só nesse momento; o resto do programa continua sem ela.
3. No Zadig: **Options → List All Devices**, escolha **RP2 Boot (Interface 1)**, confira que o driver é **WinUSB** e clique em **Install Driver** (ou **Replace Driver**); espere terminar e feche.
4. O programa percebe sozinho quando o driver aparece e **confirma que enxerga a ODE** (só lê, não grava nada). Se algo falhar, a mensagem diz o que tentar: recusou a permissão de administrador (clique em **Sim** quando o Windows perguntar), o Zadig não terminou (feche, desconecte o USB e conecte de novo com o botão BOOTSEL apertado) ou a ODE ainda não foi reconhecida.

É uma vez só. O programa **não instala o driver em silêncio**: o Windows só aceita drivers assinados, e o Zadig cria a assinatura na hora, com a sua confirmação. **Gravar** e **Voltar à original** não precisam do driver: só copiam o arquivo para a unidade RPI-RP2.

> **Não desligue a ODE nem tire o USB durante a gravação** (poucos segundos). Se algo der errado, a placa continua podendo entrar em BOOTSEL: basta repetir. Nunca conecte o USB do PC com o console ligado.

## 5. Usando o menu

Ao ligar o console, toca a **abertura em vídeo** escolhida em **Opções → Abertura** (A, B, C, X ou Start pula) e o menu abre com **todos os jogos em ordem alfabética**, com a capa e os dados do jogo escolhido. Com muitos jogos no pendrive, a lista pode demorar alguns segundos depois da abertura: o menu mostra **Lendo a lista de jogos...** e ela aparece sozinha (firmware 1.1.2).

| Botão | Na lista de jogos |
|---|---|
| **▲ ▼** | sobe e desce na lista |
| **L / R** (ou **◄ ►**) | pula uma tela inteira de jogos |
| **A** (ou **Start**) | inicia o jogo |
| **X** | abre as **Opções** |

Parado num jogo que tem **vídeo de prévia** no pendrive ([seção 4.5](#45-vídeos)), depois de alguns segundos o vídeo toca no lugar da capa, com som; mexer na lista volta para a capa e para o som.

Quando você escolhe um jogo, o 3DO reinicia e carrega o disco. Para voltar ao menu, desligue e ligue o console.

## 6. Opções

| | |
|---|---|
| ![Opções](imagens/opcoes.png) | ![Informações](imagens/opcoes-info.png) |

| Item | O que faz |
|---|---|
| **Tamanho da tela** | Diminui o menu de 100 % até 80 %, em passos de 5 %, para TVs que cortam as bordas. Ajuste com **◄ ►**. |
| **Idioma / Language** | Textos do menu em português ou em inglês. Troque com **◄ ►**. |
| **Prévia em vídeo** | Liga ou desliga o vídeo de prévia no lugar da capa ([seção 4.5](#45-vídeos)). Troque com **◄ ►**. |
| **Som da prévia** | Liga ou desliga o som do vídeo de prévia (só para os vídeos gravados com som). Troque com **◄ ►**. |
| **Abertura** | Vídeo que toca ao ligar o console: um dos quatro (ManiacoOS1, ManiacoOS2, ManiacoOS3, 3DO), **Aleatória** ou **Desligada**. Troque com **◄ ►**; vale a partir da próxima vez que ligar. |
| **Gerenciar saves** | Abre o gerenciador de saves (**A**). |
| **Atualizar firmware** | Grava a `SataTo3DO.uf2` do pendrive ([seção 9](#9-atualizar-a-firmware)). |
| **Informações** | Abre (**A**) a tela com a versão do menu, a versão da firmware da ODE e quantos jogos, capas, vídeos de prévia e jogos com gênero/estúdio/ano há no pendrive. |

Embaixo das Opções aparecem a versão do menu e a da firmware da ODE. **▲ ▼** escolhem o item; **B** volta para a lista. O tamanho da tela, o idioma, a prévia em vídeo, o som da prévia e a abertura ficam salvos na memória do 3DO (NVRAM) ao voltar.

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

### Voltar à firmware original

Voltar à firmware original só é possível pelo PC, em modo BOOTSEL, com o backup que você salvou antes da primeira gravação ([seção 2](#2-primeira-instalação-da-firmware) e [seção 4.8](#48-firmware-da-ode-backup-e-volta-à-original)).

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
| **Com muitos jogos (ou depois de organizar ou renomear muitas pastas), o 3DO fica em tela preta ou mostra a tela dele em vez do menu** | A firmware antiga só avisa o 3DO depois de montar a lista inteira, e o 3DO desiste antes. Atualize para a **firmware 1.1.2** ([seção 9](#9-atualizar-a-firmware); se o menu não abre, tire jogos do pendrive para atualizar, ou use o `SataTo3DO_forcar.uf2`). Se o diretório ficou cheio de entradas apagadas, refaça também o pendrive: copie tudo para uma pasta do PC, formate o pendrive em **FAT32** e copie de volta. |
| **Renomeei a pasta de um jogo e a capa sumiu** | A capa e as informações ficaram com o nome antigo. Use **Renomear jogo…** no programa do PC; se já renomeou pelo Windows, renomeie também os arquivos de `covers/originais` e clique em **Gerar capas no pendrive**. |
| **A firmware ManiacoODE não funcionou na minha placa** | Volte à original: ODE em modo BOOTSEL e **Voltar à original** na tela Firmware do programa (ou copie o seu `firmware-original-….uf2` para a unidade RPI-RP2). [Seção 4.8](#48-firmware-da-ode-backup-e-volta-à-original). |
| **Não salvei o backup antes de gravar** | Sem o backup não há como voltar ao arquivo exato da sua placa. Peça a firmware original ao vendedor ou fabricante da sua ODE (ou ao projeto SataTo3DO) e grave-a em modo BOOTSEL. Daqui em diante, salve o backup antes de qualquer gravação pelo PC. |
| **O backup diz que falta o driver, ou "não achei a ODE"** | Confira se a unidade **RPI-RP2** aparece (modo BOOTSEL, console desligado). Se aparece e o backup ainda falha, clique em **Instalar o driver USB…** na tela Firmware ([seção 4.8](#48-firmware-da-ode-backup-e-volta-à-original)). |
| **Uma versão repetida aparece no menu** | A ODE lista os jogos de todas as pastas, inclusive de `_duplicados`. Abra essa pasta no pendrive e tire (ou apague) o que não quer na lista. |
| **O programa não abre o Zadig / diz que a permissão foi recusada** | Clique em **Sim** quando o Windows perguntar se o instalador pode fazer alterações. Sem a permissão de administrador o driver não instala. |
| **Aparece "Sem informações" em jogos que têm capa** | Sem informações é gênero, estúdio e ano (não a capa). Escolha o jogo e use **Informações do jogo…** para preencher. |
| **Quero voltar ao menu anterior** | Apague o `boot.iso` e renomeie o `boot_anterior.iso` (ou o antigo) de volta para `boot.iso`. |
