# Sistema de estimativa de risco de fogo com Arduino e IA

Projeto acadêmico que utiliza dados meteorológicos e aprendizado de máquina para estimar o **índice de risco de fogo**. A proposta é integrar o modelo a um Arduino, utilizando principalmente leituras de **temperatura e umidade do ar**.

## Notebooks

- **`SerraDaPaulistaQueimadas.ipynb`**: estudo inicial dos focos de fogo próximos à Serra da Paulista e integração de dados do INPE com informações meteorológicas.
- **`ModeloRiscoFogo2024.ipynb`**: preparação da base anual, criação das variáveis, treinamento e comparação dos modelos.

> Os arquivos podem ser renomeados antes da publicação para usar os nomes acima.

## Base de dados

Foi utilizado o dataset [Queimadas INPE](https://www.kaggle.com/datasets/saviovianna/queimadas-inpe), que combina focos detectados pelo INPE com dados meteorológicos do Open-Meteo.

Os 12 arquivos mensais de 2024 foram unidos, totalizando aproximadamente **3,2 milhões de registros**. Para aproximar o treinamento da região do projeto, foram considerados os biomas **Cerrado** e **Mata Atlântica**.

Principais variáveis:

- temperatura do ar;
- umidade relativa do ar;
- mês do ano;
- bioma;
- índice `risco_fogo`, utilizado como variável de saída.

A umidade relativa foi calculada a partir da temperatura e do ponto de orvalho pela fórmula de Magnus.

## Modelos testados

- modelo de referência pela média;
- árvore de decisão;
- Gradient Boosting;
- rede neural MLP (Perceptron Multicamadas).

O melhor resultado obtido até o momento foi com a **MLP**:

- **R²:** aproximadamente `0,62`;
- **MAE:** aproximadamente `0,10`, na escala de 0 a 1.

Isso indica que o modelo explicou cerca de 62% da variação do índice e apresentou erro absoluto médio próximo de 10 pontos percentuais.

## Melhorias realizadas

- união dos 12 meses de 2024;
- filtro dos biomas Cerrado e Mata Atlântica;
- cálculo da umidade relativa do ar;
- remoção de registros incompletos;
- representação cíclica do mês com seno e cosseno;
- separação temporal entre treino e teste;
- comparação com um modelo simples de referência;
- teste regional utilizando somente dados de São Paulo;
- criação do déficit de pressão de vapor (DPV) usando apenas temperatura e umidade;
- treinamento e ajuste de diferentes algoritmos.

## Funcionamento proposto

1. O Arduino coleta temperatura e umidade do ambiente.
2. O sistema acrescenta o mês e o bioma previamente configurado.
3. O modelo estima um índice entre 0 e 1.
4. O resultado é convertido em um nível de alerta, como mínimo, baixo, médio, alto ou crítico.

## Limitações

O `risco_fogo` é um **índice de condições favoráveis ao fogo**, e não uma probabilidade calibrada de uma nova queimada. Além disso, a base utilizada contém registros associados a focos já detectados. Portanto, o projeto deve ser apresentado como um protótipo acadêmico de estimativa e alerta preventivo.

## Tecnologias

- Python;
- Pandas e NumPy;
- Scikit-learn;
- Google Colab;
- Arduino e sensores de temperatura e umidade.

## Status

Projeto parcialmente concluído. As próximas etapas incluem validar a MLP em diferentes períodos, transformar o índice em níveis de alerta e integrar o modelo ao protótipo com Arduino.
