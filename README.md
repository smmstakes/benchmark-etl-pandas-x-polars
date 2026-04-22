# Comparacao Entre Pandas x Polars

Este repositorio apresenta um estudo pratico de desempenho entre as bibliotecas **Pandas** e **Polars** para manipulacao de dados em larga escala.

O projeto foi desenvolvido no contexto da disciplina **Metodologia Cientifica (CIC0200 - 2026.1)** e serve como base para um artigo academico, com foco em:

- comparar tempos de execucao em operacoes comuns de engenharia de dados;
- observar ganhos de desempenho entre abordagens eager e lazy;
- gerar artefatos visuais para discussao dos resultados.

## Objetivo do Projeto

O objetivo principal e responder, com dados empiricos:

- qual biblioteca executa mais rapido operacoes classicas de ETL/analise;
- em quais tipos de operacao o ganho de desempenho e mais expressivo;
- qual o impacto do modo **lazy** do Polars no tempo total da pipeline.

As operacoes comparadas sao:

- `load_dataset`
- `count_nulls`
- `drop_nulls`
- `create_column`
- `filter`
- `groupby_agg`
- `sort`

## Estrutura do Repositorio

```text
.
├── LICENSE
├── README.md
├── requirements.txt
├── images/
│   └── (graficos gerados na analise)
├── notebooks/
│   ├── analise-pandas-x-polars.ipynb
│   └── experimentacao-pandas-e-polars.ipynb
├── perf_times/
│   ├── pandas_benchmark_results.json
│   ├── polars_benchmark_results.json
│   └── polars_lazy_benchmark_results.json
└── raw_data/
    └── example_dataset.csv
```

### O que cada parte faz

- **notebooks/experimentacao-pandas-e-polars.ipynb**:
  - gera (se necessario) um dataset sintetico grande;
  - executa os benchmarks com Pandas e Polars;
  - registra os tempos de cada etapa;
  - salva os resultados em arquivos JSON dentro de `perf_times/`.

- **notebooks/analise-pandas-x-polars.ipynb**:
  - le os JSONs de benchmark;
  - organiza os tempos por operacao;
  - gera graficos comparativos;
  - salva visualizacoes em `images/`.

- **perf_times/**:
  - guarda os tempos brutos de execucao para reproducibilidade;
  - permite repetir apenas a analise sem rodar todo o benchmark novamente.

- **raw_data/**:
  - contem o dataset utilizado nos testes (`example_dataset.csv`).

- **images/**:
  - recebe as figuras exportadas pela etapa de analise.

## Requisitos

- Python 3.11+ (recomendado)
- pip
- ambiente virtual (venv)
- Jupyter Notebook ou JupyterLab

Dependencias Python estao em `requirements.txt`.

## Instalacao

### 1. Clone o repositorio

```bash
git clone <url-do-seu-repositorio>
cd comparacao-entre-pandas-x-polars
```

### 2. Crie e ative um ambiente virtual

Linux/macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Windows (PowerShell):

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Instale as dependencias

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

## Como Rodar os Notebooks

Voce pode executar pelos caminhos abaixo.

### Opcao A: JupyterLab / Jupyter Notebook

No diretorio raiz do projeto:

```bash
jupyter lab
```

ou

```bash
jupyter notebook
```

Depois, abra os notebooks na pasta `notebooks/`.

### Opcao B: VS Code

1. Abra a pasta do projeto no VS Code.
2. Selecione o interpretador Python do ambiente virtual `.venv`.
3. Abra os notebooks em `notebooks/`.
4. Execute as celulas em ordem.

## Ordem Recomendada de Execucao

Para reproduzir integralmente o estudo:

1. Execute `notebooks/experimentacao-pandas-e-polars.ipynb` para (re)gerar benchmarks.
2. Verifique os arquivos atualizados em `perf_times/`.
3. Execute `notebooks/analise-pandas-x-polars.ipynb` para gerar graficos e interpretacoes.
4. Consulte os resultados em `images/` para uso no artigo.

Observacao: o notebook de experimentacao pode criar um dataset muito grande (dezenas de milhoes de linhas), exigindo bastante memoria RAM e tempo de execucao.

## Resultados Atuais (resumo)

Com base nos JSONs atualmente versionados em `perf_times/`:

| Operacao | Pandas (s) | Polars (s) | Ganho aproximado (Pandas/Polars) |
|---|---:|---:|---:|
| load_dataset | 64.92 | 3.47 | 18.72x |
| count_nulls | 8.97 | 0.01 | 797.88x |
| drop_nulls | 25.07 | 7.30 | 3.43x |
| create_column | 0.35 | 0.20 | 1.75x |
| filter | 5.69 | 0.23 | 24.88x |
| groupby_agg | 10.17 | 0.38 | 26.71x |
| sort | 57.45 | 16.94 | 3.39x |

Tempo total aproximado:

- **Pandas (eager):** 172.61s
- **Polars (eager):** 28.53s
- **Polars (lazy):** 4.29s

Comparativos globais:

- Polars eager foi aproximadamente **6.05x** mais rapido que Pandas.
- Polars lazy foi aproximadamente **40.28x** mais rapido que Pandas.
- Polars lazy foi aproximadamente **6.66x** mais rapido que Polars eager.

## Metodologia (visao geral)

- O dataset de teste e composto por multiplas colunas numericas, categoricas e temporais.
- As mesmas operacoes sao executadas nas duas bibliotecas para comparacao justa.
- Os tempos sao coletados com medicao por etapa e salvos em JSON.
- A analise utiliza graficos para comparacao direta por operacao e por tempo total.

## Reprodutibilidade e Boas Praticas

- Execute os notebooks com o mesmo ambiente definido em `requirements.txt`.
- Evite executar aplicacoes pesadas em paralelo durante os benchmarks.
- Para comparacoes mais estaveis, rode os testes mais de uma vez e analise media/desvio.
- Mantenha versoes fixas das bibliotecas para reduzir variacoes entre execucoes.

## Limitacoes do Estudo

- Os tempos dependem do hardware local (CPU, RAM, disco).
- Uma unica configuracao de dataset pode nao representar todos os cenarios reais.
- Resultados podem variar com versoes diferentes de Pandas, Polars e Python.

## Sugestoes de Extensao

- adicionar repeticoes por experimento e intervalo de confianca;
- incluir medicao de uso de memoria;
- testar diferentes tamanhos de dataset;
- comparar tambem com outras bibliotecas (ex.: Dask, DuckDB, PySpark);
- automatizar a pipeline em script unico para execucao de ponta a ponta.

---

## Autor

- **Matheus Duarte da Silva**

## Licenca

Este projeto esta licenciado sob os termos descritos em `LICENSE`.
