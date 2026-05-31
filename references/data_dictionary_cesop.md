# Dicionário de Dados — Projeto EDA: Percepção dos Brasileiros acerca da Democracia

## 1. Informações Gerais

Este arquivo documenta as variáveis disponíveis no dataset utilizado no projeto de **Exploratory Data Analysis (EDA)** sobre a **percepção dos brasileiros acerca da democracia**.

- **Fonte principal:** CESOP — Centro de Estudos de Opinião Pública
- **Arquivo principal:** `04832.SAV`
- **Formato original:** SPSS (`.sav`)
- **Tema:** Percepção dos brasileiros acerca da democracia
- **Objetivo do dicionário:** registrar o significado das colunas e dos códigos numéricos utilizados na base.

> **Nota:** Os labels neste dicionário foram extraídos diretamente de `meta.variable_value_labels` via `pyreadstat` e refletem exatamente o que consta no arquivo SPSS.

---

## 2. Observações Metodológicas

A base contém variáveis codificadas numericamente. Portanto, várias colunas apresentam valores como `1`, `2`, `3`, `99`, etc., que representam categorias qualitativas.

---

# 3. Variáveis Demográficas

## 3.1 `SEXO`

**Descrição:** Sexo do entrevistado.

| Código | Label no SPSS |
|---:|---|
| 1 | MAS |
| 2 | FEM |

---

## 3.2 `FX_ID`

**Descrição:** Faixa etária do entrevistado.

| Código | Label no SPSS |
|---:|---|
| 1 | 16 E 17 |
| 2 | 18 A 24 |
| 3 | 25 A 34 |
| 4 | 35 A 44 |
| 5 | 45 A 54 |
| 6 | 55 A 64 |
| 7 | 65 E MAIS |

---

## 3.3 `ESCOLARIDADE`

**Descrição:** Escolaridade declarada pelo entrevistado.

| Código | Label no SPSS |
|---:|---|
| 1 | Analfabeto |
| 2 | Sabe ler/ escrever, mas não cursou escola |
| 3 | Pré-escola (ou 1º ano) |
| 4 | 1ª série (ou 2º ano) |
| 5 | 2ª série (ou 3º ano) |
| 6 | 3ª série (ou 4º ano) |
| 7 | 4ª série (ou 5º ano) |
| 8 | 5ª série (ou 6º ano) |
| 9 | 6ª série (ou 7º ano) |
| 10 | 7ª série (ou 8º ano) |
| 11 | 8ª série (ou 9º ano) |
| 12 | 1ª série |
| 13 | 2ª série |
| 14 | 3ª série |
| 15 | Superior incompleto |
| 16 | Superior completo |

---

## 3.4 `RACA`

**Descrição:** Raça/cor autodeclarada pelo entrevistado.

| Código | Label no SPSS |
|---:|---|
| 1 | Branca |
| 2 | Preta |
| 3 | Parda |
| 4 | Amarela |
| 5 | Indígena |
| 9 | Não respondeu |

---

## 3.5 `RELIGIAO`

**Descrição:** Religião declarada pelo entrevistado.

| Código | Label no SPSS |
|---:|---|
| 1 | Católica Apostólica Romana |
| 2 | Assembléia de Deus |
| 3 | Batista/ Metodista/ Presbiteriana |
| 4 | Universal do Reino de Deus |
| 5 | Deus é Amor |
| 6 | Evangelho Quadrangular |
| 7 | Igreja Internacional da Graça |
| 8 | Renascer em Cristo |
| 9 | Sara nossa terra |
| 10 | Outras Evangélicas específicas |
| 11 | Evangélica - Não sabe especificar |
| 12 | Adventista |
| 13 | Testemunha de Jeová |
| 14 | Judaica |
| 15 | Espírita/ Kardecista |
| 16 | Afro-Brasileiras (Umbanda, Candomblé, etc) |
| 17 | Orientais (Budismo, Islamismo, etc) |
| 18 | Outras religiões |
| 19 | É religioso mas não segue nenhuma/ Agnóstico |
| 20 | Ateu, não tem religião |
| 99 | Não respondeu |

---

# 4. Variáveis Econômicas

## 4.1 `REND1`

**Descrição:** Renda pessoal do entrevistado, em salários mínimos.

| Código | Label no SPSS |
|---:|---|
| 1 | MAIS DE 20 |
| 2 | MAIS DE 10 A 20 |
| 3 | MAIS DE 5 A 10 |
| 4 | MAIS DE 2 A 5 |
| 5 | MAIS DE 1 A 2 |
| 6 | ATÉ 1 |
| 98 | NÃO TEM RENDIMENTO PESSOAL |
| 99 | NÃO RESPONDEU |

---

## 4.2 `REND2`

**Descrição:** Renda familiar do entrevistado, em salários mínimos.

| Código | Label no SPSS |
|---:|---|
| 1 | MAIS DE 20 |
| 2 | MAIS DE 10 A 20 |
| 3 | MAIS DE 5 A 10 |
| 4 | MAIS DE 2 A 5 |
| 5 | MAIS DE 1 A 2 |
| 6 | ATÉ 1 |
| 98 | NÃO TEM RENDIMENTO PESSOAL |
| 99 | NÃO RESPONDEU |

---

# 5. Variáveis Geográficas

## 5.1 `REGIAO`

**Descrição:** Região do Brasil onde o entrevistado reside.

| Código | Label no SPSS |
|---:|---|
| 1 | NORTE |
| 2 | NORDESTE |
| 3 | SUDESTE |
| 4 | SUL |
| 5 | CENTRO OESTE |

---

## 5.2 `COND`

**Descrição:** Condição do município.

| Código | Label no SPSS |
|---:|---|
| 1 | CAPITAL |
| 2 | PERIFERIA |
| 3 | INTERIOR |

---

## 5.3 `PORTE`

**Descrição:** Porte do município em número de habitantes.

| Código | Label no SPSS |
|---:|---|
| 1 | ATÉ 5.000 |
| 2 | DE 5.001 A 10.000 |
| 3 | DE 10.001 A 20.000 |
| 4 | DE 20.001 A 50.000 |
| 5 | DE 50.001 A 100.000 |
| 6 | DE 100.001 A 500.000 |
| 7 | MAIS DE 500.000 |

---

# 6. Perguntas da Pesquisa

## 6.1 `P01A`

**Pergunta:**
Você se lembra em quem votou para deputado(a) estadual nas eleições gerais de 2022?

| Código | Label no SPSS |
|---:|---|
| 1 | Sim |
| 2 | Não |
| 3 | Não votou em 2022 (Esp.) |
| 99 | Não respondeu |

---

## 6.2 `P01B`

**Pergunta:**
Você se lembra em quem votou para deputado(a) federal nas eleições gerais de 2022?

| Código | Label no SPSS |
|---:|---|
| 1 | Sim |
| 2 | Não |
| 3 | Não votou em 2022 (Esp.) |
| 99 | Não respondeu |

---

## 6.3 `P01C`

**Pergunta:**
Você se lembra em quem votou para senador(a) nas eleições gerais de 2022?

| Código | Label no SPSS |
|---:|---|
| 1 | Sim |
| 2 | Não |
| 3 | Não votou em 2022 (Esp.) |
| 99 | Não respondeu |

---

## 6.4 `P02_1`, `P02_2`, `P02_3`

**Pergunta:**
Qual destas propostas você acha que deveria ser prioridade de um(a) político(a)? (1º, 2º e 3º lugar)

| Código | Label no SPSS |
|---:|---|
| 1 | Reduzir as desigualdades sociais |
| 2 | Combater o preconceito (racismo, homofobia, diferença de classe social, etc.) |
| 3 | Aumentar os impostos de grandes fortunas (ou dos mais ricos) |
| 4 | Incentivar a geração de empregos |
| 5 | Combater as mudanças climáticas/desmatamento |
| 6 | Ampliar o uso de energias renováveis |
| 7 | Preservar os valores ligados à família |
| 8 | Defender a igualdade entre homens e mulheres |
| 9 | Melhorar a qualidade da saúde |
| 10 | Melhorar a qualidade da educação |
| 11 | Reduzir a violência |
| 12 | Ampliar os espaços de participação política da população |
| 99 | Não sabe/ Não respondeu |

---

## 6.5 `P03_1` a `P03_6`

**Pergunta:**
Algumas pessoas dizem que a divulgação de fake news — notícias ou conteúdos falsos — pode prejudicar a democracia. Quais dessas opções você acredita que poderiam contribuir no combate à divulgação de fake news?

(`P03_1` = 1ª menção, `P03_2`…`P03_6` = menções adicionais espontâneas)

| Código | Label no SPSS |
|---:|---|
| 1 | Ampliar a regulamentação, as regras a serem cumpridas pelas plataformas digitais (Facebook, Youtube, WhatsApp, etc.) |
| 2 | Responsabilizar e punir as empresas de tecnologia/comunicação que não removerem postagens com conteúdos falsos |
| 3 | Ampliar a regulamentação para usuários que divulgam fake news, criadas por eles próprios ou por terceiros |
| 4 | Responsabilizar e punir os usuários que divulgam ou compartilham postagens com notícias ou conteúdos falsos |
| 5 | Ampliar a regulamentação para políticos que divulgam fake news, criadas por eles próprios ou por terceiros |
| 6 | Responsabilizar, punir ou caçar políticos que divulgam ou compartilham postagens com notícias ou conteúdos falsos |
| 99 | Não sabe/ Não respondeu |

---

## 6.6 `P04`

**Pergunta:**
Você diria que tem muita vontade, alguma vontade ou nenhuma vontade de participar da vida política na sua cidade?

| Código | Label no SPSS |
|---:|---|
| 1 | Muita vontade |
| 2 | Alguma vontade |
| 3 | Nenhuma vontade |
| 99 | Não sabe/ Não respondeu |

---

# 7. Variáveis Técnicas

## 7.1 `ID_Ipec`

**Descrição:** Identificador do entrevistado ou registro na base. Sem value labels.

---

## 7.2 `DATA_ENTREVISTA`

**Descrição:** Data da entrevista. Sem value labels (string no SPSS, convertida para datetime no pipeline).

---

## 7.3 `TIPO_COLETA`

**Descrição:** Tipo ou método de coleta da entrevista.

| Código | Label no SPSS |
|---:|---|
| 1 | Face a face |

---

## 7.4 `IDADE`

**Descrição:** Idade do entrevistado em anos completos. Variável numérica contínua. Sem value labels.
