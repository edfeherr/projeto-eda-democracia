# Projeto EDA — Percepção dos Brasileiros Acerca da Democracia e Inadimplência no Brasil

## Entregáveis Principais

| Entregável | Link |
|------------|------|
| Apresentação Final (Parte A + Parte B) | [reports/envios/Projeto_EDA_ Democracia_e_Inadimplência_Brasil.pdf](reports/envios/Projeto_EDA_%20Democracia_e_Inadimplência_Brasil.pdf) |
| Relatório Final — Parte A (Democracia) | [reports/envios/relatorio_a.pdf](reports/envios/relatorio_a.pdf) |
| Relatório Final — Parte B (Inadimplência) | [reports/envios/relatorio_b.pdf](reports/envios/relatorio_b.pdf) |
| Vídeo | [reports/envios/](reports/envios/relatorio_b.pdf) |

---

## 1. Visão Geral

Este repositório contém o desenvolvimento do **Projeto I — EDA (Exploratory Data Analysis)** da disciplina de **Ciência de Dados**, dividido em duas partes:

- **Parte A — Percepção dos Brasileiros Acerca da Democracia**: análise exploratória de uma pesquisa de opinião do **CESOP — Centro de Estudos de Opinião Pública**, correlacionada com dados eleitorais reais do **TSE — Tribunal Superior Eleitoral** (comparecimento e abstenção nas eleições de 2022).
- **Parte B — Inadimplência de Pessoas Físicas no Brasil**: análise de série temporal da taxa de inadimplência de pessoas físicas, relacionando-a com a Taxa Selic e a taxa de câmbio (dólar), com dados obtidos diretamente da API do **Banco Central do Brasil (SGS)**.

A conexão entre as duas partes é o tema geral do projeto: como percepções e comportamentos declarados pelos brasileiros (Parte A) e indicadores econômicos observados (Parte B) podem ser analisados com as mesmas ferramentas de ciência de dados — carregamento, limpeza, análise exploratória, visualização e técnicas estatísticas.

---

## 2. Temas do Projeto

### 2.1 Parte A — Percepção dos Brasileiros Acerca da Democracia

O projeto busca investigar questões como:

- Qual é o perfil sociodemográfico dos entrevistados?
- Os brasileiros se lembram em quem votaram nas eleições gerais de 2022?
- Quais propostas são consideradas prioridade para políticos?
- Quais medidas são consideradas importantes no combate à disseminação de fake news?
- Qual é o nível de vontade dos brasileiros de participar da vida política em sua cidade?
- Há relação entre a percepção declarada de participação política e os dados reais de comparecimento/abstenção eleitoral?

### 2.2 Parte B — Inadimplência de Pessoas Físicas no Brasil

O projeto busca investigar questões como:

- Como evoluiu a taxa de inadimplência de pessoas físicas no Brasil desde 2011?
- Existe sazonalidade no comportamento da inadimplência ao longo do ano?
- Qual a relação entre a Taxa Selic, o câmbio (dólar) e a inadimplência, inclusive considerando defasagens (lags) temporais?
- A série de inadimplência é estacionária? Como ela pode ser modelada e projetada (ARIMA) para os próximos meses?

---

## 3. Bases de Dados Utilizadas

O projeto utiliza três fontes de dados: duas para a Parte A e uma para a Parte B.

### 3.1 Base principal (Parte A) — CESOP

Fonte:

**CESOP — Centro de Estudos de Opinião Pública**

Arquivos principais:

```text
04832.SAV
quest_04832.pdf
TF_04832.pdf
```

Descrição:

A base do CESOP contém dados de uma pesquisa de opinião pública sobre democracia, participação política, prioridades políticas, fake news, lembrança de voto e características sociodemográficas dos entrevistados.

Principais grupos de variáveis:

- Sexo
- Idade e faixa etária
- Escolaridade
- Raça/cor
- Religião
- Renda pessoal
- Renda familiar
- Região
- Condição do município
- Porte do município
- Lembrança de voto em 2022
- Prioridades políticas
- Opinião sobre combate às fake news
- Vontade de participar da vida política local

---

### 3.2 Base complementar (Parte A) — TSE

Fonte:

**TSE — Tribunal Superior Eleitoral**

Tema:

**Perfil de Comparecimento e Abstenção — Eleições Gerais 2022**

Descrição:

A base do TSE contém informações sobre eleitores aptos, comparecimento e abstenção nas eleições de 2022, segmentadas por variáveis como estado, município, zona eleitoral, gênero, faixa etária, grau de instrução, raça/cor e outras características.

Principais variáveis utilizadas:

- Unidade da Federação
- Município
- Gênero
- Faixa etária
- Grau de instrução
- Raça/cor
- Quantidade de eleitores aptos
- Quantidade de eleitores que compareceram
- Quantidade de eleitores que se abstiveram

A base complementar é usada para comparar a **participação política declarada** na pesquisa do CESOP com a **participação eleitoral observada** nos dados oficiais do TSE.

---

### 3.3 Base utilizada na Parte B — Banco Central do Brasil (SGS)

Fonte:

**Banco Central do Brasil — Sistema Gerenciador de Séries Temporais (SGS)**, acessado diretamente via API pública (`https://api.bcb.gov.br/dados/serie/bcdata.sgs.<codigo>/dados?formato=json`).

Séries utilizadas:

- **21082** — Taxa de inadimplência das operações de crédito, recursos livres, pessoas físicas;
- **3695** — Taxa de câmbio (dólar);
- **4390** — Taxa Selic.

O período analisado começa em 2011, totalizando 182 observações mensais (até abril/2026). Diferentemente da Parte A, essa base **não precisa ser baixada manualmente**: o notebook `03_parte_b.ipynb` busca os dados diretamente da API no momento da execução.

---

## 4. Acesso aos Dados

### 4.1 Dados da Parte A (CESOP + TSE)

Os arquivos de dados brutos não são versionados diretamente neste repositório devido ao tamanho elevado das bases, especialmente os arquivos do TSE.

A pasta `raw/`, contendo os dados brutos do CESOP e do TSE, foi compactada e disponibilizada em uma pasta pública no Google Drive:

[Download dos dados brutos — Google Drive](https://drive.google.com/drive/folders/1nscFRJuwoXh2f1xIYhltxRY_OX_vMer7?usp=sharing)

Após o download, o arquivo compactado deve ser descompactado dentro da pasta `data/` do projeto. A estrutura esperada é:

```text
data/
  raw/
    cesop/
      04832.SAV
      quest_04832.pdf
      TF_04832.pdf

    tse/
      perfil_comparecimento_abstencao_2022/
        perfil_comparecimento_abstencao_2022_BRASIL.csv
        perfil_comparecimento_abstencao_2022_SP.csv
        perfil_comparecimento_abstencao_2022_MG.csv
        ...

  processed/
```

Mais detalhes sobre a organização dos dados estão disponíveis em [`data/README_DATA.md`](data/README_DATA.md).

### 4.2 Dados da Parte B (BCB SGS)

Não é necessário baixar nada: o notebook `03_parte_b.ipynb` consulta a API do BCB SGS diretamente durante a execução (ver seção 3.3). A pasta `data/parte_b/` contém cópias locais (`.csv`) das séries utilizadas, geradas a partir das mesmas séries do BCB.

---

## 5. Estrutura do Repositório

A estrutura principal do projeto está organizada da seguinte forma:

```text
Projeto_EDA_Democracia/

├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── README_DATA.md
│   ├── raw/            (CESOP/TSE — não versionado)
│   ├── processed/      (bases tratadas — não versionado)
│   └── parte_b/        (cópias locais das séries do BCB SGS)
│
├── notebooks/
│   ├── 01_exploracao_base_parte_a.ipynb
│   ├── 02_analises_exploratorias_parte_a.ipynb
│   └── 03_parte_b.ipynb
│
├── references/
│   ├── data_dictionary_cesop.md
│   └── data_dictionary_tse.md
│
├── reports/
│   ├── relatorio_a.md         (relatório — Parte A)
│   ├── relatorio_b.md         (relatório — Parte B)
│   ├── imagens/                (gráficos exportados pelos notebooks)
│   └── envios/                 (PDFs finais entregues — ver seção "Entregáveis Principais")
│
└── src/
```
---

## 6. Descrição das Pastas

### `data/`

Contém a documentação sobre os dados utilizados no projeto (`README_DATA.md`), além das subpastas `raw/`, `processed/` (CESOP/TSE, ignoradas pelo Git) e `parte_b/` (séries do BCB SGS).

---

### `notebooks/`

Contém os notebooks Jupyter utilizados no desenvolvimento do projeto:

- `01_exploracao_base_parte_a.ipynb`: carregamento, limpeza, recodificação e salvamento das bases CESOP e TSE em parquet (Parte A);
- `02_analises_exploratorias_parte_a.ipynb`: análises exploratórias univariadas, bivariadas e cruzamento CESOP × TSE (Parte A);
- `03_parte_b.ipynb`: coleta via API do BCB SGS, decomposição sazonal, análise de correlação/defasagens, testes de estacionariedade e modelagem ARIMA (Parte B).

---

### `references/`

Contém documentos auxiliares do projeto, como dicionários de dados e descrições das variáveis.

Arquivos principais:

- `data_dictionary_cesop.md`: dicionário de dados da base CESOP;
- `data_dictionary_tse.md`: dicionário de dados da base TSE.

A Parte B não possui dicionário próprio, pois utiliza diretamente as séries documentadas do BCB SGS (códigos 21082, 3695 e 4390 — ver seção 3.3).

---

### `reports/`

Pasta destinada aos artefatos finais do projeto:

- `relatorio_a.md` / `relatorio_b.md`: textos do trabalho (introdução, métodos, resultados, discussão) das Partes A e B;
- `apresentacao.md`: roteiro de apresentação cobrindo as duas partes em 5 minutos;
- `apresentacao.pptx`: slides gerados a partir do roteiro;
- `imagens/`: gráficos exportados pelos notebooks, usados nos relatórios;
- `envios/`: versões em PDF dos relatórios e da apresentação final, prontas para entrega (ver seção "Entregáveis Principais").

---

### `src/`

Pasta reservada para scripts Python reutilizáveis (funções de limpeza de dados, visualização e utilitários). Atualmente vazia — as funções auxiliares estão definidas diretamente nos notebooks.

---

## 7. Como Executar o Projeto

### 7.1 Clonar o repositório

```bash
git clone https://github.com/edfeherr/projeto-eda-democracia.git
```

Entrar na pasta do projeto:

```bash
cd Projeto_EDA_Democracia
```

---

### 7.2 Criar ambiente virtual

No Windows PowerShell:

```bash
python -m venv .venv
```

Ativar o ambiente virtual:

```bash
.\.venv\Scripts\Activate
```

---

### 7.3 Instalar dependências

Com o ambiente virtual ativado, execute:

```bash
pip install -r requirements.txt
```

---

### 7.4 Baixar os dados (somente Parte A)

Acesse o Google Drive público:

[Download dos dados brutos — Google Drive](https://drive.google.com/drive/folders/1nscFRJuwoXh2f1xIYhltxRY_OX_vMer7?usp=sharing)

Baixe o arquivo compactado contendo a pasta `raw/` e descompacte o conteúdo dentro de `data/`, ficando:

```text
data/raw/cesop/
data/raw/tse/
```

A Parte B não exige download: os dados são obtidos via API do BCB SGS no momento da execução do notebook (é necessária conexão com a internet).

---

### 7.5 Executar os notebooks

Após instalar as dependências e (para a Parte A) posicionar os dados corretamente, abra os notebooks na pasta `notebooks/`:

```text
01_exploracao_base_parte_a.ipynb
02_analises_exploratorias_parte_a.ipynb
03_parte_b.ipynb
```

Os notebooks `01` e `02` (Parte A) devem ser executados em ordem, pois o segundo depende dos arquivos parquet gerados pelo primeiro em `data/processed/`. O notebook `03` (Parte B) é autossuficiente e pode ser executado de forma independente.

---

## 8. Caminhos Utilizados nos Notebooks

### Parte A

Os notebooks assumem que os dados estão organizados em caminhos relativos.

Exemplo para leitura da base CESOP:

```python
import pyreadstat

df_cesop, meta = pyreadstat.read_sav("../data/raw/cesop/04832.SAV")
```

Exemplo para leitura da base TSE:

```python
import pandas as pd

df_tse = pd.read_csv(
    "../data/raw/tse/perfil_comparecimento_abstencao_2022/perfil_comparecimento_abstencao_2022_BRASIL.csv",
    sep=";",
    encoding="latin1"
)
```

Caso ocorra erro de caminho, verifique se a pasta `raw/` foi descompactada corretamente dentro de `data/`.

### Parte B

O notebook `03_parte_b.ipynb` busca os dados diretamente da API do BCB SGS, sem depender de arquivos locais:

```python
import pandas as pd

def get_bcb_data(codigo, nome_coluna):
    url = f'https://api.bcb.gov.br/dados/serie/bcdata.sgs.{codigo}/dados?formato=json'
    df = pd.read_json(url)
    df['data'] = pd.to_datetime(df['data'], format='%d/%m/%Y')
    df.set_index('data', inplace=True)
    df.columns = [nome_coluna]
    return df

inadimplencia = get_bcb_data(21082, 'Inadimplencia_PF')
dolar = get_bcb_data(3695, 'Dolar')
selic = get_bcb_data(4390, 'Selic')
```

---

## 9. Metodologia Geral

### Parte A

A metodologia segue uma abordagem de análise exploratória de dados:

1. Coleta e organização das bases;
2. Leitura dos arquivos;
3. Validação da estrutura dos dados;
4. Tratamento de valores ausentes e códigos especiais;
5. Recodificação de variáveis categóricas;
6. Construção de tabelas de frequência;
7. Análises univariadas;
8. Análises bivariadas;
9. Visualizações com Matplotlib e Seaborn;
10. Cruzamento entre variáveis da pesquisa CESOP e indicadores do TSE;
11. Interpretação dos resultados;
12. Elaboração de conclusões e limitações.

### Parte B

A metodologia segue uma abordagem de análise de séries temporais:

1. Coleta dos dados via API do BCB SGS;
2. Consolidação das séries (inadimplência, Selic, dólar) e filtragem do período a partir de 2011;
3. Análise da evolução histórica por períodos;
4. Decomposição sazonal da série de inadimplência;
5. Análise de correlação entre as séries, incluindo defasagens (lags) temporais;
6. Testes de estacionariedade (ADF) na série original e diferenciada;
7. Modelagem ARIMA e geração de projeção (forecast) para os meses seguintes;
8. Interpretação dos resultados, limitações e conclusões.

---

## 10. Estratégia Analítica

### 10.1 Análise da base CESOP (Parte A)

Foco:

- perfil dos entrevistados;
- lembrança de voto em 2022;
- prioridades políticas;
- percepção sobre fake news;
- vontade de participar da política local.

Possíveis cruzamentos:

- vontade de participar da política por faixa etária;
- vontade de participar da política por escolaridade;
- vontade de participar da política por renda;
- vontade de participar da política por região;
- prioridades políticas por perfil sociodemográfico.

---

### 10.2 Análise da base TSE (Parte A)

Foco:

- comparecimento eleitoral;
- abstenção eleitoral;
- perfil dos eleitores por região;
- diferenças por gênero, idade, escolaridade e raça/cor.

Indicadores derivados:

```text
taxa_comparecimento = QT_COMPARECIMENTO / QT_APTOS
taxa_abstencao = QT_ABSTENCAO / QT_APTOS
```

---

### 10.3 Cruzamento CESOP + TSE (Parte A)

A base CESOP representa percepção e opinião declarada. A base TSE representa comportamento eleitoral observado.

O cruzamento compara, principalmente por região e perfil sociodemográfico:

- vontade declarada de participar da política;
- lembrança do voto nas eleições de 2022;
- taxa real de comparecimento;
- taxa real de abstenção.

Exemplo de pergunta analítica:

> Regiões com menor vontade declarada de participar da vida política também apresentam maior taxa de abstenção eleitoral?

---

### 10.4 Análise da série de inadimplência (Parte B)

Foco:

- evolução histórica da taxa de inadimplência de pessoas físicas desde 2011, em diferentes períodos econômicos;
- sazonalidade mensal da inadimplência;
- correlação entre inadimplência, Selic e dólar, com e sem defasagem (lag);
- estacionariedade da série (testes ADF) e modelagem ARIMA para projeção futura.

Exemplo de pergunta analítica:

> A Taxa Selic afeta a inadimplência de forma imediata ou com defasagem de alguns meses?

---

## 11. Dicionários de Dados

Os dicionários de dados do projeto estão disponíveis na pasta `references/` (apenas para a Parte A — ver seção 6).

### CESOP

```text
references/data_dictionary_cesop.md
```

Contém:

- descrição das variáveis;
- códigos das respostas;
- perguntas da pesquisa;
- categorias demográficas, econômicas e geográficas.

### TSE

```text
references/data_dictionary_tse.md
```

Contém:

- descrição das variáveis do TSE;
- códigos de gênero, estado civil, escolaridade, raça/cor e demais atributos;
- métricas de comparecimento e abstenção;
- variáveis derivadas recomendadas.

---

## 12. Reprodutibilidade

Para reproduzir o projeto em outro computador, siga esta ordem:

1. Clone o repositório;
2. Crie e ative o ambiente virtual;
3. Instale as dependências com `requirements.txt`;
4. Para a Parte A: baixe os dados pelo Google Drive e descompacte a pasta `raw/` dentro de `data/`;
5. Para a Parte B: nenhuma ação extra é necessária (apenas conexão com a internet, pois os dados vêm da API do BCB SGS);
6. Execute os notebooks na ordem indicada (01 e 02 para a Parte A, 03 para a Parte B).

O projeto foi estruturado para funcionar com caminhos relativos, desde que a estrutura de pastas seja respeitada.

---

## 13. Versionamento

Este projeto utiliza Git e GitHub para versionamento.

Arquivos versionados:

- notebooks;
- documentação;
- dicionários de dados;
- relatórios e materiais de apresentação (`reports/`);
- arquivos de configuração.

Arquivos não versionados (ver `.gitignore`):

- dados brutos e processados (`data/raw/`, `data/processed/`);
- arquivos `.csv`, `.sav` e `.zip`;
- ambiente virtual.

---

## 14. Referências

- CESOP — Centro de Estudos de Opinião Pública.
- TSE — Tribunal Superior Eleitoral.
- Banco Central do Brasil — Sistema Gerenciador de Séries Temporais (SGS), séries 21082 (inadimplência PF), 3695 (dólar) e 4390 (Selic).
- Documentação oficial da base de comparecimento e abstenção eleitoral de 2022.
- Materiais da disciplina de Ciência de Dados.
- Bibliotecas Python utilizadas no projeto:
  - pandas;
  - numpy;
  - pyreadstat;
  - pyarrow;
  - matplotlib;
  - seaborn;
  - statsmodels (decomposição sazonal, testes ADF e modelagem ARIMA — Parte B).
