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
   * [Modelo](#modelo)
   * [Resultados](#resultados)
   * [Tecnologias](#tecnologias)
<!--te-->

# Sobre
Este repositório contém os arquivos para execução do projeto desenvolvido em Trabalho de Diplomação juntamente com o artigo em formato .pdf

# Tabela de Conteudo
Encontra-se nesse repositório 3 arquivos .csv são eles:
informacoes_carro.csv
posto.csv
abastecimentossimexc.csv

Encontra-se também 1 arquivo .ipynb 
TDFinal.ipynb

Também encontra-se neste repositório 1 arquivo .pdf:
artigoFinal.pdf

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

```python
def converterGrausParaRad(numero):
    rad = (numero/180)*math.pi
    return rad  
```

```python
def calcularDistancia (lat_ref,lon_ref, lat_posto, lon_posto):
  distancia   = raioDaTerra * math.acos( 
      math.cos( converterGrausParaRad(lat_ref) )
    * math.cos( converterGrausParaRad(lat_posto) ) 
    * math.cos( converterGrausParaRad(lon_ref ) - converterGrausParaRad(lon_posto) )
    + math.sin( converterGrausParaRad(lat_ref) ) 
    * math.sin( converterGrausParaRad(lat_posto) )
  )
  return distancia 
```
As duas funções acima fazer parte da fórmula de Haversine para cálculo de distancias.

```python
def menor(listaDePostos):
  # print("LISTA DE POSTOS",listaDePostos)
  auxiliar    = listaDePostos[:]
  auxiliar.sort()               #Ordena de forma crescente
  menorValor  = auxiliar[0]
  #return [(n, listaDePostos[n]) for n, x in enumerate(listaDePostos) if x==menorValor]
  indiceValor = listaDePostos.index(menorValor)
  idByIndice  = indiceValor + 1
  #print("LISTA AUXILIAR", auxiliar)
  return (idByIndice)
```
Função me retorna o posto mais próximo baseado na lista de postos que recebe como parametro.

```python
def calcularposto(latitude_ref,longitude_ref):
  distancia1  = calcularDistancia(latitude_ref, longitude_ref, -21.79143, -46.56805)    #posto 1 da planiha PostoEletricaNovacar
  distancia2  = calcularDistancia(latitude_ref, longitude_ref, -21.78646, -46.57236)    #posto 2 da planiha PostoFepasa
  distancia3  = calcularDistancia(latitude_ref, longitude_ref, -21.79900, -46.59849)    #posto 3 da planiha PostoPUCMinasPocosdeCaldas
  distancia4  = calcularDistancia(latitude_ref, longitude_ref, -21.83674, -46.55918)    #posto 4 da planiha PostoIFPocosdeCaldas
  distancia5  = calcularDistancia(latitude_ref, longitude_ref, -21.7908093,-46.5498371) #posto 5 da planilha Posto top
  distancia6  = calcularDistancia(latitude_ref, longitude_ref, -21.795591,-46.5513821)  #posto 6 da planilha Ponto Uau
  distancia7  = calcularDistancia(latitude_ref, longitude_ref, -21.8102542,-46.5441723) #posto 7 da planilha Posto Parque
  distancia8  = calcularDistancia(latitude_ref, longitude_ref, -21.8069073,-46.5796204) #posto 8 da planilha Posto Morumbi
  distancia9  = calcularDistancia(latitude_ref, longitude_ref, -21.8018071,-46.5715523) #posto 9 da planilha Posto Sta Angela
  distancia10 = calcularDistancia(latitude_ref, longitude_ref, -21.7983916,-46.5357675) #posto 10 da planilha Posto Dom Bosco	            
  distancia11 = calcularDistancia(latitude_ref, longitude_ref, -21.7758277,-46.6034023) #posto 11 da planilha Posto Shop                
  distancia12 = calcularDistancia(latitude_ref, longitude_ref, -21.7553693,-46.6076978) #posto 12 da planilha Posto Vale das Antas
  distancia13 = calcularDistancia(latitude_ref, longitude_ref, -21.801942,-46.5206)     #posto 13 da planilha Posto Estancia        
  distancia14 = calcularDistancia(latitude_ref, longitude_ref, -21.8067234,-46.5074679) #posto 14 da planilha Posto Pinheiros
  distancia15 = calcularDistancia(latitude_ref, longitude_ref, -21.8236759,-46.5299395) #posto 15 da planilha Posto Contorno
  distancia16 = calcularDistancia(latitude_ref, longitude_ref, -21.8379415,-46.5752329) #posto 16 da planilha Posto Kenedy	              
  distancia17 = calcularDistancia(latitude_ref, longitude_ref, -21.7797745,-46.6313701) #posto 17 da planilha Posto Bortolan
  distancia18 = calcularDistancia(latitude_ref, longitude_ref, -21.7889947,-46.6405133) #posto 18 da planilha Posto Chacara
  distancia19 = calcularDistancia(latitude_ref, longitude_ref, -21.8408097,-46.6771472) #posto 19 da planilha Posto Marco	                
  distancia20 = calcularDistancia(latitude_ref, longitude_ref, -21.7657759,-46.5225668) #posto 20 da planilha Posto Primavera
  distancia21 = calcularDistancia(latitude_ref, longitude_ref, -21.7812984,-46.4945621) #posto 21 da planilha Posto PC Estancia
  distancia22 = calcularDistancia(latitude_ref, longitude_ref, -21.8299229,-46.4513139) #posto 22 da planilha Posto Laranjeiras
  distancia23 = calcularDistancia(latitude_ref, longitude_ref, -21.7986392,-46.4538438) #posto 23 da planilha Posto Morada            
  distancia24 = calcularDistancia(latitude_ref, longitude_ref, -21.8196195,-46.5958688) #posto 24 da planilha Posto Europa
  distancia25 = calcularDistancia(latitude_ref, longitude_ref, -21.8499213,-46.5711072) #posto 25 da planilha Posto Nacoes
  #return print(distancia1,distancia2,distancia3,distancia4 )
  return menor([distancia1,distancia2,distancia3,distancia4,distancia5,distancia6,distancia7,distancia8,distancia9,distancia10,distancia11,distancia12,distancia13,distancia14,distancia15,distancia16,distancia17,distancia18,distancia19,distancia20,distancia21,distancia22,distancia23,distancia24,distancia25]) # Passando lista de postos como argumento
```
Funcao recebe latitude e longitude como parametro e faz o uso da formula de Haversine declarada nas funções anteriores, dessa maneira tem se uma lista na saída com os calculos das distancias da localizacao passada ate cada posto.

```python
class MF(nn.Module):
    def __init__(self, num_users, num_items,emb_size=100000):
        super(MF, self).__init__()
        self.user_emb = nn.Embedding(emb_size ,num_users )
        self.item_emb = nn.Embedding(emb_size ,num_users )
        print(self.user_emb)
        print(self.item_emb)
        # initializing our matrices with a positive number generally will yield better results
        self.user_emb.weight.data.uniform_(0, 0.5)
        self.item_emb.weight.data.uniform_(0, 0.5)
        
    def forward(self, u, v):
        u = self.user_emb(u)
        v = self.item_emb(v)
        return (u*v).sum(1)  # taking the dot product
```
Essa funcao é onde se constrói a Matriz de Fatoração, utilizada na filtragem colaborativa

```python
def train_epocs(model, epochs=10, lr=0.01, wd=0.0):
    optimizer = torch.optim.Adam(model.parameters(), lr=lr, weight_decay=wd)
    model.train()
    for i in range(epochs):

        usernames = torch.LongTensor(train_df.veiculos_eletricos_id.values)
        id_station = torch.LongTensor(train_df.idposto.values)
        ratings = torch.FloatTensor(train_df.avaliacao.values)
        y_hat = model(usernames, id_station)
        loss = F.mse_loss(y_hat, ratings)
        optimizer.zero_grad()  # reset gradient
        loss.backward()
        optimizer.step()
        print(loss.item())
    test(model)
```
Função de treinamento do modelo. Em cada iteração, a função de treinamento está atualizando nosso modelo para abordar um MSE menor (erro quadrático médio). Esta é a ideia da descida gradiente.

```python
def test(model):
    model.eval()
    user = torch.LongTensor(test_df.veiculos_eletricos_id.values)
    id_station = torch.LongTensor(test_df.idposto.values)
    ratings = torch.FloatTensor(test_df.avaliacao.values)
    y_hat = model(user, id_station)
    loss = F.mse_loss(y_hat, ratings)
    print("test loss %.3f " % loss.item()) 
```
E por fim a funcao de teste que é utilizada dentro da funcao de treinamento como pode observar a ultima linha, é ela quem testa e nos dá o percentual de perda.

# Modelo
Para o nosso Sistema de Recomendador, baseado em geolocalização a primeira etapa definida é o pré-processamento das informações, que chega através de um arquivo de formato csv, onde dentro deste arquivo está as informações de todos os veículos, nele o sistema faz três pré-processamentos.
Pré-processamento de abastecimento – Que irá verificar se o veículo fez o abastecimento, caso fez é adicionado \textit{'True’} na coluna ‘Abastecido’.

Pré-processamento de localidade do abastecimento – toda coluna ‘abastecido’ \textit{‘True’} através deste pré-processamento irá verificar onde foi feito o abastecimento e adicionará na coluna ‘idposto’ a ’ID’ do posto correspondente, utilizando a fórmula de Haversine e os campos de latitude e longitude para conseguir realizar os cálculos.

Pré-processamento de Afinidade – É verificado veículo por veículo e calculado baseado na quantidade de vezes que abasteceu em determinado posto.

Após todos os pré-processamentos dos dados o arquivo está pronto para ser feito as recomendações, que no final teremos três tipos de recomendações:

A primeira é baseada na afinidade do motorista com determinado posto, ou seja, aquele posto que ele mais abasteceu.

A segunda recomendação foi desenvolvida em Deep Learning e utilizando a filtragem colaborativa para a recomendação, baseado na similaridade de usuários com as avaliações dadas a cada posto.

A terceira é baseada na fórmula de Haversine onde se calcula a distância entre pontos no globo Terrestre, essa fórmula me traz o posto mais próximo do motorista, essa recomendação também resolve um dos problemas da filtragem colaborativa que é o \textit{Cold Start}.Através dela é possível recomendar algum posto para um usuário novo e também através dela é possível recomendar um posto que ainda não possua recomendação.

A recomendação utilizando a filtragem colaborativa, por ela pertencer a uma rede neural, essa rede precisa de um pré-processamento próprio onde, é feito a matriz de fatoração e no nosso caso as linhas dentro dessa matriz são os usuários e as colunas são os postos, dentro da célula é o valor da avaliação.
Após essa montagem da matriz a rede neural também atribui pesos a essas notas para conseguir encontrar a similaridade, após isso a rede faz o treinamento e teste, então o modelo fica pronto, somente após o modelo estar pronto que é possível recomendar os postos para determinado usuário.

![alt text](https://github.com/Gabrielgsn30/SistemaRecomendador/blob/main/img/fig4.png)

Todas as etapas da figura:


	1.Leitura dos arquivos.csv
	
	2.Pré-processamento de abastecimento
	
	3.Pré-processamento da localização do abastecimento
	
	4.Pré-processamento da afinidade do motorista
	
	5.Entrando na Rede Neural
		5.1.Pré-processamento da Rede Neural
		
		5.2.Transformação dos dados
		
		5.3.Geração do Modelo
		5.4.Modelo pronto(treinado e testado)
	
	6.Requisião do usuário
	
	7.Recomendação baseada em afinidade
	
	8.Recomendação baseada na localização Atual
	
	9.Recomendação baseada na filtragem colaborativa(Rede Neural)
  
# Resultados

# Tecnologias




