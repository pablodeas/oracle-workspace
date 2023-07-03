# Oracle - Workspace

Esse é um passo a passo de como instalar e utilizar o [Oracle-Database], mais especificamente o 19c, com o Docker.

Também vou levar em consideração que o utilizador deste passo a passo já possui conhecimentos básicos em Docker.

- Em primeiro lugar, é necessário ter o Docker instalado na máquina. Entre no site e siga o passo a passo:

https://www.docker.com/

- Para realizar a instalação do [Banco de Dados Oracle], deve-se entrar no link abaixo e clonar o repositório para sua máquina local:

https://github.com/oracle/docker-images

- Agora, faça o download do Oracle Database 19c para Linux x86-64:

http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html

- Recorte e cole o arquivo LINUX.X64_193000_db_home.zip para o diretório docker-images/OracleDatabase/SingleInstance/dockerfiles/19.3.0:

- Caso você faça a instalação de outra versão do Oracle Database, é necessário recortar e colar no diretório correspondente a versão.

- Existe um arquivo .sh que irá fazer a instalação do container. Retorne um diretório e execute o comando:<br />
./buildContainerImage.sh -v 19.3.0 -e

- Dependendo da versão, será necessário alterar a numeração do comando acima.

- Esse comando irá baixar o oraclelinux:7-slim, irá atualizar os pacotes com o gerenciador YUM, fará a instalação do banco no OS e também fará todas as configurações necessárias automaticamente.

- Ao final, teremos a seguinte imagem:
![alt text](https://www.oracle.com/technetwork/es/images/image021-5592437.png)

- Para verificar se as imagens foram instaladas: <br />
docker images

- Agora, precisamos criar o Banco de Dados. Utilizamos o seguinte comando:<br />
docker run --name oracle19c -p 1521:1521 -p 5500:5500 -v /home/pablodeas/Workspace/Projects/pessoal/oracle-server-docker:/home/pablodeas/Workspace/Projects/pessoal/oracle-server-docker/oracle-datafiles oracle/database:19.3.0-ee

- O primeiro caminho passado no comando acima será o responsável por armazenar os datafiles do banco de dados e o segundo é o caminho com os arquivos de instalação. Faça de acordo com o seu sistema operacional e os seus diretórios.

- Ao final da instalação, teremos o seguinte:
![alt text](https://www.oracle.com/technetwork/es/images/image025-5592442.png)

- No início da instalação pelo docker run, uma senha foi gerada.
![alt text](https://www.oracle.com/technetwork/es/images/image026-5592443.png)

- Caso não tenha sido, altere para a senha que preferir com o seguinte:<br />
docker exec oracle19c ./setPassword.sh Minha_Nova_Senha

- Para verificar se o Banco de Dados está em execução:<br />
docker ps -a
![alt text](https://www.oracle.com/technetwork/es/images/image028-5592445.png)

- Parâmetros para conectar no SQLDeveloper ou outro Gerenciador:
![alt text](https://www.oracle.com/technetwork/es/images/image029-5592446.png)

- Para conectar no SQLPLUS:<br />
sqlplus sys / Welcome1 @ \ "localhost: 1521 / orclpdb1 \" as sysdba<br />
ou<br />
docker exec -ti afc1b3ccf194 sqlplus / as sydba<br />
ou<br />
docker exec -ti afc1b3ccf194 sqlplus PABLO/SENHA@orclpdb1

- Para acessar o Oracle Enterprise Manager Database Express:<br />
https://localhost:5500/em/shell

- Para parar o Banco de Dados<br />
docker stop oracle19c

- Para iniciar o Banco de Dados<br />
docker start oracle19c

- Para verificar os logs<br />
docker logs oracle19c

[Referências:]<br />
https://www.oracle.com/br/technical-resources/articles/database-performance/oracle-db19c-com-docker.html
