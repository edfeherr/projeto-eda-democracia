# Data Dictionary — TSE: Comparecimento e Abstenção 2022

## 1. Visão Geral

Este documento descreve as variáveis do dataset público do **Tribunal Superior Eleitoral (TSE)** referente ao tema **Comparecimento e Abstenção — Eleições 2022**.

O objetivo deste dicionário é apoiar o projeto de EDA sobre **Percepção dos Brasileiros acerca da Democracia**.

---

## 2. Uso no Projeto

A base do TSE será utilizada como **base pública complementar** à pesquisa do CESOP.

A principal hipótese de integração é comparar indicadores declarados de participação política na pesquisa CESOP com indicadores eleitorais observados no TSE.

---

## 3. Variáveis Administrativas

### DT_GERACAO

**Descrição:** Data da extração dos dados para geração do arquivo.


---

### HH_GERACAO

**Descrição:** Hora da extração dos dados para geração do arquivo, com base no horário de Brasília.

---

### ANO_ELEICAO

**Descrição:** Ano de referência da eleição para geração do arquivo.

---

### NR_TURNO

**Descrição:** Número do turno da eleição.

| Código | Significado |
|---:|---|
| 1 | Primeiro turno |
| 2 | Segundo turno |

---

## 4. Variáveis Geográficas

### SG_UF

**Descrição:** Sigla da unidade da federação onde ocorreu a eleição.

---

### CD_MUNICIPIO

**Descrição:** Código TSE do município onde ocorreu a eleição.

---

### NM_MUNICIPIO

**Descrição:** Nome do município onde ocorreu a eleição.

---

### NR_ZONA

**Descrição:** Número da zona eleitoral em que ocorreu a eleição.

---

## 5. Variáveis de Gênero

### CD_GENERO

**Descrição:** Código do gênero da eleitora ou eleitor.

| TSE `CD_GENERO` | TSE `DS_GENERO` | 
|---:|---|
| 2 | Masculino |
| 4 | Feminino | 

---

### DS_GENERO

**Descrição:** Descrição do gênero da eleitora ou eleitor.

---

## 6. Variáveis de Estado Civil

### CD_ESTADO_CIVIL

**Descrição:** Código do estado civil da eleitora ou eleitor.

| Código | Valor |
|---:|---|
| 1 | Solteiro(a) |
| 3 | Casado(a) |
| 5 | Viúvo(a) |
| 7 | Separado(a) judicialmente |
| 9 | Divorciado(a) |

---

### DS_ESTADO_CIVIL

**Descrição:** Descrição do estado civil da eleitora ou eleitor.

---

## 7. Variáveis de Faixa Etária

### CD_FAIXA_ETARIA

**Descrição:** Código da faixa etária da eleitora ou eleitor.

---

### DS_FAIXA_ETARIA
 
**Descrição:** Descrição da faixa etária da eleitora ou eleitor.

---

## 8. Variáveis de Grau de Instrução

### CD_GRAU_INSTRUCAO

**Descrição:** Código do grau de instrução da eleitora ou eleitor.

| Código | Valor |
|---:|---|
| 1 | Analfabeto |
| 2 | Lê e escreve |
| 3 | Ensino fundamental incompleto |
| 4 | Ensino fundamental completo |
| 5 | Ensino médio incompleto |
| 6 | Ensino médio completo |
| 7 | Superior incompleto |
| 8 | Superior completo |

---

### DS_GRAU_INSTRUCAO

**Descrição:** Descrição do grau de instrução da eleitora ou eleitor.

---

## 9. Variáveis de Raça/Cor

### CD_COR_RACA

**Descrição:** Código da raça/cor da eleitora ou eleitor.

| Código | Valor |
|---:|---|
| 1 | Branca |
| 2 | Preta |
| 3 | Parda |
| 4 | Amarela |
| 5 | Indígena |
| 6 | Não informado |

---

### DS_COR_RACA

**Descrição:** Descrição da raça/cor da eleitora ou eleitor.

---

## 10. Variáveis de Populações Específicas

### CD_QUILOMBOLA

**Descrição:** Código que indica se a eleitora ou o eleitor é de origem de grupo quilombola.

| Código | Valor |
|---:|---|
| 1 | Sim |
| 2 | Não |

---

### DS_QUILOMBOLA

**Descrição:** Indica se a eleitora ou o eleitor é de origem de grupo quilombola.

---

### CD_INTERPRETE_LIBRAS

**Descrição:** Código que indica se a eleitora ou o eleitor é intérprete de Libras.

| Código | Valor |
|---:|---|
| 1 | Sim |
| 2 | Não |

---

### DS_INTERPRETE_LIBRAS

**Descrição:** Indica se a eleitora ou o eleitor é intérprete de Libras.

---

### CD_IDENTIDADE_GENERO
  
**Descrição:** Código do gênero ao qual a eleitora ou eleitor se identifica.

| Código | Valor |
|---:|---|
| 1 | Cisgênero |
| 2 | Transgênero |
| 3 | Prefere não informar |
| -1 | Não informado |

---

### DS_IDENTIDADE_GENERO

**Descrição:** Descrição do gênero ao qual a eleitora ou eleitor se identifica.

---

### CD_IDIOMA_INDIGENA
  
**Descrição:** Código que indica se a eleitora ou eleitor domina alguma língua indígena.

| Código | Valor |
|---:|---|
| 1 | Sim |
| 2 | Não |

---

### DS_IDIOMA_INDIGENA

**Descrição:** Indica se a eleitora ou eleitor domina alguma língua indígena.

---

### CD_GRUPO_INDIGENA
 
**Descrição:** Código da etnia, povo ou grupo indígena ao qual a eleitora ou eleitor pertence.

---

### DS_GRUPO_INDIGENA

**Descrição:** Descrição da etnia, povo ou grupo indígena ao qual a eleitora ou eleitor pertence.

---

### CD_IDIOMA_QUILOMBOLA

**Descrição:** Código que indica se a eleitora ou eleitor domina alguma língua quilombola.

| Código | Valor |
|---:|---|
| 1 | Sim |
| 2 | Não |

---

### DS_IDIOMA_QUILOMBOLA

**Descrição:** Indica se a eleitora ou eleitor domina alguma língua quilombola.

---

## 11. Variáveis de Comparecimento e Abstenção

### QT_APTOS
 
**Descrição:** Quantidade de eleitoras ou eleitores aptos a votar.

---

### QT_COMPARECIMENTO

**Descrição:** Quantidade de eleitoras ou eleitores aptos que compareceram para votar.

---

### QT_ABSTENCAO

**Descrição:** Quantidade de eleitoras ou eleitores aptos que não compareceram para votar.

---

### QT_COMPAREC_DEFICIENTES

**Descrição:** Quantidade de eleitoras ou eleitores aptos, com deficiência ou mobilidade reduzida, que compareceram para votar, conforme o perfil.

---

### QT_ABST_DEFICIENTES

**Descrição:** Quantidade de eleitoras ou eleitores aptos, com deficiência ou mobilidade reduzida, que não compareceram para votar, conforme o perfil.

---

### QT_COMPAREC_TTE
 
**Descrição:** Quantidade de eleitoras ou eleitores aptos que solicitaram transferência temporária de eleitor (TTE) e compareceram para votar.

---

### QT_ABST_TTE
  
**Descrição:** Quantidade de eleitoras ou eleitores aptos que solicitaram transferência temporária de eleitor (TTE) e não compareceram para votar.

---

### QT_COMPAREC_FACULTATIVO

**Descrição:** Quantidade de eleitoras ou eleitores com voto facultativo que compareceram para votar.

---

### QT_ABST_FACULTATIVO

**Descrição:** Quantidade de eleitoras ou eleitores com voto facultativo que não compareceram para votar.

---

### QT_COMPAREC_OBRIGATORIO

**Descrição:** Quantidade de eleitoras ou eleitores com voto obrigatório que compareceram para votar.

---

### QT_ABST_OBRIGATORIO

**Descrição:** Quantidade de eleitoras ou eleitores com voto obrigatório que não compareceram para votar.

---

### QT_COMPAREC_DEFIC_FACULTATIVO

**Descrição:** Quantidade de eleitoras ou eleitores aptos, com deficiência ou mobilidade reduzida, com voto facultativo e que compareceram para votar, conforme o perfil.

---

### QT_ABST_DEFIC_FACULTATIVO
 
**Descrição:** Quantidade de eleitoras ou eleitores aptos, com deficiência ou mobilidade reduzida, com voto facultativo e que não compareceram para votar, conforme o perfil.

---

### QT_COMPAREC_DEFIC_OBRIGATORIO

**Descrição:** Quantidade de eleitoras ou eleitores aptos, com deficiência ou mobilidade reduzida, com voto obrigatório e que compareceram para votar, conforme o perfil.

---

### QT_ABST_DEFIC_OBRIGATORIO
 
**Descrição:** Quantidade de eleitoras ou eleitores aptos, com deficiência ou mobilidade reduzida, com voto obrigatório e que não compareceram para votar, conforme o perfil.
