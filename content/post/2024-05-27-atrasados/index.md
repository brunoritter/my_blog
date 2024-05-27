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

O nome do desafio é right on time, e tem como pré requisitos conhecimentos em fundamentos de estatística, distribuições de probabilidade, e capacidade de programar pequenas simulações "na unha". Parece divertido, e além disso, outra coisa que busquei exercitar neste post foi a tradução de conceitos estatísticos em uma linguagem mais simples e acessível.

O exercício é o seguinte: preciso marcar um compromisso com 4 amigos, e é importante que todos estejam presentes às 18h. Conhecendo meus amigos, sei que têm o hábito de se atrasar. Sei inclusive *como* - ou, ainda melhor, o *quanto* - eles costumam se atrasar. O tempo de chegada de cada um deles pode ser descrito como uma variável aleatória que segue uma distribuição normal, com média na hora marcada, e desvio padrão de 10 minutos. 

**Qual é o horário que devo marcar para ter 99% de confiança de que todos meus amigos estarão presentes às 18h?**

Eis a questão.

Primeiro, vamos entender o que as informações disponibilizadas significam. Sabemos que a média do tempo de chegada é o tempo marcado, ou seja, o *atraso médio* é `\(\mu = \mathbf{0} min\)`. E também sabemos *desvio padrão* do atraso médio é de `\(\sigma = 10  min\)`. 

Isso nos dá uma enorme previsibilidade sobre o comportamento dos nossos amigos, e já nos permite começar a fazer algumas simulações e visualizar nossas probabilidades.

Vamos começar simulando 1000 encontros com um amigo, e em seguida montar um histograma dos atrasos nestes encontros.


```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# Usamos o numpy para obter 1000 amostras de uma distribuição de atrasos
np.random.seed(1568)
atraso_medio = 0
desvio_padrão = 10
sims = 1000 # define o número de simulações
atrasos = np.random.normal(loc=atraso_medio, scale=desvio_padrão, size=sims)

# Usamos o seaborn para plotar um histograma, com curva de densidade
sns.histplot(atrasos, kde=True, bins=30, color='orange')

# Configurando o título e os rótulos
plt.title('Histograma com Curva de Densidade')
plt.xlabel('Atraso (min)')
plt.ylabel('Frequência')

# Exibindo o gráfico
plt.show()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-1-1.png" width="672" />

Aqui temos uma visão completa do que significam os parâmetros da distribuição. Percebemos que nosso amigo se atrasou com a mesma frequência que se adiantou. Embora na maior parte das vezes o atraso é próximo de 10min, em algumas situações eu tive de esperar mais de 30min.

Qual a probabilidade dele se atrasar? Podemos ver o registro das nossas simulações e calcular em qual proporção dos encontros ele se atrasou para obtermos uma estimativa.


```python
contagem_atrasos = np.sum(atrasos > 0) 
p_atraso = contagem_atrasos/sims
print(f"Nosso amigo se atrasou em {contagem_atrasos} da simulações. Estimamos que a probabilidade de atraso é de {p_atraso*100}%")
```

Nosso amigo se atrasou em 488 da simulações. Estimamos que a probabilidade de atraso é de 48.8%

E este, meus caros, foi o método de Monte Carlo em ação. 