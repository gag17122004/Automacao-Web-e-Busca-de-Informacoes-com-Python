# Automacao-Web-e-Busca-de-Informacoes-com-Python

cambio de moedas mundiais.

Automação Web e Busca de Informações com Python
Desafio:
Trabalhamos em uma importadora e o preço dos nossos produtos é vinculado a cotação de:

Dólar
Euro
Ouro
Precisamos pegar na internet, de forma automática, a cotação desses 3 itens e saber quanto devemos cobrar pelos nossos produtos, considerando uma margem de contribuição que temos na nossa base de dados.

Base de Dados: https://drive.google.com/drive/folders/1KmAdo593nD8J9QBaZxPOG1yxHZua4Rtv?usp=sharing

Para isso, vamos criar uma automação web:

Usaremos o selenium
Importante: baixar o webdriver
Agora vamos atualiza a nossa base de preços com as novas cotações
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
​
navegador = webdriver.Chrome()
​
navegador.get("https://www.google.com/")
​
navegador.find_element(
    'xpath', 
    '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys("cotação dólar")
​
navegador.find_element(
    'xpath', 
    '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys(Keys.ENTER)
​
cotacao_dolar = navegador.find_element(
    'xpath', 
    '//*[@id="knowledge-currency__updatable-data-column"]/div[1]/div[2]/span[1]').get_attribute('data-value')
​
print(cotacao_dolar)
​
navegador.get("https://www.google.com/")
​
navegador.find_element(
    'xpath', 
    '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys("cotação euro")
​
navegador.find_element(
    'xpath', 
    '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys(Keys.ENTER)
​
cotacao_euro = navegador.find_element(
    'xpath', 
    '//*[@id="knowledge-currency__updatable-data-column"]/div[1]/div[2]/span[1]').get_attribute('data-value')
​
print(cotacao_euro)
​
navegador.get("https://www.melhorcambio.com/ouro-hoje")
​
cotacao_ouro = navegador.find_element(
    'xpath', 
    '//*[@id="comercial"]').get_attribute('value')
​
cotacao_ouro = cotacao_ouro.replace(',', '.')
​
print(cotacao_ouro)
​
navegador.quit()
​
5.1007
5.533519898000001
311.26
Importando a base de dados
import pandas as pd
​
tabela = pd.read_excel('produtos.xlsx')
display(tabela)
Produtos	Preço Original	Moeda	Cotação	Preço de Compra	Margem	Preço de Venda
0	Câmera Canon	999.99	Dólar	5	4999.95	1.40	6999.930
1	Carro Renault	4500.00	Euro	6	27000.00	2.00	54000.000
2	Notebook Dell	899.99	Dólar	5	4499.95	1.70	7649.915
3	IPhone	799.00	Dólar	5	3995.00	1.70	6791.500
4	Carro Fiat	3000.00	Euro	6	18000.00	1.90	34200.000
5	Celular Xiaomi	480.48	Dólar	5	2402.40	2.00	4804.800
6	Joia 20g	20.00	Ouro	350	7000.00	1.15	8050.000
Atualizando os preços e o cálculo do Preço Final
tabela[Moeda] == 'Dólar'
​
tabela.loc[tabela["Moeda"] == "Dólar", "Cotação"] = float(cotacao_dolar)
​
display(tabela)
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
~\AppData\Local\Temp\ipykernel_7068\4122198259.py in <module>
----> 1 tabela[Moeda] == 'Dólar'
      2 
      3 tabela.loc[tabela["Moeda"] == "Dólar", "Cotação"] = float(cotacao_dolar)
      4 
      5 display(tabela)

NameError: name 'Moeda' is not defined

Agora vamos exportar a nova base de preços atualizada
​
!pip install selenium
​
Requirement already satisfied: selenium in c:\users\fabio\anaconda3\lib\site-packages (4.7.2)
Requirement already satisfied: urllib3[socks]~=1.26 in c:\users\fabio\anaconda3\lib\site-packages (from selenium) (1.26.11)
Requirement already satisfied: trio-websocket~=0.9 in c:\users\fabio\anaconda3\lib\site-packages (from selenium) (0.9.2)
Requirement already satisfied: certifi>=2021.10.8 in c:\users\fabio\anaconda3\lib\site-packages (from selenium) (2022.9.14)
Requirement already satisfied: trio~=0.17 in c:\users\fabio\anaconda3\lib\site-packages (from selenium) (0.22.0)
Requirement already satisfied: sortedcontainers in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (2.4.0)
Requirement already satisfied: sniffio in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.2.0)
Requirement already satisfied: cffi>=1.14 in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.15.1)
Requirement already satisfied: attrs>=19.2.0 in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (21.4.0)
Requirement already satisfied: outcome in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.2.0)
Requirement already satisfied: exceptiongroup>=1.0.0rc9 in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.1.0)
Requirement already satisfied: idna in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (3.3)
Requirement already satisfied: async-generator>=1.9 in c:\users\fabio\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.10)
Requirement already satisfied: wsproto>=0.14 in c:\users\fabio\anaconda3\lib\site-packages (from trio-websocket~=0.9->selenium) (1.2.0)
Requirement already satisfied: PySocks!=1.5.7,<2.0,>=1.5.6 in c:\users\fabio\anaconda3\lib\site-packages (from urllib3[socks]~=1.26->selenium) (1.7.1)
Requirement already satisfied: pycparser in c:\users\fabio\anaconda3\lib\site-packages (from cffi>=1.14->trio~=0.17->selenium) (2.21)
Requirement already satisfied: h11<1,>=0.9.0 in c:\users\fabio\anaconda3\lib\site-packages (from wsproto>=0.14->trio-websocket~=0.9->selenium) (0.14.0)
​
