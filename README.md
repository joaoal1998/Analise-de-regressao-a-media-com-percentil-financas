# Análise Quantitativa do Ibovespa - Estratégia de Reversão à Média

## 📊 Visão Geral
Este projeto implementa uma estratégia de trading quantitativo baseada em reversão à média para o índice Ibovespa (^BVSP), utilizando análise estatística avançada e detecção de anomalias.

## 🛠️ Instalação e Configuração

### Dependências Necessárias
```python
# Pip install yfinance==0.2.58
```

### Importação das Bibliotecas
```python
import yfinance as yf
import pandas as pd
import numpy as np
from scipy import stats
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import warnings
warnings.filterwarnings('ignore')
```

## 📈 Coleta e Preparação dos Dados

### Download dos Dados Históricos
- **Ticker**: ^BVSP (Ibovespa)
- **Período**: 2016-01-01 a 2025-12-31
- **Fonte**: Yahoo Finance via yfinance

```python
ticker = '^BVSP'
dtini = '2016-01-01'
dtfin = '2025-12-31'
df = yf.download(ticker, dtini, dtfin, auto_adjust=False)
```

## 🔍 Análise Exploratória de Dados (EDA)

### Informações Básicas
- Dimensões do dataset
- Tipos de dados
- Valores ausentes
- Duplicatas
- Estatísticas descritivas

### Análise Estatística Detalhada
Para cada coluna do dataset:
- Média e Mediana
- Moda
- Desvio Padrão
- Assimetria (Skewness)
- Curtose (Kurtosis)
- Valores mínimos e máximos

## 📊 Construção da Estratégia

### Cálculo do Delta
O **Delta** representa a diferença entre o preço de fechamento ajustado e sua média móvel:
```python
df['MM'] = df['Adj Close'].rolling(20).mean()
df['Delta'] = df['Adj Close'] - df['MM']
```

### Método de Bandas Melhoradas
Utiliza percentis móveis para detecção robusta de anomalias:

#### Parâmetros:
- **Janela**: Tamanho da janela móvel (padrão: 20 períodos)
- **Percentil**: Percentual de dados fora das bandas (padrão: 2%)

#### Cálculo das Bandas:
- **Banda Superior**: Percentil 98% dos valores Delta
- **Banda Inferior**: Percentil 2% dos valores Delta
- **Média Móvel**: Mediana móvel (robusta para outliers)

## 🎯 Lógica da Estratégia

### Detecção de Anomalias
- **Anomalia Superior**: Delta > Banda Superior
- **Anomalia Inferior**: Delta < Banda Inferior

### Regras de Trading
- **Sinal de Compra**: Quando Delta < Banda Inferior E Delta > -5000
- **Horizonte**: Operações com duração de dias especificados
- **Tipo**: Long-only (apenas compras)

### Filtros de Segurança
- Filtro adicional para evitar compras em quedas extremas (Delta > -5000)

## 📊 Análise de Performance

### Métricas Calculadas
- **Total de Trades**: Número de operações executadas
- **Taxa de Acerto**: Percentual de trades positivos
- **Retorno Total**: Performance acumulada da estratégia
- **Retorno Médio por Trade**: Rentabilidade média das operações
- **Sharpe Ratio**: Relação risco-retorno ajustada
- **Drawdown Máximo**: Maior perda a partir do pico

### Análise de Anomalias
- Frequência de detecção de anomalias
- Distribuição entre anomalias superiores e inferiores
- Percentual de dias com anomalias detectadas

## 📈 Visualizações

### Gráfico Triplo Interativo
1. **Delta com Bandas**: Visualização das bandas e série Delta
2. **Sinais de Trading**: Pontos de entrada no gráfico de preços
3. **Performance Acumulada**: Evolução do retorno da estratégia

## 🔧 Parâmetros Configuráveis

### Estratégia Principal
- `janela`: Tamanho da janela móvel (padrão: 10)
- `percentil`: Percentil para bandas (padrão: 5)
- `alvo_dias`: Duração das operações (padrão: 5)

### Personalização
Todos os parâmetros podem ser ajustados para otimização da estratégia conforme diferentes condições de mercado.

## 📋 Resultados e Interpretação

### Estatísticas da Série Delta
- Análise de normalidade dos dados
- Momentos estatísticos (média, variância, assimetria, curtose)
- Identificação de padrões de reversão

### Performance da Estratégia
- Retornos absolutos e relativos
- Análise de risco-retorno
- Comparação com buy-and-hold

## 🚀 Execução

Para executar a análise completa:
```python
df_resultado = executar_analise_completa(df)
```

Esta função executará:
1. Análise exploratória completa
2. Implementação da estratégia
3. Cálculo de performance
4. Geração de visualizações interativas

## 📊 Considerações Técnicas

### Robustez Estatística
- Uso de percentis em vez de desvios padrão para maior robustez
- Mediana móvel para reduzir impacto de outliers
- Análise de normalidade da série Delta

### Limitações
- Estratégia long-only (não considera vendas a descoberto)
- Não considera custos de transação
- Baseada apenas em dados históricos
- Assume liquidez suficiente para execução

## 🎯 Próximos Passos

- Implementação de stop-loss e take-profit
- Análise de diferentes horizontes temporais
- Inclusão de custos de transação
- Teste em outros ativos
- Implementação de estratégias short

Disclaimer: Esta ferramenta é apenas para fins educacionais e de pesquisa. Não constitui aconselhamento financeiro. Sempre consulte um profissional qualificado antes de tomar decisões de investimento.
