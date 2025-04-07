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
Fonte: Global Data on Sustainable Energy

Origem: Kaggle

URL: https://www.kaggle.com/datasets/anshtanwar/global-data-on-sustainable-energy

Formato dos Dados: CSV

Descrição:
Esse dataset reúne informações globais sobre energia sustentável, englobando dados como indicadores de produção, consumo, e outras métricas relevantes para a análise do setor de energia em diversos países.

![seleção](https://github.com/user-attachments/assets/a7a6b0b1-0b9b-44b0-b1b2-3ff22b3be8ae)


### Extração dos dados
A extração foi realizada via API do Kaggle integrada com Python. Foi configurado o arquivo kaggle.json com as credenciais de acesso à conta do Kaggle para permitir o uso da API.

Um script em Python executou o comando:

![extração](https://github.com/user-attachments/assets/dfdf5aa1-5398-4a2c-a07a-26599d3251b7)

### Tratamentos iniciais
Foram realizados tratamentos de normalização de chave da base dados e inclusão de dimesões (continente, países signatários do Acordo de Paris) com base eminformações obtidas no site:

https://brasilescola.uol.com.br/geografia/acordo-paris.htm#:~:text=Resumo%20sobre%20o%20Acordo%20de%20Paris,-Acordo%20de%20Paris&text=%C3%89%20ratificado%20por%20194%20partes,aumento%20de%201%2C5%20%C2%BAC.

## Modelagem 
Será avaliada a qualidade da modelagem dos dados (1,0 pt) e documentação do Catálogo de Dados (1,0 pt).

Definição da Estrutura: Criação de um modelo que organiza como os dados serão armazenados, estruturados e relacionados. Isso pode incluir modelos conceituais, lógicos e físicos.

Boas Práticas: Aplicação de técnicas como normalização ou, quando necessário, desnormalização para otimizar a performance e garantir a integridade dos dados.

Diagramas e Relacionamentos: Uso de diagramas (como ER, por exemplo) para visualizar e documentar entidades, atributos e os relacionamentos entre eles.

![diagrama ER](https://github.com/user-attachments/assets/479f6b7d-fac2-4fba-8c76-01af248c99da)


Aderência aos Requisitos: Garantia de que o modelo atende às necessidades do negócio e facilita análises futuras.

Documentação do Catálogo de Dados:

Repositório Central: Criação de um documento ou sistema (como um catálogo de dados) que liste e descreva todos os conjuntos de dados disponíveis.

### Metadados
Após a carga dos dados na plataforma Databricks, foi realizada inserção dos metadados utilizando SQL via notebook.

![paises_metadados_SQL](https://github.com/user-attachments/assets/efa6c3bc-edd9-41ff-9f27-7eb495eafb67)

![metadados_continente](https://github.com/user-attachments/assets/bd27e292-6486-4a96-96af-a8d69918e825)

![metadados_dados](https://github.com/user-attachments/assets/14c9c9dc-51d9-4d34-af1c-39db12c8312a)

![metadados_paises](https://github.com/user-attachments/assets/3cf544c2-ad09-460c-aa40-92aa89149566)


## Carga  
Inicialmente foi necessário criar o cluster no qual os dados seriam armazenados, seguindo a configuração padrão do Databrick para clusters.

![criação CLUSTER](https://github.com/user-attachments/assets/71ef5276-17f4-48e3-a771-47c5cb961fd6)


Em seguida foram criadas as 3 tabelas através do **Create Table with UI** e em seguida relaizados ajustes nos DataTypes e CollumnNames.

![criação tabela](https://github.com/user-attachments/assets/732e44f9-e6f2-4926-a265-504a8504c5aa)
![ajustes de tipos](https://github.com/user-attachments/assets/58ac2cb9-ea4b-4d3a-8e70-54d98f4018f0)

Processo de Ingestão:

Pipeline de arga: Descrição do fluxo que leva os dados coletados e transformados até a sua persistência na nuvem.

Tipo de Carga: Especificar se é full load ou incremental, e as estratégias de atualização (merge, upsert, etc.).

Persistência na Nuvem (Databricks):

Ambiente: Configuração do cluster Databricks e versão do Spark utilizada.

Destino: Armazenamento dos dados em tabelas Delta Lake ou outros formatos otimizados para consultas e análises.

Organização: Estruturação dos dados (ex: particionamento por data ou categoria) e controle de versionamento.

Validação: Procedimentos de verificação da integridade dos dados após a carga, com logs e monitoramento para identificar possíveis falhas.

## Análise 
Serão avaliados a análise de qualidade dos dados (1,0 pt) e da solução do problema de forma correta (0 pt) e bem analisada pela discussão a partir das respostas obtidas (1,0 pt).

## Autoavaliação 
Será avaliada a autoavaliação do aluno com as questões pertinentes sobre o atingimento de seus objetivos traçados no início do trabalho.
