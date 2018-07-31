# HBase - Comandos utéis
O intuito deste artigo é listar vários comandos utéis no dia a dia do HBase com exemplos e assim servir de guia, principalmente para quem está começando. Os comandos seguirão uma ordem começando da criação do namespace até sua exclusão.
```sh
--criar o name space onde serão armazenado as tabelas que utilizaremos
create_namespace 'namespace'
create_namespace 'eas'

--listar todos os namespace
list_namespace

--criar tabela com família de coluna
create 'namespace:table_name', {NAME=>'column_family'}, {NAME=>'column_family'}
create 'eas:escola', {NAME=>'aluno'}, {NAME=>'instrutor'}

--adicionar uma família de coluna a tabela
alter 'namespace:table_name', 'column_family'
alter 'eas:escola', 'avaliacao'

--exibir a descrição da tabela
desc 'namespace:table_name'
desc 'eas:escola'

--Listar todas as tabelas criadas no HBase
list 

--Listar todas as tabelas criadas no HBase que contenham as letras '*sco*' em seu nome 
list '*sco*'

--Listar todas as ta


--inserir um registro na tabela
put 'table_name', 'rowkey', 'column_family:column_name', 'value'
put 'escola', '1', 'aluno:nome', 'João'
put 'escola', '1', 'aluno:sobrenome', 'Silva'
put 'escola', '1', 'instrutor:nome', 'José'
put 'escola', '1', 'avalicao:disciplina', 'Português'
put 'escola', '2', 'aluno:nome', 'Pedro'
put 'escola', '2', 'aluno:sobrenome', 'Oliveira'
put 'escola', '2', 'instrutor:nome', 'Marcos'
put 'escola', '2', 'avalicao:disciplina', 'Matemática'

--Buscar todos os dados da família de colunas 'aluno' da tabela 'escola'
scan 'table_name', {COLUMNS=>'column_family'}
scan 'escola', {COLUMNS=>'aluno'}

--Limita a busca de registros da família de colunas 'aluno' da tabela 'escola' apenas nos dois primeiros registros
scan 'table_name', {COLUMNS=>'column_family', LIMIT=>number_lines}
scan 'escola', {COLUMNS=>'aluno', LIMIT=>2}

--Buscar somente o primeiro registro da família de colunas 'aluno' da tabela 'escola'
scan 'table_name', {VERSIONS=>1}
scan 'escola', {VERSIONS=>1}



```
