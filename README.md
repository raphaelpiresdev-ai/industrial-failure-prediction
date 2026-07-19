# industrial-failure-prediction
Pipeline de Machine Learning em Python para previsão de falhas mecânicas em sensores industriais (Projeto Final - Módulo 1).

# 🛠️ Sistema de Manutenção Preditiva Industrial com Machine Learning

Pipeline fim a fim de Ciência de Dados para modelagem, validação competitiva e diagnóstico de estabilidade de classificadores binários aplicados à previsão de falhas em ativos industriais.

---

## 📌 Visão Geral do Projeto

Este projeto implementa uma solução de inteligência artificial para prever falhas em maquinários industriais com base em leituras históricas de sensores (como Temperatura, RPM e Vibração). O objetivo principal é homologar um modelo de alta confiabilidade que minimize paradas não planejadas na linha de produção e otimize os custos de manutenção preventiva.

A arquitetura do projeto foi desenhada sob um **rigoroso critério de governança estatística**, garantindo a eliminação completa de vazamento de dados (*data leakage*), o controle de super-ajuste (*overfitting*) e a aderência estrita às diretrizes operacionais da empresa.

---

## 🏗️ Arquitetura do Pipeline (As 7 Fases)

O desenvolvimento foi dividido de forma modular em 7 fases sequenciais dentro do ambiente de desenvolvimento:

*   **Fase 1: Análise Exploratória de Dados (EDA):** Identificação do perfil estatístico dos sensores e diagnóstico do severo desbalanceamento de classes (máquinas normais vs. com falha).
*   **Fase 2: Divisão Estratégica da Base:** Separação imediata dos dados em conjuntos de Treino e Teste, blindando a base de teste antes de qualquer etapa de processamento.
*   **Fase 3: Balanceamento de Classes:** Aplicação do método SMOTE (Oversampling) de forma restrita **apenas aos dados de treino**, evitando contaminação estatística.
*   **Fase 4: Escalonamento e Ajuste de Escala:** Padronização estatística (`StandardScaler`) aplicada de forma isolada para algoritmos geométricos (KNN) e preservação dos dados brutos para algoritmos de árvore.
*   **Fase 5: Engenharia de Atributos:** Seleção e transformação das variáveis preditoras com base nas grandezas físicas do maquinário.
*   **Fase 6: Ajuste de Parâmetros e Combate ao Overfitting:** Treinamento competitivo dos modelos com monitoramento do *Gap* de generalização.
*   **Fase 7: Avaliação da Acurácia e Veredito Final:** Homologação do classificador definitivo com validação complementar por Matrizes de Confusão.

---

## 📊 Governança Estatística: O Critério do Gap de Estabilidade

Seguindo as premissas do comitê de avaliação, o indicador primário de estabilidade do modelo é o **Gap de Generalização**, calculado estritamente como:

$$Gap = \text{Acurácia de Teste} - \text{Acurácia de Treino}$$

A partir do comportamento dessa métrica, estabelecemos três diagnósticos operacionais:
1. **Risco de Underfitting:** Acurácia de treino inferior a **90%** (modelo incapaz de aprender o padrão).
2. **Risco de Overfitting:** Gap negativo e inferior a **-3,00%** (modelo memorizou o treino e perdeu desempenho no teste).
3. **Altamente Estável / Excelente Ajuste:** Gaps contidos entre **0% e -3,00%** combinados com alta acurácia (ponto ideal de equilíbrio).

### 📊 Painel Geral de Experimentos

| Algoritmo | Acurácia Treino | Acurácia Teste (Alvo) | Gap (Teste - Treino) | Diagnóstico Operacional |
| :--- | :---: | :---: | :---: | :--- |
| **Árvore (Profundidade=3)** | 86,31% | 93,35% | +7,04% | 🛑 Risco de Underfitting |
| **KNN (K=3)** | 98,53% | 94,15% | -4,38% | 🚨 Risco de Overfitting (Ruído do SMOTE) |
| **Árvore (Ilimitada)** | 99,95% | 96,70% | -3,25% | 🚨 Risco de Overfitting (Memorização) |
| **LightGBM Personalizado** | **99,57%** | **97,80%** | **-1,77%** | 🏆 **Altamente Estável / Excelente Ajuste** |

---

## 🏆 Veredito Final e Recomendação Institucional

O modelo **LightGBM (200 árvores, Learning Rate = 0.03)** foi formalmente selecionado e homologado para implantação no ambiente produtivo devido aos seguintes fatores:

1. **Liderança Absoluta na Métrica Alvo:** Atingiu a maior **Acurácia Global de Teste (97,80%)**, superando a Árvore de Decisão em 1,10% e o KNN em 3,65%.
2. **Robustez Matemática contra Instabilidade:** Foi o algoritmo baseado em árvores que melhor controlou o sobreajuste, contendo seu Gap em apenas **-1,77%**, mantendo-se seguro dentro do limite crítico de -3,00%.
3. **Plus Analítico (Minimização de Erros Absolutos):** A validação por matrizes de confusão provou a eficiência prática do modelo. O LightGBM foi o classificador que gerou o menor impacto operacional, cometendo apenas **44 erros de predição em um universo de 2.000 amostras inéditas**, contra 66 erros da Árvore e 117 do KNN.

---

## 📁 Estrutura de Pastas do Repositório

```text
├── data/               # Datasets industriais (omitidos no controle de versão)
├── notebooks/          # Notebook (.ipynb) com a execução das 7 fases
├── outputs/            # figura4.png (Matrizes de Confusão salvas em alta resolução)
└── README.md           # Documentação técnica e governança do projeto
