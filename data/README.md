# Dados do Projeto

Esta pasta contém as instruções para organização dos dados utilizados no projeto **EDA — Percepção dos Brasileiros Acerca da Democracia**.

Os dados brutos e processados não são versionados diretamente neste repositório devido ao tamanho elevado dos arquivos, especialmente os arquivos do TSE.

---

## 1. Por que os dados não estão no GitHub?

O GitHub não é adequado para versionar arquivos muito grandes, como bases `.csv`, `.sav` ou arquivos compactados `.zip` com centenas de MB ou alguns GB.

Neste projeto, a base complementar do TSE possui arquivos de grande volume. Por isso, os dados foram disponibilizados separadamente em uma pasta pública no Google Drive.

O repositório GitHub contém:

- código;
- notebooks;
- documentação;
- dicionários de dados;
- README;
- requirements;
- materiais do projeto.

O Google Drive contém:

- dados brutos do CESOP;
- dados brutos do TSE;
- arquivos originais utilizados no projeto.

---

## 2. Download dos dados

A pasta `raw/` completa foi compactada e disponibilizada em Google Drive público.

Acesse o link abaixo para baixar os dados:

[Download dos dados brutos — Google Drive](https://drive.google.com/drive/folders/1nscFRJuwoXh2f1xIYhltxRY_OX_vMer7?usp=sharing)

Após o download, descompacte o arquivo dentro da pasta `data/` deste projeto.

---

## 3. Estrutura esperada

Depois de baixar e descompactar o arquivo, a estrutura local deve ficar assim:

```text
data/
  README.md

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
        perfil_comparecimento_abstencao_2022_RJ.csv
        perfil_comparecimento_abstencao_2022_BA.csv
        ...

  processed/
```

A pasta `raw/` deve ficar diretamente dentro de `data/`.

O caminho correto é:

```text
data/raw/
```

E não:

```text
data/raw/raw/
```

Caso a descompactação crie uma pasta duplicada, reorganize manualmente os arquivos para manter a estrutura correta.


## 4. Resumo para reprodução

Para reproduzir o projeto:

1. Clone o repositório no GitHub;
2. Crie e ative o ambiente virtual;
3. Instale as dependências com `requirements.txt`;
4. Baixe a pasta `raw/` compactada no Google Drive;
5. Descompacte `raw/` dentro da pasta `data/`;
6. Confirme se a estrutura ficou como `data/raw/cesop/` e `data/raw/tse/`;
7. Execute os notebooks na ordem indicada no README principal.
        