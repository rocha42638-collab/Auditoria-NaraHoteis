# NaraHoteis — Consultoria de Dados

> Diagnóstico e Painel Gerencial para Rede Hoteleira do Estado do Rio de Janeiro

---

## Contexto

A **NaraHoteis** é uma rede hoteleira com **12 unidades distribuídas em 4 regiões** do estado do Rio de Janeiro. A diretoria identificou queda de receita em algumas unidades e contratou uma consultoria de dados para:

- Auditar e tratar os dados operacionais provenientes do sistema legado
- Responder perguntas de negócio reais com base nos dados históricos
- Entregar um painel gerencial para apoio à tomada de decisão

---

## Estrutura do Repositório

```
Auditoria-NaraHoteis/
│
├── Auditoria_NaraHoteis_2026.ipynb       # Etapa 1 — Auditoria das 6 bases de dados
├── Limpeza_Dados_Reservas.ipynb          # Etapa 1 — Tratamento da tabela fato (reservas)
├── Projeto_BI.pbix                       # Etapa 5 — Painel gerencial no Power BI
├── Relatorio_Achados_NaraHoteis.pdf      # Relatório completo dos achados
├── Relatorio_Nara_Hoteis.pdf             # Relatório de apresentação
└── README.md
```

---

## As 12 Unidades

| Região | Unidades |
|---|---|
| **Capital** | Ipanema · Barra da Tijuca · Centro · Santa Teresa |
| **Baixada Fluminense** | Nova Iguaçu Centro · Nova Iguaçu Park |
| **Serra** | Petrópolis · Teresópolis · Friburgo |
| **Costa Verde** | Paraty · Angra dos Reis · Mangaratiba |

---

## Tecnologias Utilizadas

![Python](https://img.shields.io/badge/Python-3.14-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-Estatística-013243?logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualização-orange)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-Machine%20Learning-F7931E?logo=scikit-learn)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi)
![MySQL](https://img.shields.io/badge/MySQL-Banco%20de%20Dados-4479A1?logo=mysql)

---

## Etapas do Projeto

### Etapa 1 — Auditoria e Limpeza de Dados

Os 6 arquivos CSV chegaram do sistema legado com diversas inconsistências. O grupo realizou auditoria completa de cada base, sem nenhuma indicação prévia dos problemas.

**Principais problemas encontrados e tratados:**

| Base | Problemas Identificados |
|---|---|
| `reservas.csv` | Datas como `object` · `id_canal` com 112 NaN · Avaliações inválidas (0, 11, -3) · Diárias com valores impossíveis · Formas de pagamento sem padronização |
| `unidades.csv` | Região com variações (`capital`, `CAP`) · Categoria sem padrão (`3*`, `Três Estrelas`) · 2 NaN em `num_quartos_total` |
| `clientes.csv` | `tipo_cliente` inconsistente · `estado_origem` divergente · ID duplicado · 29 NaN em `faixa_etaria` |
| `tipos_quarto.csv` | Descrição sem padronização · `valor_diaria_base` com símbolo `R$` · Linha duplicada |
| `canais_venda.csv` | `nome_canal` sem padronização · `comissao_pct` com símbolo `%` |
| `funcionarios.csv` | Salário como texto com aspas · Salário negativo · Outlier extremo (R$ 999.999) · `data_admissao` como `object` |

> Os tratamentos seguiram os critérios do **Comunicado Oficial COM-2025-047** emitido pelo Departamento de TI da NaraHoteis.

---

### Etapa 2 — Análise Estatística

Com as bases limpas, o grupo respondeu **6 perguntas de negócio** da diretoria:

**P1 — Comportamento das diárias e distribuição do faturamento**
- Receita total da rede: **R$ 5.201.900,00**
- Diária média: R$ 569,69 | Mediana: R$ 420,00
- Quarto mais reservado: Standard (R$ 280) — representa 25% das reservas

**P2 — Unidades operando além da capacidade**
- Resultado: **nenhuma** das 2.457 reservas ultrapassou a capacidade máxima

**P3 — Unidades com receita discrepante**
- Metodologia: IQR (Intervalo Interquartílico)
- Q1 = R$ 438.877 | Q3 = R$ 498.985 | IQR = R$ 60.107
- **Outliers identificados:** NaraHoteis Centro (R$ 195.140) e NaraHoteis Nova Iguaçu Centro (R$ 169.960)

**P4 — Relação entre RevPAR e avaliação dos hóspedes**
- **Correlação de Pearson: 0,637** — correlação forte e positiva
- Unidades com maior avaliação tendem a apresentar RevPAR mais alto

**P5 — Modelo preditivo de RevPAR**
- Modelo: Regressão Linear Simples (`sklearn.LinearRegression`)
- X: avaliação média | y: RevPAR por unidade
- A reta de tendência confirma a relação positiva entre avaliação e desempenho financeiro

**P6 — Variabilidade regional do faturamento**

| Região | Desvio Padrão |
|---|---|
| Baixada Fluminense | R$ 232.616 — maior variabilidade  |
| Capital | R$ 138.452 |
| Costa Verde | R$ 46.658 |
| Serra | R$ 13.215 — mais homogênea  |

---

### Etapas 3, 4 e 5 — Painel, Banco de Dados e Power BI

- **Etapa 3:** Painel visual com Matplotlib (2×2)
- **Etapa 4:** Banco `narahoteis_db` criado no MySQL com 6 tabelas e relacionamentos via chave estrangeira
- **Etapa 5:** Dashboard no Power BI com filtro por região, KPIs de performance e comparativos temporais (DAX)

---

## Principais Achados

> As unidades **NaraHoteis Centro** e **NaraHoteis Nova Iguaçu Centro** são as responsáveis pela queda de receita identificada pela diretoria. Ambas apresentam faturamento abaixo do limite estatístico, RevPAR muito inferior à média da rede e avaliações dos hóspedes significativamente mais baixas (~2,2 vs ~8,0 das demais).
>
> A correlação de 0,637 entre avaliação e RevPAR sugere que **investir na experiência do hóspede nessas unidades pode ser o caminho mais eficiente para recuperar a receita perdida**.

---

## Sobre o Projeto

Projeto final do curso **UC2 — Analista de Dados**  
**Instituição:** SENAC RJ  
**Professor:** Cassio Ribeiro  

---

*"Os dados têm uma história para contar. Cabe ao grupo descobri-la."*  
— Prof. Cassio Ribeiro
