# webAnalytics

# Aabrir o navegador
Automatizando processo web utilizando Pandas e Selinum
from selenium import webdriver
navegador = webdriver.Chrome()
navegador.get("https://google.com.br")

# Importar a base de dados
import pandas as pd
tabela = pd.read_excel("commodities.xlsx")
display(tabela)

# Fazer a leitura da página
for linha in tabela.index:
    produto = tabela.loc[linha, "Produto"]
    print(produto)
    produto = produto.replace("ó", "o").replace("ã","a").replace("ç","c").replace("ú","u").replace("é","e").replace("á","a") # Formata o link para tirar caracters
    
    link = f"https://www.melhorcambio.com/{produto}-hoje"
    navegador.get(link)

    preco = navegador.find_element('xpath', '//*[@id="comercial"]').get_attribute('value') # Busca pela posição do elemento a ser alterado e atribui o que será altera. 
    preco = preco.replace(".", "",).replace(",", ".")
    print(preco)
    tabela.loc[linha, "Preço Atual"] = float(preco)

# .click() -> clicar
# .send_keys("texto") -> escrever
# .get_attribute() -> pegar um valor

# Preencher a coluna de comprar
tabela["Comprar"] = tabela["Preço Atual"] < tabela["Preço Ideal"]
display(tabela)

# Exportar a base para o Excel
tabela.to_excel("commodities_atualizado.xlsx", index = False)
navegador.quit()
