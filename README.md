# PUC-RIO-DTSC-MVP1
![image](https://github.com/user-attachments/assets/e7b32e82-8451-4d0f-82ef-302993d4c875)

## Objetivo
A transição energética para fontes mais limpas é um dos desafios globais mais importantes da atualidade. O Acordo de Paris, assinado em 2015, estabeleceu metas para redução de emissões de gases de efeito estufa, promovendo uma maior adoção de energias renováveis e uma redução do consumo de combustíveis fósseis.

Este trabalho busca entender se os países estão cumprindo suas metas, quais estão se destacando na transição energética e quais enfrentam dificuldades. Além disso, será avaliado o impacto da saída ou da mudança de postura de alguns países em relação ao acordo, na política energética global.

Questionamentos:
1.	Quais países possuem o maior consumo de energia total?
2.	Quais países são mais dependentes de fontes não renováveis?
3.	Quais países possuem maior participação de fontes renováveis?
4.	Como a matriz energética de cada país evoluiu ao longo do tempo?
5.	Há padrões regionais no consumo de energia (exemplo: Europa vs América do Sul)?
6.	Quais países estão reduzindo o consumo de fontes fósseis ao longo do tempo?
7.	Os países signatários do Acordo de Paris estão reduzindo suas emissões de carbono?
8.	Quais países estão mais alinhados com as metas do Acordo de Paris?
9.	Houve impacto significativo no consumo energético de países que saíram do Acordo de Paris?
10.	A saída ou redução de compromisso de alguns países afetou o progresso global na redução de emissões?


## Coleta
### Fonte de dados

Origem dos Dados:
World Energy Statistics (2000-2024) - Dataset “Global Data on Sustainable Energy” obtido via Kaggle.

Descrição:
Esse dataset reúne informações globais sobre energia sustentável, englobando dados como indicadores de produção, consumo, e outras métricas relevantes para a análise do setor de energia em diversos países coletados de relatórios de energia governamentais, artigos de pesquisas ambientais e pesquisas de produção de energia. Última atualização em março de 2025

![seleção](https://github.com/user-attachments/assets/a7a6b0b1-0b9b-44b0-b1b2-3ff22b3be8ae)
Licença: CC0: Public Domain


### Extração dos dados
Não foi possível fazer a extração dos dados diretamente para o ambiente do Databricks Community, sendo necessário realizar o download para a máquina e posterior upload no cluster.  A extração foi realizada via API do Kaggle integrada com Python. Foi configurado o arquivo de autenticação `kaggle.json` com as credenciais de acesso à conta do Kaggle.

Um script em Python executou o comando:

![extração](https://github.com/user-attachments/assets/dfdf5aa1-5398-4a2c-a07a-26599d3251b7)

### Tratamentos iniciais
Foram realizados tratamentos de normalização de chave da base dados e inclusão de dimesões (continente, países signatários do Acordo de Paris) com base em informações obtidas no site:

## Carga  
Inicialmente foi necessário criar o cluster no qual os dados seriam armazenados, seguindo a configuração padrão do Databrick para clusters.

![criação CLUSTER](https://github.com/user-attachments/assets/71ef5276-17f4-48e3-a771-47c5cb961fd6)


Em seguida foi realizado o full load dos dados brutos e a partir do notebook criado no momento da carga foram separadas e criadas as 3 tabelas como relatado no item **Tratamentos iniciais - Transformação**.

![criação tabela](https://github.com/user-attachments/assets/732e44f9-e6f2-4926-a265-504a8504c5aa)
![ajustes de tipos](https://github.com/user-attachments/assets/58ac2cb9-ea4b-4d3a-8e70-54d98f4018f0)

Persistência na Nuvem: (Databricks Community): Como o Databricks Community desativa o cluster, o dado, nesse caso NÃO é persistente, precisando ser recarregado a cada nova entrada na plataforma


### Tratamentos iniciais - Transformação
Foram realizados tratamentos de normalização de chave da base dados, já iniciando o processo de modelagem, e inclusão de dimesões (continente, países signatários do Acordo de Paris) com base em informações obtidas no site:
https://brasilescola.uol.com.br/geografia/acordo-paris.htm#:~:text=Resumo%20sobre%20o%20Acordo%20de%20Paris,-Acordo%20de%20Paris&text=%C3%89%20ratificado%20por%20194%20partes,aumento%20de%201%2C5%20%C2%BAC.


![Tranformacoes iniciais](https://github.com/user-attachments/assets/1c963369-1ccc-45e4-a596-eb9c7d425738)

O processo completo pode ser visto no Notebook **Transformações do dado bruto**



## Modelagem 

Foi adotada uma estrutura de modelo de dados em **Esquema Snowflake**, típica de ambientes de Data Warehouse, com o objetivo de organizar e facilitar a análise de indicadores energéticos, ambientais e econômicos por país e ano.

A modelagem segue os princípios de normalização e separação de dimensões, garantindo reuso, clareza e escalabilidade mesmo que, para esse caso, não haja nova acoplagem de dados mais recentes. 

**Componentes principais**

Tabela Fato - `dados`: armazena os indicadores quantitativos por país e ano (ex: acesso à eletricidade, emissões de CO₂, crescimento do PIB, etc.)

Chave de ligação: `id_pais`

Tabelas de Dimensão 
`paises`: contém atributos geográficos e demográficos de cada país (nome, localização, área, densidade, etc.), ligada à tabela fato via id_pais
`continentes`: representa os continentes associados aos países, normalizada como uma dimensão da dimensão `paises`, ligada via `id_continente`

<img src="img/diagrama_ER.png" alt="Diagrama Relacional" width="500">

<img src="img/diagrama_conceitual.png" alt="Diagrama Conceitual" width="500">


[📄 Catálogo de Dados (PDF)](img/Catálogo%20de%20dados.pdf)

### Metadados
Após a carga dos dados na plataforma Databricks e as tranformações, foi realizada inserção dos metadados utilizando SQL via notebook.


## Análise 

### Qualidade dos dados

![qualidade_dados](https://github.com/user-attachments/assets/540cf834-7b55-4986-81ee-c67828e511a1)
![qualidade_paises](https://github.com/user-attachments/assets/4d86a33d-bb01-46dd-a24b-390510c95364)

### Solução do problema
A primeira que questão proposta `Quais países possuem o maior consumo de energia total?` não estipulava um período de análise. Logo, determinei o recorte de últimos 5 anos (2015-2020). Realizei as queries com 2 focos: o consumo total do país e o consumo dos países per capita para  entender se haveria correspondência.

| país           | Consumo total de eletricidade em TWh | Consumo médio per capita |
|----------------|----------------------------|--------|
| China          | 40754.39                  |26813.78|
| United States  | 24626.97                  |78254.74|
| India          | 8958.35                   |6367|
| Japan          | 5987.22                   |40861.68|
| Canada         | 3816.42                   |108686.58|


Abaixo os países com maior consumo per capita

|país	    |Consumo médio per capita |
|-------    |-------------------|
|Qatar	    |211679.29|
|Iceland	|179074.73|
|Singapore	|159826.6|
|Bahrain	|148473.13|
|United Arab Emirates|	134892.35|

`Quais países são mais dependentes de fontes não renováveis?`

Os dados revelam que 11 países dependem 100% de fontes não renováveis (combustíveis fosseis e nuclear)

1. Gambia
2. Guinea-Bissau
3. Grenada
4. Djibouti
5. Kiribati
6. Nauru
7. Turkmenistan
8. Saint Lucia
9. Solomon Islands
10. Bahamas
11. Comoros

## Autoavaliação 
A seleção e análise dos dados foi algo relativamento simples. A configuração do ambiente de nuvem no Databricks foi algo bem penoso com muito retrabalho, o cluster criado no Databricks desapareceu algumas vezes após a criação. Não consegui entender a razão desse desaparecimento, mas como é possível clonar o cluster desativado, fui realizando esse processo a cada etapa. A princípio também optei por realizar realizar as transformações de forma externa por ter mais familiaridade com o python. Em dado momento resolvi encarar o desafio de executar as transformações dentro do notbook Databricks e após perceber que a execução dos códigos é levemente diferente utilizei uma ferramenta de IA para ajudar nessa adaptação.
