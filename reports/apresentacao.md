# Apresentação — Percepção dos Brasileiros Acerca da Democracia
### Roteiro de slides (limite: 5 minutos)

> **Como usar este arquivo:** cada bloco separado por `---` é um slide. Os tópicos em
> **negrito** são o que entra no slide; o texto em *itálico* sob "🎤 Narração" é o que você
> fala (não vai na tela). O tempo sugerido por slide soma ~5 min. Mantenha os slides
> enxutos — fale os detalhes, não os escreva.

**Orçamento de tempo (10 slides ≈ 5 min):**

| # | Slide | Tempo |
|---|-------|:---:|
| 1 | Capa | 0:15 |
| 2 | A pergunta | 0:30 |
| 3 | Dados e método | 0:40 |
| 4 | O que os brasileiros declaram | 0:45 |
| 5 | O comportamento real (TSE) | 0:35 |
| 6 | 🎯 Escolaridade alinha (ρ = 1,00) | 0:45 |
| 7 | 🎯 Região dissocia (ρ = −0,30) | 0:40 |
| 8 | Idade: o U-invertido | 0:25 |
| 9 | Conclusão | 0:35 |
| 10 | Limitações + encerramento | 0:20 |

---

## Slide 1 — Capa (0:15)

# Percepção dos Brasileiros Acerca da Democracia
### Opinião declarada (CESOP) × comportamento eleitoral real (TSE 2022)

- Projeto I — EDA | Ciência de Dados | IMT
- *[Nomes dos integrantes do grupo]*

🎤 **Narração:** *"Nosso projeto cruza o que os brasileiros **dizem** sobre política com
o que eles **fazem** nas urnas — opinião declarada versus comportamento real."*

---

## Slide 2 — A pergunta (0:30)

### O voto obrigatório esconde o engajamento real?

- No Brasil, votar é **obrigação legal** para a maioria.
- **Pergunta norteadora:** grupos que votam mais também **querem** participar mais da
  política? Onde percepção e comportamento se **alinham** — ou **divergem**?

🎤 **Narração:** *"Como o voto é obrigatório, alto comparecimento não prova interesse.
Queríamos saber: disposição declarada e participação real andam juntas? E em quais
dimensões — região, escolaridade, idade — elas se separam?"*

---

## Slide 3 — Dados e método (0:40)

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

## Slide 4 — O que os brasileiros declaram (0:45)

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

## Slide 5 — O comportamento real (TSE 2022) (0:35)

### Quem comparece? A escolaridade separa

- Comparecimento nacional: **79,4%**
- Por **escolaridade**: de **49% (analfabetos)** a **88% (superior)** → ~25–38 p.p.
- Por **região**: amplitude de **apenas ~3 p.p.**

> *Figura sugerida:* `reports/imagens/06_taxa_comparecimento_escolaridade.png`

🎤 **Narração:** *"No comportamento real, a escolaridade discrimina muitíssimo mais que a
geografia: quase 40 pontos entre analfabetos e superior, contra só 3 pontos entre regiões.
Primeira pista de que o eixo estruturante é a escolaridade."*

---

## Slide 6 — 🎯 Núcleo: escolaridade ALINHA (0:45)

### Percepção e comportamento sobem juntos — ρ = 1,00

- A cada degrau de escolaridade sobem **juntos**:
  - Comparecimento real (TSE): ~62% → ~87%
  - Disposição declarada (CESOP): ~11% → ~34%
- **Correlação perfeita (ρ = 1,00)** — a renda reproduz o mesmo gradiente

> *Figura sugerida:* `reports/imagens/07_cruz_vontade_comparecimento_escolaridade.png`

🎤 **Narração:** *"Aqui está o achado central. Por escolaridade, as duas bases concordam
perfeitamente: quanto mais escolaridade, mais a pessoa vota **e** mais quer participar.
Correlação de 1,00. E a renda mostra exatamente o mesmo padrão — o que reforça o
resultado."*

---

## Slide 7 — 🎯 Núcleo: região DISSOCIA (0:40)

### Por região, percepção e comportamento se OPÕEM — ρ = −0,30

- **Sul:** vota mais (~81%), mas é o **mais desinteressado** (~81% "nenhuma vontade")
- **Norte:** declara a **maior lembrança de voto (41%)**, mas tem o **menor comparecimento
  real (78%)**
- **Comparecer ≠ querer participar**

> *Figura sugerida:* `reports/imagens/07_cruz_vontade_comparecimento_regiao.png`

🎤 **Narração:** *"Já pela região, o resultado se inverte: correlação negativa. O Sul vota
muito mas é o que menos quer participar; o Norte é o oposto. Ou seja, geografia confunde —
comparecer não é o mesmo que se engajar."*

---

## Slide 8 — Idade: o U-invertido (0:25)

### Engajamento concentrado na meia-idade

- Sobe até **35–54 anos**, cai nas **duas pontas**
- Jovens 16–17: **comparecem (~83%)** mas sem interesse (~9%) → voto facultativo
- Idosos 65+: caem nas **duas** medidas

🎤 **Narração:** *"A idade é um segundo eixo: o engajamento forma um U-invertido — pico na
meia-idade e queda nos extremos. Jovens votam por facultatividade, sem interesse; idosos
recuam nas duas pontas."*

---

## Slide 9 — Conclusão (0:35)

### É a escolaridade — não a região — que organiza o engajamento

- **Único eixo** em que opinião e comportamento se **alinham** (ρ = 1,00)
- O brasileiro retratado **comparece** (79,4%), mas **não quer participar** e **não lembra**
  do voto → quer **resultados** mais que **tomar parte**

🎤 **Narração:** *"A tese do trabalho: no Brasil, é a escolaridade que organiza o
engajamento político — o único eixo em que o que se diz e o que se faz coincidem. O
eleitorado comparece, mas em sua maioria não deseja participar: quer resultados, não
processo."*

---

## Slide 10 — Limitações e encerramento (0:20)

### Honestidade metodológica

- **Comparação ecológica:** as bases não medem os mesmos indivíduos (sem conclusão
  individual)
- **Viés amostral:** a CESOP super-representa os mais escolarizados → engajamento real
  tende a ser **ainda menor**

# Obrigado!
*Perguntas?*

🎤 **Narração:** *"Por fim, duas ressalvas: a comparação é entre grupos, não indivíduos; e
a amostra puxa para os mais escolarizados, então o desinteresse real deve ser ainda maior.
Obrigado!"*

---

> **Dicas de ensaio:**
> - Cronometre: os slides 6 e 7 (o núcleo) merecem o maior tempo; corte detalhe dos slides
>   3–5 se estourar.
> - Use **uma figura grande por slide** nos slides 4–8; deixe o texto curto.
> - Frase de efeito para fechar a defesa: *"Votar é obrigatório; querer participar, não — e
>   é a escolaridade que faz a diferença."*
