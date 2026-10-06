# Respostas · Avaliação Prática de Docker · Cooperativa AgroVale (Turma A)

Nome: Arthur Reis de Oliveira
Matrícula: 26128968
Usuário do GitHub: Arthur08270
Usuário do Docker Hub: arthur2700

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile ou compose vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
Eu usei a base "FROM nginx:1.27-alpine". O seu tamanho final foi de 73.6 MB

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.


## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
Esse é o nome da imagem: arthur2700/agrovale-portal:1.0-26128968. Esse é o link: https://hub.docker.com/r/arthur2700/agrovale-portal

4. Por que o `docker login` foi feito com um token de acesso e não com a senha da conta?
O login foi feito com um token de acesso porque ele é uma credencial específica para autenticação 
no Docker Hub, podendo enviar imagens sem utilizar especificamente a senha da conta.

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | Copiar os arquivos do site para a pasta servida pelo Nginx | Faltava o COPY da pasta site/ para /usr/share/nginx/html/ | Ao acessar http://localhost:7068, aparecia a página padrão Welcome to nginx! em vez da página de manutenção | Adicionei COPY site/ /usr/share/nginx/html/ ao Dockerfile |
| 2 | | | | |
| 3 | | | | |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
A opção -p segue o formato PORTA_DO_HOST:PORTA_DO_CONTAINER.
-p 7042:80 - A porta 7042 do computador é direcionada para a porta 80 do container.
-p 80:7042 - A porta 80 do computador é direcionada para a porta 7042 do container.
Portanto, o número à direita dos dois-pontos é a porta do container.

## Parte 4 · docker-compose.yml

7. No serviço `blog`, por que `WORDPRESS_DB_HOST` recebe `db` e não `localhost`?
Porque `db` é o nome do serviço do MariaDB no docker-compose.yml. Os serviços compartilham a mesma rede do Docker e podem se localizar pelo nome do serviço.
Se fosse utilizado `localhost`, o WordPress tentaria acessar o banco dentro do próprio container do WordPress, e não o container do MariaDB.

8. Por que o serviço `db` não publica a porta 3306? Se precisar consultar o banco, como faz sem publicar a porta? Mostre o comando.
O banco não precisa publicar a porta 3306 para o computador porque o WordPress acessa o MariaDB diretamente pela rede interna do Docker usando o nome do serviço `db`.
Se fosse necessário consultar o banco, seria possível executar o cliente MariaDB dentro do própriocontainer, por exemplo:docker compose exec db mariadb -u agrovale -p agrovale_blog. Assim, não é necessário expor a porta 3306 para o host.

## Parte 5 · Persistência

9. Quais comandos você usou para derrubar e subir a stack? Qual comando teria apagado o post que você criou, e por quê?
Para derrubar a stack, usei: docker compose down
Para subir novamente, usei: docker compose up -d
O comando que poderia apagar o post seria: docker compose down -v
Isso acontece porque a opção `-v` remove os volumes nomeados da stack. Como os dados do WordPress e do MariaDB estavam armazenados nos volumes, removê-los faria com que os dados persistidos fossem apagados.

10. Código de conclusão impresso pelo verificador:

```
AGROVALE-26128968-67097F73
```
