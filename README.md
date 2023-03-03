# YOLO - You Only Look Once

##### Recebe este nome pois, o modelo não necessita de ficar recebendo milhoes ou milhares de inputs de imagem. O modelo funciona através de um conceito chamado de "single pass", ou seja ele recebe o input da imagem apenas uma única vez, tanto é que devido a está caracteristica de "visualizar a imagem" uma única vez é que recebe este nome.

<br>

## Funcionamento da YOLO

- O algortimo recebe a imagem de entrada e realiza um "fatiamento" (<i>slicing</i>) nesta imagem formando um <b>Grid</b>. Este <b>Grid</b> por sua vez pode assumir a dimensão de <b>S x S</b>.

<br>

- Normalmente nos projetos onde se utiliza YOLO os <b>Grids</b> tem um padrão de dimensão de <b>13 x 13</b>, entretanto isso depende da versão que se está utilizando da YOLO, pois a título de curiosidade o YOLO_v1 (Versão 1.0) seus <b>Grids</b> assumiam tamanhos de <b> 7 x 7</b>, enquanto já na YOLO_v4 têm-se grids de <b>19 x 19</b>.

<br>

- Cada célula deste grid ou seja cada quadradinho do grid é responsável de realizar a predição de até <b>5 Bouding Box</b>. Isso se deve ao fato de que dentro de um mesmo quadradinho do grid podemos ter 1 ou mais objetos nesta cena. Portanto, com <b>5 BBox</b>. O YOLO, tenta contornar o problema de ter mais de 1 objeto na mesma célula de grid.

<br>

- <b> As Bouding Box são basicamente caixas ou normalmente retângulos que são desenhados ao redor do objeto detectado e informam a posição do objeto na cena ou na imagem.)</b>

<br>

- Como dito anteriormente dentro de cada quadradinho ou célula do grid temos até 5 Bouding Box e sendo assim a YOLO retorna um valor de confiabilidade e quanto maior esta confiabilidade mais grossa são as bordas da Bouding Box, ou seja uma, pontuação de quanto de certeza que o algoritmo tem de que naquele quadradinho se tem pelo menos uma bouding box que contém um objeto nela, entretanto é importante salientar que podemos ter mais de um objeto.

<br>

- <b>Outro ponto importante é que esse valor ou pontuação não diz respeito ao objeto ou ao tipo de objeto que tem ali mas sim a Bouding Box propriamente dito.</b>

<br>

- Exemplo se tivermos um Carro azul ou um Carro amarelo ou uma Bicicleta , essa pontuação ela não irá alterar pelo simples fato de termos objetos diferentes. Pois como explicado esta pontuação diz respeito a Bounding Box e não aos objetos presentes na cena ou imagem.

<br>

- Depois que a YOLO ja gerou as Bbox,ou seja, já sabe onde está localizado cada objeto na cena. Para cada BBox, os quadradinhos ou células irão realizar a "previsão" dos objetos baseados nas classes em que o modelo foi treinado,fornecendo assim um valor de probabilidade para cada classe ou objeto presenta na imagem, ou seja, se eu tiver 100 objetos na cena o modelo irá gerar 100 valores de probabilidade. E sendo assim ele vai escolher o objeto que tem a maior probabilidade de ser um objeto que se quer classificar.
  <br>

- Este valor de confiança para cada Bbox juntamente com os valores de probilidade de cada objeto ou classe são combinados em um score final que irá informar a porcentagem de o objeto contido ali classificando se é um cachorro ou um carro por exemplo.

<br>

- Como visto a YOLO divide a imagem de input em grids de 13 x 13 o que totaliza 169 quadrinhos ou células e como visto em cada quadradinho temos até 5 Bbox, logo teremos na cena 845 Bbox na cena, entretanto a maior parte destas caixas terão um intervalo de confiança muito baixo, sendo assim a YOLO so considera como "relevante" as Bbox que tenham um Score final acima de 30%

<br>

- O YOLO por padrão foi <b> pré treinado</b> com um dataset ou conjunto de dados chamado <b><b?> COCO (Common Objects in Context) que possui 80 Classes diferentes</b>. Quando baixamos o COCO basicamente estamos buscando os pesos pré treinados para que seja possível executar o YOLO para estas classes pré treinadas e assim conseguiremos obter as deteccões dos objetos.

- <b>Consulte o site oficial do dataset COCO: <https://cocodataset.org/#home> </b>

<br>

# Detectando objetos com YOLO v4

<b> 1. Primeiro passo </b> : Download do Darknet

<br>

```
git clone https://github.com/AlexeyAB/Darknet
```

<br>

<b> 2. Segundo passo </b>: Após clonar o repositório do autor oficial da darknet devemos fazer o seguinte:

- Navegue até a pasta darknet que foi <b>clonada no Primeiro passo</b> com o seguinte comando:

<br>

```
cd darknet
```

<br>

- Agora dentro do diretório darknet rode o comando `ls` para visualizar as pastas e arquivos dentro do projeto
  feito isso compile o projeto com o comando

<br>

```
make
```

<br>

<b> 3. Terceiro passo </b>: Com a darknet compilada devemos baixar os pesos do modelo pré-treinado como dito a YOLO, utiliza Redes Neurais Convolucionais <b>CNNs</b> e as CNNs possuem um conjunto de pesos e para se ter acesso aos <b><i>Detectores</b></i> devemos baixar estes pesos do modelo que ja foi treinado, para fazer isso devemos utilizar:

<br>

- Disclaimer: Caso não tenha o wget, instalar o wget veja abaixo maneiras para se instalar o wget para MacOs e Ubuntu

<br>

- MacOS:

```ini
brew install wget (necessário HomeBrew)
```

<br>

- Ubuntu 20.04 LTS:

```ini
sudo apt-get update
sudo apt-get install wget
```

<br>

- <b> Disclaimer 2</b>: Se você possuir os privilégios de usuário rode todos os comandos sem o sudo.

- Quando tudo estiver certo rode então o comando:

<br>

```ini
wget https://github.com/AlexeyAB/darknet/releases download/darknet_yolo_v3_optimal/yolov4.weights

```

<br>

<b> 4. Quarto passo </b>: Dentro da pasta darknet rode o comando `ls`, pois você precisa estar dentro da pasta do projeto caso contrario o detector não será carregado pois ele irá retornar um erro dizendo que não econtrou o arquivo ou diretório

<br>

- Agora dentro do diretório DarkNet rode o seguinte comando:

```
./darknet detect cfg/yolov4.cfg yolov4.weights data/person.jpg

```

- Feito isso, o processo de teste irá iniciar e como neste primeiro momento não estaremos utilizando GPUs, não teremos o poder do paralelismo , portanto este processo pode levar mais ou menos tempo dependendo do processador da sua máquina.

- Após o processo, o darknet irá gerar uma nova imagem no diretório `darknet` que é justamente a imagem com os objetos detectados sendo assim, teremos o seguinte arquivo:

<br>

```
 predictions.jpg
```

### Treino e resultado

<br>

<img src="https://user-images.githubusercontent.com/88453005/222822472-7224e06e-17ce-4365-a15e-7abe24a05455.png">

<br>
<br>

### Resultados com a CPU Mac M1 (arm64) Apple Silicon

<br>

|  CPU   | Time (s) | Person | Dog | Horse |     |     |     |
| :----: | :------: | -----: | --: | ----: | --: | --: | --: |
| Mac M1 |  7, 27   |   100% | 99% |   98% |     |     |
|        |          |        |     |       |     |     |     |

<br>

<img src=https://user-images.githubusercontent.com/88453005/222823017-b7f547c3-539a-42b0-b9ca-ad55d7f16e11.png>
