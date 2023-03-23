# Analise de dados
# Um projeto de analise de dados em python com objeto de receber um banco de dados, tratar valores errados ou faltantes e mostrar varios tipos de tabela com os valores do banco de dados


# CODE




#Importação de bibliotecas 
import pandas as pd 
import seaborn as sns 
import matplotlib.pyplot as plt
import plotly.express as px 

#Importar base de dados
base_credit= pd.read_csv('Repositorio\credit_data.csv')


#income = renda anual
#age = idade
#loan = divida existente 
#default = 0 pessoa pagou o emprestimo / default = 1 pessoa não pagou o emprestimo

# TRATAMENTO DE VALORES FALTANTES

ver_valor_faltante = base_credit.isnull().sum()
# print(ver_valor_faltante)

# Como temos 3 valores faltantes em idade
# iremos pegar a media de idade da base de dados e colocar nos valores faltantes

media_idade = base_credit['age'][base_credit['age'] > 0].mean()

# print(base_credit.loc[pd.isnull(base_credit['age'])]) # Vemos todos os valores nulos de idade

base_credit['age'].fillna(base_credit['age'].mean(), inplace = True) # Preenchemos os valores NaN ou Nulos 

# print(base_credit.loc[pd.isnull(base_credit['age'])]) # verificamos se tem valores nulos (agora todos valores inexistentes estão usando a media de idade)


# Verificar valores a baixo de 0 
print(base_credit.describe())

#percebemos que o minimo de idade tem um valor de -52, ou seja um erro no banco de dados

#então iremos tratar esse valor inconscistente localizando todos valores

print(base_credit.loc[base_credit['age'] < 0, 'age']) 
#os IDs menor que 0 são o 15, 21 e o 26

base_credit.loc[base_credit['age'] < 0, 'age'] = media_idade

print(base_credit.head(27))

#Vemos que todos dados menor do que 0 agora estão com a media de idade que é de 40.92

# -------------------------------------------------------------------

# Agora que ja tratamos os dados faltantes, iremos usar 3 tipos de graficos para ver as 
# informações do banco de dados 

# GRAFICO DE PAGAMENTO
plt.title("Grafico de pagamento")
sns.countplot(x = base_credit['default'])
plt.show()
# Aqui conseguimos ver a quantidade de pessoas que pagaram e não pagaram o emprestimo
# sendo 0 = pagou o emprestimo e 1 = não pagou o emprestimo

# ----------------------------------------------------------

#Grafico de relação entre idade e pagamento

grafico = px.scatter_matrix(base_credit, dimensions=['age', 'income'], color = 'default')
grafico.show()

#Ele irá abrir uma aba no seu navegador com um grafico de relação entre idade e pagamento
# (ATENÇÃO: Se o grafico não abrir bastar clicar no X e recarregar a pagina, não feche a aba apenas o X de parar de carregar a pagina e carregar de novo, caso não abra direto)

# Com esse grafico ja vemos uma relação e um padrão claro entre a idade das pessoas e o pagamento da divida
# sendo que pessoas mais jovens tem menor costume de pagar o emprestimo

# ------------------------------------------------------------------------------------------------

#com esse grafico iremos conseguir ver a questão de idade e divida
# para analisar esse vemos que poucas tem divida acima de 8 mil por exemplo

x = base_credit['age']
y = base_credit['loan']
plt.scatter(x, y, color='purple')
plt.xlabel('Idade')
plt.ylabel('Empréstimo')
plt.show()
