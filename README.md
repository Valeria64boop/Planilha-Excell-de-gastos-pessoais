# Planilha-Excell-de-gastos-pessoais
Mediante a planilha o chatgpt analisou e criou os códigos

import matplotlib.pyplot as plt

# Agrupar por mês e tipo, somando os valores
mensal = df.groupby(['mês', 'tipo'])['valor'].sum().unstack().fillna(0)

# Plotar o gráfico
mensal.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Entradas e Saídas Mensais')
plt.xlabel('Mês')
plt.ylabel('Valor (R$)')
plt.legend(title='Tipo')
plt.grid(True)
plt.tight_layout()
plt.show()
# Filtrar apenas as saídas
despesas = df[df['tipo'] == 'SAÍDA']

# Agrupar por categoria, somando os valores
categoria_despesas = despesas.groupby('categoria')['valor'].sum().sort_values(ascending=False)

# Plotar o gráfico de pizza
categoria_despesas.plot(kind='pie', autopct='%1.1f%%', figsize=(8, 8))
plt.title('Distribuição de Despesas por Categoria')
plt.ylabel('')
plt.show()
# Ordenar o DataFrame por data
df = df.sort_values(by='data')

# Calcular o saldo diário
df['saldo'] = df.apply(lambda row: row['valor'] if row['tipo'] == 'ENTRADA' else -row['valor'], axis=1)

# Calcular o saldo acumulado
df['saldo_acumulado'] = df['saldo'].cumsum()

# Plotar o gráfico
plt.figure(figsize=(12, 6))
plt.plot(df['data'], df['saldo_acumulado'], marker='o')
plt.title('Evolução do Saldo Acumulado')
plt.xlabel('Data')
plt.ylabel('Saldo Acumulado (R$)')
plt.grid(True)
plt.tight_layout()
plt.show()

# Agrupar por operação bancária, contando as ocorrências e somando os valores
operacoes = df.groupby('operação bancaria').agg({'valor': ['count', 'sum']})

# Renomear as colunas para melhor compreensão
operacoes.columns = ['Quantidade', 'Valor Total']

# Exibir o DataFrame resultante
print(operacoes)
