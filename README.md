# HBase - Comandos utéis
O intuito deste artigo é listar vários comandos utéis do HBase no dia a dia com exemplos: 
```sh
--criar tabela com família de coluna
create 'table_name', {NAME=>'column_family'}, {NAME=>'column_family'}
create 'escola', {NAME=>'aluno'}, {NAME=>'instrutor'}

--adicionar uma família de coluna a tabela
alter 'table_name', 'column_family'
alter 'escola', 'avaliacao'

--exibir a descrição da tabela
desc 'table_name'
desc 'escola'

--inserir um registro na tabela
put 'table_name', 'rowkey', 'column_family:column_name', 'value'
put 'escola, '1', 'aluno:nome', 'João'
put 'escola', '1', 'aluno:sobrenome', 'Silva'
put 'escola', '1', 'instrutor:nome', 'José'
put 'escola', '1', 'avalicao:disciplina', 'Português'

--Buscar todos os dados da família de colunas 'aluno' da tabela 'escola'
scan 'table_name', {COLUMNS=>'column_family'}
scan 'escola', {COLUMNS=>'aluno'}

--Buscar somente o primeiro registro da família de colunas 'aluno' da tabela 'escola'
scan 'table_name', {COLUMNS=>'column_family', LIMIT<=number_lines}
scan 'escola', {COLUMNS=>'aluno', LIMIT<=1}

--Buscar somente o primeiro registro da família de colunas 'aluno' da tabela 'escola'
scan 'table_name', {VERSIONS=>1}
scan 'escola', {VERSIONS=>1}



```
