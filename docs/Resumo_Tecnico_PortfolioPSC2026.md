# Resumo Técnico — Plataforma de Portfólio PSC 2026
**Última atualização:** Março 2026

---

## O que é

Plataforma web para apresentação visual dos projetos culturais do Polo Sociocultural Sesc Paraty. Composta por dois arquivos HTML standalone que carregam dados de CSVs hospedados no GitHub Pages.

Funciona como vitrine institucional, repositório de projetos e ferramenta de difusão cultural.

---

## Arquivos

| Arquivo | Função | Tamanho |
|---|---|---|
| `Portfolio_PSC2026.html` | Landing page com grid de 36 projetos | ~60KB |
| `projeto.html` | Template de página individual (renderiza qualquer projeto via slug) | ~40KB |

Ambos são standalone (sem servidor), mas dependem de `fetch` para carregar dados do GitHub Pages.

---

## Fontes de dados externas

| URL | Conteúdo | Usado por |
|---|---|---|
| `antoniogarciacouto.github.io/psc2026/data/portfolio_2026.csv` | Dados dos projetos | Ambos |
| `antoniogarciacouto.github.io/agendaPSC/PSC_Agenda2026.csv` | Agenda de atividades | `projeto.html` |
| `antoniogarciacouto.github.io/psc2026/imagens/{slug}/capa.jpg` | Imagens de capa | Ambos |
| `antoniogarciacouto.github.io/psc2026/imagens/{slug}/{slug}_XX.jpg` | Imagens de galeria | `projeto.html` |
| `antoniogarciacouto.github.io/psc2026/imagens/institucional/logo.png` | Logomarca | `Portfolio_PSC2026.html` |

---

## Portfolio_PSC2026.html — Detalhes

### Header
- Logo do PSC (JPEG com fundo preto, tratada com `filter: brightness(0) invert(1)`)
- "Programação 2026" como subtítulo na linha das métricas
- Métricas: número de projetos (dinâmico) + "250 mil" previsão de atendimento (fixo)
- Links para Instagram, YouTube, Facebook com ícones SVG

### Filtros (barra sticky com dropdowns)
- **Portfólio:** Todos, Difusão, Formação, Experimentação, Pesquisa
- **Área:** Todas + áreas culturais dinâmicas do CSV
- **Unidade:** Todas, Caborê, Santa Rita
- **Busca:** campo de texto com busca em nome, descrição, áreas e portfólios
- Filtros são combinados (AND): Formação + Música mostra só projetos que são Formação E Música
- Dropdown ativo muda de cor (fundo laranja claro, borda accent)

### Cards de projeto
- Imagem de capa com fallback para placeholder colorido por área (gradiente + ícone + padrão SVG)
- Badges: área cultural (canto inferior esquerdo) e unidade (canto superior direito)
- Corpo: título, descrição (3 linhas), meta de pessoas + tags de portfólio com dots coloridos
- Clique navega para `projeto.html?id={slug}`
- Hover: elevação + zoom na imagem

### Carregamento de dados
1. Tenta `fetch` do CSV no GitHub Pages
2. Parseia com auto-detecção de separador (`,` ou `;`) e remoção de BOM
3. Converte cada linha em objeto de projeto com campos normalizados
4. Se o fetch falhar, usa dados embarcados como fallback (JSON inline no HTML)

### Cores por portfólio
| Portfólio | Cor |
|---|---|
| Difusão | `#c2420c` |
| Formação | `#0891b2` |
| Experimentação | `#7c3aed` |
| Pesquisa | `#15803d` |

### Cores por área cultural
| Área | Cor | Ícone |
|---|---|---|
| Artes Cênicas | `#e85d3a` | 🎭 |
| Artes Visuais | `#7c3aed` | 🎨 |
| Música | `#0891b2` | 🎵 |
| Literatura | `#b45309` | 📖 |
| Memória Social e Patrimônio Cultural | `#15803d` | 🏛️ |
| Audiovisual | `#4338ca` | 🎬 |
| Arte Educação | `#c026d3` | ✨ |
| Desenvolvimento Físico-Esportivo | `#dc2626` | 🏃 |
| Educação Ampliada | `#0d9488` | 🔧 |
| Trabalho Social com Grupos | `#ea580c` | 🤝 |
| Turismo Social | `#059669` | 🗺️ |

---

## projeto.html — Detalhes

### Navegação
- URL: `projeto.html?id={slug}` (ex: `?id=levantes-daqui`)
- Back nav: botão fixo no canto superior esquerdo com logo + "Projetos"
- Footer: botão "Ver todos os projetos" + logo em opacidade reduzida

### Blocos modulares (renderizados condicionalmente)
1. **Hero** — capa fullscreen (85vh) com gradient overlay, tags de área, título e unidade
2. **Sobre o Projeto** — campo `descricao` do CSV
3. **Objetivo** — campo `objetivo` do CSV
4. **Dimensões** — cards de pessoas/serviços/orçamento + tags de tipos de serviço
5. **Agenda 2026** — timeline com atividades matched do CSV da agenda (async)
6. **Galeria** — auto-discovery de imagens `_01.jpg` a `_30.jpg` (async)
7. **Vídeo** — embed YouTube do campo `video` do CSV
8. **Extras** — título + texto + links do campos `extra_*` do CSV
9. **Footer** — navegação de volta

### Agenda — Mapeamento por keywords
O mapa `AGENDA_MATCH` associa cada slug a termos buscados nos títulos da agenda:
```javascript
'levantes-daqui': ['Levantes Daqui'],
'clube-sesc-de-leitores': ['Clube Sesc de Leitores'],
'minha-musica': ['Minha Música', 'Audições Minha Música'],
// ...
```
Atividades matched são exibidas com título, período, espaço, horário, status (cor-coded) e barra de timeline proporcional ao ano.

### Galeria — Auto-discovery
Em vez de depender de um campo no CSV, o template tenta carregar imagens sequencialmente:
```
imagens/{slug}/{slug}_01.jpg → existe? mostra
imagens/{slug}/{slug}_02.jpg → existe? mostra
imagens/{slug}/{slug}_03.jpg → não existe → para
```
Máximo: 30 imagens. Se nenhuma existir, bloco não aparece.

### Lightbox
- Clique na imagem abre lightbox fullscreen
- Navegação: setas laterais ou teclado (← → Esc)

---

## Colunas do CSV `portfolio_2026.csv`

| Coluna | Tipo | Obrigatória | Descrição |
|---|---|---|---|
| `slug` | texto | ✅ | Identificador URL e pasta de imagens |
| `nome` | texto | ✅ | Nome do projeto (como aparece no site) |
| `Portfolio` | JSON array | | Portfólios: `["Difusão","Formação"]` |
| `Peso Portfolio` | número | | Reservado para uso futuro |
| `area` | texto | ✅ | Área cultural principal |
| `areas_secundarias` | texto | | Áreas adicionais separadas por `;` |
| `unidade` | texto | ✅ | Caborê ou Santa Rita |
| `descricao` | texto | ✅ | Texto de apresentação |
| `objetivo` | texto | ✅ | Objetivo do projeto |
| `pessoas` | número | | Meta de pessoas atendidas |
| `servicos` | número | | Número de serviços planejados |
| `orcamento` | número | | Orçamento em R$ |
| `servicos_tipos` | texto | | Tipos separados por `;` |
| `video` | URL | | Link do YouTube |
| `galeria_qtd` | número | | Não usado (galeria é auto-discovery) |
| `extra_titulo` | texto | | Título do bloco extra |
| `extra_conteudo` | texto | | Texto do bloco extra |
| `extra_links` | texto | | Links formato `titulo\|url` separados por `;` |

---

## Identidade visual

### Landing page (Portfolio_PSC2026.html)
- **Background:** `#faf8f5` (creme claro)
- **Tipografia:** Instrument Serif (display) + DM Sans (corpo)
- **Acento:** `#c2420c` (terracota)
- **Cards:** fundo branco, sombra sutil, gradientes vibrantes por área

### Página de projeto (projeto.html)
- **Background:** `#09090b` (dark)
- **Tipografia:** Syne (display, ultra-bold) + Space Grotesk (corpo)
- **Acento:** `#ff5722` (laranja vibrante)
- **Noise texture** sutil sobre todo o fundo

---

## Observações para edição futura

- O `Portfolio_PSC2026.html` contém dados embarcados como fallback (JSON inline). Esses dados ficam desatualizados se o CSV mudar. O CSV sempre tem prioridade quando o fetch funciona.
- O mapa `AGENDA_MATCH` no `projeto.html` precisa ser atualizado manualmente quando novos projetos são adicionados ao portfólio e à agenda.
- O mapa `SLUG_MAP` no `Portfolio_PSC2026.html` é usado apenas como fallback quando os dados vêm dos embarcados (nomes antigos em uppercase). Quando o CSV carrega, os slugs vêm direto do CSV.
- A logo é JPEG com fundo preto (não PNG transparente). Requer filtro CSS para inversão.
- A coluna `galeria_qtd` no CSV não é mais usada (a galeria usa auto-discovery). Pode ser removida do Lists se desejado.
