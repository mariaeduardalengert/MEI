# Caixa MEI

Controle de caixa para MEI de serviços, em regime de caixa. O aplicativo fica neste repositório (GitHub Pages) e os dados ficam numa planilha Google do seu Drive.

Este repositório é público e **não guarda dado nenhum**. Nunca coloque extratos, valores ou planilhas aqui.

Arquivos: `index.html` (o app), `config.js` (onde você cola o ID do Google) e `.nojekyll` (opcional).

Os nomes de menus do GitHub e do Google mudam de tempos em tempos. Se algo não estiver exatamente onde está escrito, procure pelo nome parecido.

## Antes de começar

Decida em qual conta Google ficarão os dados. O ideal é uma conta só do MEI (pode ser um Gmail novo), com verificação em duas etapas ligada. Assim o contador pode receber acesso à planilha sem misturar com sua vida pessoal. Usar a conta pessoal funciona, mas mistura tudo.

O GitHub guarda só o código. Pode ser a conta que você já usa.

## Parte A: colocar o app no GitHub (10 minutos)

1. Entre em github.com e clique em **New repository**.
2. Nome: `caixa-mei`. Marque **Public**. O GitHub Pages gratuito só publica repositório público. Clique em **Create repository**.
3. Clique em **uploading an existing file**, arraste `index.html` e `config.js` (e `.nojekyll`, se aparecer) e clique em **Commit changes**.
4. Vá em **Settings**, depois **Pages**. Em *Build and deployment*, escolha **Deploy from a branch**, branch **main**, pasta **/ (root)**, e clique em **Save**.
5. Em 1 a 2 minutos o endereço aparece no topo da página, no formato `https://SEU-USUARIO.github.io/caixa-mei/`. Anote.

## Parte B: liberar o Google (15 minutos)

1. Abra console.cloud.google.com com a conta Google que guardará os dados.
2. Crie um projeto chamado **Caixa MEI**.
3. Em **APIs e serviços**, depois **Biblioteca**, ative duas APIs: **Google Sheets API** e **Google Drive API**.
4. Abra a **tela de consentimento OAuth** (hoje chamada Google Auth Platform):
   - Tipo de usuário: **Externo**.
   - Nome do app: Caixa MEI. Informe seu e-mail de suporte e de contato.
   - Em **Acesso a dados** (escopos), adicione `https://www.googleapis.com/auth/drive.file`. É o menor acesso possível: o app só enxerga a planilha que ele mesmo criou, nunca seus outros arquivos.
   - Em **Público**, adicione seu e-mail como usuário de teste. Depois clique em **Publicar app**. Em modo de teste, o Google pede nova permissão a cada 7 dias. Se aparecer o aviso de app não verificado, é o seu próprio app e você pode continuar.
5. Em **Clientes** (ou **Credenciais**), clique em **Criar cliente OAuth**, tipo **Aplicativo da Web**.
   - Em **Origens JavaScript autorizadas**, informe `https://SEU-USUARIO.github.io` (sem barra no fim e sem `/caixa-mei`).
   - Crie e copie o **ID do cliente**. Ele termina em `.apps.googleusercontent.com`. Esse ID não é segredo.

## Parte C: ligar o app ao Google

1. No GitHub, abra o arquivo `config.js` e clique no lápis (editar).
2. Cole o ID do cliente entre as aspas de `googleClientId` e clique em **Commit changes**.
3. Espere 1 a 2 minutos.

## Parte D: primeiro uso e teste

1. Abra o endereço do app e toque em **Conectar ao Google**. Escolha a conta, aceite a permissão.
2. O app cria no seu Drive a planilha **Caixa MEI - Dados**.
3. Toque em **Começar com meus dados**, vá em **Ajustes** e cadastre suas contas na aba **Contas** (corrente, aplicações, dinheiro em espécie), com o saldo inicial de cada uma.
4. Teste: lance uma receita de R$ 1,00, abra a planilha por Ajustes, **Abrir planilha**, e confira a linha. Depois exclua o teste no app.
5. No celular, abra o endereço e use **Adicionar à tela inicial**.

## Como a planilha funciona

- Aba `Lancamentos`: uma linha por lançamento. Aba `Config`: seus ajustes.
- Você pode filtrar, criar tabelas dinâmicas e compartilhar com o contador (como leitor).
- O histórico de versões da planilha (Arquivo, Histórico de versões) serve de backup.
- Não renomeie as abas nem o arquivo, não apague o cabeçalho e não mude a ordem das colunas. As colunas K (`conta`) e L (`conta_destino`) são novas: deixe vazias para usar as contas padrão, ou escreva o nome da conta. Transferência entre contas (`transf`, `aplicacao`, `resgate`) usa as duas. O app encontra a planilha pelo nome.
- Se editar à mão: data no formato `AAAA-MM-DD`, valor positivo e tipo entre `receita`, `despesa`, `aplicacao`, `resgate`, `rendimento`, `ret_isenta`, `ret_trib`, `aporte`. Linhas inválidas são ignoradas nos cálculos, e o app avisa em Ajustes.

## Lançar o histórico desde a abertura

Em **Ajustes**, informe o mês de abertura do MEI (`AAAA-MM`). O app calcula o limite proporcional do primeiro ano (R$ 6.750 por mês de atividade, contando o mês de abertura) e não marca como atrasado o DAS dos meses anteriores à abertura.

Escolha um dos dois caminhos e não misture:
- **Histórico completo desde a abertura:** lance tudo e deixe os saldos iniciais e o "lucro já guardado" zerados. Se havia dinheiro na conta antes do primeiro lançamento, informe só esse saldo.
- **Só a partir de uma data:** informe os saldos de extrato da véspera e o lucro já guardado. Para o limite do MEI, lance ao menos a receita de cada mês do ano corrente.

Lançar por mês (uma receita por atividade, despesas por categoria) é suficiente. Depois confira o saldo de cada conta com o extrato, na aba **Contas**, em **Conferir com o extrato**. Para as aplicações, digite o saldo líquido do banco no fim do mês e o app calcula o rendimento para você lançar.

Para muitas linhas, cole direto na aba `Lancamentos` da planilha e toque em **Atualizar da planilha**. O app aceita:
- Data como data do Google ou `dd/mm/aaaa`; tipo como `Receita`, `Despesa`, `Aplicação`, `Resgate`, `Rendimento`, `Lucro isento`, `Lucro tributável` ou `Aporte`.
- Atividade como `Comunicação`, `Patinação` ou `Geral` (obrigatória nas receitas), valor como número ou `1.250,50`, e nota fiscal como `sim` ou `não`.
- Coluna `id` em branco. DAS sem competência é lido como o mês anterior à data de pagamento.
Linhas que o app não entende são ignoradas e contadas em Ajustes.

## Limites que você precisa conhecer

- Ao abrir o app, pode ser preciso tocar em **Conectar** de novo. O Google renova a permissão a cada hora e não deixa o app fazer isso sozinho em todos os casos.
- Sem internet, o app não grava.
- Se o app mostrar erro de API não ativada, volte à Parte B, passo 3, e ative a API que o erro citar.
- Se o app abrir sem pedir Google, o `config.js` está vazio: nesse caso ele guarda os dados só no navegador.

## Trazer os dados do app antigo

No app antigo, vá em Ajustes e **Baixar backup (JSON)**. No novo, depois de conectar, use **Importar backup**.


## Atualizar para esta versão

Para trocar só o app, no repositório clique em `index.html`, depois no lápis, apague o conteúdo e cole o novo. Ou use **Add file > Upload files** e envie o novo `index.html` por cima. **Não substitua o `config.js`**: é nele que está o seu ID do cliente.

Os dados antigos continuam valendo: lançamentos sem conta caem em "Conta corrente" e "Aplicações", e os saldos iniciais que você já tinha passam para essas contas. Na primeira conexão o app acrescenta as colunas `conta` e `conta_destino` ao cabeçalho da planilha. Antes de abrir o app novo, apague qualquer coluna extra que você tenha criado à direita de `atualizado_em` (por exemplo a fórmula de conferência na coluna L).


## Abas da planilha (versão com fechamento de mês)

A planilha **Caixa MEI - Dados** agora tem quatro abas, e o app cria as que faltarem na primeira conexão:

- **Lancamentos**: um lançamento por linha, como antes.
- **Contas**: `id`, `nome`, `tipo` (corrente, aplicacao ou especie), `saldo_inicial` e `ativa` (sim ou não). Você pode editar aqui e usar **Atualizar da planilha** no app.
- **Categorias**: `tipo` (receita ou despesa), `nome` e `marcador`. A linha com marcador `das` é a categoria da DAS. O app reconhece a DAS por esse marcador e também por variações do nome, como "DAS" e "DAS-MEI".
- **Config**: ajustes gerais, incluindo o fechamento dos meses. Não edite.

**Fechamento de mês:** em Apuração, o botão **Fechar** trava o mês e os anteriores no app e guarda uma assinatura dos números. Se alguém mexer na planilha depois, o painel avisa. A planilha em si não pode ser travada pelo app, então use o **Histórico de versões** do Google (Arquivo) para ver quem mudou o quê.


## Ícone do atalho no celular

Os arquivos `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png`, `icon.svg` e `manifest.webmanifest` ficam na mesma pasta do `index.html`. Para trocar o ícone, substitua os PNGs por imagens suas, quadradas, com o mesmo nome e tamanho. O celular guarda o ícone na hora em que o atalho é criado, então depois de trocar é preciso apagar o atalho antigo e criar de novo.
