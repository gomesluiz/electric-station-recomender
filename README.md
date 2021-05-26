# Sistema Recomendador
Uma Abordagem Aplicável de Deep Learning para Sistemas Recomendadores baseado em Geolocalização

Com a crescente disseminação de informações e dados, muito em decorrer do avanço tecnológico que estamos passando, surge à necessidade de sugerir conteúdos para seus usuários, levando isso para um contexto de geolocalização, os motoristas, por exemplo, tem a necessidade de saber onde se encontra e onde se localiza postos de abastecimentos próximos a sua geolocalização. 
Neste contexto surge o problema de motoristas terem um sistema recomendador, baseado na sua localização e outras preferências. Na cidade de Poços de Caldas, está inserido o projeto Poços + Inteligente, onde, um dos seus pontos, será a implantação de postos de abastecimentos, para carros elétricos, e assim surgiu a necessidade da recomendação para motoristas de carros elétricos, que por meio de um aplicativo haverá um Sistema Recomendador baseado em determinadas regras e preferências para se fazer a recomendação.

<!--ts-->
   * [Sistema Recomendador](#SistemaRecomendador)
   * [Sobre](#Sobre)
   * [Tabela de Conteudo](#tabela-de-conteudo)
   * [Instalação](#Instalacao)
   * [Como usar](#como-usar)
   * [Tabela de variaveis](#tabela-de-variaves)
   * [Tabela de Funcoes](#tabela-de-funcoes)
   * [Resultados](#resultados)
   * [Tecnologias](#tecnologias)
<!--te-->

# Sobre
Este repositório contém os arquivos para execução do projeto desenvolvido em Trabalho de Diplomação juntamente com o artigo em formato .pdf

# Tabela de Conteúdo
Encontra-se nesse repositório 3 arquivos .csv são eles:
-informacoes_carro.csv
-posto.csv
-abastecimentossimexc.csv
Encontra-se também 1 arquivo .ipynb 
-TDFinal.ipynb

Também encontra-se neste repositório 1 arquivo .pdf:
-artigoFinal.pdf

# Instalação
Não é necessário nenhuma instalação na maquina, basta apenas criar um notebook no Google Colab.

# Como usar
Para a execução do projeto como descrito acima, após a criação do notebook no Google Colab, deverá fazer a importação do arquivo TDFinal.ipynb, após isso o notebook ja irá trazer em tela todo o código.
Antes da execução porém deverá importar os arquivos csv também:
informacoes_carro.csv,
posto.csv,
abastecimentossimexc.csv

# Tabela de variaveis
Variaveis    | Descricao
-------------------------- | ------------------------------------------------------------------:
raioDaTerra                | Utilizada para a funcao que realiza calculos de distancia
carros                     | Utilizado para fazer a leitura do informacoes_carros.csv original
carrosfiltrados            | Recebe a variavel carros porem descartando colunas sem informações
dfcarros                   | Um dataframe da variavel carrosfiltrados
dfcarrosposots             | Um dataframe da variavel dfcarros apos ter sido realizado o 1º pré-processamento
arq_csv                    | Utilizado para fazer a leitura do abastecimentossimexc.csv arquivo simulado ja pré-processado, sem a necessidade que o arquivo passa através do pré-processamento igual ao original(IMPORTANTE destacar que caso venha bastante dados do arquivo original não é necessario utilizar dados simulados neste caso e somente nesse foi necessario para a simulacao e apresentacao deste modelo)
ids_carros                 | variavel que recebe id dos veiculos da variavel arq_csv
carro                      | Um dataframe da variavel carrosfiltrados
filtro                     | Variaveis utilizadas para o pré-processamento de afinidade 
posicoes                   | Variaveis utilizadas para o pré-processamento de afinidade 
tabela_filtrada            | Variaveis utilizadas para o pré-processamento de afinidade 
tamanho_total              | Variaveis utilizadas para o pré-processamento de afinidade 
ocorrencias                | Variaveis utilizadas para o pré-processamento de afinidade 
afinidade                  | Variaveis utilizadas para o pré-processamento de afinidade 
users_ratings              | Utilizado para receber uma copia da variavel arq_csv
num_users                  | Utilizada para encontrar ids exclusivos dos veiculos(usuarios no sistema)
num_station                | Utilizada para encontrar ids exclusivos dos postos de abastecimentos
model                      | Variavel que recebe uma funcao declarada MF passando as variaveis citadas acima como parametro
train_df                   | Variavel utilizada para treinamento do modelo
teste_df                   | Variavel utilizada para teste do modelo
user                       | Variavel que recebe o valor do tensor dos usuarios
station                    | Variavel que recebe o valor do tensor dos postos 
predictions                | Variavel que recebe a predicao do modelo em lista
normalized_predictions     | Variavel que faz a suavização dos dados para nós podermos manipularmos os dados
x                          | Valor atual de um ID de motorista
latitude_ref               | Valor atual da latitude do motorista
longitude_ref              | Valor atual da longitude do motorista
existe                     | Verifica se existe historico/dados desse motorista
user_id                    | Recebe a id do motorista
user                       | Recebe a id do motorista dentro do tensor passando por parametro o user_id
station                    | Recebe uma lista de ids exlcusivas de postos do tensor tambem
predictions                | Realiza a predicao passando o user e statin como parametro
sort_by_indices            | Ordena
recomendations             | Recebe a recomendacao, nesta variavale é opnde define quantas recomendacoes quer receber hoje estão 10
prox                       | Variavel que calcula o posto mais proximo da localizacao atual
dadosexcl                  | Utilizado para filtrar dados exclusivos daquele motorista
posto                      | Recebe o posto com maior afinidade daquele motorista baseado nos dados exxclusivos da variavel alterior
prox                       | Variavel que calcula o posto mais proximo da localizacao

# Tabela de Funcoes
Funcoes  | Descricao
-------- | ----------:
def converterGrausParaRad(numero):
    rad = (numero/180)*math.pi
    return rad  
    
# Resultados

# Tecnologias




