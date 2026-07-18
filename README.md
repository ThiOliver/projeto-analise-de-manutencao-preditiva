# Pipeline de Manutenção Preditiva Industrial

Projeto avaliativo do Módulo 1 do curso **Desenvolvimento de IA para Análise 
Preditiva** (SENAI). Pipeline de Ciência de Dados aplicado à Indústria 4.0 
para prever falhas mecânicas em equipamentos industriais.

## Problema

Em um parque fabril monitorado por sensores, quebras mecânicas inesperadas 
causam paradas na linha de produção, gerando prejuízo financeiro e atrasos. 
O objetivo do projeto é construir um modelo capaz de prever, com base em 
dados de sensores, se uma máquina vai falhar (classificação binária).

A variável alvo é `falha_maquina`, onde:
- `1` indica que a máquina sofreu uma falha mecânica
- `0` indica funcionamento normal


## Tecnologias utilizadas

- **Python 3.12** — linguagem principal
- **Jupyter Notebook** — desenvolvimento do pipeline
- **Pandas** e **NumPy** — manipulação de dados
- **Matplotlib** e **Seaborn** — visualizações
- **Scikit-learn** — modelos de Machine Learning (KNN e Árvore de Decisão)
- **Imbalanced-learn** — técnica de balanceamento SMOTE
- **Git** e **GitHub** — controle de versão com fluxo GitFlow

## Como executar

### Pré-requisitos

- Python 3.12 ou superior
- Git

### Passo a passo

1. Clone o repositório:

```bash
git clone https://github.com/ThiOliver/projeto-analise-de-manutencao-preditiva.git
cd projeto-analise-de-manutencao-preditiva
```

2. Crie um ambiente virtual:

```bash
python -m venv .venv
```

3. Ative o ambiente virtual:

```bash
# Linux / macOS
source .venv/bin/activate

# Windows
.venv\Scripts\activate
```

4. Instale as dependências:

```bash
pip install -r requirements.txt
```

5. Abra o notebook:

```bash
jupyter notebook analise.ipynb
```

6. Execute as células em ordem, de cima para baixo.

## Fases do Pipeline

O projeto está estruturado em 7 fases, organizadas em blocos no notebook 
principal.

### Fase 1 — Análise Exploratória (EDA)

Investigação inicial do dataset: dimensões, tipos de dados, estatísticas 
descritivas e visualizações (histograma, gráfico de barras da variável 
alvo, heatmap de correlação de Pearson). 

**Principais descobertas:** desbalanceamento severo (3,4% de falhas), 
multicolinearidade entre temperaturas (0,88) e entre rotação/torque (-0,88), 
e nenhuma variável isolada com correlação forte com o alvo (máx. 0,19).

### Fase 2 — Limpeza e Tratamento de Dados

Remoção de duplicatas, imputação de 500 valores ausentes por coluna e 
análise de outliers via boxplot.

**Decisão técnica:** imputação por **média** nas colunas simétricas 
(temperaturas e torque) e por **mediana** na coluna `velocidade_rotacao_rpm` 
(assimetria = 1,995). Outliers foram mantidos por representarem 
provavelmente as próprias falhas mecânicas.

### Fase 3 — Feature Engineering

Criação da coluna `potencia`, calculada como `velocidade_rotacao_rpm × torque_nm`. 
Representa o esforço total do motor e combina duas variáveis que, isoladamente, 
não tinham correlação forte com a falha.

### Fase 4 — Divisão e Balanceamento

- Conversão da coluna categórica `tipo` (L, M, H) em colunas numéricas via One-Hot Encoding.
- Divisão em treino (80%) e teste (20%) com `stratify=y` para manter a proporção de falhas.
- Aplicação do **SMOTE** apenas nos dados de treino, para evitar vazamento de dados.

### Fase 5 — Escalonamento de Variáveis

Aplicação do `StandardScaler` apenas nos dados destinados ao KNN 
(algoritmo baseado em distância). A Árvore de Decisão utilizou os dados 
originais, pois não é sensível à escala das variáveis.

### Fase 6 — Ajuste de Parâmetros e Combate ao Overfitting

Treinamento de cada modelo com 3 valores diferentes de hiperparâmetro, 
comparando a acurácia no treino e no teste para identificar overfitting.

- **KNN:** testado com K = 3, 5 e 7
- **Árvore de Decisão:** testada com max_depth = 3, 5 e None

### Fase 7 — Avaliação da Acurácia e Veredito Final

Cálculo da acurácia final dos dois modelos com as configurações escolhidas 
e comparação para escolha do modelo a ser adotado.


## Resultados

| Modelo | Configuração | Acurácia no Teste |
|---|---|---|
| KNN | K = 3 | **92,65%** |
| Árvore de Decisão | max_depth = 5 | 91,20% |

**Modelo adotado: KNN com K = 3.**

A escolha foi baseada na maior acurácia obtida nos dados de teste, com um 
nível de overfitting aceitável e compatível com as outras configurações testadas.

## Vídeo de apresentação

📹 [Assista à apresentação do projeto no Google Drive](https://drive.google.com/file/d/1fGBb_vAApC_m0UK-F2OqqdRXv9_tF0k6/view?usp=drive_link)

## Autor

Desenvolvido por **Thiago Oliveira** como projeto avaliativo do Módulo 1 
do curso **Desenvolvimento de IA para Análise Preditiva** — SENAI/SC.