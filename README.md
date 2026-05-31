# Projeto EDA — Percepção dos Brasileiros Acerca da Democracia

## 1. Visão Geral

Este repositório contém o desenvolvimento do **Projeto I — EDA (Exploratory Data Analysis)** da disciplina de **Ciência de Dados**.

O objetivo principal do projeto é realizar uma análise exploratória de dados sobre a **percepção dos brasileiros acerca da democracia**, utilizando como base principal uma pesquisa de opinião do **CESOP — Centro de Estudos de Opinião Pública**.

Além da base principal, o projeto utiliza uma base pública complementar do **TSE — Tribunal Superior Eleitoral**, com dados de **comparecimento e abstenção nas eleições de 2022**, permitindo relacionar percepções declaradas sobre participação política com indicadores eleitorais observados.

---

## 2. Tema do Projeto

**Percepção dos Brasileiros Acerca da Democracia**

O projeto busca investigar questões como:

- Qual é o perfil sociodemográfico dos entrevistados?
- Os brasileiros se lembram em quem votaram nas eleições gerais de 2022?
- Quais propostas são consideradas prioridade para políticos?
- Quais medidas são consideradas importantes no combate à disseminação de fake news?
- Qual é o nível de vontade dos brasileiros de participar da vida política em sua cidade?
- Há relação entre a percepção declarada de participação política e os dados reais de comparecimento/abstenção eleitoral?

---

## 3. Bases de Dados Utilizadas

O projeto utiliza duas fontes principais de dados.

### 3.1 Base principal — CESOP

Fonte:

**CESOP — Centro de Estudos de Opinião Pública**

Tema:

**Percepção dos Brasileiros Acerca da Democracia**

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

### 3.2 Base complementar — TSE

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

A base complementar será usada para comparar a **participação política declarada** na pesquisa do CESOP com a **participação eleitoral observada** nos dados oficiais do TSE.

---

## 4. Acesso aos Dados

Os arquivos de dados brutos não são versionados diretamente neste repositório devido ao tamanho elevado das bases, especialmente os arquivos do TSE.

A pasta `raw/`, contendo os dados brutos do CESOP e do TSE, foi compactada e disponibilizada em uma pasta pública no Google Drive.

Acesse os dados pelo link abaixo:

[Download dos dados brutos — Google Drive]([INSERIR_LINK_DO_GOOGLE_DRIVE_AQUI])

Após o download, o arquivo compactado deve ser descompactado dentro da pasta `data/` do projeto.

A estrutura esperada é:

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

Mais detalhes sobre a organização dos dados estão disponíveis em:

[`data/README.md`](data/README.md)

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
│   ├── README.md
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── 01_exploracao_base.ipynb
│   ├── 02_tratamento_dados.ipynb
│   └── 03_eda_democracia.ipynb
│
├── references/
│   ├── data_dictionary_cesop.md
│   └── data_dictionary_tse.md
│
├── reports/
│   ├── imagens/
│   ├── slides/
│   └── relatorio.md
│
└── src/
    ├── preprocessing.py
    ├── visualization.py
    └── utils.py
```

Observação:

Algumas pastas podem estar vazias em etapas iniciais do projeto. O Git não versiona pastas vazias automaticamente.

---

## 6. Descrição das Pastas

### `data/`

Contém a documentação sobre os dados utilizados no projeto.

Os dados brutos e processados não são versionados diretamente no GitHub por causa do tamanho elevado dos arquivos.

---

### `data/raw/`

Pasta esperada para armazenar os dados brutos baixados do Google Drive.

Essa pasta é ignorada pelo Git.

---

### `data/processed/`

Pasta destinada a armazenar bases tratadas ou intermediárias geradas durante o projeto.

Essa pasta também é ignorada pelo Git.

---

### `notebooks/`

Contém os notebooks Jupyter utilizados no desenvolvimento do projeto.

Sugestão de organização:

- `01_exploracao_base.ipynb`: carregamento, limpeza, recodificação e salvamento das bases CESOP e TSE em parquet ✅;
- `02_tratamento_dados.ipynb`: análises exploratórias unidimensionais e bivariadas (em desenvolvimento);
- `03_eda_democracia.ipynb`: análise principal com cruzamento CESOP × TSE (em desenvolvimento).

---

### `references/`

Contém documentos auxiliares do projeto, como dicionários de dados e descrições das variáveis.

Arquivos principais:

- `data_dictionary_cesop.md`: dicionário de dados da base CESOP;
- `data_dictionary_tse.md`: dicionário de dados da base TSE.

---

### `reports/`

Pasta destinada aos artefatos finais do projeto, como:

- relatório;
- imagens dos gráficos;
- slides;
- materiais de apresentação.

---

### `src/`

Pasta destinada a scripts Python reutilizáveis.

Exemplos de uso:

- funções de limpeza de dados;
- funções de visualização;
- funções auxiliares para tratamento de variáveis.

---

## 7. Como Executar o Projeto

### 7.1 Clonar o repositório

```bash
git clone [INSERIR_LINK_DO_REPOSITORIO_AQUI]
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

### 7.4 Baixar os dados

Acesse o Google Drive público:

[Download dos dados brutos — Google Drive]([INSERIR_LINK_DO_GOOGLE_DRIVE_AQUI])

Baixe o arquivo compactado contendo a pasta `raw/`.

Depois, descompacte o conteúdo dentro da pasta:

```text
data/
```

A estrutura final deve ficar assim:

```text
data/raw/cesop/
data/raw/tse/
```

---

### 7.5 Executar os notebooks

Após instalar as dependências e posicionar os dados corretamente, abra os notebooks na pasta:

```text
notebooks/
```

Ordem recomendada de execução:

```text
01_exploracao_base.ipynb
02_tratamento_dados.ipynb
03_eda_democracia.ipynb
```

---

## 8. Caminhos Utilizados nos Notebooks

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

---

## 9. Metodologia Geral

A metodologia do projeto segue uma abordagem de análise exploratória de dados.

Etapas previstas:

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

---

## 10. Estratégia Analítica

A análise será conduzida em duas frentes.

### 10.1 Análise da base CESOP

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

### 10.2 Análise da base TSE

Foco:

- comparecimento eleitoral;
- abstenção eleitoral;
- perfil dos eleitores por região;
- diferenças por gênero, idade, escolaridade e raça/cor.

Indicadores derivados possíveis:

```text
taxa_comparecimento = QT_COMPARECIMENTO / QT_APTOS
taxa_abstencao = QT_ABSTENCAO / QT_APTOS
```

---

### 10.3 Cruzamento CESOP + TSE

A base CESOP representa percepção e opinião declarada.

A base TSE representa comportamento eleitoral observado.

O cruzamento buscará comparar, principalmente por região:

- vontade declarada de participar da política;
- lembrança do voto nas eleições de 2022;
- taxa real de comparecimento;
- taxa real de abstenção.

Exemplo de pergunta analítica:

> Regiões com menor vontade declarada de participar da vida política também apresentam maior taxa de abstenção eleitoral?

---

## 11. Dicionários de Dados

Os dicionários de dados do projeto estão disponíveis na pasta `references/`.

### CESOP

Arquivo:

```text
references/data_dictionary_cesop.md
```

Contém:

- descrição das variáveis;
- códigos das respostas;
- perguntas da pesquisa;
- categorias demográficas, econômicas e geográficas.

### TSE

Arquivo:

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
4. Baixe os dados pelo Google Drive;
5. Descompacte a pasta `raw/` dentro de `data/`;
6. Execute os notebooks na ordem indicada.

O projeto foi estruturado para funcionar com caminhos relativos, desde que a estrutura de pastas seja respeitada.

---

## 13. Versionamento

Este projeto utiliza Git e GitHub para versionamento.

Arquivos versionados:

- notebooks;
- documentação;
- dicionários de dados;
- scripts auxiliares;
- arquivos de configuração;
- relatórios e materiais de apresentação.

Arquivos não versionados:

- dados brutos;
- dados processados;
- arquivos `.csv`;
- arquivos `.sav`;
- arquivos `.zip`;
- ambiente virtual.

Esses arquivos são ignorados pelo `.gitignore`.

---

## 14. Uso de IA

Ferramentas de Inteligência Artificial foram utilizadas como apoio no desenvolvimento do projeto.

Principais usos:

- estruturação do projeto;
- organização dos diretórios;
- criação dos dicionários de dados;
- sugestões de metodologia;
- apoio na definição de análises exploratórias;
- auxílio na escrita de documentação;
- apoio na interpretação de resultados;
- revisão de código e troubleshooting.

As decisões metodológicas, análises finais e conclusões devem ser revisadas criticamente pelo grupo.

---

## 15. Referências

- CESOP — Centro de Estudos de Opinião Pública.
- TSE — Tribunal Superior Eleitoral.
- Documentação oficial da base de comparecimento e abstenção eleitoral de 2022.
- Materiais da disciplina de Ciência de Dados.
- Bibliotecas Python utilizadas no projeto:
  - pandas;
  - numpy;
  - pyreadstat;
  - pyarrow;
  - matplotlib;
  - seaborn.

---

## 16. Status do Projeto

Status atual:

```text
Em desenvolvimento
```

Etapas concluídas:

- definição do tema;
- organização inicial do repositório;
- criação do ambiente virtual;
- documentação das bases (dicionários CESOP e TSE);
- versionamento no GitHub;
- disponibilização dos dados brutos em Google Drive público;
- preenchimento do `requirements.txt`;
- carregamento e validação da base CESOP (2.000 registros, 27 variáveis);
- carregamento e validação da base TSE (~2.3 GB, 10 colunas selecionadas via `usecols=`, encoding `latin1`);
- limpeza e recodificação das variáveis CESOP (value labels, NaN, variáveis derivadas: `ESCOL_GRUPO`, `RENDA_GRUPO`);
- limpeza da base TSE (remoção de linhas com `QT_APTOS=0`, padronização de strings, conversão para `category`);
- criação de variáveis derivadas TSE (`TAXA_COMPARECIMENTO`, `TAXA_ABSTENCAO`, `REGIAO`);
- criação de agregações TSE por UF (`tse_uf`) e por perfil demográfico nacional (`tse_perfil`);
- validação das taxas TSE (intervalo [0, 1] verificado);
- salvamento das bases tratadas em parquet (`data/processed/`): `cesop_clean`, `tse_clean`, `tse_uf`, `tse_perfil`;
- revisão e simplificação do dicionário de dados TSE (`references/data_dictionary_tse.md`).

Próximas etapas:

- análises exploratórias unidimensionais (perfil dos respondentes);
- análises bivariadas (cruzamentos por escolaridade, renda, região);
- cruzamento CESOP × TSE (disposição declarada vs. comparecimento real);
- testes estatísticos (qui-quadrado, correlação de Spearman);
- construção dos gráficos e conclusões;
- elaboração do relatório e slides;
- gravação do vídeo de apresentação.