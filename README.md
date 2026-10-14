# Painel de Controle Orçamentário & Financeiro

Dashboard executivo de execução orçamentária e financeira em **um único arquivo HTML autossuficiente** — sem servidor, sem build, sem instalação de nada.

Construído a partir da base `REL ORC_FINANCEIRO (WebApp).xlsm` — SIAFEM / SIGEF.

---

## Arquivo autossuficiente (funciona offline)

O `index.html` **não depende de nenhuma CDN**. Estão embutidos dentro do próprio arquivo:

| Recurso | Versão | Licença |
|---|---|---|
| Chart.js | 4.4.1 | MIT |
| Tailwind CSS | 3.4.16 | MIT |
| SheetJS (`xlsx`) | 0.20.3 | Apache-2.0 |
| Spectral, Public Sans, IBM Plex Mono | subset latino, pesos 400–700 | SIL Open Font License |

Consequências práticas:

- Abre e funciona **sem internet** — inclusive com duplo clique no arquivo, direto do disco (`file://`).
- Funciona em **rede corporativa restrita** que bloqueia CDNs externas.
- **Gráficos, tipografia e leitura de planilha** operam offline (nada é baixado em tempo de execução).
- Arquivo maior (~2 MB) porque carrega os recursos dentro de si — o preço de não ter dependências.

Para conferir a autossuficiência: nenhuma tag do documento aponta para endereço externo (sem `<script src="http…">`, `<link href="http…">` ou `url(http…")`).

---

## Atualizar os dados: importar ou arrastar

O painel já vem com os dados embutidos, então funciona imediatamente. Há **três formas** de atualizar, todas equivalentes:

1. **Arrastar** o arquivo `REL ORC_FINANCEIRO (.xlsm / .xlsx)` para **qualquer ponto da janela** — aparece uma moldura tracejada pedindo para soltar.
2. **Arrastar** sobre a área tracejada do bloco **Atualização de dados**, no fim do painel (logo acima das notas metodológicas).
3. **Clicar** em **Atualizar planilha** (cabeçalho) ou em **Arraste a planilha aqui** e escolher o arquivo.

A leitura acontece **localmente no seu navegador** (SheetJS) — nenhum dado é transmitido. Todos os painéis, KPIs e gráficos são recalculados na hora, e o selo ao lado do título passa a indicar a planilha carregada.

Para voltar à base embutida, recarregue a página.

Formatos aceitos: `.xlsx`, `.xlsm`, `.xls`, `.csv`.

---

## Modo escuro

Botão no cabeçalho (ícone de sol / lua), ao lado de **Atualizar planilha**.

- A preferência é **gravada no navegador** e aplicada antes da primeira pintura (sem piscar).
- Na primeira visita, segue a **preferência do sistema operacional**.
- Gráficos, tabelas, barras, selos e cores semânticas acompanham o tema (paleta resolvida por variáveis CSS).
- O **cabeçalho e o menu lateral** seguem o tema: claros no modo claro (texto escuro) e escuros no modo escuro.
- **A impressão e o PDF gerado pelo navegador saem sempre em tema claro**, independentemente do modo na tela.

---

## Conteúdo do painel

| Seção | O que mostra |
|---|---|
| Régua de Execução | LOA → supressão → disponível → a liquidar → a pagar → pago, com largura proporcional ao valor |
| KPIs do exercício | Disponível, Empenhado, Liquidado, Pago, A Liquidar, Saldo a Pagar + leitura analítica automática |
| Execução Mensal | Empenho, liquidação e pagamento mês a mês |
| Composição do Pago | Distribuição por grupo de natureza da despesa |
| Serviço da Dívida | Amortização × juros por contrato, com busca e seleção individual (grupos 2 e 6) |
| Categorias | Despesas por grupo, abertas por subação |
| Pré-Empenho | Saldo por grupo e detalhamento crédito → pré-empenho → saldo |
| Supressão | LOA → dotação autorizada e movimentos mensais por subação |
| Consulta NE | Crédito, cotas, empenhado, pago e a liquidar por nota de empenho |
| Pagamentos | Fluxo financeiro por mês e volume de documentos (ordens bancárias, NEs, PPs) |
| Fontes de Recurso | Execução por fonte (Lei + Crédito, Empenhado, Liquidado, Pago) |
| Comparativo | Série histórica por exercício, com variação percentual e gráfico |

Filtro de exercício (reconstruído a partir da planilha), filtro por grupo de natureza, busca nas tabelas, exportação por impressão, layout responsivo e respeito a `prefers-reduced-motion`.

---

## Publicar no GitHub Pages

### Opção A — script automático

Pré-requisito: [GitHub CLI](https://cli.github.com) autenticado (`gh auth login`).

```bash
cd painel-orcamentario
chmod +x deploy.sh
./deploy.sh                     # cria o repositório "painel-orcamentario"
./deploy.sh meu-painel          # ou escolha outro nome
```

### Opção B — pela interface do GitHub

1. Crie um repositório em <https://github.com/new>.
2. Envie o `index.html` e o `.nojekyll` para a raiz.
3. **Settings → Pages** → *Build and deployment → Source*: **Deploy from a branch**; *Branch*: **main** e **/ (root)**. Salve.
4. Em ~1 minuto: `https://SEU-USUARIO.github.io/painel-orcamentario/`.

### Opção C — via linha de comando (git puro)

```bash
cd painel-orcamentario
git init -b main
git add -A
git commit -m "Publica painel de controle orçamentário e financeiro"
git remote add origin https://github.com/SEU-USUARIO/painel-orcamentario.git
git push -u origin main
```

Depois ative o Pages em **Settings → Pages** como na Opção B.

---

## Pontos de atenção

- **`.nojekyll`** — não remova. Impede o GitHub Pages de processar os arquivos com Jekyll.
- **Repositório privado** — o GitHub Pages em repositórios privados exige plano pago (Pro, Team ou Enterprise). Para uso gratuito, mantenha o repositório **público**.
- **Dados expostos no código-fonte** — todos os valores ficam embutidos no `index.html`; quem abrir o código-fonte lê subações, contratos, notas de empenho e valores. Se algum campo não deve circular, filtre antes de publicar.
- **Privacidade da atualização** — a leitura da planilha ocorre inteiramente no navegador.
- **Nome do repositório** — um repositório chamado `SEU-USUARIO.github.io` publica na raiz: `https://SEU-USUARIO.github.io/`.
- **Arquivo grande** — cerca de 2 MB, por conter as bibliotecas e as fontes. É o custo de não depender de CDN.

---

## Identificação institucional

Este arquivo **não traz** brasão ou logotipo de órgão — dependem dos ativos oficiais. Quando disponíveis, devem ser inseridos no cabeçalho (`<header>`), preferencialmente em SVG ou PNG embutido, para preservar a autossuficiência.

---

## Notas metodológicas

- **Base orçamentária** — aba `REL ORCA`: Lei + Crédito, Empenhado, Liquidado e Pago por ano, mês, grupo de natureza e subação. A dotação inicial (LOA) e a supressão vêm da aba `SUPRESSAO`.
- **Fluxo financeiro** — aba `REL EXEC`: o campo `R$ OB` se repete por linha (a mesma ordem bancária aparece em várias linhas); o total considera **cada OB uma única vez**. Por isso o ledger difere um pouco do Pago orçamentário — são visões distintas.
- **Painéis do exercício corrente** — Pré-Empenho, Supressão e Consulta de NE usam o exercício presente nessas abas da planilha.

---

## Estrutura

```
.
├── index.html    # o painel completo (dados + bibliotecas + fontes embutidos)
├── .nojekyll     # desativa o processamento Jekyll no Pages
├── deploy.sh     # publicação automática via GitHub CLI
└── README.md     # este arquivo
```
