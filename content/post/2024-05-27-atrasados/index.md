---
title: 'Probabilidades: uma solução analítica e uma simulação de Monte Carlo'
author: Bruno Ritter
date: '2024-05-27'
slug: atrasados
categories:
  - Data Science
tags:
  - probabilidade
  - data science
  - estatística
---



Seguindo a jornada iniciada no primeiro post de resolver os desafios do data puzzles para tirar a poeira depois de alguns anos longe dos notebooks, neste post vamos explorar o desafio dos amigos atrasados.

O nome do desafio é right on time, e tem como pré requisitos conhecimentos em fundamentos de estatística, distribuições de probabilidade, e capacidade de programar pequenas simulações "na unha". Parece divertido.

O exercício é o seguinte: preciso marcar um compromisso com 4 amigos, e é importante que todos estejam presentes às 18h. Conhecendo meus amigos, sei que têm o hábito de se atrasar. Sei inclusive *como* - ou, ainda melhor, o *quanto* - eles costumam se atrasar. O tempo de chegada de cada um deles pode ser descrito como uma variável aleatória que segue uma distribuição normal, com média na hora marcada, e desvio padrão de 10 minutos. 

**Qual é o horário que devo marcar para ter 99% de confiança de que todos meus amigos estarão presentes às 18h?**

Eis a questão.

Primeiro, vamos entender o que as informações disponibilizadas significam. Sabemos que a média do tempo de chegada é o tempo marcado, ou seja, o *atraso médio* é `\(\mu = \mathbf{0}min\)`. E também sabemos *desvio padrão* do atraso médio é de `\(\sigma = 10 min\)`. 


```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# Gerando dados de exemplo
data = np.random.normal(loc=0, scale=10, size=1000)

# Criando o gráfico de densidade de probabilidade
sns.kdeplot(data, shade=True)

# Configurando o título e os rótulos
plt.title('Gráfico de Densidade de Probabilidade')
plt.xlabel('Valor')
plt.ylabel('Densidade')

# Exibindo o gráfico
plt.show()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-1-1.png" width="672" />
