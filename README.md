# Desafio Técnico - Estágio em Ciência de Dados | FGV IBRE

## Sobre o projeto

Este projeto foi desenvolvido como solução para o desafio técnico do processo seletivo de Estágio em Ciência de Dados do FGV IBRE.

A proposta do desafio foi construir um mini motor de busca semântico aplicado a notícias econômicas brasileiras, com foco em demonstrar habilidades de tratamento de dados textuais, uso de embeddings e recuperação de informação baseada em similaridade semântica.

A solução foi organizada em três etapas principais:

- limpeza e tratamento dos textos brutos
- geração de embeddings semânticos
- implementação de um mecanismo de busca textual baseado em similaridade

---

## Estrutura do projeto

```text
noticias_brutas.json
        ↓
┌──────────────────────────────────────┐
│ Etapa 1 — Limpeza e tratamento       │
│ • remoção de HTML                    │
│ • normalização textual               │
│ • remoção de ruídos/metadados        │
└──────────────────────────────────────┘
        ↓
dados limpos
        ↓
┌──────────────────────────────────────┐
│ Etapa 2 — Geração de embeddings      │
│ • sentence-transformers              │
│ • representação semântica vetorial   │
└──────────────────────────────────────┘
        ↓
embeddings
        ↓
┌──────────────────────────────────────┐
│ Etapa 3 — Busca semântica            │
│ • cosine similarity                  │
│ • ranking por relevância             │
└──────────────────────────────────────┘
```
---

## Como executar

### 1. Clone o repositório

```bash
git clone https://github.com/thiagovicenterj21/desafio-ciencia-de-dados-ibre.git
```

### 2. Entre na pasta do projeto

```bash
cd desafio-ciencia-de-dados-ibre
```

### 3. Instale as dependências

```bash
pip install -r requirements.txt
```

### 4. Execute o notebook

Abra o arquivo:

```text
desafio_ibre_busca_semantica.ipynb
```

O notebook contém todo o pipeline do projeto, desde a leitura dos dados até a execução das consultas de busca semântica.

---

## Etapa 1 — Limpeza dos dados

O primeiro passo foi tratar os textos brutos das notícias, que continham diversos ruídos intencionais incluídos no desafio.

Entre os tratamentos realizados:

- remoção de tags HTML
- conversão de entidades HTML (`&amp;`, `&eacute;`, etc.)
- remoção de links
- remoção de timestamps e metadados incorporados ao texto
- normalização de espaços em branco
- identificação de casos extremos, como notícias com conteúdo muito curto

O resultado desse processamento foi salvo no arquivo:

```text
dados/noticias_limpas.csv
```

Essa etapa foi importante para garantir que os embeddings fossem gerados a partir de textos mais consistentes e semanticamente úteis.

---

## Etapa 2 — Geração de embeddings

Para representar semanticamente os textos, utilizei o modelo:

```python
paraphrase-multilingual-MiniLM-L12-v2
```

da biblioteca `sentence-transformers`.

Escolhi esse modelo por alguns motivos:

- possui suporte multilíngue, incluindo português
- funciona bem para tarefas de similaridade semântica
- é relativamente leve e fácil de reproduzir localmente

Como o desafio utiliza um corpus pequeno, optei por uma solução simples e eficiente, sem adicionar complexidade desnecessária.

Também decidi combinar **título + corpo da notícia** na geração dos embeddings, já que os títulos carregam bastante informação relevante para a busca.

---

## Etapa 3 — Busca semântica

A etapa final consistiu na implementação de um mecanismo de busca baseado em similaridade de cosseno.

Fluxo utilizado:

1. converter cada notícia em embedding vetorial
2. converter a consulta do usuário em embedding
3. calcular similaridade entre consulta e notícias
4. ordenar os resultados por relevância

Consultas utilizadas para validação:

- "mudanças na taxa de juros"
- "mercado de trabalho e desemprego"
- "inflação e preços ao consumidor"

---

## Resultados

De forma geral, os resultados foram satisfatórios.

### Mudanças na taxa de juros

A busca retornou corretamente notícias relacionadas ao Copom, Selic e expectativas para política monetária, mostrando boa aderência ao tema consultado.

---

### Mercado de trabalho e desemprego

Os resultados incluíram notícias sobre taxa de desemprego, desemprego juvenil e indicadores ligados ao mercado de trabalho, com boa coerência semântica.

---

### Inflação e preços ao consumidor

Os resultados incluíram documentos relacionados ao tema inflacionário, como notícias sobre inflação ao produtor.

Em alguns casos, também apareceram conteúdos tangencialmente relacionados, como câmbio e expectativas macroeconômicas.

Esse comportamento é esperado considerando:

- o tamanho reduzido do corpus
- o uso de um modelo generalista
- a proximidade semântica entre temas macroeconômicos

Mesmo assim, a abordagem se mostrou funcional para o objetivo proposto.

---

## Por fim

O projeto atende aos requisitos do desafio e demonstra uma abordagem completa para um problema simples de busca semântica, passando por limpeza textual, representação vetorial e recuperação de informação.

Foi uma experiência interessante para aplicar conceitos de NLP de forma prática em um contexto econômico.
