# Featuring engineering

## Seleção de features

* análise de correlação: identica features altamente correlacionadas com as variáveis alvo
* RFECV: Recursive Feature Elimination with Cross-Validation: algoritmo que remove features menos importantes

## Transformação de features

* Codificação de variáveis categóricas: transforma variaveis categóricas em numéricas, como One-hot encoding
* Normalização e padronização: ajusta a esacala das features para melhorar a performance do modelo
* engenharia de recursos: cria novas features a partir das existentes, como interações, polinômios, etc.

## Criação de features

* Combinação de features: combinação de features existentes para criar novas features
* extração de features: extrai características relevantes de dados não estruturados, como texto, imagens, etc.

## Validação

* Cross-validation: técnica para avaliar a performance do modelo cruzando diferentes conjuntos de treinamento e teste
* Curva ROC e AUC: métricas para avaliar a performance de modelos de classificação

## Ferramentas

- Featuretools: biblioteca para automatizar a engenharia de features
- Scikit-learn: biblioteca de machine learning com diversas ferramentas para engenharia de features
- Pandas: biblioteca para manipulação de dados, útil para transformação e criação de features
- Feature Stora Amazon SageMaker: serviço da AWS para armazenar e compartilhar features entre modelos
