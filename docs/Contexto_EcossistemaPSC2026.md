# Contexto — Ecossistema de Ferramentas Digitais PSC 2026
**Última atualização:** Março 2026

---

## Visão geral

Conjunto de ferramentas digitais para o **Polo Sociocultural Sesc Paraty (PSC)**, cobrindo gestão interna, programação cultural e comunicação institucional. O ecossistema opera em duas camadas complementares:

1. **Painéis de Gestão** — ferramentas internas em HTML standalone para gestão orçamentária, operacional e de agenda
2. **Plataforma de Portfólio** — site institucional voltado a público externo e interno, apresentando os projetos culturais de forma visual e navegável

O PSC opera em três unidades: **Caborê**, **Santa Rita** e **Base PSC**.

O desenvolvimento é iterativo: Antonio (gestor do PSC) fornece feedback contínuo, e os arquivos são atualizados e entregues via outputs para download. Para editar um painel em nova sessão, o arquivo HTML atual deve ser enviado como upload, pois os arquivos do projeto são cópias somente leitura.

---

## Camada 1 — Painéis de Gestão (uso interno)

Arquivos HTML standalone (sem dependência de servidor), acessados localmente ou via SharePoint.

### `index.html`
**Hub central de navegação** — página de entrada do ecossistema
- Lista todos os painéis disponíveis e em construção
- Botão "← PSC" em cada painel retorna a esta página

### `Painel_PSC2026.html`
**Painel Gerencial** — leitura para diretores (DSCLA e DIOP)
- Orçamento total: R$ 33,4 milhões (programação + operação)
- 7 abas: Visão Geral, Metas de Atendimento, Custo por Atendimento, Por Projeto, Centro de Custos, Por Conta, Execução Financeira
- Dados embutidos como JSON
- Fontes: `data.json`, `detail.json`, `metas.json`, `Info_Projetos_2026_Integra.xlsx`
- Documentação detalhada: `Resumo_Tecnico_PainelPSC2026.md`

### `Painel_areasPSC2026.html`
**Painel Operacional por Área** — uso pelos coordenadores de área cultural
- 7 áreas culturais, 4 abas por área
- Registro por serviço individual, execução financeira editável
- Storage compartilhado via `window.storage` com `shared: true`

### `Agenda_PSC2026.html`
**Agenda de Programação** — gestão de espaços e atividades
- 7 modos de visualização
- Importação via CSV do Microsoft Lists (`PSC_Agenda2026.csv`)
- Repositório: `github.com/antoniogarciacouto/agendaPSC`
- Documentação detalhada: `Resumo_Tecnico_AgendaPSC2026.md`

### `Painel_Publicos_Oficinas.html`
**Perfil de Público** — dados demográficos dos participantes

### Painéis planejados
- `Execucao_PSC2026.html` — Execução orçamentária e de metas
- `Unidades_PSC2026.html` — Detalhes das unidades
- `Paraty_PSC2026.html` — Contexto territorial de Paraty
- `Relatorios_PSC2026.html` — Consolidação para relatórios periódicos

---

## Camada 2 — Plataforma de Portfólio (público externo e interno)

Site institucional para apresentação visual dos projetos culturais. Dois arquivos HTML que carregam dados de CSV hospedado no GitHub Pages.

### `Portfolio_PSC2026.html`
**Landing page — grid de projetos**
- 36 projetos culturais em cards com imagem de capa, descrição, público previsto e portfólio
- Filtros por dropdown: Portfólio (Difusão, Formação, Experimentação, Pesquisa), Área cultural, Unidade
- Busca por texto
- Header com logo institucional, métricas e links para redes sociais
- Carrega dados do CSV `data/portfolio_2026.csv` via GitHub Pages (fallback para dados embarcados)
- Imagens de capa: `imagens/{slug}/capa.jpg` via GitHub Pages
- Identidade visual: fundo claro (#faf8f5), tipografia Instrument Serif + DM Sans, cores vibrantes por área

### `projeto.html`
**Página individual de projeto** — template único, renderizado por slug via URL (`projeto.html?id={slug}`)
- Blocos modulares (só aparecem se o dado existir no CSV):
  1. Hero com capa.jpg
  2. Apresentação do projeto
  3. Objetivo
  4. Dimensões (cards de pessoas/serviços/orçamento + tipos de serviço)
  5. Agenda 2026 (timeline com atividades do CSV da agenda, auto-matched por keywords)
  6. Galeria de imagens (auto-discovery: tenta _01.jpg, _02.jpg... até falhar)
  7. Vídeo do YouTube (se campo `video` preenchido no CSV)
  8. Conteúdos extras opcionais
  9. Navegação de volta para a landing page
- Identidade visual: dark theme (#09090b), tipografia Syne + Space Grotesk, acento laranja (#ff5722)
- Carrega dados de dois CSVs:
  - `portfolio_2026.csv` (dados do projeto) via GitHub Pages do repo `psc2026`
  - `PSC_Agenda2026.csv` (dados da agenda) via GitHub Pages do repo `agendaPSC`

---

## Repositórios GitHub

### `antoniogarciacouto/psc2026`
Repositório principal do portfólio. GitHub Pages ativo.
- `data/portfolio_2026.csv` — dados dos projetos (exportado do Microsoft Lists)
- `imagens/{slug}/capa.jpg` — imagens de capa por projeto
- `imagens/{slug}/{slug}_01.jpg` ... — imagens de galeria por projeto
- `imagens/institucional/logo.png` — logomarca do PSC

Estrutura de pastas de imagens: uma pasta por projeto, nomeada pelo slug (ex: `levantes-daqui/`). Dentro: `capa.jpg` + imagens de apoio numeradas.

### `antoniogarciacouto/agendaPSC`
Repositório da agenda. GitHub Pages ativo.
- `PSC_Agenda2026.csv` — agenda de programação (exportado do Microsoft Lists)
- `index.html` — painel da Agenda PSC

---

## Fontes de dados

| Fonte | Formato | Usado por | Localização |
|---|---|---|---|
| `portfolio_2026.csv` | CSV (Lists) | Portfolio, projeto.html | GitHub `psc2026/data/` |
| `PSC_Agenda2026.csv` | CSV (Lists) | projeto.html, Agenda_PSC2026.html | GitHub `agendaPSC/` |
| `data.json` | JSON | Painel Gerencial | Arquivo do projeto Claude |
| `detail.json` | JSON | Painel Gerencial | Arquivo do projeto Claude |
| `metas.json` | JSON | Painel Gerencial | Arquivo do projeto Claude |
| `Info_Projetos_2026_Integra.xlsx` | Excel | Fonte primária de projetos | Arquivo do projeto Claude |

---

## Mapeamento Agenda ↔ Portfólio

O `projeto.html` cruza dados da agenda com o portfólio via mapa de keywords (`AGENDA_MATCH`). Cada slug do portfólio tem uma lista de termos que são buscados nos títulos da agenda. Se encontrar match, o bloco de timeline aparece na página do projeto. Se não encontrar, o bloco não aparece.

O mapa está embarcado no JS do `projeto.html`. Novos projetos na agenda precisam ter keywords adicionadas ao mapa para aparecerem na página individual.

---

## Padrões técnicos

### Painéis de gestão
- HTML standalone, JavaScript vanilla, sem framework
- Fontes: DM Sans + DM Mono (via Google Fonts)
- Tema escuro: `#0f0e0d` background, `#d4a843` acento dourado
- Storage: `window.storage` nativo do Claude artifacts
- Convenção: `NomePainel_PSC2026.html`

### Plataforma de portfólio
- HTML standalone, JavaScript vanilla, sem framework
- Dados carregados via `fetch` de CSV no GitHub Pages (fallback para dados embarcados)
- Imagens via GitHub Pages
- Parser CSV auto-detecta separador (`,` ou `;`) e lida com BOM UTF-8
- Landing page: tema claro, Instrument Serif + DM Sans
- Página de projeto: tema escuro, Syne + Space Grotesk
- Convenção: `Portfolio_PSC2026.html` + `projeto.html`

### Convenções de imagem
- Capas: JPG, 1200×675px (ideal) ou 800×450px (mínimo)
- Galeria: JPG, numeração `{slug}_01.jpg`, `_02.jpg`...
- Logo: `imagens/institucional/logo.png` (JPEG com fundo preto, requer `filter: brightness(0) invert(1)` em fundos escuros)

---

## Fluxo de atualização

### Para atualizar dados dos projetos:
1. Editar no Microsoft Lists
2. Exportar como CSV para `data/portfolio_2026.csv`
3. Subir no GitHub (`psc2026/data/`)
4. O site atualiza automaticamente (carrega CSV a cada acesso)

### Para atualizar agenda:
1. Editar no Microsoft Lists
2. Exportar como CSV para `PSC_Agenda2026.csv`
3. Subir no GitHub (`agendaPSC/`)
4. As timelines nos projetos atualizam automaticamente

### Para adicionar imagens:
1. Criar/usar pasta `imagens/{slug}/` no GitHub (`psc2026`)
2. Subir `capa.jpg` para o card do grid
3. Subir `{slug}_01.jpg`, `_02.jpg`... para a galeria
4. Galeria aparece automaticamente (auto-discovery)

### Para editar painéis de gestão:
1. Enviar o HTML atual como upload na conversa
2. Editar iterativamente
3. Baixar o output atualizado

---

## Contexto institucional

**Antonio** — Gestor do PSC, interlocutor principal
- Trabalha com DSCLA (conceitual/estratégico/programático) e DIOP (financeiro/RH/TI/infraestrutura)
- Filosofia: decisões baseadas em dados, integração entre planejamento e execução

**Portfólios programáticos:** Difusão, Formação, Experimentação, Pesquisa
- Cada projeto pertence a um ou mais portfólios
- Campo `Portfolio` no CSV (formato JSON array: `["Difusão","Formação"]`)

**Sistema INTEGRA** — fonte oficial de metas de atendimento

**Metodologia de rateio de custos indiretos:**
```
Custo c/ rateio = (orc_subat + (orc_subat / total_programacao) × total_indireto) / pessoas
total_indireto    = R$ 17.640.903
total_programacao = R$ 15.317.228
```
