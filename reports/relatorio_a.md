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
comparecimento e abstenção das Eleições Gerais de 2022 (8.785.738 linhas-detalhe;
311,5 milhões de eleitores aptos). O pipeline de dados foi construído em Python
(pandas, NumPy, pyreadstat) e as análises empregaram visualizações estatísticas
(barras empilhadas 100%, *heatmaps*, dispersão, linhas de eixo duplo) e a correlação de
Spearman entre agregados. O principal achado é que **a escolaridade — e não a região — é
o eixo que organiza o engajamento político**: é a única dimensão em que percepção
declarada e comportamento real se **alinham perfeitamente** (ρ = 1,00), enquanto, por
região, as duas dimensões chegam a se **opor** (ρ = −0,30). A **renda reproduz o mesmo
gradiente** de forma independente, e a **idade** funciona como um segundo eixo estrutural,
desenhando um **U-invertido** (engajamento concentrado na meia-idade, queda nas duas
pontas) que reaparece em três medidas distintas. Observa-se, ainda, um quadro consistente
de **baixo engajamento participativo declarado** (78% sem vontade de participar da
política local), convergente em três perguntas independentes da pesquisa e coerente com a
preferência, no caso das *fake news*, por **soluções diretas (punir) em vez de
processuais (regular)**.

---

## 1. Introdução

### 1.1 Contexto e motivação

A democracia brasileira combina **voto obrigatório** (para a maioria dos cidadãos) com
níveis variados de engajamento político voluntário. Essa combinação levanta uma questão
central: **o ato de votar — muitas vezes uma obrigação legal — reflete uma disposição
genuína de participar da vida política?** Pesquisas de opinião capturam a *percepção e a
disposição declaradas*; registros eleitorais capturam o *comportamento efetivo*. Analisar
as duas dimensões em conjunto permite verificar se elas convergem ou divergem — e, quando
divergem, em quais grupos isso ocorre.

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
   comparáveis por dimensões comuns.
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
| Q3 | Disposição declarada e comparecimento real andam juntos, por região? | 3.7.1 / 3.7.4 |
| Q4 | E por escolaridade? E por faixa etária? | 3.7.2 / 3.7.3 |
| Q5 | A amostra de opinião é representativa do eleitorado? | 3.7.5 |

---

## 2. Materiais e Métodos

### 2.1 Fontes de dados

#### 2.1.1 Base principal — CESOP/04832 (pesquisa de opinião)

- **Origem:** Centro de Estudos de Opinião Pública (CESOP/UNICAMP), estudo **04832**.
- **Formato:** arquivo SPSS (`.SAV`), lido com a biblioteca `pyreadstat`, que retorna o
  DataFrame **e** um objeto de metadados com os rótulos das variáveis e os mapeamentos
  código→categoria embutidos no arquivo.
- **Dimensão:** **2.000 respondentes × 30 variáveis**.
- **Blocos de variáveis utilizados:**

| Variável | Descrição |
|----------|-----------|
| `SEXO`, `FX_ID`, `ESCOLARIDADE`, `RACA`, `RELIGIAO` | Perfil demográfico |
| `REND1`, `REND2` | Renda pessoal e familiar (em salários mínimos) |
| `REGIAO`, `COND`, `PORTE` | Localização e porte do município |
| `P_01A`, `P_01B`, `P_01C` | Lembrança do voto em 2022 (dep. estadual, federal, senador) |
| `P_02_1`, `P_02_2`, `P_02_3` | Prioridades políticas (1ª, 2ª e 3ª escolhas) |
| `P_03_1` … `P_03_6` | Medidas consideradas úteis contra *fake news* (múltipla escolha) |
| `P_04` | Vontade de participar da vida política local |

#### 2.1.2 Base complementar — TSE (comportamento eleitoral)

- **Origem:** Tribunal Superior Eleitoral — *Perfil do Eleitorado: Comparecimento e
  Abstenção*, **Eleições Gerais de 2022**, arquivo nacional (`...BRASIL.csv`).
- **Formato:** CSV (~2,3 GB), separador `;`, encoding `latin1` (padrão das bases do TSE).
- **Dimensão após limpeza:** **8.785.738 linhas-detalhe** (combinações de UF × município ×
  zona × perfil demográfico).
- **Totais nacionais (2022):** **311.513.866 aptos**; **247.332.217 comparecimentos**;
  **64.181.649 abstenções** → comparecimento de **79,40%**.
- **Variáveis utilizadas:** `SG_UF`, `NM_MUNICIPIO`, `DS_GENERO`, `DS_ESTADO_CIVIL`,
  `DS_FAIXA_ETARIA`, `DS_GRAU_ESCOLARIDADE`, `DS_COR_RACA`, `QT_APTOS`,
  `QT_COMPARECIMENTO`, `QT_ABSTENCAO`.

> **Conexão conceitual entre as bases:** a CESOP mede *opinião e disposição declarada*; o
> TSE mede *comportamento eleitoral real*. A ponte entre elas é feita por dimensões comuns
> (região, escolaridade, faixa etária), permitindo uma **comparação ecológica** — entre
> grupos agregados, **não** entre os mesmos indivíduos.

### 2.2 Ferramentas e ambiente

- **Linguagem:** Python 3.
- **Bibliotecas:** `pandas` e `numpy` (manipulação), `pyreadstat` (leitura de `.SAV`),
  `matplotlib` e `seaborn` (visualização).
- **Formato intermediário:** **Parquet**, que preserva os tipos categóricos sem
  recodificação na releitura e reduz o tamanho em disco.
- **Reprodutibilidade:** os notebooks detectam automaticamente o ambiente
  (**Google Colab** × local) e ajustam os caminhos — atendendo ao requisito de execução
  *100% no Colab*.

### 2.3 Pipeline de preparação dos dados (Notebook 01)

O primeiro notebook não contém análise: dedica-se inteiramente à **preparação
reprodutível**, em duas frentes.

**CESOP:**
1. **Carregamento** do `.SAV` com `pyreadstat` (dados + metadados).
2. **Padronização de nomes** das variáveis de pergunta (`P1A` → `P_01A`) e conversão de
   `DATA_ENTREVISTA` para `datetime`.
3. **Tratamento de não-resposta:** o código `99` ("Não sabe/Não respondeu") é convertido em
   `NaN`. A substituição é feita **por coluna** (não globalmente), de modo a **preservar** o
   código `98` de `REND1`/`REND2` ("Não tem rendimento"), que é uma **categoria válida** —
   decisão documentada em uma tabela explícita para evitar uma generalização que apagaria
   esse valor.
4. **Rotulagem:** aplicação dos *value labels* e conversão para o tipo `category`, feita
   **após** a limpeza de `99` para preservar os `NaN` recém-inseridos.
5. **Variáveis derivadas:** agregação dos 16 níveis de `ESCOLARIDADE` em **5 grupos
   ordenados** (`ESCOL_GRUPO`: baixa escolaridade → fundamental → médio → superior
   incompleto → superior completo) e das faixas de renda em **5 faixas** (`RENDA_PESSOAL`,
   `RENDA_FAMILIAR`). Inclui-se uma **validação de cobertura** que detecta rótulos não
   mapeados (evita perdas silenciosas por divergência de grafia ou acentuação).

**TSE:**
1. **Carregamento otimizado:** leitura **apenas das colunas necessárias** (`usecols`) —
   essencial para caber na memória do Colab dado o volume de 2,3 GB.
2. **Limpeza:** remoção de combinações com `QT_APTOS = 0` (dividiriam por zero nas taxas),
   normalização de strings e conversão para `category`.
3. **Variáveis derivadas:** `TAXA_COMPARECIMENTO` e `TAXA_ABSTENCAO` (proporções em [0, 1])
   e `REGIAO` (derivada da UF pela categorização do IBGE).
4. **Agregações:** dois recortes — por **UF** (`df_tse_uf`) e por **perfil demográfico
   nacional** (`df_tse_perfil`). As taxas agregadas usam a **soma** das contagens (média
   ponderada por eleitor), e **não** a média das taxas-linha — evitando que zonas pequenas
   distorçam o indicador.
5. **Validação:** `assert` garante que todas as taxas estão no intervalo [0, 1] antes do
   salvamento.

**Saídas:** `cesop_clean.parquet`, `tse_clean.parquet`, `tse_uf.parquet`,
`tse_perfil.parquet` e `cesop_labels.json`.

### 2.4 Harmonização entre as bases

Como CESOP e TSE adotam convenções diferentes, foram criadas tabelas de correspondência
(Notebook 02):

- **Região:** unificação de grafias (ex.: `NORDESTE` → `Nordeste`).
- **Escolaridade:** os níveis do TSE são mapeados para os 5 grupos do CESOP.
- **Faixa etária:** as idades do TSE são *bucketizadas* nas mesmas faixas do CESOP.

### 2.5 Técnicas estatísticas e de visualização

- **Distribuições univariadas:** frequências e percentuais, com `n` explícito.
- **Tabelas cruzadas** (`crosstab`) normalizadas por linha (composição dentro de cada
  grupo).
- **Visualizações:** barras horizontais, **barras empilhadas 100%**, **gráficos de pizza**,
  **heatmaps**, **dispersão (scatter)** com rótulos por grupo e **linhas com eixo duplo
  (`twinx`)**.
- **Correlação de Spearman (ρ):** medida de associação **monotônica** entre os agregados,
  apropriada para poucos pontos e relações não necessariamente lineares. Usada nos
  cruzamentos CESOP × TSE.
- **Cuidados metodológicos:** denominadores corretos em perguntas de múltipla escolha
  (percentuais sobre o **total de menções**, não de respondentes), ordenação semântica das
  categorias ordinais e sinalização explícita de recortes com **baixo `n`**.

---

## 3. Resultados e Discussão

> Todos os percentuais e coeficientes a seguir foram obtidos diretamente das análises dos
> notebooks. Valores precedidos de "≈" foram lidos das figuras geradas.

### 3.1 Perfil da amostra (CESOP, n = 2.000)

- **Sexo:** 51,7% feminino, 48,3% masculino — equilibrado.
- **Faixa etária:** concentração em adultos — 25–34 anos (22,9%) e 35–44 anos (21,2%),
  somando 44,1%; jovens de 16–17 anos representam apenas 1,1%.
- **Raça/cor:** parda (46,0%) e branca (42,3%) somam quase 90%; preta (10,4%), amarela
  (0,9%) e indígena (0,4%) completam.
- **Escolaridade:** o ensino médio predomina (40,8%); a **baixa escolaridade é rara na
  amostra (3,2%)** — ponto decisivo, retomado na análise de representatividade (3.7.5).
- **Território:** a distribuição regional **acompanha o peso populacional do país** —
  Sudeste 43,2%, Nordeste 25,6%, Sul 15,2%, Norte e Centro-Oeste com 8,0% cada.
  Diferentemente da escolaridade, **geograficamente a amostra é plausível**, o que dá
  segurança às leituras por região.
- **Renda:** o perfil econômico é **predominantemente de baixa renda**. Na renda *pessoal*,
  53,7% recebem até 1 salário mínimo (incluindo 12,6% sem rendimento) e apenas 5,2% ganham
  acima de 5 SM. Na renda *familiar*, a distribuição desloca-se para cima (58,8% entre 1 e
  5 SM; faixa acima de 5 SM sobe para 12,0%), reflexo da soma de rendimentos no domicílio.
  Em ambas, o topo é residual (**acima de 20 SM: 0,4%–0,5%**, ≈ 8 a 10 pessoas) — o que
  **fundamenta as ressalvas de baixo `n`** feitas adiante.

### 3.2 Lembrança do voto em 2022 (P_01A/B/C)

| Cargo | Lembra ("Sim") | Não lembra | Não votou |
|-------|:---:|:---:|:---:|
| Deputado(a) estadual | 29,8% | 64,6% | 5,5% |
| Deputado(a) federal | 29,4% | 65,0% | 5,6% |
| Senador(a) | 29,6% | 64,9% | 5,5% |

**Achado.** A lembrança é **praticamente idêntica entre os três cargos** (≈ 30%): a maioria
(≈ 65%) **não recorda em quem votou** para o Legislativo em 2022 — indício de baixo vínculo
com a representação legislativa. Não há o gradiente "estadual < federal < senador" que se
poderia supor. O conteúdo mais informativo está nos **recortes**:

1. **Gradiente socioeconômico — o padrão mais forte.** A lembrança **quase triplica** com a
   escolaridade: 16% (baixa) → 25% → 28% → 37% → **43%** (superior completo); e o mesmo
   gradiente reaparece na **renda pessoal**: 25% (até 1 SM) → 35% (1–5 SM) → **51%** (acima
   de 5 SM). Dois gradientes monotônicos e independentes na mesma direção tornam o achado
   robusto.
2. **Idade — relação em U-invertido, não linear.** A lembrança sobe até a meia-idade e
   recua entre os idosos: 17% (16–17) → 22% → 31% → **33% (35–44)** → 32% → 32% → 26%
   (65+). O pico está na faixa economicamente ativa; a lembrança é menor nos **dois
   extremos** etários.
3. **Voto facultativo nos 16–17 anos.** Nessa faixa, 35% declararam **não ter votado**
   (contra 6–9% nas demais), reflexo direto do voto facultativo abaixo dos 18 anos
   (*ressalva:* n = 23, valor ilustrativo).
4. **Norte fora da curva.** Entre regiões a lembrança é estável (≈ 25–30%), **exceto o
   Norte: 41%** (n = 160) — a mesma região com **baixo comparecimento real** no TSE,
   tensão explorada em 3.7.

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

**O ranking muda conforme o critério.** Quando se contam **as três menções** (1ª + 2ª +
3ª), **educação salta da 6ª para a 2ª posição** (15,8%), revelando-se um tema de "consenso
secundário" — raramente a prioridade nº 1, mas citado com altíssima frequência logo em
seguida. Já *reduzir desigualdades* cai (de 3ª para 5ª) e *combater preconceitos* recua —
relatar os dois rankings evita uma leitura enganosa.

**Gradiente socioeconômico (escolaridade e renda se espelham).** Os *heatmaps* mostram
gradientes monotônicos: *reduzir desigualdades* **sobe** com escolaridade (8% → … → 21%) e
*educação* também (5% → … → 14%), enquanto *reduzir violência* **cai** (17% → … → 7%) e
*saúde* **cai com a renda** (24% até 1 SM → 10% no topo). Leitura defensável (à la
Inglehart, materialismo × pós-materialismo): camadas **baixas** priorizam pautas
**materiais e imediatas**; camadas **altas**, pautas **estruturais** — achado robusto por
aparecer em duas variáveis independentes.

**Recortes adicionais.** O **Norte** é o único onde *emprego* (21%) supera *saúde* (19%),
com *desigualdades* também alta (20%); a *violência* é regional (alta no Sudeste e
Nordeste, baixa no Norte e Centro-Oeste). **Mulheres priorizam saúde muito mais** (24% vs
16%); **pretos priorizam combater preconceitos** mais que brancos (18% vs 10%). Em todos os
recortes, "ampliar a participação política" permanece no fundo (1%–3%) — o desinteresse
pelo processo político não é uma média que esconde subgrupos, e sim **transversal**.

### 3.4 Combate às *fake news* (P_03)

Pergunta de múltipla escolha (1 a 6 medidas), com **3.105 menções feitas por 2.000
respondentes** — percentuais sobre o total de menções. Quatro leituras se complementam:

1. **Punir mais que regular — já na medida mais citada.** Dois terços das menções
   (**66,9%**) são de punição/responsabilização contra **33,1%** de regulamentação. As
   **três medidas mais citadas são todas punitivas**: punir usuários (26,6%, a nº 1), punir
   empresas (21,7%) e punir políticos (18,6%).
2. **Quem responsabilizar: o usuário, não o político.** Por ator, as menções dividem-se em
   usuários (36,7%), empresas/plataformas (35,9%) e políticos (27,3%) — o cidadão comum é o
   alvo mais citado e o político, o menos.
3. **Punir indivíduos, regular plataformas.** Cruzando tipo × ator, a punição responde por
   **72,5% das menções sobre usuários** e 68,0% sobre políticos, caindo para **60,3% sobre
   empresas** — as plataformas são o ator para quem mais se admite regulação no lugar de
   punição direta.
4. **Preferências concentradas.** Metade dos respondentes (49,0%) citou **uma única
   medida** e 74,7% citaram 1 ou 2 (média ≈ 1,55); apenas 2,1% (n = 41) marcaram todas as
   seis.

**Achado.** Uma opinião pública que pede **respostas diretas e punitivas** — sobretudo
dirigidas ao usuário — em vez de soluções regulatórias ou processuais.

### 3.5 Vontade de participar da política local (P_04)

| Nível | % | n |
|-------|:---:|:---:|
| Nenhuma vontade | **78,0%** | 1.559 |
| Alguma vontade | 13,2% | 264 |
| Muita vontade | 8,5% | 169 |

**Achado.** O desengajamento declarado é expressivo e **transversal**: **78% não têm
nenhuma vontade** de participar da política local; a disposição total ("alguma" + "muita")
é de apenas ≈ 21,7%. Os recortes não revelam nenhum bolsão de engajamento, mas mostram
quem se afasta mais ou menos:

1. **Gradiente socioeconômico — o eixo mais forte.** A disposição quase **triplica** com a
   escolaridade: a "nenhuma vontade" cai de 89% (baixa escolaridade) para 66% (superior
   completo), com a disposição subindo de ≈ 11% para 34%; a renda repete o padrão (≈ 18% →
   33%). É a contraparte, na própria CESOP, do gradiente confirmado no cruzamento com o TSE
   (3.7.2).
2. **Idade — o mesmo U-invertido da lembrança de voto.** Os 16–17 anos têm 91% de nenhuma
   vontade; o engajamento atinge o pico entre 35 e 54 anos (≈ 25–26% de disposição) e cai
   aos 65+ (85% nenhuma).
3. **Diferenças menores, porém consistentes.** Homens declaram disposição um pouco maior
   que mulheres (23% vs 20%); entre os grandes grupos de raça/cor, os pretos têm a maior
   (26%); por região, Centro-Oeste (25%) e Sudeste (24%) lideram, e Sul e Norte ficam no
   piso (19%) — o lado CESOP da dissociação regional explorada em 3.7.1.
4. **Desengajamento universal.** Em **nenhum subgrupo** — nem o mais escolarizado, nem o de
   maior renda, nem a faixa mais ativa — a maioria deseja participar: mesmo no superior
   completo, 66% não têm nenhuma vontade. As diferenças entre grupos são de *grau*, não de
   *direção*.

### 3.6 Panorama eleitoral — TSE 2022

- **Comparecimento nacional: 79,40%** (abstenção: 20,60%), sobre 311,5 milhões de aptos.
- **Por escolaridade — o grande separador.** A taxa sobe de forma monotônica de
  **49,3% (analfabetos)** a **87,6% (superior completo)** — quase **38 pontos** de diferença
  (≈ 25 p.p. mesmo agrupando os menos escolarizados em "baixa escolaridade ≈ 62%"). É a
  maior amplitude de todo o estudo.
- **O gradiente é universal; a geografia é secundária.** O cruzamento região × escolaridade
  mostra que o gradiente **se repete dentro de cada região**, com as regiões **convergindo
  no topo** (superior completo entre 87% e 89% em todas) e **divergindo apenas na base**
  (analfabetos de 40% no Sudeste a 57% no Nordeste). No agregado, a diferença regional é
  pequena (Sul ≈ 81,0% no teto, Norte ≈ 78,1% no piso; **≈ 3 p.p.**).
- **Por idade — jovens facultativos engajados, idosos em colapso.** A curva sobe a um platô
  em 50–60 anos (87,6%) e depois **despenca**: 63,1% aos 70 e apenas 29,3% aos 80. Já os
  jovens de **16–17 (voto facultativo) comparecem ≈ 83%** — mais que os eleitores de 30 anos
  (80,1%). Como o eleitorado se concentra entre 25 e 50 anos, esses extremos pouco deslocam
  a média nacional.
- **Por gênero — efeito pequeno e condicionado pela escolaridade.** Entre analfabetos os
  homens comparecem mais (54% vs 45%), mas a vantagem se inverte no superior completo (88%
  mulheres vs 87% homens).

**Achado.** A escolaridade discrimina o comparecimento **muito mais** que a região (≈ 25
p.p. contra ≈ 3 p.p.) — pista de que a escolaridade, não a geografia, é o fator
estruturante.

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
declaram menos vontade de participar**; Centro-Oeste e Sudeste fazem o inverso. Por região,
alto comparecimento **não significa** alta disposição — chegam a se opor. (Ressalva: as
bases não medem os mesmos indivíduos; a comparação é *ecológica*.)

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
duas bases concordam plenamente** — o achado central do trabalho.

#### 3.7.3 Por faixa etária — convergência no miolo, divergência nas pontas

No intervalo de **25 a 54 anos**, comparecimento (TSE) e disposição (CESOP) sobem juntos,
com pico em 45–54. Nas extremidades, divergem: os jovens de **16–17 comparecem (≈ 83%) mas
têm disposição mínima (≈ 9%)** — exercem o voto facultativo sem engajamento declarado
correspondente; aos **65+**, ambos caem (comparecimento para ≈ 57%). O formato em sino é
compartilhado, mas as pontas revelam o descompasso entre **votar** e **querer participar**.

#### 3.7.4 Abstenção × desinteresse, por região (espelho de 3.7.1)

O padrão de divergência se confirma pelo lado negativo: o **Sul** tem a **menor abstenção**
(≈ 19%) mas o **maior desinteresse declarado** (≈ 81,5% de "nenhuma vontade"); o **Norte**,
a maior abstenção (≈ 21,9%). Não há paralelismo entre abstenção e desinteresse —
reforçando que, regionalmente, comportamento e percepção se dissociam.

#### 3.7.5 Representatividade da amostra

| Escolaridade | CESOP (amostra) | TSE (eleitorado) |
|--------------|:---:|:---:|
| Baixa escolaridade | 3,2% | 11,2% |
| Ensino fundamental | 31,7% | 29,5% |
| Ensino médio | 40,8% | 43,0% |
| Superior incompleto + completo | 24,1% | 16,3% |

A amostra **sub-representa os menos escolarizados** e **sobre-representa o ensino superior**
(24,1% contra 16,3% no eleitorado). Por idade, na mesma direção: **sub-representa os 65+**
(9,0% vs 14,6%) e **sobre-representa os adultos de 18–34 anos** (37,9% vs 32,5%). Como a
disposição e o comparecimento crescem com a escolaridade e caem entre os idosos, **os dois
vieses empurram no mesmo sentido**: a amostra provavelmente **superestima** o engajamento —
o desinteresse real da população tende a ser **ainda maior** que os 78% observados.

#### 3.7.6 Fake news × comparecimento (exploratório)

Indicador de baixa variância (quase todos citam alguma medida), portanto pouco
discriminante. Vale reter da seção 3.4 que a ênfase recai sobre **punição (66,9%)** mais que
regulamentação — em linha com a preferência por soluções diretas observada em outras
seções.

### 3.8 Padrões transversais (síntese analítica)

1. **A escolaridade é o eixo estruturante — e o único alinhador.** É a dimensão em que
   percepção (CESOP) e comportamento (TSE) concordam perfeitamente (ρ = 1,00) e a que mais
   discrimina o comparecimento (≈ 25 p.p. contra ≈ 3 p.p. da região). O achado é robusto por
   **convergência de fontes**: a **renda reproduz o mesmo gradiente** de forma independente
   (lembrança de 25%→51%, disposição de ≈ 18%→33%) e ele atravessa medidas **declaradas** e
   **reais**.
2. **A geografia dissocia percepção e comportamento — Sul e Norte como faces opostas**
   (ρ = −0,30). O **Sul** vota mais mas é o mais desinteressado; o **Norte** declara a maior
   lembrança de voto (41%) e tem o menor comparecimento real (78,1%). Comparecer não é o
   mesmo que querer participar.
3. **A idade desenha um U-invertido recorrente.** O mesmo formato aparece na lembrança de
   voto (3.2), na disposição declarada (3.5) e no comparecimento real (3.6): engajamento
   concentrado na meia-idade, queda nas duas pontas — jovens de 16–17 comparecem sem
   interesse correspondente; idosos caem nas duas medidas.
4. **Desengajamento participativo convergente — e preferência por soluções diretas.** 78%
   sem vontade (P_04), "ampliar participação" como penúltima prioridade (2%, P_02) e ≈ 65%
   sem lembrança do voto legislativo (P_01) apontam para um vínculo fraco com a política
   processual; a questão das *fake news* (3.4) reforça por outro ângulo a inclinação por
   respostas diretas (punir) em vez de processuais (regular).
5. **O eixo socioeconômico organiza também o conteúdo das demandas.** Não só *quanto* as
   pessoas se engajam, mas *o que* priorizam: camadas de menor escolaridade/renda valorizam
   pautas **materiais** (saúde, violência); as de maior, pautas **estruturais** (desigualdade,
   educação).

---

## 4. Limitações

- **Comparação ecológica.** CESOP e TSE **não medem os mesmos indivíduos**; as associações
  são entre **agregados** (regiões, faixas, grupos) e **não autorizam conclusões sobre
  indivíduos** (risco de *falácia ecológica*).
- **Correlação, número de pontos e forma da relação.** O ρ de Spearman descreve associação e
  direção, não causa. As correlações ecológicas apoiam-se em **poucos pontos agregados** —
  cinco regiões e cinco grupos de escolaridade —, funcionando como indicadores de direção e
  força aparente, **não como estimativas com significância estatística**. O ρ = 1,00 por
  escolaridade reflete a **monotonicidade perfeita entre cinco grupos ordenados**, não a
  ausência de ruído individual; a relação por idade, por ser **não monotônica**, não se
  resume a um único coeficiente.
- **Viés amostral (escolaridade e idade).** A CESOP sobre-representa os mais escolarizados e
  os adultos jovens, e sub-representa a baixa escolaridade e os idosos; como ambos os vieses
  empurram na mesma direção, as leituras de magnitude devem ser **conservadoras** (o
  engajamento real tende a ser ainda menor que o observado).
- **Recortes com baixo `n`.** Categorias pouco frequentes (ex.: renda "acima de 20 SM" com
  n ≈ 7; faixa 16–17 com n = 23; raça amarela e indígena) são ilustrativas, não conclusivas.

---

## 5. Conclusão

A análise sustenta uma tese central: **no Brasil, é a escolaridade — e não a região — que
organiza o engajamento político**, sendo a única dimensão em que **percepção declarada
(CESOP) e comportamento eleitoral real (TSE) se alinham** (ρ = 1,00); a **renda reproduz o
mesmo gradiente** e a **idade** funciona como um segundo eixo estrutural, concentrando o
engajamento na meia-idade e deprimindo-o nas duas pontas. Geograficamente, ao contrário, as
duas dimensões chegam a se **opor** (ρ = −0,30): o **Sul** vota muito mas quer pouco
participar, e o **Norte** faz o inverso (maior lembrança declarada, menor comparecimento
real).

Soma-se a isso um quadro consistente de **baixo engajamento participativo declarado** —
manifesto de forma convergente em três perguntas distintas (P_01, P_02, P_04) e coerente com
a preferência por **soluções diretas** (punir, no caso das *fake news*) em vez de
processuais. Do ponto de vista do tema — *Percepção dos Brasileiros Acerca da Democracia* —
o estudo retrata um eleitorado que **comparece às urnas** (79,4% em 2022), sustentado em
parte pela obrigatoriedade, mas que, em sua maioria, **não deseja participar ativamente** da
política local e **não recorda** seu voto legislativo: quer **resultados** (saúde, emprego)
mais do que **tomar parte**. O trabalho cumpre o objetivo de **correlacionar uma pesquisa de
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
