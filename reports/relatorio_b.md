# Inadimplência de Pessoas Físicas no Brasil
### Análise de séries temporais e projeção via ARIMA a partir de dados do Banco Central

**Disciplina:** Ciência de Dados — Projeto I (EDA, *Exploratory Data Analysis*)
**Instituição:** Instituto Mauá de Tecnologia (IMT)
**Tema:** Comportamento histórico e projeção da inadimplência de pessoas físicas (PF)
**Bases:** Banco Central do Brasil (SGS) — Inadimplência PF (série 21082), Taxa Selic
(série 4390) e Taxa de Câmbio/Dólar (série 3695)
**Ano:** 2026

---

## Resumo

Este trabalho investiga o comportamento da **inadimplência de pessoas físicas (PF)** no
Brasil entre **março de 2011 e abril de 2026** (182 observações mensais), relacionando-a
com duas variáveis macroeconômicas de controle: a **Taxa Selic** (custo do dinheiro) e a
**Taxa de Câmbio** (Dólar, como vetor inflacionário). Os dados foram extraídos
diretamente, via API, do **Sistema Gerenciador de Séries Temporais (SGS) do Banco Central
do Brasil**. A análise combina decomposição de série temporal (tendência, sazonalidade e
resíduo), estudo de correlação com defasagens (*lags*) de 3 e 6 meses, diagnóstico de
estacionariedade (teste **Augmented Dickey-Fuller**, ADF) e um modelo **ARIMA(1,1,1)**
para projeção de curto prazo. Os principais achados são: (1) a inadimplência segue
**ciclos macroeconômicos estruturais**, não eventos isolados; (2) existe uma
**sazonalidade mensal robusta**, com pico em abril/maio e alívio em dezembro; (3) a
**Selic é o principal preditor do calote**, mas com **efeito defasado de 6 meses**
(r = 0,774, contra r = 0,527 no mês corrente); (4) o **Dólar tem efeito fraco e indireto**,
atuando via pressão inflacionária sobre a Selic; e (5) o modelo ARIMA(1,1,1), ajustado
sobre a série diferenciada (estacionária, ADF p = 0,0356), projeta **tendência de alta
contínua** para os próximos seis meses, coerente com o efeito defasado dos juros altos
acumulados.

---

## 1. Introdução

### 1.1 Contexto e motivação

A inadimplência de pessoas físicas é um **termômetro da saúde financeira das famílias** e
da estabilidade do sistema de crédito. Diferentemente de indicadores de mercado
financeiro, ela reflete diretamente o orçamento doméstico: capacidade de pagamento de
contas, cartões e financiamentos. Por abranger um período de mais de 15 anos (2011–2026),
a série captura **múltiplos ciclos econômicos** — recessão, recuperação, choque pandêmico
e o atual ciclo de juros altos —, o que a torna um caso de estudo rico para técnicas de
**análise de séries temporais**.

### 1.2 Problema de pesquisa

O projeto parte da seguinte pergunta norteadora:

> **Como a inadimplência de pessoas físicas se relaciona com a política monetária (Selic)
> e com o câmbio (Dólar) ao longo do tempo — e essa relação é imediata ou defasada? Existe
> um padrão sazonal recorrente, independente do ciclo econômico? E o que um modelo
> preditivo simples indica sobre a trajetória de curto prazo?**

### 1.3 Objetivos

**Objetivo geral.** Caracterizar o comportamento histórico da inadimplência de pessoas
físicas no Brasil e seu mecanismo de transmissão com a Taxa Selic e o Câmbio,
produzindo uma projeção de curto prazo.

**Objetivos específicos:**

1. Coletar, em tempo real, as séries de inadimplência PF, Selic e Dólar diretamente da
   API do Banco Central (SGS).
2. Decompor a série de inadimplência em **tendência, sazonalidade e resíduo**, associando
   cada componente a eventos macroeconômicos reais.
3. Quantificar a sazonalidade mensal e identificar os meses de maior pressão e de maior
   alívio orçamentário.
4. Medir a correlação entre inadimplência e os indicadores macroeconômicos, com e sem
   defasagens temporais (*lags* de 3 e 6 meses).
5. Diagnosticar a estacionariedade da série (teste ADF) e ajustar um modelo **ARIMA**
   para projeção de 6 meses.

### 1.4 Perguntas analíticas derivadas

| # | Pergunta | Seção |
|---|----------|-------|
| Q1 | Como a inadimplência evoluiu entre 2011 e 2026, e quais eventos macroeconômicos explicam os principais movimentos? | 3.1 |
| Q2 | A série tem tendência e sazonalidade bem definidas, ou seu comportamento é majoritariamente ruído? | 3.2 |
| Q3 | Existe um padrão sazonal mensal — e em que meses ele se manifesta? | 3.3 |
| Q4 | A Selic e o Dólar antecipam (ou acompanham) o movimento da inadimplência? Com que defasagem? | 3.4 |
| Q5 | A série é estacionária? O que isso implica para a modelagem? | 3.5 |
| Q6 | O que um modelo ARIMA projeta para os próximos meses, e essa projeção é consistente com os achados anteriores? | 3.6 |

---

## 2. Materiais e Métodos

### 2.1 Fontes de dados

Todas as séries foram obtidas **diretamente via API** do **Sistema Gerenciador de Séries
Temporais (SGS) do Banco Central do Brasil**, em formato JSON, garantindo dados oficiais
e atualizados.

| Série | Código SGS | Descrição |
|-------|:---:|-----------|
| `Inadimplencia_PF` | 21082 | Taxa de inadimplência — recursos livres — pessoas físicas (%) |
| `Selic` | 4390 | Taxa Selic mensal (%) |
| `Dolar` | 3695 | Taxa de câmbio (R$/US$) |

- **Período analisado:** março de 2011 a abril de 2026 (**182 observações mensais**), após
  consolidação das três séries e remoção de períodos sem sobreposição (`dropna()`).
- **Função de coleta:** uma função utilitária (`get_bcb_data`) monta a URL da API SGS por
  código de série, converte a coluna `data` para `datetime`, define-a como índice e
  renomeia a coluna de valores — padronizando a leitura das três séries.

### 2.2 Ferramentas e ambiente

- **Linguagem:** Python 3.
- **Bibliotecas:** `pandas` e `numpy` (manipulação), `matplotlib` e `seaborn`
  (visualização), `statsmodels` (decomposição sazonal, ACF/PACF, teste ADF e modelo
  ARIMA).
- **Acesso a dados:** chamadas HTTP diretas (`pd.read_json`) à API pública do BCB —
  dispensa download manual de arquivos.

### 2.3 Pipeline analítico

1. **Tratamento temporal.** Após a consolidação, calculam-se **médias móveis de 6 e 12
   meses** sobre a inadimplência para remover o ruído mensal e evidenciar a tendência de
   longo prazo. As médias móveis são usadas apenas para visualização, não para os modelos.
2. **Decomposição aditiva.** A série `Inadimplencia_PF` é decomposta com
   `seasonal_decompose` (modelo aditivo, período = 12 meses) em **Tendência +
   Sazonalidade + Resíduo**, permitindo separar o que é ciclo estrutural do que é ruído.
3. **Análise sazonal por boxplot.** Os meses são extraídos do índice temporal e ordenados
   categoricamente (janeiro → dezembro); calculam-se estatísticas descritivas
   (`describe()`) por mês para quantificar o padrão sazonal.
4. **Engenharia de *lags*.** Criam-se versões defasadas de Selic e Dólar em **3 e 6
   meses** (`shift(3)`, `shift(6)`), testando a hipótese de que o efeito da política
   monetária sobre o crédito é cumulativo, não imediato.
5. **Matriz de correlação.** Calcula-se a correlação de **Pearson** entre a inadimplência
   e as variáveis (originais e defasadas), visualizada em *heatmap*.
6. **Diagnóstico de estacionariedade.** Aplicam-se os correlogramas **ACF/PACF** e o
   **teste Augmented Dickey-Fuller (ADF)** sobre a série original; em seguida, sobre a
   série **diferenciada** (`diff()`, d = 1), repetindo o teste ADF para confirmar a
   estacionariedade.
7. **Modelagem ARIMA.** Ajusta-se um modelo **ARIMA(1, 1, 1)** sobre a série original
   (a diferenciação d = 1 é parte da especificação do modelo) e gera-se uma projeção
   (*forecast*) para os **6 meses seguintes**.

### 2.4 Técnicas estatísticas e de visualização

- **Médias móveis (6 e 12 meses)** para suavização e identificação de tendência.
- **Gráficos de eixo duplo (`twinx`)** para comparar, na mesma escala temporal,
  inadimplência × Selic e inadimplência × Dólar.
- **Decomposição aditiva de série temporal** (tendência/sazonalidade/resíduo).
- **Boxplots mensais** com média (marcador) para caracterizar a sazonalidade.
- **Correlação de Pearson** com defasagens (*lags*) de 3 e 6 meses, em *heatmap*.
- **Correlogramas ACF/PACF** e **teste ADF** para diagnóstico de estacionariedade.
- **Modelo ARIMA(1,1,1)** com avaliação via *summary* (significância dos coeficientes,
  teste de Ljung-Box nos resíduos) e *forecast* de 6 passos.

---

## 3. Resultados e Discussão

> Todos os valores a seguir foram obtidos diretamente das saídas e gráficos do notebook
> `03_parte_b.ipynb`.

### 3.1 Evolução histórica da série (2011–2026)

A série de inadimplência PF, suavizada pelas médias móveis de 6 e 12 meses, revela uma
**forte característica cíclica**, associada aos principais movimentos macroeconômicos do
Brasil na última década e meia. Quatro períodos se destacam:

- **A escalada da crise (2015–2017).** Após um período de baixa até 2014, a inadimplência
  acelera fortemente, com pico entre 2016 e 2017 — reflexo da recessão econômica (alta
  inflação, desemprego em alta, Selic em elevação). No mesmo intervalo, o **Dólar dispara
  em paralelo** (de ≈ R$ 2,50 para quase R$ 4,00), evidenciando a perda do grau de
  investimento, a desvalorização do Real e a pressão inflacionária sobre o orçamento das
  famílias.
- **Recuperação gradual (2018–início de 2020).** Tendência de queda consistente — uma
  "desalavancagem" das famílias —, impulsionada pelo controle inflacionário e pela
  redução progressiva da Selic às suas mínimas históricas. O Dólar, embora em nível mais
  alto, se estabiliza direcionalmente, permitindo um respiro inflacionário.
- **O choque da pandemia (2020–2021).** Ocorre uma **queda atípica e contraintuitiva** na
  inadimplência, justamente num momento de crise sanitária em que se esperaria o
  contrário. O fenômeno é explicado por **intervenções governamentais** — injeção de
  liquidez via Auxílio Emergencial e prorrogação/renegociação de dívidas pelos bancos.
  Visualmente, há uma **quebra de padrão** (descorrelação): o Dólar sofre novo choque de
  alta (superando R$ 5,00, por pânico global e fuga de capital), enquanto a inadimplência
  **afunda**, mascarada pelas injeções de liquidez.
- **A ressaca inflacionária e o pico recente (2022–2026).** Passado o efeito dos
  auxílios, a série retoma escalada agressiva até 2024; uma leve queda em 2025 é
  rapidamente revertida, atingindo o **pico máximo da série histórica em 2026**. O Dólar
  se consolida em patamares historicamente elevados, atuando como **vetor constante de
  inflação** (combustíveis, alimentos, importados), corroendo o poder de compra e
  dificultando a recuperação financeira das famílias.

### 3.2 Decomposição da série (tendência, sazonalidade, resíduo)

A decomposição aditiva (período = 12 meses) confirma e detalha o ciclo descrito em 3.1:

- **Tendência.** Desalavancagem contínua de 2013 a meados de 2015; rampa de subida
  íngreme na crise de 2016; estabilização seguida de queda em 2018–2020; **vale (mínimo
  histórico) em 2021**, atribuído aos auxílios e renegociações da pandemia; a partir daí,
  **inversão brutal de tendência**, com crescimento contínuo até o ápice em 2026.
  **Conclusão estatística:** a inadimplência no Brasil é **fortemente ditada por ciclos
  estruturais da economia**, e não por eventos passageiros.
- **Sazonalidade.** Padrão **repetitivo entre os mesmos limites** ano a ano — prova
  matemática de que existe sazonalidade na inadimplência: independentemente do ciclo
  econômico, há meses em que ela sistematicamente sobe e meses em que sistematicamente
  cai (detalhado em 3.3).
- **Resíduo.** Comportamento **anômalo em 2020–2021**, com oscilações muito bruscas
  (picos para cima e para baixo) — isola o **"efeito choque" da pandemia**, um evento
  exógeno tão fora da curva que nem a tendência suave nem a sazonalidade anual conseguem
  explicá-lo, transbordando para o resíduo.

### 3.3 Sazonalidade mensal (boxplots e estatísticas descritivas)

| Mês | Média | Mediana | Desvio-padrão | Mín. | Máx. |
|-----|:---:|:---:|:---:|:---:|:---:|
| Janeiro | 3,192 | 3,19 | 0,529 | 2,14 | 4,26 |
| Fevereiro | 3,235 | 3,26 | 0,541 | 2,23 | **4,44** |
| Março | 3,226 | 3,23 | 0,510 | 2,13 | 4,33 |
| **Abril** | **3,315** | 3,305 | 0,523 | 2,19 | **4,44** |
| Maio | 3,292 | 3,30 | 0,432 | 2,33 | 4,04 |
| Junho | 3,160 | 3,17 | 0,412 | 2,27 | 3,73 |
| Julho | 3,199 | 3,17 | 0,417 | 2,31 | 3,77 |
| Agosto | 3,221 | 3,13 | 0,447 | 2,31 | 3,95 |
| Setembro | 3,191 | 3,14 | 0,462 | 2,28 | 3,91 |
| Outubro | 3,225 | 3,17 | 0,501 | 2,28 | 4,00 |
| Novembro | 3,193 | 3,14 | 0,514 | 2,23 | 4,05 |
| **Dezembro** | **3,090** | **2,99** | 0,509 | **2,11** | 4,02 |

**Achados:**

1. **O ciclo de pressão e o ápice da "ressaca" financeira.** A inadimplência escala nos
   primeiros meses do ano e atinge suas **maiores médias absolutas em abril (3,31%) e
   maio (3,29%)**. Esse movimento documenta a "ressaca" financeira: o acúmulo de despesas
   sazonais (IPVA, IPTU, material escolar) comprime a renda no início do ano, levando ao
   pico de calotes nos meses seguintes.
2. **O alívio de dezembro.** Dezembro é o mês de maior alívio: **menor média (3,09%),
   menor mediana (2,99%) e menor mínimo da série (2,11%)** — efeito do **13º salário**,
   que permite às famílias regularizarem dívidas correntes no fim do ano.
3. **Variabilidade e suscetibilidade a choques.** Os primeiros meses do ano apresentam os
   **maiores desvios-padrão** (fevereiro: 0,541; abril: 0,523) e os **valores máximos
   absolutos da série** (4,44% em fevereiro e abril). Em anos de crise, a inadimplência
   reage com muito mais força e volatilidade no primeiro quadrimestre.
4. **Consistência central (ancoragem).** Apesar dos picos de estresse, a mediana
   mantém-se na faixa de **3,1%–3,2%** na maior parte do ano — a série tem uma ancoragem
   estrutural de longo prazo, mas é extremamente sensível aos choques de liquidez
   ditados pelo calendário anual.

**Conclusão.** A análise descritiva confirma numericamente uma sazonalidade clara: um
**estrangulamento orçamentário no primeiro semestre** (ápice em abril/maio) e uma
**recuperação vigorosa por injeção de liquidez extra no final do ano** (dezembro).

### 3.4 Correlação com Selic e Dólar (com defasagens)

| Variável | Correlação com Inadimplência_PF |
|----------|:---:|
| Inadimplencia_PF | 1,000 |
| **Selic_Lag6** | **0,774** |
| Selic_Lag3 | 0,673 |
| Selic (mês corrente) | 0,527 |
| Dolar_Lag6 | −0,211 |
| Dolar_Lag3 | −0,249 |
| Dolar (mês corrente) | −0,282 |

**Achados:**

1. **Forte sensibilidade à Taxa Selic.** Existe correlação positiva e direta entre a
   Selic e a inadimplência: o encarecimento do crédito é o principal motor do calote no
   varejo.
2. **Efeito de transmissão defasado — o achado mais expressivo.** A correlação com a
   Selic do mês corrente é moderada (**r = 0,527**), mas **cresce com a defasagem**:
   r = 0,673 em 3 meses e atinge **r = 0,774 em 6 meses (Selic_Lag6)** — a correlação mais
   forte de toda a tabela. **Interpretação:** a política monetária tem efeito de
   transmissão **lento** sobre o crédito a pessoas físicas. Quando o Banco Central eleva
   os juros, as famílias inicialmente sustentam os pagamentos usando reservas ou
   rearranjos orçamentários; o esgotamento financeiro — e a consequente explosão da
   inadimplência — reflete, com maior precisão, o cenário de juros de **cerca de um
   semestre atrás**.
3. **Baixa relevância direta do câmbio.** As correlações com o Dólar são **fracas e
   inversas** (entre −0,211 e −0,282), o que descarta um efeito imediato do tipo "dólar
   sobe, calote sobe": diferente do endividamento corporativo (com dívidas em moeda
   estrangeira), a PF é praticamente imune a choques cambiais de curto prazo no carnê ou
   na fatura. Ainda assim, como visto em 3.1, o **Dólar atua de forma estrutural e
   indireta**: sua alta pressiona a inflação, que obriga o BC a elevar a Selic — a Selic
   é, portanto, a **variável de transmissão efetiva** para a inadimplência.

### 3.5 Estacionariedade: ACF/PACF e teste ADF

**Série original:**

- Os correlogramas mostram **decaimento muito lento no ACF** — sintoma clássico de série
  não-estacionária (com tendência), refletindo a **"longa memória"** da inadimplência:
  estoques de dívida não se liquidam rapidamente no varejo. O **PACF exibe forte pico no
  primeiro lag**, traduzindo a forte **inércia** do indicador — o nível do mês atual é
  altamente dependente do mês anterior.
- **Teste ADF:** estatística = **−2,0337**, p-value = **0,2720** (> 0,05) → **a série é
  NÃO-ESTACIONÁRIA** (não se rejeita a hipótese nula); média e variância não são
  constantes ao longo do tempo, sendo ditadas por quebras estruturais e tendências de
  longo prazo.

**Implicação.** A não-estacionariedade é crucial por dois motivos: (i) cruzar diretamente
valores brutos da inadimplência com Selic/Dólar (que também têm tendência própria) em
modelos lineares geraria **relações espúrias**; e (ii) modelos como o ARIMA exigem uma
componente integrada (*d*) para tratar a tendência.

**Série diferenciada (d = 1):** o novo teste ADF resulta em **p-value = 0,0356** (< 0,05)
→ a série de **variações mensais é estacionária**, requisito atendido para a modelagem.

### 3.6 Modelo ARIMA(1,1,1) e projeção (forecast)

Com base na análise dos correlogramas da série diferenciada, ajustou-se um
**ARIMA(1, 1, 1)** sobre `Inadimplencia_PF` (182 observações, mar/2011–abr/2026):

| Parâmetro | Coeficiente | Erro-padrão | z | P>\|z\| |
|---|:---:|:---:|:---:|:---:|
| AR(1) | 0,8871 | 0,113 | 7,834 | 0,000 |
| MA(1) | −0,7780 | 0,148 | −5,241 | 0,000 |
| σ² | 0,0096 | 0,001 | 12,332 | 0,000 |

- **Coeficientes significativos** (p < 0,001 para AR e MA): o modelo capturou com sucesso
  tanto a **inércia do mês anterior** (componente autorregressiva) quanto o **ajuste a
  choques recentes** (componente de médias móveis).
- **Resíduos bem comportados:** teste de **Ljung-Box, Prob(Q) = 0,47** — não há
  autocorrelação residual significativa; o modelo extraiu o padrão matemático da série,
  restando apenas ruído branco.

**Projeção (6 meses).** O modelo sinaliza **tendência de alta contínua** da inadimplência
para o horizonte projetado. Essa projeção é **coerente com o achado de 3.4**: como o ciclo
de alta da Selic possui transmissão defasada de ≈ 6 meses, o encarecimento do crédito
acumulado no semestre anterior continuará pressionando o orçamento das famílias,
justificando a continuidade da escalada projetada.

---

## 4. Limitações

- **Correlação ≠ causalidade.** As correlações de Pearson (3.4) — mesmo com defasagens —
  descrevem **associação linear**, não uma relação causal comprovada; outros fatores
  macroeconômicos não incluídos (desemprego, inflação ao consumidor, massa salarial)
  também influenciam a inadimplência e podem confundir o efeito atribuído à Selic.
- **Modelo univariado.** O ARIMA(1,1,1) usa apenas a própria série histórica da
  inadimplência — não incorpora diretamente Selic ou Dólar como variáveis exógenas
  (um modelo ARIMAX/SARIMAX poderia explorar essa relação de forma mais direta).
- **Horizonte curto de projeção.** O *forecast* de 6 meses é adequado à inércia da série,
  mas projeções mais longas tendem a perder precisão, sobretudo diante de choques exógenos
  como o identificado no resíduo de 2020–2021 (pandemia).
- **Decomposição aditiva e período fixo (12 meses).** A decomposição assume sazonalidade
  de amplitude constante e período anual fixo; mudanças estruturais no padrão sazonal
  (por exemplo, novas políticas de crédito) não seriam capturadas por esse modelo.
- **Eventos exógenos não modelados.** O choque da pandemia (2020–2021) e suas
  intervenções (Auxílio Emergencial, renegociações bancárias) não são variáveis explícitas
  do modelo — seu efeito aparece apenas indiretamente, no componente de resíduo.

---

## 5. Conclusão

A análise sustenta que a inadimplência de pessoas físicas no Brasil **não é um indicador
errático**, mas um fenômeno **previsível e fortemente dependente do cenário de juros**.
Três conclusões centrais se destacam:

1. **O veredito da Selic.** A Taxa Selic é o **principal preditor do calote**, mas com um
   **efeito defasado de aproximadamente 6 meses** (r = 0,774 contra r = 0,527 no mês
   corrente) — decisões de política monetária tomadas hoje moldam o perfil de crédito do
   semestre seguinte. O Dólar, por sua vez, atua apenas de forma **indireta e
   estrutural**, via pressão inflacionária sobre a própria Selic.
2. **Sazonalidade e calendário.** Existe um **padrão cultural de endividamento no primeiro
   quadrimestre** (impostos e gastos de início de ano, com pico em abril/maio), que é
   **sistematicamente aliviado pela injeção de liquidez do 13º salário em dezembro**.
3. **Resiliência e tendência atual.** A decomposição revela que a série é **ditada por
   ciclos estruturais** (crise de 2016, recuperação 2018–2020, choque pandêmico 2020–2021,
   ressaca inflacionária 2022–2026), e o modelo ARIMA(1,1,1) — com coeficientes
   significativos e resíduos sem autocorrelação (Ljung-Box) — projeta **pressão ascendente
   contínua** para os próximos meses, sugerindo que o custo do crédito acumulado ainda não
   foi totalmente absorvido pelo orçamento familiar.

Em suma, o estudo indica que **estratégias de concessão de crédito e políticas públicas
devem antecipar ciclos de juros com pelo menos dois trimestres de antecedência** para
mitigar riscos de insolvência sistêmica, e que o planejamento financeiro das famílias —
e das instituições que as atendem — deve considerar tanto o **calendário sazonal**
(pressão no 1º quadrimestre, alívio em dezembro) quanto o **efeito defasado da política
monetária**.

---

## 6. Reprodutibilidade

1. O notebook **`notebooks/03_parte_b.ipynb`** é **autossuficiente**: não depende de
   arquivos locais de dados, pois coleta as séries **diretamente da API do Banco Central
   (SGS)** em tempo de execução.
2. Bibliotecas necessárias: `pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`.
3. Basta executar as células em ordem; a única dependência externa é **conexão com a
   internet** para acesso à API `https://api.bcb.gov.br`.
4. O notebook é **executável no Google Colab** sem necessidade de montar o Google Drive
   ou baixar arquivos adicionais.

---

## 7. Referências

- Banco Central do Brasil. *Sistema Gerenciador de Séries Temporais (SGS)* — séries 21082
  (Inadimplência PF), 4390 (Selic) e 3695 (Taxa de câmbio/Dólar).
- McKinney, W. *pandas: powerful Python data analysis toolkit*.
- Hunter, J. D. *Matplotlib: A 2D Graphics Environment*; Waskom, M. *seaborn*.
- Seabold, S.; Perktold, J. *statsmodels: Econometric and statistical modeling with
  Python* (decomposição sazonal, ACF/PACF, teste de Dickey-Fuller, ARIMA).
- Dickey, D. A.; Fuller, W. A. *Distribution of the Estimators for Autoregressive Time
  Series with a Unit Root* (teste ADF).
- Box, G. E. P.; Jenkins, G. M. *Time Series Analysis: Forecasting and Control*
  (metodologia ARIMA).

---

## 8. Declaração de uso de Inteligência Artificial

Em conformidade com as diretrizes do projeto, declara-se o uso de **assistência de IA**
(modelo de linguagem, via Claude Code) nas seguintes etapas: estruturação e revisão do
código dos notebooks, padronização das visualizações, redação e organização deste
relatório. **Todas as
decisões analíticas, a seleção das bases, a interpretação dos resultados e a validação
final foram conduzidas e revisadas pelos autores.** Os valores numéricos relatados foram
extraídos das análises e figuras produzidas pelos próprios notebooks do projeto.

---

*Documento gerado para o Projeto I — EDA da disciplina de Ciência de Dados.*
