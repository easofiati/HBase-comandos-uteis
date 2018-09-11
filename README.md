# HBase - Comandos utéis
O intuito deste artigo é listar vários comandos utéis no dia a dia do HBase com exemplos e assim servir de guia, principalmente para quem está começando. Os comandos seguirão uma ordem começando da criação do namespace até sua exclusão.
```sh
--criar o name space onde serão armazenado as tabelas que utilizaremos. Por padrão todas as tabelas do HBase são criadas no namespace default
sintax: create_namespace 'namespace'
create_namespace 'eas'

--listar todos os namespace
list_namespace

--criar tabela com família de coluna
sintax: create 'namespace:table_name', {NAME=>'column_family'}
create 'eas:escola', {NAME=>'aluno'}, {NAME=>'instrutor'}

--listar todas as tabelas criadas no HBase
list 

--listar todas as tabelas criadas no HBase que contenham as letras '*sco*' em seu nome 
list '*sco*'

--listar todas as tabelas criadas em um namespace
list_namespace_tables 'eas'

--adicionar uma família de coluna a tabela
sintax: alter 'namespace:table_name', 'column_family'
alter 'eas:escola', 'avaliacao'

--exibir a descrição da tabela
sintax: desc 'namespace:table_name'
desc 'eas:escola'

--alterar as propriedades de uma tabela, no caso iremos alterar as propriedades de compressão dos dados da tabela
sintax: alter 'namespace:table_name',{NAME=>'column_family', COMPRESSION=>'value'}
alter 'eas:escola',{NAME=>'aluno', COMPRESSION=>'snappy'}
alter 'eas:escola',{NAME=>'aluno', DATA_BLOCK_ENCODING=>'FAST_DIFF'}

--exibir a descrição da tabela com as alterações realizadas no passo anterior
sintax: desc 'namespace:table_name'
desc 'eas:escola'

--inserir um registro na tabela
sintax: put 'namespace:table_name', 'rowkey', 'column_family:column_name', 'value'
put 'eas:escola', '1', 'aluno:nome', 'Elissandro'
put 'eas:escola', '1', 'aluno:sobrenome', 'Sofiati'
put 'eas:escola', '1', 'instrutor:nome', 'Domingos'
put 'eas:escola', '1', 'avaliacao:disciplina', 'Matematica'
put 'eas:escola', '2', 'aluno:nome', 'Pedro'
put 'eas:escola', '2', 'aluno:sobrenome', 'Oliveira'
put 'eas:escola', '2', 'instrutor:nome', 'Marcos'
put 'eas:escola', '2', 'avaliacao:disciplina', 'Matematica'
put 'eas:escola', '3', 'aluno:nome', 'Maria'
put 'eas:escola', '3', 'aluno:sobrenome', 'Santos'
put 'eas:escola', '3', 'instrutor:nome', 'Ana'
put 'eas:escola', '3', 'avaliacao:disciplina', 'Biologia'
put 'eas:escola', '4', 'aluno:nome', 'Joao'
put 'eas:escola', '4', 'aluno:sobrenome', 'Silva'
put 'eas:escola', '4', 'instrutor:nome', 'Jose'
put 'eas:escola', '4', 'avaliacao:disciplina', 'Portugues'
put 'eas:escola', '10', 'aluno:nome', 'Jeferson'
put 'eas:escola', '10', 'aluno:sobrenome', 'Rocha'
put 'eas:escola', '10', 'instrutor:nome', 'Eder'
put 'eas:escola', '10', 'avaliacao:disciplina', 'Geografia'

--Buscar todos os dados da família de colunas 'aluno' da tabela 'escola'
sintax: scan 'namespace:table_name', {COLUMNS=>'column_family'}
scan 'eas:escola', {COLUMNS=>'aluno'}

--Limitar a busca de registros da família de colunas 'aluno' da tabela 'escola' apenas nos dois primeiros registros
sintax: scan 'namespace:table_name', {COLUMNS=>'column_family', LIMIT=>number_of_lines}
scan 'eas:escola', {COLUMNS=>'aluno', LIMIT=>2}

--Alterar o conteúdo da coluna 'nome' da família de colunas 'aluno' da tabela 'escola' da rowkey '2' para 'Paulo'
put 'eas:escola', '2', 'aluno:nome', 'Paulo'

--Buscar as duas últimas versões de todas as colunas da família de colunas 'aluno' da tabela 'escola'
sintax: scan 'namespace:table_name', {COLUMNS=>'column_family', VERSIONS=>version_number}
scan 'eas:escola', {COLUMNS=>'aluno', VERSIONS=>2}

--Buscar todas as colunas de todas as famílias de coluna de uma tabela para uma 'rowkey' específica
sintax: get 'namespace:table_name', 'rowkey'
get 'eas:escola', '2'

--Buscar todas as colunas de somente uma determinada família de coluna de uma tabela para uma 'rowkey' específica
sintax: get 'namespace:table_name', 'rowkey', {COLUMN=>'column_family'}
get 'eas:escola', '2', {COLUMN=>'aluno'}

--Buscar somente uma coluna de somente uma determinada família de coluna de uma tabela para um registro específico
sintax: get 'namespace:table_name', 'rowkey', {COLUMN=>'column_family:column'}
get 'eas:escola', '2', {COLUMN=>'aluno:nome'}

--Atribuir o 'timestamp' no momento do 'put'. Nesse exemplo estaremos atribuindo o 'timestamp' da coluna 'nome' da família de colunas 'aluno' da tabela 'escola' da rowkey '10' nas demais colunas, deixando assim todas as colunas com o mesmo 'timestamp'. O seu 'timestamp' com certeza é diferente do que estou inserindo abaixo, então atente-se a este ponto.
put 'eas:escola', '10', 'aluno:sobrenome', 'Rocha', 1535660485214
put 'eas:escola', '10', 'instrutor:nome', 'Eder', 1535660485214
put 'eas:escola', '10', 'avaliacao:disciplina', 'Geografia', 1535660485214

--Buscar todas as colunas de uma tabela para uma 'rowkey' específica com um determinado 'timestamp'
sintax: get 'namespace:table_name', 'rowkey', {COLUMN=>'column_family', TIMESTAMP=>timestamp}
get 'eas:escola', '10', {TIMESTAMP=>1535660485214}

--Buscar todas as colunas de uma tabela que tenham uma rowkey que inicie com '1'
scan 'eas:escola', {ROWPREFIXFILTER => '1'}

--Buscar todas as colunas das rowkey's que atendam um determinado filtro
scan 'namespace:table_name', {FILTER => "(SingleColumnValueFilter('column_family','column',operator,'comparator'))"}
scan 'eas:escola', {FILTER => "(SingleColumnValueFilter('aluno','sobrenome',=,'binary:Pereira'))"}
scan 'eas:escola', {FILTER => "(SingleColumnValueFilter('aluno','sobrenome',=,'regexstring:.*ei*'))"}
scan 'eas:escola', {FILTER => "(SingleColumnValueFilter('aluno','sobrenome',=,'regexstring:.*ei*')) AND (SingleColumnValueFilter('aluno','nome',=,'binary:Pedro'))"}

--Buscar em uma determinada tabela, qualquer coluna que tenha um determinado valor
scan 'eas:escola', {FILTER => "(ValueFilter(=,'binary:Silva'))"}



--Compactar os dados de uma tabela, sendo que já alteramos as propriedades de uma tabela anteriormente para que a mesma permita compactação
sintax: major_compact 'namespace:table_name'
major_compact 'eas:escola'




```
