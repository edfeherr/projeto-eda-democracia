# Dicionário de Dados — TSE: Comparecimento e Abstenção 2022

## Como carregar a base

```python
colunas_tse = [
    'SG_UF', 'NM_MUNICIPIO',
    'DS_GENERO', 'DS_ESTADO_CIVIL',
    'DS_FAIXA_ETARIA', 'DS_GRAU_ESCOLARIDADE', 'DS_COR_RACA',
    'QT_APTOS', 'QT_COMPARECIMENTO', 'QT_ABSTENCAO',
]

df_tse = pd.read_csv(
    PATH_TSE_CSV,
    sep=';',
    encoding='latin1',
    usecols=colunas_tse,
)
```

**Arquivo:** `perfil_comparecimento_abstencao_2022_BRASIL.csv`  
**Separador:** `;`  
**Encoding:** `latin1`  
**Total de colunas no arquivo original:** 43 — carregamos apenas as 10 necessárias via `usecols=`.

---

## Colunas utilizadas

### SG_UF

Sigla da unidade da federação.

Exemplos: `AC`, `AL`, `AM`, `AP`, `BA`, `CE`, `DF`, `ES`, `GO`, `MA`, `MG`, `MS`, `MT`, `PA`, `PB`, `PE`, `PI`, `PR`, `RJ`, `RN`, `RO`, `RR`, `RS`, `SC`, `SE`, `SP`, `TO`.

---

### NM_MUNICIPIO

Nome do município.

Exemplos: `ABADIA DE GOIÁS`, `ABADIA DOS DOURADOS`, `ABADIÂNIA`.

---

### DS_GENERO

Gênero do eleitor ou eleitora.

| Valor |
|-------|
| `MASCULINO` |
| `FEMININO` |

---

### DS_ESTADO_CIVIL

Estado civil do eleitor ou eleitora.

| Valor |
|-------|
| `SOLTEIRO` |
| `CASADO` |
| `VIÚVO` |
| `SEPARADO JUDICIALMENTE` |
| `DIVORCIADO` |

---

### DS_FAIXA_ETARIA

Faixa etária do eleitor ou eleitora. Os valores são idades individuais em anos.

Exemplos: `16 anos`, `17 anos`, `18 anos`, `19 anos`, ..., `70 anos`.

---

### DS_GRAU_ESCOLARIDADE

Grau de escolaridade do eleitor ou eleitora.

| Valor |
|-------|
| `ANALFABETO` |
| `LÊ E ESCREVE` |
| `ENSINO FUNDAMENTAL INCOMPLETO` |
| `ENSINO FUNDAMENTAL COMPLETO` |
| `ENSINO MÉDIO INCOMPLETO` |
| `ENSINO MÉDIO COMPLETO` |
| `SUPERIOR INCOMPLETO` |
| `SUPERIOR COMPLETO` |

---

### DS_COR_RACA

Raça/cor do eleitor ou eleitora.

| Valor |
|-------|
| `BRANCA` |
| `PRETA` |
| `PARDA` |
| `AMARELA` |
| `INDÍGENA` |
| `NÃO INFORMADO` |

---

### QT_APTOS

Quantidade de eleitores aptos a votar no grupo (combinação de município + zona + perfil demográfico).

Tipo: inteiro.

---

### QT_COMPARECIMENTO

Quantidade de eleitores aptos que compareceram para votar.

Tipo: inteiro.

---

### QT_ABSTENCAO

Quantidade de eleitores aptos que não compareceram para votar.

Tipo: inteiro.


