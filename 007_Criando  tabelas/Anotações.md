# PostgreSQL



### Criação tabelas



#### Tabelas

- **Contêm colunas (campos) e linhas de dados (registros);**
- **Cada coluna possui um tipo de dado (que define o tipo de dado que pode ser inserido nela), definido quando criamos uma tabela;**



#### Tipos de dados (alguns exemplos)

- **Booleano**

  - **true ou false:**

    | Tipo de dado |   Descrição   |                      Exemplos                      |
    | :----------: | :-----------: | :------------------------------------------------: |
    |   BOOLEAN    | True ou False | Verificar a existência de um produto em um estoque |

- **Texto/String**

  - **char, varchar e text:**

    | Tipo de dado |                          Descrição                           |              Exemplos               |
    | :----------: | :----------------------------------------------------------: | :---------------------------------: |
    |   CHAR(N)    |   Cadeia de caracteres de comprimento N (comprimento fixo)   |          Gênero ou estado           |
    |  VARCHAR(N)  | Cadeia de caracteres de comprimento N (comprimento variável) |            Nome ou email            |
    |     TEXT     |   Cadeia de caracteres de comprimento variável e ilimitado   | Comentários ou reviews (avaliações) |

    **Utilizando o tipo `CHAR`, a string deve ter examente, `N` caracteres, do contrário, se menor, o restante será aramzenado com espaços em brancos e se maior, receberemos um erro;**

- **Numérico**

  - **inteiro e numero de ponto flutuante:**

    |  Tipo de dado  |                Descrição                |      Exemplos       |
    | :------------: | :-------------------------------------: | :-----------------: |
    |      INT       |            Números inteiros             | Idade ou quantidade |
    | NUMERIC (P, S) |            Números decimais             |   Altura ou preço   |
    |     SERIAL     | Números inteiros (auto incremento de 1) |  id (PRIMARY KEY)   |

    **No tipo `NUMERIC`: `P` (precision/precisão - número máximo de dígitos significativos) e `S` (scale/escala - número de dígitos após o ponto/da parte fracionária);**

- **Data/Hora**

  - **date, time, timestamp e interval:**

    | Tipo de dado |      Descrição      |         Exemplos         |
    | :----------: | :-----------------: | :----------------------: |
    |     TIME     |      HH:MM:SS       |                          |
    |     DATE     |     YYYY-MM-DD      |   Data de aniversário    |
    |  TIMESTAMP   | YYYY-MM-DD HH:MM:SS | Momento da compra/pedido |

    - **`TIME`: formato 24 horas;**
    - **`YYYY`: pode ser torcado por `AAAA` (corresponde ao ano);**

- **UUID**

  - **Identificadores universalmente exclusivos (códigos exclusivos que garantem que tenhamos  identificadores exclusivos, para linhas específicas);**

- **Array**

  - **Matrizes (unidimensionais) que armazenam diferentes tipos de dados (números, textos, etc.);**

- **JSON**

- **Hstore**

  - **Armazena pares chave-valor em um único valor;**

- **Tipos especiais**

  - **Armazenam endereços de rede e dados geométricos**

- **Para saber mais:**

  **https://www.postgresql.org/docs/current/datatype.html**

- **OBS.:**
  - **Ao criar bancos de dados, devemos considerar cuidadosamente, quais tipos de dados devem ser utilizados para que os dados sejam armazenados;**



#### ENUM (Enumerated Types)

| Tipo de dado |                          Descrição                           |       Exemplos       |
| :----------: | :----------------------------------------------------------: | :------------------: |
|     ENUM     | Lista de valores definidos pelo usuário, que podemos aplicar na coluna | Informação de gênero |

- **Exemplo:**

  ```postgresql
  CREATE TYPE gender AS ENUM ('Masculino', 'Feminino', 'Outro');
  
  CREATE TABLE person (
      name text,
      person_gender gender 
  );
  
  -- Qualquer valor diferente que for inserido, acaba gerando um erro
  ```

  

#### Chave primária (primary key) e chave estrangeira (foreign key)

- **Chave primária**
  - **Coluna ou combinação de colunas, utilizadas para identificar um registro/uma linha exclusiva de uma tabela;**
  - **Baseada em um número inteiro e exclusivo;** 
    - **Os valores de uma coluna que representa a chave primária, devem ser únicos e não nulo ... o campo da coluna deve possuir algum valor (esse valor deve existir, ou seja, não devemos ter um "dado ausente" em uma coluna que representa a chave primária);**
    - **Não são obrigatórias, mas é uma boa prática;**
    - **É recomendado a existência de uma única coluna que representa a chave primária, por tabela;**
- **Chave estrangeira**
  - **São utilizadas para a criação de vínculos da sua tabela, com outras, criando assim, relacionamentos**
  - **Coluna ou uma combinção de colunas de uma tabela, que identifica exclusivamente, uma linha/registro em uma outra tabela;**
  - **Essas colunas possuem valores que correspondem aos valores de outras colunas (que representam a chave primária) de outras tabelas (uma coluna de uma tabela que referencia os valores de uma outra coluna, em uma outra tabela, o que cria uma relação entre os dados de ambas as tabelas);**
  - **A tabela que contém a chave estrangeira, é chamada de tabela filho;**
  - **Já a tabela a qual a chave estrangeira faz referência (a tabela que possui a chave primária), é chamada de referência/tabela pai;**
  - **Dependendo dos relacionamentos que uma tabela tem com outra, ela pode ter várias chaves estrangeiras (uma tabela pode ter várias chaves estrangeiras/nenhuma);**
  - **OBSs.:**
    - **Para verificar as chaves da tabela e os seus tipos (primária ou estrangeira), podemos (no PGAdmin):**
      1. **Ir até o banco, em `Schemas`, acessar as `Tables`, escolher uma tabela, ir em `Constraints` e lá veremos a chave primária, uma chave na cor dourado e as chaves estrangeiras (uma chave sobre a outra), na cor prata/cinza;**
      2. **Uma outra maneira é clicando sobre algumas dessas chaves e no menu suspenso, acessar a opção `Properties...`, onde uma espécie de modal será aberto e nele acessamos a guia `Columns`, onde podemos ver as tabela a qual aquela chave estrangeira faz referência (no caso de chaves primárias, essa guia não é exibida);**
      3. **Por último, no menu que fica no topo, temos a opção `Dependencies`, onde podemos deixar lá marcado, voltar lá nas chaves e selecionar alguma. No caso de chaves primárias, nada será exibido (não existirá informações de dependência), mas para chaves estrangeiras, veremos as informações de dependência para aquela coluna específica, bem como as tabela para qual é feita a referência;**
    - **No próprio PGAdmin, quando realizamos uma consulta, podemos identificar uma chave primária vendo o texto que vem logo abaixo do nome da coluna. Para chaves primeiras, além do tipo (que podemos ver em todas as colunas), vemos entre chaves, o acrônimo para chave primária, `PK`;**



#### Constraints (restrições)

- **Regras (restrições) aplicadas as colunas de uma tabela, para impedir que dados inválidos sejam inseridos no banco de dados (ou que as coluna contenham apenas determinados valores);**

- **Constraints podem ser divididas em duas categorias:**

  - **Constraints de coluna**
    - **Aplicadas em uma coluna, para que os dados a serem inseridos atendam a determinadas condições;**
  - **Constraints de tabela**
    - **Aplicadas a toda a a tabela;**

- **Algumas restrições de coluna (exemplos de constraints):**

  - **`NOT NULL`**
    - **Não poderemos inserir valores nulos na coluna;**
    - **É obrigatório definirmos um dado para a coluna, na operação de inserção de dados (não poderemos criar um registro sem informar um dado para a coluna);**
  - **`UNIQUE`**
    - **Garante que teremos valores diferentes (únicos/exclusivos) em uma coluna (a coluna deve conter valores únicos e não duplicados);**
  - **`PRIMARY KEY`**
    - **Identifica cada linha da coluna como uma chave primária (um dado exclusivo);**
  - **`FOREIGN KEY`**
    - **Restringe os dados com base em dados de outras tabelas (deve haver uma correspondência entre esses daods);**
  - **`CHECK`**
    - **Garante que todos os valores em uma coluna, satisfaçam algumas condições específicas;**
  - **`EXCLUSION`**
    - **Garante que se quaisquer duas linhas forem comparadas na(s) coluna(s) ou expressão(ões) especificada(s) usando o(s) operador(es) especificado(s), nem todas essas comparações retornarão `true`;**
  - **`DEFAULT`**
    - **Define um valor padrão para uma coluna;**
    - **Ao criar um registro no banco e não inserir nenhum dado para aquela coluna, ela será preenchida com o valor padrão, definido na criação da tabela;**
    - **Se não definirmos um valor padrão para a coluna e ao criar um registro no banco, não inserirmos uma valor para a coluna, ela será preenchida com o `null`;**
    - **`DEFAULT valor`**
  - **`NOW()`**
    - **Retorna a data e a hora atuais;**
    - **Utilizado como valor padrão de uma coluna (as colunas que representam o momento de criação do registro/de atualização);**
    - **`DEFAULT NOW()`**

- **Para saber mais:**

  **https://www.postgresql.org/docs/current/ddl-constraints.html**

- **Agora, exemplos de restrições de tabela:**
  - **`CHECK (condição)`**
    - **Podemos passar uma condição adicional que sempre será verificada ao inserirmos ou atualizarmos os dados em toda a tabela;** 
  - **`REFERENCES`**
    - **Restringe os valores armazenados em uma coluna, que também devem existir em uma out coluna de uma outra tabela;**
  - **`UNIQUE(colunas)`**
    - **Força os valores armazenados nas colunas listadas dentro dos parênteses, a serem únicos;**
  - **`PRIMARY KEY(colunas)`**
    - **Permite definir várias colunas como chave primária;**



#### Criação de tabelas

- **Sintaxe**

  ```postgresql
  CREATE TABLE nome_da_tabela(
  	nome_da_coluna_1 TIPO_DE_DADO restrições_de_coluna,
      nome_da_coluna_2 TIPO_DE_DADO restrições_de_coluna,
      .
      .
      .
      nome_da_coluna_n TIPO_DE_DADO restrições_de_coluna,
      restrições_de_tabela restrições_de_tabela
  ) INHERITS nome_de_tabela_existente;
  ```

  - **OBSs.:**

    - **`CREATE TABLE nome_da_tabela`: cria a tabela com o nome especificado;**

    - ```postgresql
      nome_da_coluna_1 TIPO_DE_DADO restrições_de_coluna,
      nome_da_coluna_2 TIPO_DE_DADO restrições_de_coluna,
      .
      .
      .
      nome_da_coluna_n TIPO_DE_DADO restrições_de_coluna
      
      -- Colunas que irão compor a tabela;
      -- Informamos o nome da coluna, o tipo de dado e as restrições (constraints);
      -- As colunas são separadas por vírgula;
      ```

    - **`restrições_de_tabela`: restrições que podemos aplicar em nível de tabela;**

    - **`INHERITS nome_de_tabela_existente`: se a tabela a ser criada tem algum tipo de relacionamento com outra, e queremos que ela acabe herdando dessa outra tabela;**

- **Normalmente, cada tabela tem uma coluna na qual definimos a chave primária;**

  - **Para colunas que representam a chave primária, definimos o tipo de dado `SERIAL` (gera uma sequência de números inteiros, exclusivos e de forma automática);**
    - **Se uma linha for removida posteriormente, a sequência criada pelo tipo `SERIAL`, não será ajustada;**
  - **Ainda em colunas que representam a chave primária, definimos a `constraint`  `PRIMARY KEY`;**

- **A query de criação de tabela, só deve ser executada uma única vez;**

- **Agora, para criar chaves estrangeiras (uma coluna que faz referência a colunas de outras tabelas), utilizamos a palavra-chave `REFERENCES`:**

  ```postgresql
  nome_da_coluna TIPO_DE_DADO REFERENCES "nome_da_tabela_referenciada"("coluna_de_referência")
  
  -- Podemos passar as informações entre aspas, ou não
  ```


- **Para removermos/excluirmos uma tabela de um banco de dados, utilizamos o comando `DROP TABLE nome_da_tabela`;**

  

#### Inserção de dados

- **Adiciona/Insere linhas (dados) em uma tabela;**

- **Sintaxe:**

  ```postgresql
  -- Inserindo uma única linha
  INSERT INTO Tabela (coluna_1, coluna_2, ..., coluna_n)
  VALUES (valor_1, valor_2, ..., valor_n);
  
  -- Inserindo várias linhas
  INSERT INTO Tabela (coluna_1, coluna_2, ..., coluna_n)
  VALUES 
  	(valor_1, valor_2, ..., valor_n),
  	(valor_1, valor_2, ..., valor_n),
  	.
  	.
  	.
  	(valor_1, valor_2, ..., valor_n);
  
  --  Para valores de texto, devemos inserir o valor entre 
  -- apóstrofos ("aspas simples") ou aspas duplas
  ```

- **Inserindo valores de uma outra tabela:**

  ```postgresql
  INSERT INTO Tabela (coluna_1, coluna_2, ..., coluna_n)
  SELECT coluna_1, coluna_2, ..., coluna_n
  FROM Outra_Tabela
  WHERE condição;
  ```

- **OBSs.: **

  - **Os valores devem estar na mesma ordem em que as colunas, para que haja a inserção correta dos dados;**
  - **Ao inserir dados de outras tabelas, devemos verificar os tipos de dados que cada coluna aceita, se são compatíveis com os dados que estão vindos da outra tabela;**
  - **Já as colunas que correspondem a chave primária, não precisam receber valores, já que geralmente são definidas com o tipo `SERIAL`, onde os dados são preenchidos automaticamente;**
  - **Agora, ao inserir dados que corresponde as chaves estrangeiras, como por exemplo o `id` de um usuário (onde temos uma tabela de usuários e cada usuário possui um `id`), em uma tabela de compras, devemos inserir um `id` de usuário que exista, caso contrário, receberemos um erro (o `id` que estamos tentando inserir, não pertence a tabela de usuários, por exemplo);**
    - **Ao inserir um dado em uma tabela, que possua a `constraint` de chave estrangeira, devemos garantir que esse dado exista na outra tabela, na qual aquela coluna faz referência, caso contrário, seremos impedidos de inserir aquele dado;**



#### Atualizando dados

- **Ao atualizarmos os dados de uma tabela, alteramos os valores de um registro da tabelas;**

- **Sintaxe:**

  ```postgresql
  UPDATE Tabela
  SET 
  	coluna_1 = novo_valor,
  	coluna_2 = novo_valor,
  	.
  	.
  	.
  	coluna_N = novo_valor,
  WHERE condição;
  ```

  - **Passamos uma condição para informar quando os dados devem ser atualizados (para que eles sejam atualizados se uma determinada condição for atendida);**

  - **Sem a condição, todos os registros (todas as linhas), acabam sendo atualizados;**

  - **Também podemos atualizar os dados de uma coluna, com base em uma outra coluna, da mesma tabela:**

    ```postgresql
    UPDATE Tabela
    SET coluna_A = coluna_B;
    ```

  - **Assim como podemos atualizar os dados de uma coluna, com base em um outra coluna, de uma outra tabela (isso é conhecido como `UPDATE join`):**

    ```postgresql
    UPDATE Tabela_A
    SET coluna_Tabela_A = Tabela_B.coluna
    FROM Tabela_B
    WHERE Tabela_A.id = Tabela_B.id;
    ```

  - **Outra coisa que podemos fazer é retornar os dados (as linhas) afetados pela atualização:**

    ```postgresql
    UPDATE Tabela
    SET coluna_A = coluna_B;
    RETURNING coluna_A;
    ```

    

#### Remoção de dados

- **Usamos a cláusula `DELETE`, para remover linhas de uma tabela;**

- **Sintaxe:**

  ```postgresql
  DELETE FROM Tabela
  WHERE condição;
  ```

  - **Devemos utilizar a cláusula `WHERE`, para não acabarmos removendo todas as linhas da tabela;**

  - **Podemos remover as linhas de uma tabela, com base nas informações de outra tabela:**

    ```postgresql
    DELETE FROM Tabela_A
    USING Tabela_B
    WHERE Tabela_A.coluna_id = Tabela_B.coluna_id;
    ```

  - **Também podemos utilizar a cláusula `RETURNING`, para retornar os dados (as linhas) que foram removidos;**
  - **Uma query de remoção de dados, só deve ser executada uma única vez, já que ao remover os dados, aquele registro passa a não existir mais e assim não se tem mais o que remover;**



#### Alterando a estrutura de uma tabela

- **Podemos alterar a estrutura de uma tabela, através da cláusula `ALTER`;**

  - **Podemos adicionar/remover os nomes das colunas;**
  - **Alterar os tipos de dados das colunas;**
  - **Definir valores padrões para as colunas;**
  - **Adicionar restrições (`constraints`);**
  - **Renomear a tabela;**

- **Alguns exemplos:**

  ```postgresql
  ALTER TABLE nome_da_tabela ação;
  
  -- Adicionando coluna (s)
  ALTER TABLE nome_da_tabela
  ADD COLUMN nova_coluna TIPO_DA_COLUNA RESTRIÇÔES,
  ADD COLUMN nova_coluna TIPO_DA_COLUNA RESTRIÇÔES,
  .
  .
  .
  ADD COLUMN nova_coluna TIPO_DA_COLUNA RESTRIÇÔES,;
  
  -- Removendo coluna (s)
  ALTER TABLE nome_da_tabela
  DROP COLUMN coluna;
  
  -- Alterando o tipo de dado da coluna
  ALTER TABLE nome_da_tabela
  ALTER COLUMN nome_da_coluna TYPE novo_tipo;
  
  -- Alterando as constraints de coluna (s)
  ALTER TABLE nome_da_tabela
  ALTER COLUMN coluna
  SET DEFAULT valor;
  
  -- Removendo as constraints de coluna (s)
  ALTER TABLE nome_da_tabela
  ALTER COLUMN coluna
  DROP DEFAULT;
  
  -- Removedo uma constraint de coluna
  ALTER TABLE nome_da_tabela
  ALTER COLUMN coluna
  DROP nome_da_constraint;
  
  -- Removendo/Adicionando constraints não nulas
  ALTER TABLE nome_da_tabela
  ALTER COLUMN coluna
  SET/DROP NOT NULL;
  
  -- Adicionando uma constraint
  ALTER TABLE nome_da_tabela
  ALTER COLUMN coluna
  ADD CONSTRAINT nome_da_constraint;
  
  -- Renomeando tabela
  ALTER TABLE nome_da_tabela
  RENAME TO novo_nome;
  
  -- Renomeando coluna
  ALTER TABLE nome_da_tabela
  RENAME COLUMN nome_da_coluna TO novo_nome;
  ```

- **Para saber mais:**

  **https://www.postgresql.org/docs/current/sql-altertable.html**



#### Removendo colunas

- **Para podermos remover completamente, uma coluna, utilizamos a cláusula `DROP`;**

- **Sintaxe:**

  ```postgresql
  ALTER TABLE nome_da_tabela
  DROP COLUMN nome_da_coluna;
  ```

  - **Se a coluna que queremos remover, for referenciada em uma outra coluna, não poderemos remové-la (já que outras "entidades" dependem dela), utilizando a query exibida acima. Para remové-la de fato, devemos adicionar a cláusula `CASCADE`, para remover a coluna e descartar suas dependências:**

    ```postgresql
    ALTER TABLE nome_da_tabela
    DROP COLUMN nome_da_coluna CASCADE;
    ```

  - **Se tentarmos remover uma coluna inexistente, será retornado um erro. Para podermos evitar esse erro, podemos adicionar a cláusula `IF EXISTS`:**

    ```postgresql
    ALTER TABLE nome_da_tabela
    DROP COLUMN IF EXISTS nome_da_coluna;
    ```

    **Utilizando esta cláusula, se tentarmos remover uma coluna que não existe, receberemos um aviso (informando que a coluna não existe), ao invés de um erro;**

  - **Podemos também remover várias colunas de uma só vez:**

    ```postgresql
    ALTER TABLE nome_da_tabela
    DROP COLUMN coluna_1,
    DROP COLUMN coluna_2,
    .
    .
    .
    DROP COLUMN coluna_N;
    ```

    

#### CHECK Constraints 

- **Nos permite a criação de restrições mais personalizadas, que aderems a uma determinada condição;**

- **Exemplo:**

  ```postgresql
  CREATE TABLE user(
  	user_id SERIAL PRIMARY KEY,
      name VARCHAR(50) NOT NULL,
      email VARCHAR(50) NOT NULL UNIQUE,
      age SMALLINT CHECK( age >= 18)
  );
  ```

  - **Também podemos criar condições baseadas em outras colunas;**
  - **Se as condições não forem atentidas, receberemos um erro ao tentar realizar a inserção dos dados;**
  - **A contagem feita pelo tipo `SERIAL` é feita mesmo com os erros de inserção de dados (ao tentar fazer uma inserção e obter um erro, aquela inserção não cria um registro na nossa tabela, mas a contagem é iniciada e, ao realizar a próxima inserção, e ela ser feita da maneira correta e conseguirmos inserir os dados com sucesso, o `id` dessa tabela, onde temos a chave primária, irá registrar o valor dois, ou seja, a contagem é feita para as tentativas que não obtemos sucesso, apenas o registros não são inserido na tabela);**



#### Tabelas de junção

- **Tabela que faz referencia a outras tabelas (utilizada para unir outras tabelas);**

- **A chave primária acaba sendo uma cobinação entre as colunas que representam a chave estrangeira;**

- **Exemplo:**

  ```postgresql
  -- Um tabela que liga as tabelas de filme e atores
  
  -- Tabela de filmes
  SELECT * FROM filmes;
  
  -- Tabela de atores
  SELECT * FROM atores;
  
  -- Tabela de junção
  CREATE TABLE filmes_atores(
  	filme_id INT REFERENCES filmes(filme_id),
      ator_id INT REFERENCES atores(ator_id),
      PRIMARY KEY(filme_id, ator_id)
  )
  ```

  
