# Análise exploratória e previsão de retenção de Funcionários


```
Esse documento é composto pelas seguintes seções:

1. Contexto e Objetivo
2. Estrutura dos Dados
3. Considerações Sobre os Dados
4. Tecnologias Utilizadas
5. Principais Descobertas da Exploração
6. Modelo Escolhido e Resultados
7. Recomendações
```

## Objetivo 

Devido à preocupação com a satisfação e a alta rotatividade dos colaboradores, a empresa `Salifort Motors` decidiu realizar uma pesquisa interna. O objetivo principal é identificar pontos de melhoria no ambiente de trabalho e aumentar a capacidade de prever a retenção de funcionários. Com isso, a empresa busca agir preventivamente para evitar desligamentos, reduzindo custos com recrutamento, seleção e treinamento ao longo do tempo.

Este projeto visa analisar dados coletados dos colaboradores, identificar padrões relacionados à retenção e propor recomendações baseadas em evidências. Ao final, espera-se fornecer insights práticos que possam apoiar a tomada de decisão estratégica da empresa, promovendo um ambiente de trabalho mais saudável e sustentável.

## Estrutura dos dados

Cada linha do dataset representa um funcionário, com as seguintes colunas:
- `satisfaction_level`: Nível de satisfação do funcionário (0 a 1)
- `last_evaluation`: Avaliação do funcionário (0 a 1)
- `number_project`: Número de projetos do funcionário
- `average_montly_hours`: Média de horas trabalhadas no mês
- `time_spend_company`: Tempo de empresa do funcionário (anos)
- `Work_accident`: Se o funcionário sofreu um acidente de trabalho (0 ou 1)
- `left`: Se o funcionário deixou a empresa (0 ou 1)
- `promotion_last_5years`: Se o funcionário recebeu promoção nos últimos 5 anos (0 ou 1)
- `Department`: Departamento do funcionário
- `salary`: Faixa salarial do funcionário (low, medium, high)

## Considerações sobre os dados

Baseado nas informações, esse dataset é provavelmente resultado da união de fontes distintas, como `pesquisas de satisfação`, `avaliações de desempenho` e `registros de funcionários`, que podem vir de arquivos ou bancos de dados. Em um caso real, seria necessário um trabalho de integração e limpeza dos dados para que tal estrutura fosse alcançada com as informações necessárias.

Além disso, é fundamental garantir a privacidade e a segurança dos dados dos funcionários, em conformidade com a Lei Geral de Proteção de Dados (LGPD). Isso inclui a anonimização de informações pessoais e a adoção de práticas éticas no tratamento dos dados, assegurando que nenhum dado sensível ou identificador, como nomes ou IDs, esteja presente no dataset. O respeito à legislação e à ética é essencial para proteger os direitos dos colaboradores e manter a confiança no processo de análise.

## Tecnologias Utilizadas

```
Linguagem: 
Python

Bibliotecas:
- NumPy ( Para alguns cálculos em arrays )
- Pandas ( Para Manipulação dos Dados )
- Matplotlib ( Para Visualização de Dados )
- Seaborn ( Para Visualização dos Dados )
- Scikit-learn ( Para Modelagem e Avaliação de Modelos )

IDE: 
Visual Studio Code
```


## Principais Descobertas Da Exploração

#### 1. Satisfação dos Funcionários

Foi percebido que funcionários com 4 anos de empresa apresentaram menor média de satisfação e também maior variabilidade na pontuação, e após esse período, a satisfação tende a aumentar. Então algo está acontecendo com esse grupo de funcionários que pode estar levando à insatisfação.



![Satisfaction Level PorTempo](SatisfactionLevelPorTempo.png)


<br>

_Vamos investigar!_
<br>

Investigando mais a fundo esses funcionários, também viu-se que seus salários estão concentrados entre baixos/médios, o que pode indicar uma possível insatisfação com a remuneração.

![Níveis De Salário](SalarioFuncionariosCom4Anos.png)

Não é dito explicitamente as faixas de salários já que se tratam de dados fictícios, mas de forma geral o que se vê é que os funcionários com 4 anos de empresa aparentemente estão em uma faixa salarial que não condiz com suas expectativas ou com o mercado, o que pode levar à insatisfação e eventual desistência.

Mas o que poderia justificar o menor salário, talvez seja algumas das seguintes possibilidades:

- Carga menor de trabalho e menor atuação em projetos
- Menor pontuação de desempenho

Essas variáveis também foram investigadas:

#### Carga de Trabalho
![Projetos e Horas Por Grupo de Funcionários](ProjetosEHoras4AnosvsOutros.png)

<br>

#### Desempenho:
![Distribuição da Última Avaliação](DistribuicaoLastEvaluation.png)

Na verdade, o oposto foi encontrado. Funcionários com 4 anos de empresa estão trabalhando mais horas e participando de mais projetos, além de terem uma média/mediana de avaliação maior do que os demais grupos. 

Agora, vamos descobrir como é a rotatividade desses funcionários, em comparação aos demais, afinal no fim das contas eles pode não ser necessariamente os que mais geram rotatividade.

![Rotatividade Geral vs 4 Anos](RotatividadeGeralvs4Anos.png)

_Aqui usei uma métrica de rotatividade, que elimina a diferença na quantidade dos dois grupos, como uma forma de 'normalizar' a comparação._

E na verdade confirmando a premissa, de fato são o que mais geram rotatividade. Então muito provavelmente o que está acontecendo é que esses funcionários estão se sentindo desvalorizados, e por isso acabam saindo da empresa.

## Modelo escolhido e resultados

O modelo escolhido foi o Decision Tree, pois é capaz de lidar com dados não balanceados que é o caso deste dataset (80% dos funcionários não saem da empresa, enquanto 20% saem). Além de não exigir uma premissão com relação a distribuição dos dados, o que é uma vantagem em relação a outros modelos como Regressão Logística.

A métrica escolhida para avaliar o modelo foi o Recall, visto que o objetivo é identificar e prever funcionários que deixarão a empresa, a fim de evitar custos com recrutamento e seleção.
Essa métrica não se importa com falsos positivos, o que não é um problema, visto que com certeza o custo de tentar manter um funcionário, será menor em comparação ao custo de perder um funcionário além de que o modelo não tem a pretensão de prever 100% dos casos corretamente, mas sim de identificar os funcionários com maior risco de saída.

### Resultados do Modelo Com Satisfaction Level

Após treinar o primeiro modelo, obtivemos os seguintes resultados:

```
Modelo Com Satisfaction Level:

Accuracy: 0.977
Precision: 0.942
➡️ Recall: 0.963 ⬅️
F1 Score: 0.952
```
Podemos ver que o modelo tem uma excelente capacidade de prever a saída de funcionários. E aqui de forma ranqueada as principais influências da saída de funcionários:

```
satisfaction_level: 0.603 ± 0.013
number_project: 0.517 ± 0.007
last_evaluation: 0.479 ± 0.010
average_montly_hours: 0.407 ± 0.014
time_spend_company: 0.229 ± 0.004
Work_accident: 0.007 ± 0.002
salary_low: 0.006 ± 0.002
salary_medium: 0.001 ± 0.001
promotion_last_5years: 0.000 ± 0.000
```

A métrica utilizada foi a _Permutation Importance_, que mede a importância de cada variável ao permutar seus valores e observar o impacto na performance do modelo.

Podemos ver que o nível de satisfação é o fator mais importante, seguido pelo número de projetos e a última avaliação. A carga horária média mensal também é um fator relevante, mas está de certa forma ligada a quantidade de projetos, então podemos dizer que o número de projetos é o principal fator relacionado à carga horária.

Porém, após os resultados, pensei que poderia ser uma melhor ideia remover a variável `satisfaction_level` pois ela acaba tendo um peso muito grande no modelo, e talvez esconder outros insights interessantes, que possibilitem uma ação mais direta, pois não há como agir diretamente no nível de satisfação do funcionário, mas sim em fatores que podem influenciar esse nível.

### Resultados do Segundo Modelo Sem Satisfaction Level

```
Modelo sem satisfaction_level:

Accuracy: 0.944
Precision: 0.850 
➡️ Recall: 0.931 ⬅️
F1 Score: 0.888
```
E novamente, o permutation importance nos trouxe os seguintes resultados:

```
last_evaluation: 0.586 ± 0.014
average_montly_hours: 0.556 ± 0.011
number_project: 0.423 ± 0.007
time_spend_company: 0.246 ± 0.008
Work_accident: 0.000 ± 0.000
promotion_last_5years: 0.000 ± 0.000
salary_low: 0.000 ± 0.000
salary_medium: 0.000 ± 0.000
```

Agora o nível de satisfação deu lugar à variável `last_evaluation`, que representa a última avaliação do funcionário, onde tomar uma ação é mais direto e intuitivo. 

Seguido pela quantidade de `horas e número de projetos`, que também são fatores que podem ser ajustados diretamente pela empresa. Essas variáveis são linearmente correlacionadas, como podemos ver na imagem abaixo, dessa forma, podemos considerar apenas o número de projetos como o principal fator relacionado à carga horária, e alvo das ações da empresa.

![ProjetosVsHoras](ProjetosVsHoras.png)


## Recomendações

Baseado nas variáveis de maior impacto, e após analisar mais afundo essas variáveis, vamos ver ações que irão ajudar a diminuir a rotatividade de funcionários, e melhorar a satisfação dos mesmos. _A ideia aqui é focar naqueles que saíram pra entender os prováveis motivos_.

![Saídas](PrincipaisVariáveisPorSaída.png)

Quando olhamos para o nível de satisfação no gráfico acima, uma grande parte dos funcionários que saem da empresa estão com nível de satisfação abaixo de 0.6. Como uma das variáveis de maior importância é a carga de trabalho, vamos filtrar esses funcionários e ver como estão os projetos estão distribuídos entre eles.


![Número de projetos Por Baixa Satisfação](NumProjetosBaixaSatisfacao.png)

Podemos ver que esses funcionários estão com picos de saída com 1-2 projetos ou acima de 5 projetos, o que indica que eles podem estar sobrecarregados ou não desafiados o suficiente.

**Então o ideal é que cada funcionário tenha entre 3 e 5 projetos.**

E se olharmos o **gráfico abaixo**, vemos que há espaço pra uma adequação de número de projetos, existem funcionários com 2-3 anos de empresa com menos de 3 projetos, esses seriam os primeiros a receberem projetos de funcionários com 4 anos de empresa, que na maioria, estão sobrecarregados, que se dá provavelmente por estarem melhores nas avaliações de desempenho na média. [Sessão de Exploração](#desempenho)


🟢 O que se espera é que ao longo do tempo, com os projetos melhores distribuídos, isso impacte no nível de satisfação. Isso ainda sem pensar em aumentar o salário, dessa forma evitando custos maiores de imediato, e ainda tendo um impacto significativo.

![NúmerodeProjetosPorGrupodeFuncionários](NúmerodeProjetosPorGrupodeFuncionários.png)

<br>

Ainda há um caso identificado na base, que mesmo funcionários que estão com número ideal de projetos (entre 3-5 projetos) e acima dos 4 anos de empresa, estão saindo, o que vai de encontro com a recomendação acima. (**Gráfico abaixo**)

<br>

![% Funcionários com Número Ideal de Projetos e Saíram](NumProjetosMaisde4AnosESairam.png)


<br>

*Aqui está a possível explicação para essas saídas*

<br>


![Promoção Funcionários com mais de 4](QtdFuncionariosComPromocaoMaisde4Anos.png)

Apenas **1 funcionário** dentre aqueles que já estão a pelo menos 5 anos na empresa recebeu uma promoção. O que poderia facilmente gerar incômodo e sensação de desvalorização.

<br>


![Última Avaliação Funcionários Que Saíram E Mais de 5 Anos](ÚltimaAvaliaçãoFuncionáriosQueSaíramEMaisde5Anos.png)

Além disso a grande maioria desses mesmos colaboradores tiveram uma pontuação excelente (acima 0.8), indo ao encontro dos resultados do segundo modelo, que trouxe sendo a de mais impacto na saída de um funcionário. O que traz a ideia de que esses funcionários estão se sentindo esquecidos e não suficientemente valorizados.

<br>



Agora o Salário destes:

<br>


![Salário Funcionarios Com Mais De 4 Anos E Saíram.png](SalárioFuncionariosComMaisDe4AnosESaíram.png)

A maioria deles recebem um salário de nível baixo, que quase certamente não está acompanhando, tanto seu tempo de empresa, quanto o resultado da avaliação. Espera-se que estes colaboradores possuam pelo menos um salário de nível médio para alto.

🟢 O recomendado é que haja um acompanhamento melhor desses funcionários tendo uma política de reajuste salarial e promoções mais bem definidas, pois a empresa não parece possuir um monitoramento eficiente, o que faz com que percam talentos valiosos.


## Conclusão

Como visto, há uma alta rotatividade principalmente com funcionários que estão a 4 anos na empresa, e baseado na exploração e resultados do modelo, o que mais impacta é a carga de trabalho de cada funcionário determinado pelo número de projetos, então chegou-se a conclusão que o número ideal pra cada um é entre 3 e 5 projetos.

Investigando mais, determinou-se também, que há funcionários acima dos 4 anos, que mesmo com o número ideal de projetos, estão saindo. E descobriu-se que os motivos são a falta de promoção ou aumento do salário. O que deveria ser ajustado, visto que estes funcionários também tem um desempenho excelente, baseado em suas avaliações, então afim de evitar frustração e mais saídas é crucial que essa ação seja tomada.

Chegou se também a conclusão, que por esses motivos, a companhia necessita, de uma política de promoções / reajuste salarial, talvez inicialmente baseado na avaliação de desempenho, enquanto que em paralelo administra melhor a carga horária e número de projetos dos seus funcionários.

> **Importante**: Após explorar os funcionários que ainda se mantiveram na empresa, descobriu-se que `53.15%` deles estão em risco de saída(baseado nas variáveis identificadas na exploração e resultado dos modelos). Além disso, usando o mesmo conceito para aqueles que saíram, verificou se que dos `3571` que saíram, `3547` (`99.33%`) foram pelos motivos destacados, ou seja, apenas 24 saíram por outros motivos.