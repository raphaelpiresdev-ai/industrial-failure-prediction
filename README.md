# PredictaMaintenance AI — Sistema de Manutenção Preditiva Industrial

Pipeline fim a fim de Ciência de Dados para modelagem, validação competitiva e diagnóstico de estabilidade de classificadores binários aplicados à previsão de falhas em ativos industriais.

---

## Visão Geral do Projeto

Este projeto implementa o **PredictaMaintenance AI**, uma solução de inteligência artificial para prever falhas em maquinários industriais com base em leituras históricas de sensores (como Temperatura, RPM e Torque). O objetivo principal é homologar um modelo de alta confiabilidade que minimize paradas não planejadas na linha de produção e otimize os custos de manutenção preventiva.

A arquitetura do projeto foi desenhada sob um **rigoroso critério de governança estatística**, garantindo a eliminação completa de vazamento de dados (*data leakage*), o controle de super-ajuste (*overfitting*) e a aderência estrita às diretrizes operacionais da empresa.

---

## Tecnologias e Ferramentas Utilizadas

O sistema foi desenvolvido utilizando o ecossistema científico da linguagem **Python 3.10+**:

*   **Processamento e Manipulação:** `Pandas`, `NumPy`
*   **Machine Learning (Modelagem e Métricas):** `Scikit-Learn`, `LightGBM`
*   **Balanceamento de Dados:** `Imbalanced-Learn (SMOTE)`
*   **Visualização de Dados:** `Matplotlib`, `Seaborn`
*   **Ambiente de Desenvolvimento:** `Jupyter Notebook` / `VS Code`

---

## Arquitetura do Pipeline (As 7 Fases)

O desenvolvimento foi dividido de forma modular em 7 fases sequenciais dentro do ambiente de desenvolvimento:

*   **Fase 1: Análise Exploratória de Dados (EDA):** Identificação do perfil estatístico dos sensores, diagnóstico do desbalanceamento de classes e mapeamento de *outliers*.
*   **Fase 2: Limpeza e Tratamento de Dados (Data Prep):** Identificação e tratamento de dados ausentes por meio de imputação estatística (Média/Mediana) e feature selection. 
*   **Fase 3: Feature Engineering:** Criação de novas variáveis preditoras baseadas nas grandezas físicas do maquinário.
*   **Fase 4: Divisão e Balanceamento dos Dados:** Separação estrita de preditoras ($X$) e alvo ($y$) com divisão em 80% treino e 20% teste, utilizando amostragem estratificada. Aplicação da técnica SMOTE **exclusivamente na base de treino** para mitigar o severo desbalanceamento sem causar vazamento de dados (*data leakage*).
*   **Fase 5: Escalonamento de Variáveis (StandardScaler):** Padronização estatística (`StandardScaler`) aplicada de forma isolada para algoritmos geométricos (KNN) e preservação dos dados brutos para algoritmos de árvore.
*   **Fase 6: Ajuste de Parâmetros e Combate ao Overfitting:** Treinamento competitivo dos modelos com monitoramento do *Gap* de generalização. Uso da acurácia para medir a taxa de acerto de cada modelo.
*   **Fase 7: Avaliação da Acurácia e Veredito Final:** Homologação do classificador definitivo com validação complementar por Matrizes de Confusão.

---

## Governança Estatística: O Critério do Gap de Estabilidade

Seguindo as premissas do comitê de avaliação, o indicador primário de estabilidade do modelo é o **Gap de Generalização**, calculado estritamente como:

$$Gap = \text{Acurácia de Teste} - \text{Acurácia de Treino}$$

A partir do comportamento dessa métrica, estabelecemos três diagnósticos operacionais:
1. **Risco de Underfitting:** Acurácia de treino inferior a **90%** (modelo incapaz de aprender o padrão).
2. **Risco de Overfitting:** Gap negativo e inferior a **-3,00%** (modelo memorizou o treino e perdeu desempenho no teste).
3. **Altamente Estável / Excelente Ajuste:** Gaps contidos entre **0% e -3,00%** combinados com alta acurácia (ponto ideal de equilíbrio).

### Painel Geral de Experimentos

| Algoritmo | Acurácia Treino | Acurácia Teste (Alvo) | Gap (Teste - Treino) | Diagnóstico Operacional |
| :--- | :---: | :---: | :---: | :--- |
| **Árvore (Profundidade=3)** | 86,31% | 93,35% | +7,04% | 🛑 Risco de Underfitting |
| **KNN (K=3)** | 98,53% | 94,15% | -4,38% | 🚨 Risco de Overfitting (Ruído do SMOTE) |
| **Árvore (Ilimitada)** | 99,95% | 96,70% | -3,25% | 🚨 Risco de Overfitting (Memorização) |
| **LightGBM Personalizado** | **99,57%** | **97,80%** | **-1,77%** | 🏆 **Altamente Estável / Excelente Ajuste** |

---

## Veredito Final e Recomendação Institucional

O modelo **LightGBM (200 árvores, Learning Rate = 0.03)** foi formalmente selecionado e homologado para implantação no ambiente produtivo devido aos seguintes fatores:

1. **Liderança Absoluta na Métrica Alvo:** Atingiu a maior **Acurácia Global de Teste (97,80%)**, superando a Árvore de Decisão em 1,10% e o KNN em 3,65%.
2. **Robustez Matemática contra Instabilidade:** Foi o algoritmo baseado em árvores que melhor controlou o sobreajuste, contendo seu Gap em apenas **-1,77%**, mantendo-se seguro dentro do limite crítico de -3,00%.
3. **Plus Analítico (Minimização de Erros Absolutos):** A validação por matrizes de confusão provou a eficiência prática do modelo. O LightGBM gerou o menor impacto operacional, cometendo apenas **44 erros de predição em um universo de 2.000 amostras inéditas**, contra 66 erros da Árvore e 117 do KNN (Visualização disponível em `outputs/figura4.png`).

---

## Como Executar o Sistema

Siga as instruções abaixo para reproduzir o pipeline de análise e modelagem em sua máquina local:

### 1. Pré-requisitos

Certifique-se de ter o **Python 3.10 ou superior** e o `pip` instalados.

### 2. Clonar o Repositório

No terminal, clone o projeto com o comando:
git clone https://github.com/raphaelpiresdev-ai/industrial-failure-prediction

### 3. Entrar na pasta do projeto e Instalar as Dependências

cd industrial-failure-prediction
pip install -r requirements.txt

### 4. Abrir e Executar o Pipeline

Inicie o ambiente do Jupyter ou abra a pasta diretamente no **VS Code** para interagir com o código:

1. Na pasta `notebook`, abra o arquivo `.ipynb` do projeto.
2. Execute as células sequencialmente da **Fase 1 à Fase 7**.

Os relatórios analíticos serão impressos em tela e os gráficos de validação serão salvos automaticamente na pasta `outputs/`.

---

## Melhorias Futuras

Para as próximas iterações deste software, mapeamos as seguintes evoluções de engenharia:

1. **Otimização de Hiperparâmetros:** Implementar uma busca automatizada via Bayesian Optimization ou Optuna para refinar ainda mais os parâmetros do LightGBM.

2. **Pipeline de MLOps:** Desenvolver uma API utilizando FastAPI para expor o modelo como um microsserviço de inferência em tempo real.

3. **Monitoramento de Data Drift:** Integrar ferramentas como o Evidently AI para monitorar se os dados dos sensores industriais mudaram de perfil ao longo do tempo, disparando alertas para retreinamento.

---

## Video apresentação

Link do video explicativo do projeto: https://drive.google.com/drive/folders/1QzVsN33sN66PnwwNEtzHiLPrwxnPnJ-n?usp=sharing

---

## Estrutura de Pastas do Repositório

```text
INDUSTRIAL-FAILURE-PREDICTION
├── data/               # Dataset industrial e anotações do departamento de engenharia
├── notebook/           # Notebook (.ipynb) com a execução das 7 fases rigorosas
├── outputs/            # figura4.png (Matrizes de Confusão salvas em alta resolução)
├── README.md           # Documentação técnica e governança do projeto
└── requirements.txt    # Dependências globais do ambiente
```

**Status do Projeto:** 🟢 Concluído e pronto para homologação em ambiente de produção (Deploy).
