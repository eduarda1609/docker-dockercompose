# Docker e Docker Compose

## O que é Docker?

O Docker é uma plataforma de virtualização baseada em containers, criada para simplificar a criação, o empacotamento e a execução de aplicações em qualquer ambiente, independentemente de sistema operacional ou infraestrutura.

Diferente das máquinas virtuais tradicionais, o Docker não replica um sistema completo, ele compartilha o kernel do sistema operacional do host, tornando a execução mais leve e rápida. Ou seja, em vez de virtualizar todo o hardware, ele isola processos dentro de containers. Essa técnica garante uma maior consistência entre os ambientes de desenvolvimento e produção, reduzindo erros.

Container é um pacote isolado e leve que junta o seu código com absolutamente tudo o que ele precisa para rodar (versão da linguagem, bibliotecas e configurações). O erro de "funciona na minha máquina" acontece porque o ambiente do desenvolvedor (ex: Windows com Python 3.10) é diferente do servidor (ex: Linux com Python 3.8), o que quebra a aplicação. O docker resolve isso porque você não envia mais apenas o código, você envia o ambiente inteiro empacotado. Como o container carrega sua própria infraestrutura lá dentro, se ele rodou no seu notebook, rodará de forma idêntica em qualquer outro lugar.

## Imagem vs Container

Um container do Docker é um ambiente de runtime com todos os componentes necessários, como código, dependências e bibliotecas, para executar o código da aplicação sem usar dependências da máquina host. Esse runtime de contêiner é executado no mecanismo em um servidor, máquina ou instância de nuvem. O mecanismo opera vários contêineres, dependendo dos atributos subjacentes disponíveis. 

Uma imagem do Docker, ou imagem de container, é um arquivo executável independente usado para criar um contêiner. Essa imagem de contêiner contém todas as bibliotecas, dependências e arquivos de que o contêiner precisa para ser executado. Uma imagem do Docker é compartilhável e portátil, então você pode implantar a mesma imagem em vários locais ao mesmo tempo, da mesma forma que um arquivo binário de software. 

Traçando um paralelo com o conceito de orientação a objeto, a imagem é a classe e o container o objeto. A imagem é a abstração da infraestrutura em estado somente leitura, de onde será instanciado o container. Todo container é iniciado a partir de uma imagem, dessa forma podemos concluir que nunca teremos uma imagem em execução. Um container só pode ser iniciado a partir de uma única imagem. Caso deseje um comportamento diferente, será necessário customizar a imagem.

## O que é um Dockerfile?

É um arquivo de texto simples que contém uma coleção de comandos ou procedimentos. Esses comandos e instruções que executamos atuam sobre a imagem base configurada para criar uma nova imagem Docker. Um Dockerfile é o código-fonte da imagem Docker. Um Dockerfile é um arquivo de texto que contém várias instruções e configurações. O comando FROM em um Dockerfile identifica a imagem base a partir da qual você está construindo a nova imagem.

Ao executar o comando docker build, o Docker lê o Dockerfile e utiliza essas instruções para construir a imagem passo a passo. (O comando docker run entra apenas na etapa seguinte, servindo para executar a imagem que já foi construída). A grande vantagem de usar um Dockerfile, em vez de simplesmente guardar e compartilhar a imagem já pronta (binária), é que ele funciona como uma receita automatizada. Toda vez que você constrói a imagem, garante que está utilizando as versões mais recentes das dependências. Isso é excelente para a segurança do projeto, pois evita que você acabe instalando e rodando aplicativos defasados ou vulneráveis

Abaixo está um exemplo de um Dockerfile ilustrando um pequeno aplicativo escrito em Python

1. Identifica a imagem base (como mencionado no seu texto)
FROM python:3.10-slim

2. Define a pasta de trabalho dentro da nova imagem
WORKDIR /app

3. Copia o código-fonte do seu computador para dentro da imagem
COPY . /app

4. Comandos e instruções que atuam sobre a imagem base
RUN pip install -r dependencias.txt

5. O que o container deve fazer quando for iniciado
CMD ["python", "meu_aplicativo.py"]

## O que é Docker Compose?

Docker Compose é uma ferramenta oficial do ecossistema Docker usada para definir, configurar e executar aplicações que dependem de múltiplos containers. Ele faz isso através de um único arquivo de texto de configuração (formato YAML), geralmente chamado de docker-compose.yml.

Se a sua aplicação precisa de um servidor web e um banco de dados, você teria que rodar comandos docker run longos e complexos para cada um, criando redes virtuais manualmente para que eles pudessem se enxergar. O Compose automatiza e documenta todo esse processo.

O docker compose pode ser usado sempre que o seu projeto for composto por mais de um serviço que precisam rodar juntos e se comunicar. Em vez de iniciar peça por peça manualmente, você descreve a arquitetura inteira no arquivo YAML. Com apenas um comando (docker compose up), ele baixa as imagens, cria os volumes, configura a rede interna e sobe todos os containers de uma só vez.

## Exemplo Prático

Quando executamos docker compose up, em vez de termos que configurar e iniciar cada parte do projeto separadamente, o Docker faz todo o trabalho de infraestrutura. Primeiro, ele analisa o arquivo docker-compose.yml para entender quais serviços definimos para o projeto (por exemplo, um banco de dados e uma API) e como eles devem se comportar. Depois docker gera uma rede virtual isolada e é por meio dela que a API consegue se comunicar diretamente com o banco de dados de forma segura, sem depender das configurações da minha máquina física.
Logo, o docker aloca um espaço seguro no disco para persistir os dados. Isso garante que as informações do banco de dados não sejam perdidas caso precisamos reiniciar ou desligar o container.
Assim, ele verifica as necessidades do projeto. Se o arquivo pedir uma imagem do PostgreSQL que ainda não temos, ele faz o download. Se precisar rodar o código, ele constrói a imagem localmente. E por fim ele começa a ligar os containers. Como o arquivo de configuração permite definir dependências, o Docker é capaz de iniciar o banco de dados primeiro e, apenas quando ele estiver pronto e rodando, iniciar o container da API.

Em resumo, ao executar o comando, o Docker lê a arquitetura que planejamos, prepara a rede e os discos, baixa o que for necessário e coloca todo o sistema no ar de forma sincronizada. Se usarmos o comando docker compose up -d, ele faz tudo isso em segundo plano e libera o terminal para continuarmos trabalhando.

## Referências

https://king.host/blog/tecnologia/o-que-e-docker/#:~:text=Docker%20%C3%A9%20uma%20plataforma%20que,forma%20consistente%20em%20qualquer%20ambiente.

https://stack.desenvolvedor.expert/appendix/docker/criandoimagem.html

https://aws.amazon.com/pt/compare/the-difference-between-docker-images-and-containers/

https://cto.ai/blog/docker-image-vs-container-vs-dockerfile/