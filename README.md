# Curso: Programação Orientada a Objetos com Java
 - http://educandoweb.com.br
 - Prof. Dr. Nelio Alves
# Capítulo: Acesso a banco de dados com JDBC
 - Objetivo geral:
 - Conhecer os principais recursos do JDBC na teoria e prática
 - Elaborar a estrutura básica de um projeto com JDBC
 - Implementar o padrão DAO manualmente com JDBC
# Visão geral do JDBC
 - JDBC (Java Database Connectivity): API padrão do Java para acesso a dados
 - Páginas oficiais: 
o https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/
o https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html
 - Pacotes: java.sql e javax.sql (API suplementar para servidores)
![jdbc](https://user-images.githubusercontent.com/86566715/151632639-f53e7787-e0ec-4296-a058-74407672efbb.PNG)

# Instalação das ferramentas:
 - Instalar o MySQL Server e o MySQL Workbench
Preparação do primeiro projeto no Eclipse
Código fonte: https://github.com/acenelio/jdbc1
Checklist:
 o Usando o MySQL Workbench, crie uma base de dados chamada "coursejdbc"
 o Baixar o MySQL Java Connector
 o Caso ainda não exista, criar uma User Library contendo o arquivo .jar do driver do MySQL
 - Window -> Preferences -> Java -> Build path -> User Libraries
 - Dê o nome da User Library de MySQLConnector
 - Add external JARs -> (localize o arquivo jar)
 o Criar um novo Java Project
 - Acrescentar a User Library MySQLConnector ao projeto
 o Na pasta raiz do projeto, criar um arquivo "db.properties" contendo os dados de conexão:
 - user=developer
 - password=1234567
 - dburl=jdbc:mysql://localhost:3306/coursejdbc
 - useSSL=false
o No pacote "db", criar uma exceção personalizada DbException
o No pacote "db", criar uma classe DB com métodos estáticos auxiliares
- Obter e fechar uma conexão com o banco
# Demo: recuperar dados
Script SQL: material de apoio ou https://github.com/acenelio/demo-dao-jdbc/blob/master/database.sql
Código fonte: https://github.com/acenelio/jdbc2

API
o Statement
o ResultSet
- first() [move para posição 1, se houver]
- beforeFirst() [move para posição 0] o next() [move para o próximo, retorna false se já estiver no último] o absolute(int) [move para a posição dada, lembrando que dados reais começam em 1]
Checklist:
o Usar o script SQL para criar a base de dados "coursejdbc"
o Fazer um pequeno programa para recuperar os departamentos
o Na classe DB, criar métodos auxiliares estáticos para fechar ResultSet e Statement
