# **Detecção de Fraude**

# **1\. Introdução**

A detecção de fraudes em transações financeiras é um dos principais desafios em sistemas de pagamento. A ocorrência de operações suspeitas impacta diretamente a segurança, a credibilidade e a sustentabilidade do negócio. Antecipar e identificar transações fraudulentas de forma automatizada é essencial para reduzir perdas financeiras, proteger clientes e otimizar processos de monitoramento. Este projeto aplica técnicas de ciência de dados e machine learning para prever a probabilidade de fraude a partir de informações históricas de clientes, terminais e transações.

## **2\. Problema Resolvido**

O problema central abordado neste projeto é a identificação automática de transações fraudulentas em um grande volume de operações. Como a taxa de fraude é baixa em comparação ao total de transações, trata-se de um problema de classificação binária desbalanceada, no qual o objetivo é aumentar a capacidade de identificar fraudes (recall) sem comprometer excessivamente a precisão e mantendo métricas de discriminação robustas, como AUC, Gini e KS.

## **3\. Modo de Resolução do Problema**

O projeto foi desenvolvido em três grandes etapas:

### **3.1. Entendimento dos Dados**

Nesta fase, foram lidos e explorados os datasets de treino, teste, clientes e terminais. As tabelas foram integradas em uma ABT (Analytical Base Table) única, enriquecendo as transações com informações cadastrais e geográficas. Foram conduzidas análises descritivas e de sanidade, verificando taxas de fraude por período, terminais com maior ocorrência e perfis de clientes envolvidos. Essa etapa trouxe insights sobre a distribuição dos dados, sazonalidade e concentração de eventos suspeitos.

### **3\. 2\. Preparação dos Dados**

A etapa seguinte foi dedicada ao feature engineering, criando variáveis que aumentam a capacidade preditiva do modelo. Foram geradas features temporais (hora, dia da semana, fim de semana, período do dia), cálculos de distância entre clientes e terminais via fórmula de Haversine, além de variáveis agregadas em janelas de tempo (somas, médias, medianas, mínimos, máximos e quantidades de transações em 1h, 24h, 7 dias etc.). Também foram incluídas flags de risco, como transações noturnas e consecutivas no mesmo terminal. Esse processo resultou em uma base enriquecida, estruturada para melhor capturar padrões de fraude.

### **3\. Modelagem Preditiva**

Com os dados preparados, foram construídos pipelines de pré-processamento que incluíram imputação de valores ausentes, codificação de variáveis categóricas via *Target Encoding* e padronização das variáveis numéricas com *StandardScaler*. Em seguida, diferentes algoritmos foram testados, incluindo Árvore de Decisão, Regressão Logística, Random Forest, Gradient Boosting, XGBoost e LightGBM. As avaliações consideraram métricas como acurácia, precisão, recall, F1-score, AUC, Gini e KS.

O modelo XGBoost, após a tunagem de hiperparâmetros com *Optuna*, apresentou o melhor equilíbrio entre performance e capacidade discriminativa. Também foram realizados testes de redução de dimensionalidade (mantendo as 50 e depois 25 variáveis mais importantes) para comparar latência e desempenho, sendo escolhida a versão com 50 variáveis, por preservar melhores resultados de classificação com tempo de predição ainda adequado para aplicações em produção.

## **4\. Estrutura do Projeto**

├── 00-Dados/                            \# Pasta com os conjuntos de dados brutos e derivados

│   ├── customer.csv                     \# Informações dos clientes

│   ├── terminal.csv                     \# Dados dos terminais de transação

│   ├── train.csv                        \# Base de treino utilizada para modelagem

│   ├── test.csv                         \# Base de teste para validação do modelo

│   └── abt\_vigente\_score.csv            \# Base tratada e pronta para scoring ou previsão

├── 01-Entendimento e Preparação dos Dados/   \# Análises e notebooks de exploração e limpeza dos dados

├── 02-Modelagem/                        \# Etapa de modelagem e avaliação dos modelos de machine learning

├── Readme                               \# Explicação do projeto

## **5\. Tecnologias Utilizadas**

* Linguagem: Python

* Bibliotecas:

  * pandas, numpy → manipulação e análise de dados

  * matplotlib, seaborn → visualização

  * scikit-learn → pré-processamento, modelagem e métricas

  * xgboost, lightgbm, catboost → algoritmos de boosting

  * optuna → tunagem de hiperparâmetros

**6\. Resultados Obtidos**

A análise exploratória inicial mostrou que a base continha 291.231 transações, envolvendo 998 clientes e 1.994 terminais diferentes, o que garantiu representatividade e robustez para os testes. A partir da preparação, foram criadas dezenas de variáveis derivadas capazes de capturar comportamentos suspeitos. Na modelagem, o XGBoost tunado se destacou, alcançando AUC-ROC de 0.7743, Gini de 0.45 e KS de 0.36, valores que indicam boa capacidade de diferenciar transações legítimas de fraudulentas. 

Além disso, foi possível simplificar a solução para apenas 50 variáveis sem perda significativa de desempenho, reduzindo a latência e tornando o modelo mais adequado para aplicação em ambientes de produção. 

O impacto direto dessa solução está na maior agilidade na identificação de fraudes, permitindo respostas quase em tempo real, na redução de perdas financeiras, ao detectar um maior número de casos suspeitos automaticamente, e no apoio estratégico às equipes de risco, que passam a contar com uma ferramenta de priorização baseada em scores de fraude. Dessa forma, o projeto entregou um pipeline robusto e escalável, capaz de fortalecer a segurança das operações e aumentar a eficiência do negócio.

