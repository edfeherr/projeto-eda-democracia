# Apresentação — Projeto EDA: Parte A (Democracia) e Parte B (Inadimplência PF)
### Roteiro de slides (limite: 5 minutos)

> **Como usar este arquivo:** cada bloco separado por `---` é um slide. Os tópicos em
> **negrito** são o que entra no slide; o texto em *itálico* sob "🎤 Narração" é o que você
> fala (não vai na tela). O tempo sugerido por slide soma ~5 min. Mantenha os slides
> enxutos — fale os detalhes, não os escreva.

**Orçamento de tempo (13 slides ≈ 5 min):**

| # | Slide | Bloco | Tempo |
|---|-------|-------|:---:|
| 1 | Capa | — | 0:10 |
| 2 | Duas perguntas, um fio condutor | — | 0:20 |
| 3 | Dados e método (Parte A) | A | 0:25 |
| 4 | O que os brasileiros declaram | A | 0:30 |
| 5 | O comportamento real (TSE) | A | 0:25 |
| 6 | 🎯 Escolaridade alinha (ρ = 1,00) | A | 0:30 |
| 7 | 🎯 Região dissocia (ρ = −0,30) | A | 0:25 |
| 8 | Conclusão — Parte A | A | 0:20 |
| 9 | Dados e método (Parte B) | B | 0:25 |
| 10 | Evolução histórica e sazonalidade | B | 0:30 |
| 11 | 🎯 O efeito defasado da Selic | B | 0:30 |
| 12 | ARIMA: o que vem pela frente | B | 0:25 |
| 13 | Conclusão geral + encerramento | — | 0:25 |

**Total: 5:00**

---

## Slide 1 — Capa (0:10)

# Projeto EDA — Ciência de Dados
### Parte A: Percepção dos Brasileiros Acerca da Democracia
### Parte B: Inadimplência de Pessoas Físicas no Brasil

- Projeto I — EDA | IMT
- *[Nomes dos integrantes do grupo]*

🎤 **Narração:** *"Nosso projeto tem duas frentes: na Parte A, cruzamos o que os
brasileiros dizem sobre democracia com o que fazem nas urnas; na Parte B, analisamos a
inadimplência das famílias e sua relação com os juros. As duas se conectam por um mesmo
fio: o comportamento real das famílias brasileiras."*

---

## Slide 2 — Duas perguntas, um fio condutor (0:20)

### O que declaramos × o que fazemos

- **Parte A:** grupos que votam mais também **querem** participar mais da política?
- **Parte B:** a inadimplência reage **na hora** aos juros, ou existe um **atraso**?
- Em ambos os casos: **percepção/decisão declarada vs. comportamento real ao longo do
  tempo**

🎤 **Narração:** *"As duas partes investigam o mesmo tipo de pergunta: existe uma diferença
entre o que se espera — vontade política, reação imediata aos juros — e o que
efetivamente acontece? Vamos começar pela Parte A."*

---

## Slide 3 — Dados e método (Parte A) (0:25)

### Duas bases, uma ponte

| Base | O que mede | Tamanho |
|------|-----------|---------|
| **CESOP/04832** | Opinião declarada | 2.000 respondentes |
| **TSE 2022** | Comportamento real | 8,8 mi linhas / 311,5 mi aptos |

- **Ponte:** dimensões comuns (região, escolaridade, idade) → **comparação ecológica**
- **Ferramentas:** Python (pandas, NumPy, pyreadstat), Matplotlib/Seaborn
- **Estatística:** correlação de **Spearman (ρ)** entre grupos

🎤 **Narração:** *"De um lado, uma pesquisa de opinião com 2 mil pessoas; do outro, o
registro oficial de quase 9 milhões de linhas do TSE. Conectamos as duas por região,
escolaridade e idade, e medimos a associação com a correlação de Spearman."*

---

## Slide 4 — O que os brasileiros declaram (0:30)

### Quatro retratos da pesquisa (CESOP)

- 🗳️ **~65% não lembram** em quem votaram para o Legislativo em 2022
- 🏥 **Prioridades:** saúde (20%), emprego (15%); "ampliar participação política" é a
  **penúltima** (2%)
- 📱 **Fake news:** preferem **punir (67%)** a **regular (33%)**
- 🙅 **78% não têm nenhuma vontade** de participar da política local

> *Figura sugerida:* `reports/imagens/05_vontade_participar_politica.png`

🎤 **Narração:** *"Quatro achados que se reforçam: a maioria não lembra do voto, dá
prioridade mínima à participação, prefere soluções punitivas para fake news e — o mais
forte — 78% não têm nenhuma vontade de participar. O retrato é de baixo engajamento."*

---

## Slide 5 — O comportamento real (TSE 2022) (0:25)

### Quem comparece? A escolaridade separa

- Comparecimento nacional: **79,4%**
- Por **escolaridade**: de **49% (analfabetos)** a **88% (superior)** → ~25–38 p.p.
- Por **região**: amplitude de **apenas ~3 p.p.**

> *Figura sugerida:* `reports/imagens/06_taxa_comparecimento_escolaridade.png`

🎤 **Narração:** *"No comportamento real, a escolaridade discrimina muitíssimo mais que a
geografia: quase 40 pontos entre analfabetos e superior, contra só 3 pontos entre regiões.
Primeira pista de que o eixo estruturante é a escolaridade."*

---

## Slide 6 — 🎯 Núcleo A: escolaridade ALINHA (0:30)

### Percepção e comportamento sobem juntos — ρ = 1,00

- A cada degrau de escolaridade sobem **juntos**:
  - Comparecimento real (TSE): ~62% → ~87%
  - Disposição declarada (CESOP): ~11% → ~34%
- **Correlação perfeita (ρ = 1,00)** — a renda reproduz o mesmo gradiente

> *Figura sugerida:* `reports/imagens/07_cruz_vontade_comparecimento_escolaridade.png`

🎤 **Narração:** *"Aqui está o achado central da Parte A. Por escolaridade, as duas bases
concordam perfeitamente: quanto mais escolaridade, mais a pessoa vota e mais quer
participar. Correlação de 1,00. E a renda mostra exatamente o mesmo padrão."*

---

## Slide 7 — 🎯 Núcleo A: região DISSOCIA (0:25)

### Por região, percepção e comportamento se OPÕEM — ρ = −0,30

- **Sul:** vota mais (~81%), mas é o **mais desinteressado** (~81% "nenhuma vontade")
- **Norte:** declara a **maior lembrança de voto (41%)**, mas tem o **menor comparecimento
  real (78%)**
- **Comparecer ≠ querer participar**

> *Figura sugerida:* `reports/imagens/07_cruz_vontade_comparecimento_regiao.png`

🎤 **Narração:** *"Já pela região, o resultado se inverte: correlação negativa. O Sul vota
muito mas é o que menos quer participar; o Norte é o oposto. Geografia confunde —
comparecer não é o mesmo que se engajar."*

---

## Slide 8 — Conclusão Parte A (0:20)

### É a escolaridade — não a região — que organiza o engajamento

- **Único eixo** em que opinião e comportamento se **alinham** (ρ = 1,00)
- O brasileiro retratado **comparece** (79,4%), mas **não quer participar** e **não lembra**
  do voto → quer **resultados** mais que **tomar parte**

🎤 **Narração:** *"Resumindo a Parte A: no Brasil, é a escolaridade que organiza o
engajamento político — o único eixo em que o que se diz e o que se faz coincidem. Agora,
vamos para a Parte B, onde também buscamos uma defasagem entre decisão e efeito — só que
no crédito."*

---

## Slide 9 — Dados e método (Parte B) (0:25)

### Série temporal direto da fonte oficial

- **Fonte:** API do **Banco Central (SGS)** — séries em tempo real
  - Inadimplência PF (21082), Selic (4390), Câmbio/Dólar (3695)
- **Período:** mar/2011 a abr/2026 — **182 observações mensais**
- **Técnicas:** decomposição aditiva, correlação com *lags* de 3 e 6 meses, teste **ADF**
  (estacionariedade), modelo **ARIMA(1,1,1)**

🎤 **Narração:** *"Na Parte B, coletamos direto da API do Banco Central três séries
mensais desde 2011: a inadimplência das famílias, a Selic e o dólar. Com 182 meses de
dados, decompomos a série, testamos correlações com defasagem e ajustamos um modelo
ARIMA para projeção."*

---

## Slide 10 — Evolução histórica e sazonalidade (0:30)

### Ciclos macroeconômicos + calendário das famílias

- **Ciclos:** alta na crise 2015–2017, queda 2018–2020, **queda atípica na pandemia**
  (auxílios/renegociações), **pico histórico em 2026**
- **Sazonalidade mensal:**
  - Pico de estresse: **abril (3,31%) e maio (3,29%)** — "ressaca" de IPVA/IPTU/material
    escolar
  - Alívio: **dezembro (3,09%)** — efeito do **13º salário**

> *Figura sugerida:* gráfico de decomposição / boxplot mensal gerado em
> `notebooks/03_parte_b.ipynb`

🎤 **Narração:** *"A inadimplência segue os grandes ciclos econômicos do país — inclusive
uma queda contraintuitiva na pandemia, por causa dos auxílios. Mas também existe um
padrão sazonal todo ano: aperta em abril e maio, e alivia em dezembro com o 13º."*

---

## Slide 11 — 🎯 Núcleo B: o efeito defasado da Selic (0:30)

### A Selic de hoje é o calote de dentro de 6 meses

| Variável | Correlação com inadimplência |
|----------|:---:|
| Selic (mês corrente) | 0,527 |
| Selic — 3 meses | 0,673 |
| **Selic — 6 meses** | **0,774** |
| Dólar (qualquer defasagem) | −0,21 a −0,28 (fraco) |

- **Dólar** tem efeito **fraco e indireto** (via inflação → Selic)

🎤 **Narração:** *"Este é o achado central da Parte B: a correlação com a Selic cresce
conforme a defasagem aumenta, chegando a 0,77 com seis meses. Ou seja, quando o Banco
Central sobe os juros hoje, o calote vai aparecer com força só dentro de seis meses — as
famílias seguram as contas até esgotar as reservas. O dólar, por sua vez, tem efeito
fraco e só indireto, via inflação."*

---

## Slide 12 — ARIMA: o que vem pela frente (0:25)

### Série não-estacionária → diferenciada → projeção de alta

- Teste **ADF**: série original **não-estacionária** (p = 0,272); diferenciada
  **estacionária** (p = 0,036)
- Modelo **ARIMA(1,1,1)**: coeficientes significativos, resíduos sem autocorrelação
  (Ljung-Box p = 0,47)
- **Forecast (6 meses): tendência de alta contínua**

> *Figura sugerida:* gráfico de forecast ARIMA gerado em `notebooks/03_parte_b.ipynb`

🎤 **Narração:** *"Como a série bruta tem tendência, diferenciamos para torná-la
estacionária e ajustamos um ARIMA(1,1,1), que passou em todos os testes de qualidade. A
projeção para os próximos seis meses aponta alta contínua — coerente com o efeito
defasado da Selic que vimos no slide anterior: os juros altos do último semestre ainda vão
pressionar o orçamento das famílias."*

---

## Slide 13 — Conclusão geral + encerramento (0:25)

### Duas defasagens, um Brasil

- **Parte A:** votar é obrigatório, mas **querer participar** depende da **escolaridade**
  — não da região
- **Parte B:** o calote de hoje reflete a **Selic de 6 meses atrás** — política monetária
  tem efeito **lento**
- **Limitações:** comparação ecológica (A) e modelo univariado (B) — caminhos para
  trabalhos futuros

# Obrigado!
*Perguntas?*

🎤 **Narração:** *"Em ambas as partes, o que vemos hoje é reflexo de algo que vem de mais
atrás: o nível educacional acumulado, no caso da democracia, e a política de juros de
meses anteriores, no caso do crédito. Entender essas defasagens é essencial para políticas
públicas mais eficazes. Obrigado!"*
