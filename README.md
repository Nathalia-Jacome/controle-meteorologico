# Controle Meteorológico — Pipeline de Coleta e Enriquecimento de Dados em Python

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=flat&logo=pandas&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google%20Colab-Notebook-F9AB00?style=flat&logo=googlecolab&logoColor=white)
![Open-Meteo](https://img.shields.io/badge/API-Open--Meteo-00A8E8?style=flat)

Projeto prático desenvolvido durante o bootcamp de **Análise de Dados da Generation Brasil**, com foco em **ingestão automatizada de dados, manipulação com Python/Pandas e enriquecimento via API REST**.

O objetivo central é demonstrar um fluxo de dados de ponta a ponta (ETL simplificado): coordenadas geográficas registradas em nuvem são lidas automaticamente em Python e enriquecidas em tempo real com dados meteorológicos da API pública **Open-Meteo**.

---

## Competências Demonstradas

* **Ingestão de dados em nuvem:** Leitura direta de bases externas (Google Sheets exportado como CSV) via URL com `pandas`.
* **Consumo de APIs REST:** Construção de requisições HTTP parametrizadas (`GET`) com a biblioteca `requests` e interpretação de respostas em formato `JSON`.
* **Manipulação de dados tabulares:** Iteração, extração de campos específicos (`Latitude`, `Longitude`, `Nome do local`) e estruturação de dados com DataFrames.
* **Visão de Pipeline de Dados:** Integração entre camada de entrada de dados, armazenamento intermediário e processamento analítico em Python.

---

## Arquitetura do Pipeline

```text
[Interface Web (Coleta)] ──► [Google Forms] ──► [Google Sheets (CSV)]
                                                         │
                                                         ▼
                                            [Python / Google Colab]
                                            • Leitura com Pandas
                                            • Iteração por registros
                                            • Requisição GET (requests)
                                                         │
                                                         ▼
                                                [API Open-Meteo]
                                            • Retorno JSON em tempo real
                                            • Variável: temperature_2m
```

---

## Foco Principal: Processamento em Python (Google Colab)

O núcleo do projeto está no notebook em Python, dividido em três etapas principais:

1. **Extração (Extract):** O script acessa o endpoint público da planilha no Google Sheets (`/export?format=csv`) e carrega os registros diretamente em um DataFrame utilizando `pd.read_csv()`.
2. **Transformação e Consulta (Transform & Enrich):** Percorre cada registro coletado (`iterrows()`), extrai o nome do local e as coordenadas geográficas e monta o dicionário de parâmetros (`latitude`, `longitude` e `current: temperature_2m`).
3. **Consumo da API:** Realiza a chamada ao endpoint `https://api.open-meteo.com/v1/forecast` via `requests.get()`, convertendo o retorno para `JSON` para análise dos dados climáticos atuais de cada ponto.

### Estrutura da Base de Entrada

| Coluna | Tipo | Descrição |
| :--- | :--- | :--- |
| `Carimbo de data/hora` | Timestamp | Data e hora do registro no formulário |
| `Nome do local:` | String | Identificação do ponto geográfico |
| `Latitude:` | Float | Coordenada de latitude em graus decimais |
| `Longitude:` | Float | Coordenada de longitude em graus decimais |

---

## Interface Auxiliar de Coleta (GitHub Pages)

Para tornar o fluxo de dados testável em tempo real sem depender de preenchimento manual direto na planilha, foi disponibilizada uma página estática simples no GitHub Pages (construída com apoio de IA como camada auxiliar de entrada):

> **[Acessar Formulário de Teste](https://nathalia-jacome.github.io/controle-meteorologico/)**

Ela captura o nome do local e as coordenadas (manualmente ou via GPS do navegador) e envia os dados para o Google Forms, alimentando automaticamente a base consumida pelo script em Python.

---

## Tecnologias Utilizadas

**Core (Análise de Dados)**
* **Python 3**
* **Pandas** — leitura e manipulação de dados tabulares
* **Requests** — consumo de API REST
* **Google Colab** — ambiente de desenvolvimento e execução
* **Open-Meteo API** — fonte de dados meteorológicos em tempo real

**Camada Auxiliar (Ingestão)**
* **Google Forms & Google Sheets** — coleta e armazenamento em nuvem
* **HTML5 / CSS3 / JavaScript + GitHub Pages** — interface leve para envio de coordenadas

---

## Como Executar o Projeto

1. Clone o repositório:
   ```bash
   git clone [https://github.com/Nathalia-Jacome/controle-meteorologico.git](https://github.com/Nathalia-Jacome/controle-meteorologico.git)
   ```
2. Abra o notebook no **Google Colab** ou execute localmente instalando as dependências:
   ```bash
   pip install pandas requests
   ```
3. Execute as células em ordem para carregar os dados atualizados da planilha e consultar a temperatura atual de cada local cadastrado.

---

## Próximos Passos (Evolução do Projeto)

* Estruturar o retorno `JSON` da API para anexar a temperatura e o horário da medição diretamente como novas colunas no DataFrame.
* Implementar tratamento de exceções (`try/except`) e validação de `status_code` nas chamadas HTTP.
* Exportar o dataset enriquecido (CSV/Parquet) para análise histórica e visualização em dashboard.

---

## Autora

**Nathalia Jacome**  
Estudante de Análise de Dados (Generation Brasil)  
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/Nathalia-Jacome)
