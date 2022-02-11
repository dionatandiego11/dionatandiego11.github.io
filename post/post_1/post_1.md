---
layout: page
title: Treinadores estrangeiros na elite do futebol brasileiro

detail_image: ../../assets/portfolio.png
---

# Analise dos treinadores estrangeiros na elite do futebol brasileiro

Com o avanço do mercado de treinadores estrangeiros no futebol brasileiro, resolvi explorar este fenómeno um pouco mais afundo. 

**Nesta análise** utilizarei as seguintes ferramentas:

- Sqlite3
- Python e Bibliotecas(Pandas e Matplotlib)
- Jupyter Notebook

Você poderá ter acesso ao Dataset no meu [Github](https://github.com/dionatandiego11/Datasets/blob/9be198537b84862ec799c4582746ef00424d5b85/treinadores.csv).

## Mãos a obra!

Primeiramente vamos importar as bibliotecas Necessárias:

```python 

    import sqlite3 as sql
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns
    %matplotlib inline
```

Em seguida vamos importar o Banco de Dados com o Pandas:
```python 
conn = sql.connect('treinadores.db')
treinadores = pd.read_sql('SELECT * FROM treinadores', conn)
```
Agora vamos pegar uma amostra do dataset:
```python 
treinadores.head(n=6)
```

<img src="body_1.png">

Já é possível perceber alguns problemas no dataset, como a má codificação do cabeçalho. 

Então vamos renomear algumas colunas que estão mal codificadas: 
```python 
treinadores = treinadores.rename(columns={'Mesesnocomando': 'Meses_No_Comando'})
treinadores = treinadores.rename(columns={'Títulos': 'Titulos'})
```

Comandos para encontrar e remover outliers do dataset, caso necessario:
```python 
    #treinadores.duplicated()
    #treinadores.dropna() 
    #treinadores.fillna() 
```
Agora vamos procurar problemas nas variaveis: 
```python 
treinadores.dtypes
```
<img src="body_2.png">

Observe que "Aproveitamento" está como string, vamos modificar para ponto flutuante:
```python 
treinadores["Aproveitamento"] = treinadores["Aproveitamento"].astype(str).astype(float)
```
Conferindo:
```python 
treinadores.dtypes
```
<img src="body_3.png">

Conferindo o resultado das transformações:
```python 
treinadores.head()
```
<img src="body_4.png">

# Após fazer o processamento dos dados, enfim vamos analisar os dados.

Primeiramente vamos observar a tendencia de aumento dos treinadores estrangeiros ao longo do tempo.
```python 
treinadores["Ano"].value_counts().sort_values().plot.bar(title="Ano com o maior nº de treindadores")
```
<img src="analise_1.png">

Aqui conhecemos a nacionalidade deles.
```python 
treinadores["Nacionalidade"].value_counts().plot.pie(ylabel='')
```
<img src="analise_2.png">

Neste gráfico podemos observar os treinadores estrangeiros que mais vezes treinaram clubes nacionais.
```python 
treinadores["Treinador"].value_counts().sort_values().plot.bar(title="Treinador")
```
<img src="analise_3.png">

Neste gráfico observamos os clubes que mais buscaram treinadores gringos ao longo dos anos.
```python 
treinadores["Time"].value_counts().sort_values().plot.bar(title="Clubes com o maior nº de Treinadores")
```
<img src="analise_4.png">

Observamos aqui a variação de aproveitamento que os treinadores tiveram no país.
```python 
df = pd.DataFrame(treinadores,columns=['Treinador','Aproveitamento'])
ax = df.plot.bar(x = 'Treinador', y= 'Aproveitamento')
```
<img src="analise_5.png">

Observe que a grande maioria dos treinadores não obtiveram glórias esportivas.
```python 
treinadores["Titulos"].value_counts().sort_values().plot.bar(ylabel='')
```
<img src="analise_6.png">

Treinadores com o maior nº de títulos.
```python 
df = pd.DataFrame(treinadores,columns=['Treinador','Titulos'])
df.groupby(['Treinador']).sum().plot(kind='pie', y='Titulos', ylabel='')
plt.legend().remove()
```
<img src="analise_7.png">

Treinadores com mais meses no comando.
```python 
df = pd.DataFrame(treinadores,columns=['Treinador','Meses_No_Comando'])
ax = df.plot.bar(x = 'Treinador', y= 'Meses_No_Comando')
```
<img src="analise_8.png">

Aqui observamos a correlação entre Aproveitamento e Meses no comando.
```python 
treinadores = pd.DataFrame(treinadores,columns=['Meses_No_Comando','Aproveitamento'])
df = pd.DataFrame(treinadores)
_= sns.lmplot(x = 'Meses_No_Comando', y= 'Aproveitamento', data=df, ci=None) 
```
<img src="analise_9.png">

Agora a correlação entre Aproveitamento e títulos.
```python 
treinadores = pd.DataFrame(treinadores,columns=['Aproveitamento', 'Titulos'])
df = pd.DataFrame(treinadores)
_= sns.lmplot(x = 'Aproveitamento',y= 'Titulos', data=df, ci=None)
```
<img src="analise_10.png">

fechando o banco de dados:
```python 
    conn.close()
```
# Conclusão

Observamos que a tendência de treinadores estrangeiros em terras tupiniquins se encontra em franca ascensão, apesar de os resultados serem "mprevisíveis" com muito altos e baixos, o que de certa forma não deixa de ser da natureza do esporte em si. 


## License

The theme is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
