# Plataforma de Projetos — Polo Sociocultural Sesc Paraty

Este projeto tem como objetivo construir uma plataforma digital para dar visibilidade aos projetos culturais do Polo Sociocultural Sesc Paraty (PSC).

A plataforma funciona como vitrine institucional, repositório de projetos, memória programática e ferramenta de difusão cultural.

Faz parte de um ecossistema maior de ferramentas digitais de gestão do PSC (painéis orçamentários, agenda, perfil de público, etc.), documentado em `Contexto_EcossistemaPSC2026.md`.

---

## Sobre o PSC

O Polo Sociocultural Sesc Paraty é uma unidade do Departamento Nacional do Sesc dedicada ao desenvolvimento, experimentação e difusão de projetos culturais em Paraty, RJ.

Opera em duas unidades programáticas (Caborê e Santa Rita) e uma base administrativa.

---

## Arquivos da plataforma

| Arquivo | Função |
|---|---|
| `Portfolio_PSC2026.html` | Landing page com grid de projetos, filtros e busca |
| `projeto.html` | Template de página individual de projeto (renderiza por slug via URL) |

Ambos são HTML standalone que carregam dados do GitHub Pages via fetch.

---

## Fonte de dados

Os projetos estão no arquivo `data/portfolio_2026.csv`, exportado do Microsoft Lists.

A agenda de atividades está em `PSC_Agenda2026.csv` no repositório `agendaPSC`.

---

## Estrutura do repositório

```
psc2026/
├── data/
│   └── portfolio_2026.csv          ← dados dos projetos (CSV do Lists)
├── imagens/
│   ├── institucional/
│   │   └── logo.png                ← logomarca do PSC
│   ├── arte-da-palavra/
│   │   ├── capa.jpg                ← imagem de capa (card do grid)
│   │   ├── arte-da-palavra_01.jpg  ← imagem de galeria
│   │   └── ...
│   ├── levantes-daqui/
│   │   ├── capa.jpg
│   │   └── ...
│   └── ...                         ← uma pasta por projeto
├── content/                        ← textos completos (reservado)
├── docs/                           ← documentação (reservado)
└── README.md
```

---

## Filtros da plataforma

Os projetos podem ser filtrados por:

- **Portfólio:** Difusão, Formação, Experimentação, Pesquisa
- **Área cultural:** Artes Cênicas, Artes Visuais, Música, Literatura, etc.
- **Unidade:** Caborê, Santa Rita
- **Busca por texto**

---

## Página individual de projeto

Cada projeto tem uma página acessada via `projeto.html?id={slug}` contendo:

- Hero com imagem de capa
- Descrição e objetivo
- Dimensões (pessoas, serviços, orçamento, tipos de serviço)
- Agenda 2026 (timeline de atividades, carregada do CSV da agenda)
- Galeria de imagens (auto-discovery)
- Vídeo do YouTube (opcional)
- Conteúdos extras (opcional)

---

## Convenções de imagem

- Capas: `imagens/{slug}/capa.jpg` — JPG, 1200×675px ideal
- Galeria: `imagens/{slug}/{slug}_01.jpg`, `_02.jpg`... — numeração sequencial
- Logo: `imagens/institucional/logo.png`

---

## Redes sociais

- Instagram: https://www.instagram.com/sescparaty/
- YouTube: https://www.youtube.com/@SescParaty/videos
- Facebook: https://www.facebook.com/sescparaty/?locale=pt_BR

---

## Linguagem visual

A landing page usa identidade clara e institucional (fundo creme, tipografia elegante).
A página de projeto usa identidade dark e contemporânea (fundo escuro, tipografia bold).
Ambas priorizam imagens grandes, navegação simples e leitura rápida.
