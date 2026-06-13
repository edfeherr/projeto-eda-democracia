# Percepção dos Brasileiros Acerca da Democracia
### Uma análise exploratória correlacionando opinião declarada (CESOP) e comportamento eleitoral observado (TSE 2022)

**Disciplina:** Ciência de Dados — Projeto I (EDA, *Exploratory Data Analysis*)
**Instituição:** Instituto Mauá de Tecnologia (IMT)
**Tema:** Percepção dos Brasileiros Acerca da Democracia
**Bases:** CESOP/04832 (pesquisa de opinião) × TSE (perfil de comparecimento e abstenção, Eleições Gerais de 2022)
**Ano:** 2026

---

## Resumo

Este trabalho investiga a percepção dos brasileiros sobre democracia e participação
política, correlacionando uma **pesquisa de opinião** do Centro de Estudos de Opinião
Pública (CESOP, estudo 04832, n = 2.000 respondentes) com uma **base pública de
comportamento eleitoral** do Tribunal Superior Eleitoral (TSE) — o perfil de
comparecimento e abstenção das Eleições Gerais de 2022 (≈ 8,8 milhões de registros;
311,5 milhões de eleitores aptos). O pipeline de dados foi construído em Python
(pandas, NumPy, pyreadstat) e as análises empregaram visualizações estatísticas
(barras empilhadas, *heatmaps*, dispersão, linhas de eixo duplo) e a correlação de
Spearman. O principal achado é que **a escolaridade — e não a região — é o eixo que
organiza o engajamento político**: é a única dimensão em que percepção declarada e
comportamento real se **alinham perfeitamente** (ρ = 1,00), enquanto, por região, as
duas dimensões chegam a se **opor** (ρ = −0,30). Observa-se, ainda, um quadro consistente
de **baixo engajamento participativo declarado** (78% sem vontade de participar da
política local), convergente em três perguntas independentes da pesquisa.

---

## 1. Introdução

### 1.1 Contexto e motivação

A democracia brasileira combina **voto obrigatório** (para a maioria dos cidadãos) com
níveis variados de engajamento político voluntário. Essa combinação levanta uma questão
central: **o ato de votar — muitas vezes uma obrigação legal — reflete uma disposição
genuína de participar da vida política?** Pesquisas de opinião capturam a *percepção e a
disposição declaradas*; registros eleitorais capturam o *comportamento efetivo*. Analisar
as duas dimensões em conjunto permite verificar se elas convergem ou divergem.

### 1.2 Problema de pesquisa

O projeto parte da seguinte pergunta norteadora:

> **Grupos com maior participação eleitoral real também demonstram maior disposição
> declarada para participar da política? Em que dimensões (região, escolaridade, idade)
> percepção e comportamento se alinham — ou divergem?**

### 1.3 Objetivos

**Objetivo geral.** Realizar uma análise exploratória da percepção dos brasileiros sobre
democracia e participação política, correlacionando-a com o comportamento eleitoral
observado em 2022.

**Objetivos específicos:**

1. Estruturar e limpar a base de opinião (CESOP) e a base eleitoral (TSE), tornando-as
   comparáveis.
2. Caracterizar a amostra e descrever as percepções declaradas (lembrança de voto,
   prioridades políticas, combate a *fake news* e vontade de participação).
3. Descrever o comparecimento e a abstenção eleitoral de 2022 por região, idade e
   escolaridade.
4. **Cruzar** percepção declarada (CESOP) e comportamento real (TSE) por grupos
   sociodemográficos, identificando padrões de convergência e divergência.

### 1.4 Perguntas analíticas derivadas

| # | Pergunta | Seção |
|---|----------|-------|
| Q1 | Qual o perfil da amostra e o que os brasileiros declaram lembrar/priorizar/desejar? | 3.1–3.5 |
| Q2 | Como variam comparecimento e abstenção por região, idade e escolaridade? | 3.6 |
| Q3 | Disposição declarada e comparecimento real andam juntos, por região? | 3.7 |
| Q4 | E por escolaridade? E por faixa etária? | 3.7 |
| Q5 | A amostra de opinião é representativa do eleitorado? | 3.7 |

---

## 2. Materiais e Métodos

### 2.1 Fontes de dados

#### 2.1.1 Base principal — CESOP/04832 (pesquisa de opinião)

- **Origem:** Centro de Estudos de Opinião Pública (CESOP/UNICAMP), estudo **04832**.
- **Formato:** arquivo SPSS (`.SAV`), lido com a biblioteca `pyreadstat`, que preserva os
  rótulos de variáveis e os mapeamentos código→categoria embutidos.
- **Dimensão:** **2.000 respondentes × 30 variáveis**.
- **Blocos de variáveis utilizados:**

| Variável | Descrição |
|----------|-----------|
| `SEXO`, `FX_ID`, `ESCOLARIDADE`, `RACA`, `RELIGIAO` | Perfil demográfico |
| `REND1`, `REND2` | Renda pessoal e familiar (em salários mínimos) |
| `REGIAO`, `COND`, `PORTE` | Localização e porte do município |
| `P_01A`, `P_01B`, `P_01C` | Lembrança do voto em 2022 (dep. estadual, federal, senador) |
| `P_02_1`, `P_02_2`, `P_02_3` | Prioridades políticas (1ª, 2ª e 3ª escolhas) |
| `P_03_1` … `P_03_6` | Medidas consideradas úteis contra *fake news* (múltipla) |
| `P_04` | Vontade de participar da vida política local |

#### 2.1.2 Base complementar — TSE (comportamento eleitoral)

- **Origem:** Tribunal Superior Eleitoral — *Perfil do Eleitorado: Comparecimento e
  Abstenção*, **Eleições Gerais de 2022**, arquivo nacional (`...BRASIL.csv`).
- **Formato:** CSV (~2,3 GB), separador `;`, encoding `latin1`.
- **Dimensão após limpeza:** **8.785.738 linhas-detalhe** (combinações de UF × município ×
  zona × perfil demográfico).
- **Totais nacionais (2022):** 311.513.866 aptos; 247.332.217 comparecimentos;
  64.181.649 abstenções.
- **Variáveis utilizadas:** `SG_UF`, `NM_MUNICIPIO`, `DS_GENERO`, `DS_ESTADO_CIVIL`,
  `DS_FAIXA_ETARIA`, `DS_GRAU_ESCOLARIDADE`, `DS_COR_RACA`, `QT_APTOS`,
  `QT_COMPARECIMENTO`, `QT_ABSTENCAO`.

> **Conexão conceitual entre as bases:** a CESOP mede *opinião e disposição declarada*; o
> TSE mede *comportamento eleitoral real*. A ponte entre elas é feita por dimensões comuns
> (região, escolaridade, faixa etária), permitindo uma **comparação ecológica** (entre
> grupos agregados, não entre indivíduos).

### 2.2 Ferramentas e ambiente

- **Linguagem:** Python 3.
- **Bibliotecas:** `pandas` e `numpy` (manipulação), `pyreadstat` (leitura de `.SAV`),
  `matplotlib` e `seaborn` (visualização).
- **Formato intermediário:** **Parquet**, que preserva tipos categóricos e reduz o tamanho
  em disco.
- **Reprodutibilidade:** os notebooks detectam automaticamente o ambiente
  (**Google Colab** × local) e ajustam os caminhos — atendendo ao requisito de execução
  *100% no Colab*.

### 2.3 Pipeline de preparação dos dados (Notebook 01)

O primeiro notebook não contém análise: dedica-se inteiramente à **preparação reprodutível**.

**CESOP:**
1. **Carregamento** do `.SAV` com `pyreadstat` (dados + metadados).
2. **Padronização de nomes** das variáveis de pergunta (`P1A` → `P_01A`) e conversão de
   `DATA_ENTREVISTA` para `datetime`.
3. **Tratamento de não-resposta:** o código `99` ("Não sabe/Não respondeu") é convertido em
   `NaN`. A substituição é feita **por coluna** (não globalmente), de modo a **preservar** o
   código `98` de `REND1`/`REND2` ("Não tem rendimento"), que é uma **categoria válida**.
4. **Rotulagem:** aplicação dos *value labels* e conversão para o tipo `category`.
5. **Variáveis derivadas:** agregação dos 16 níveis de `ESCOLARIDADE` em **5 grupos
   ordenados** (`ESCOL_GRUPO`) e das faixas de renda em **5 faixas** (`RENDA_PESSOAL`,
   `RENDA_FAMILIAR`). Inclui-se uma **validação de cobertura** que detecta rótulos não
   mapeados (evita perdas silenciosas por divergência de grafia).

**TSE:**
1. **Carregamento otimizado:** leitura **apenas das colunas necessárias** (`usecols`) —
   essencial para caber na memória do Colab dado o volume de 2,3 GB.
2. **Limpeza:** remoção de combinações com `QT_APTOS = 0` (não contribuem para taxas),
   normalização de strings e conversão para `category`.
3. **Variáveis derivadas:** `TAXA_COMPARECIMENTO` e `TAXA_ABSTENCAO` (proporções) e
   `REGIAO` (derivada da UF pela categorização do IBGE).
4. **Agregações:** dois recortes — por **UF** (`df_tse_uf`) e por **perfil demográfico
   nacional** (`df_tse_perfil`). As taxas agregadas usam a **soma** das contagens (média
   ponderada por eleitor), e não a média das taxas-linha — evitando que zonas pequenas
   distorçam o indicador.
5. **Validação:** `assert` garante que todas as taxas estão no intervalo [0, 1] antes do
   salvamento.

**Saídas:** `cesop_clean.parquet`, `tse_clean.parquet`, `tse_uf.parquet`,
`tse_perfil.parquet` e `cesop_labels.json`.

### 2.4 Harmonização entre as bases

Como CESOP e TSE adotam convenções diferentes, foram criadas tabelas de correspondência
(Notebook 02):

- **Região:** unificação de grafias (ex.: `NORDESTE` → `Nordeste`).
- **Escolaridade:** os 8 níveis do TSE são mapeados para os 5 grupos do CESOP.
- **Faixa etária:** as idades do TSE são *bucketizadas* nas mesmas faixas do CESOP.

### 2.5 Técnicas estatísticas e de visualização

- **Distribuições univariadas:** frequências e percentuais, com `n` explícito.
- **Tabelas cruzadas** (`crosstab`) normalizadas por linha (composição dentro de cada
  grupo).
- **Visualizações:** barras horizontais, **barras empilhadas 100%**, **gráficos de pizza**,
  **heatmaps**, **dispersão (scatter)** e **linhas com eixo duplo (`twinx`)**.
- **Correlação de Spearman (ρ):** medida de associação **monotônica** entre os agregados,
  apropriada para poucos pontos e relações não necessariamente lineares. Usada nos
  cruzamentos CESOP × TSE.
- **Cuidados metodológicos:** denominadores corretos em perguntas de múltipla escolha
  (percentuais sobre o **total de menções**, não de respondentes) e ordenação semântica
  das categorias ordinais.

---

## 3. Resultados e Discussão

> Todos os percentuais e coeficientes a seguir foram obtidos diretamente das análises dos
> notebooks. Valores precedidos de "≈" foram lidos das figuras geradas.

### 3.1 Perfil da amostra (CESOP, n = 2.000)

- **Sexo:** 51,7% feminino, 48,3% masculino.
- **Faixa etária:** concentração em adultos — 25–34 anos (22,9%) e 35–44 anos (21,2%);
  jovens de 16–17 anos representam apenas 1,1%.
- **Raça/cor:** parda (46,0%) e branca (42,3%) somam quase 90%; preta (10,4%), amarela
  (0,9%), indígena (0,4%).
- **Escolaridade:** ensino médio predomina (40,8%); baixa escolaridade é rara na amostra
  (3,2%) — ponto retomado na análise de representatividade (3.7).

### 3.2 Lembrança do voto em 2022 (P_01A/B/C)

| Cargo | Lembra ("Sim") | Não lembra | Não votou |
|-------|:---:|:---:|:---:|
| Deputado(a) estadual | 29,8% | 64,6% | 5,5% |
| Deputado(a) federal | 29,4% | 65,0% | 5,6% |
| Senador(a) | 29,6% | 64,9% | 5,5% |

**Achado.** A lembrança é **praticamente idêntica entre os três cargos** (≈ 30%): a maioria
(≈ 65%) **não recorda em quem votou** para o Legislativo em 2022. Não há o gradiente
"estadual < federal < senador" que se poderia supor. Nos recortes sociodemográficos, a
lembrança tende a ser **maior entre os mais escolarizados** e nas faixas adultas.

### 3.3 Prioridades políticas (P_02)

Na **primeira escolha**, os temas mais citados foram:

| Prioridade | % (1ª escolha) | n |
|------------|:---:|:---:|
| Melhorar saúde | 20,0% | 400 |
| Gerar empregos | 14,7% | 294 |
| Reduzir desigualdades | 14,3% | 286 |
| Reduzir violência | 12,5% | 250 |
| Combater preconceitos | 11,7% | 233 |
| Melhorar educação | 11,1% | 222 |
| Preservar valores familiares | 5,1% | 103 |
| Taxar grandes fortunas | 2,2% | 45 |
| **Ampliar participação política** | **2,0%** | 40 |
| Defender igualdade de gênero | 1,9% | 38 |

**Achado.** Predominam pautas de **serviços e bem-estar** (os seis primeiros temas somam
≈ 84%). Em contraste, **"ampliar a participação política" é a penúltima prioridade
(2,0%)** — primeiro indício do desengajamento processual confirmado na seção 3.5.

### 3.4 Combate às *fake news* (P_03)

Pergunta de múltipla escolha (1 a 6 medidas por respondente). Agrupando as medidas por
tipo de ação (percentual sobre o total de menções):

- **Punição / responsabilização: 66,9%** (2.077 menções)
- **Regulamentação: 33,1%** (1.028 menções)

**Achado.** Predomina, por dois para um, a preferência por **sancionar** em vez de
**normatizar** — uma inclinação por respostas diretas/punitivas.

### 3.5 Vontade de participar da política local (P_04)

| Nível | % | n |
|-------|:---:|:---:|
| Nenhuma vontade | **78,0%** | 1.559 |
| Alguma vontade | 13,2% | 264 |
| Muita vontade | 8,5% | 169 |

**Achado.** O desengajamento declarado é expressivo: **78% não têm nenhuma vontade** de
participar da política local; a disposição total ("alguma" + "muita") é de apenas ≈ 21,7%.
Esse resultado reforça a baixa prioridade dada à participação (3.3).

### 3.6 Panorama eleitoral — TSE 2022

- **Comparecimento nacional: 79,40%** (abstenção: 20,60%).
- **Por região:** Sul (≈ 81,0%) e Nordeste (≈ 80,6%) lideram; Norte (≈ 78,1%) e Sudeste
  (≈ 78,4%) ficam abaixo da média. **Amplitude pequena: ≈ 3 pontos percentuais.**
- **Por escolaridade:** gradiente **forte e crescente** — de ≈ 62% (baixa escolaridade) a
  ≈ 87% (superior completo). **Amplitude grande: ≈ 25 pontos percentuais.**
- **Por idade:** formato de sino, com pico em 45–64 anos (≈ 87–88%) e quedas nas pontas —
  efeito do **voto facultativo**: 65+ comparecem ≈ 57% e jovens de 16–17, ≈ 83%.

**Achado.** A escolaridade discrimina o comparecimento **muito mais** que a região (25 p.p.
contra 3 p.p.) — pista de que a escolaridade, não a geografia, é o fator estruturante.

### 3.7 Cruzamento CESOP × TSE (núcleo do projeto)

#### 3.7.1 Por região — *percepção e comportamento divergem* (ρ = −0,30)

| Região | Comparecimento (TSE) | Disposição (CESOP) |
|--------|:---:|:---:|
| Sul | ≈ 81,0% | 18,9% |
| Nordeste | ≈ 80,6% | 19,8% |
| Centro-Oeste | ≈ 79,0% | 25,0% |
| Sudeste | ≈ 78,4% | 23,7% |
| Norte | ≈ 78,1% | 19,4% |

A correlação de Spearman é **negativa (ρ = −0,30)**. **Sul e Nordeste votam mais, mas
declaram menos vontade de participar**; Centro-Oeste e Sudeste fazem o inverso. Ou seja,
**por região, alto comparecimento não significa alta disposição** — chegam a se opor.

#### 3.7.2 Por escolaridade — *percepção e comportamento se alinham* (ρ = 1,00)

| Escolaridade | Comparecimento (TSE) | Disposição (CESOP) |
|--------------|:---:|:---:|
| Baixa escolaridade | ≈ 62% | ≈ 11% |
| Ensino fundamental | ≈ 77% | ≈ 16% |
| Ensino médio | ≈ 83% | ≈ 21% |
| Superior incompleto | ≈ 84% | ≈ 28% |
| Superior completo | ≈ 87% | ≈ 34% |

A associação é **perfeita e positiva (ρ = 1,00)**: a cada degrau de escolaridade sobem
**juntos** o comparecimento real e a disposição declarada. **Esta é a dimensão em que as
duas bases concordam plenamente.**

#### 3.7.3 Por faixa etária — convergência no miolo, divergência nas pontas

No intervalo de **25 a 54 anos**, comparecimento (TSE) e disposição (CESOP) sobem juntos,
com pico em 45–54. Nas extremidades, divergem: os jovens de **16–17 comparecem (≈ 83%) mas
têm disposição mínima (≈ 9%)** — exercem o voto facultativo sem engajamento declarado
correspondente; aos **65+**, ambos caem.

#### 3.7.4 Abstenção × desinteresse, por região (espelho de 3.7.1)

O padrão de divergência se confirma pelo lado negativo: o **Sul** tem a **menor abstenção**
(≈ 19%) mas o **maior desinteresse declarado** (≈ 81,5% de "nenhuma vontade").

#### 3.7.5 Representatividade da amostra

| Escolaridade | CESOP (amostra) | TSE (eleitorado) |
|--------------|:---:|:---:|
| Baixa escolaridade | 3,2% | 11,2% |
| Ensino fundamental | 31,7% | 29,5% |
| Ensino médio | 40,8% | 43,0% |
| Superior incompleto | 9,8% | 5,4% |
| Superior completo | 14,3% | 10,9% |

A amostra **sub-representa os menos escolarizados** e **sobre-representa o ensino superior**
(24,1% contra 16,3% no eleitorado). Como a disposição cresce com a escolaridade (3.7.2),
isso implica que a amostra provavelmente **superestima** o engajamento — o desinteresse
real da população tende a ser **ainda maior** que os 78% observados.

### 3.8 Padrões transversais (síntese analítica)

1. **A escolaridade é o eixo estruturante e o único alinhador.** É a dimensão em que
   percepção (CESOP) e comportamento (TSE) concordam perfeitamente (ρ = 1,00), e a que mais
   discrimina o comparecimento (≈ 25 p.p.).
2. **A geografia dissocia as duas dimensões** (ρ = −0,30): votar muito não implica querer
   participar — o "paradoxo do Sul".
3. **Desengajamento participativo convergente** em três perguntas independentes: 78% sem
   vontade (P_04), "ampliar participação" como penúltima prioridade (2%, P_02) e ≈ 65% sem
   lembrança do voto legislativo (P_01).
4. **O paradoxo dos jovens de 16–17:** comparecem bastante, mas têm a menor disposição
   declarada.

---

## 4. Limitações

- **Comparação ecológica.** CESOP e TSE **não medem os mesmos indivíduos**; as associações
  são entre **agregados** (regiões, faixas, grupos) e **não autorizam conclusões sobre
  indivíduos** (risco de *falácia ecológica*).
- **Correlação ≠ causalidade.** Os coeficientes de Spearman descrevem **associação e
  direção**, não relações de causa e efeito.
- **Natureza amostral e viés de escolaridade.** A CESOP é uma amostra de opinião
  (≈ 2.000 respondentes) que sobre-representa os mais escolarizados; leituras de magnitude
  devem ser conservadoras.
- **Recortes com baixo `n`.** Categorias pouco frequentes (ex.: alguns grupos de raça/cor)
  devem ser interpretadas com cautela.

---

## 5. Conclusão

A análise sustenta uma tese central: **no Brasil, é a escolaridade — e não a região — que
organiza o engajamento político**, sendo a única dimensão em que **percepção declarada
(CESOP) e comportamento eleitoral real (TSE) se alinham** (ρ = 1,00). Geograficamente, as
duas dimensões chegam a se **opor** (ρ = −0,30): alto comparecimento não significa alta
disposição para participar, como ilustra o caso do Sul. Soma-se a isso um quadro
consistente de **baixo engajamento participativo declarado**, manifesto de forma
convergente em três perguntas distintas da pesquisa.

Do ponto de vista do tema — *Percepção dos Brasileiros Acerca da Democracia* — o estudo
mostra um eleitorado que **comparece às urnas** (79,4% em 2022), mas que, em sua maioria,
**não deseja participar ativamente** da política local e **não recorda** seu voto
legislativo. A participação eleitoral, sustentada em parte pela obrigatoriedade, **não se
traduz** automaticamente em engajamento cívico declarado — e essa tradução depende
fortemente da escolaridade. O trabalho cumpre o objetivo de **correlacionar uma pesquisa de
opinião com uma base pública** (TSE 2022), respeitando os limites de uma comparação
ecológica.

---

## 6. Reprodutibilidade

1. Os dados brutos (CESOP `.SAV` e TSE `.csv`) devem ser colocados em `data/raw/` conforme
   o `data/README.md` (o arquivo do TSE, por ser grande, é distribuído via Google Drive).
2. Executar **`notebooks/01_exploracao_base_parte_a.ipynb`** para gerar os arquivos tratados
   em `data/processed/`.
3. Executar **`notebooks/02_analises_exploratorias_parte_a.ipynb`** para reproduzir as
   análises e as figuras (salvas em `reports/imagens/`).
4. Ambos os notebooks são **100% executáveis no Google Colab** (detecção automática de
   ambiente).

---

## 7. Referências

- CESOP — Centro de Estudos de Opinião Pública (UNICAMP). *Banco de dados do estudo 04832*.
- TSE — Tribunal Superior Eleitoral. *Perfil do Eleitorado — Comparecimento e Abstenção,
  Eleições Gerais de 2022*. Repositório de Dados Eleitorais.
- IBGE — Instituto Brasileiro de Geografia e Estatística. *Divisão regional do Brasil*
  (categorização das Grandes Regiões).
- McKinney, W. *pandas: powerful Python data analysis toolkit*.
- Hunter, J. D. *Matplotlib: A 2D Graphics Environment*; Waskom, M. *seaborn*.

---

## 8. Declaração de uso de Inteligência Artificial

Em conformidade com as diretrizes do projeto, declara-se o uso de **assistência de IA**
(modelo de linguagem, via Claude Code) nas seguintes etapas: estruturação e revisão do
código dos notebooks, padronização das visualizações, redação e organização deste
relatório e verificação de consistência entre os achados e as figuras geradas. **Todas as
decisões analíticas, a seleção das bases, a interpretação dos resultados e a validação
final foram conduzidas e revisadas pelos autores.** Os valores numéricos relatados foram
extraídos das análises e figuras produzidas pelos próprios notebooks do projeto.

---

*Documento gerado para o Projeto I — EDA da disciplina de Ciência de Dados.*
