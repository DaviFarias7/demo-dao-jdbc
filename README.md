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
o No pacote "db", criar uma exceção personalizada DbException
o No pacote "db", criar uma classe DB com métodos estáticos auxiliares
- Obter e fechar uma conexão com o banco
# Demo: recuperar dados

API:

- Statement
- ResultSet
- first() [move para posição 1, se houver]
- beforeFirst() [move para posição 0] 
- next() [move para o próximo, retorna false se já estiver no último] 
- absolute(int) [move para a posição dada, lembrando que dados reais começam em 1]

Checklist:

- Usar o script SQL para criar a base de dados "coursejdbc"
- Fazer um pequeno programa para recuperar os departamentos
- Na classe DB, criar métodos auxiliares estáticos para fechar ResultSet e Statement

# Demo: inserir dados
API:
- PreparedStatement
- executeUpdate
- Statement.RETURN_GENERATED_KEYS
- getGeneratedKeys

Checklist:

- Inserção simples com preparedStatement
- Inserção com recuperação de Id

# Demo: atualizar dados

# Demo: deletar dados

Checklist:

- Criar DbIntegrityException
- Tratar a exceção de integridade referencial

# Demo: transações
Referências: https://www.ibm.com/support/knowledgecenter/en/SSGMCP_5.4.0/product-overview/acid.html

API:

- setAutoCommit(false)
- commit()
- rollback()

# Padrão de projeto DAO (Data Access Object) 
Referências:
https://www.devmedia.com.br/dao-pattern-persistencia-de-dados-utilizando-o-padrao-dao/30999
https://www.oracle.com/technetwork/java/dataaccessobject-138824.html

Ideia geral do padrão DAO:

- Para cada entidade, haverá um objeto responsável por fazer acesso a dados relacionado a esta 
entidade. Por exemplo:
   - Cliente: ClienteDao
   - Produto: ProdutoDao
   - Pedido: PedidoDao
- Cada DAO será definido por uma interface.
- A injeção de dependência pode ser feita por meio do padrão de projeto Factory

![image](https://user-images.githubusercontent.com/86566715/151978613-305dfb8e-8753-4cf3-b097-bd9daf0d1c90.png)

# Department entity class
Entity class checklist:  Attributes
- Constructors
- Getters/Setters
- hashCode and equals
- toString
- implements Serializable

![image](https://user-images.githubusercontent.com/86566715/151979927-f07dd322-faa5-4cfd-a123-e1e3c81827c6.png)

# Seller entity class

# DepartmentDao and SellerDao interfaces

# SellerDaoJDBC and DaoFactory

# findById implementation
```
SQL Query:

SELECT seller.*,department.Name as DepName
FROM seller INNER JOIN department 
ON seller.DepartmentId = department.Id 
WHERE seller.Id = ?
```
# Reusing instantiation
``
private Seller instantiateSeller(ResultSet rs, Department dep) throws SQLException {
Seller obj = new Seller();
obj.setId(rs.getInt("Id"));
obj.setName(rs.getString("Name"));
obj.setEmail(rs.getString("Email"));
obj.setBaseSalary(rs.getDouble("BaseSalary"));
obj.setBirthDate(rs.getDate("BirthDate"));
obj.setDepartment(dep);
return obj; }
private Department instantiateDepartment(ResultSet rs) throws SQLException {
Department dep = new Department();
dep.setId(rs.getInt("DepartmentId"));
dep.setName(rs.getString("DepName"));
return dep; }
``
# findByDepartment implementation
```
SQL Query:

SELECT seller.*,department.Name as DepName 
FROM seller INNER JOIN department 
ON seller.DepartmentId = department.Id
WHERE DepartmentId = ?
ORDER BY Name
INCORRECT
CORRECT
```
# findAll implementation
``
SQL Query:

SELECT seller.*,department.Name as DepName 
FROM seller INNER JOIN department 
ON seller.DepartmentId = department.Id
ORDER BY Name
``
# insert implementation
``
SQL Query:
INSERT INTO seller
(Name, Email, BirthDate, BaseSalary, DepartmentId) 
VALUES 
(?, ?, ?, ?, ?)
``
# update implementation
``
SQL Query:
UPDATE seller 
SET Name = ?, Email = ?, BirthDate = ?, BaseSalary = ?, DepartmentId = ? 
WHERE Id = ?
``
# delete implementation
``
SQL Query:
DELETE FROM seller 
WHERE Id = ?
``
# DepartmentDao implementation and test

Checklist:

- DepartmentDaoJDBC
- DaoFactory
- Program2

