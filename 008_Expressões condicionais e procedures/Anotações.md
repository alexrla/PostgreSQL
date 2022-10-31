# PostgreSQL



### Expressões condicionais e procedures



#### CASE

- **Nos permite executar instruções, se determinada condição for atendida (semelhante as instruções `IF/ELSE`);**

- **Existem duas maneiras (duas maneiras principais) de usar uma instrução `CASE`,:**

  - **Forma geral:**

    ```postgresql
    CASE
    	WHEN condição_1 THEN retorno/resultado
    	WHEN condição_2 THEN retorno/resultado
    	.
    	.
    	.
    	ELSE retorno/resultado
    END
    
    -- Exemplo:
    SELECT coluna,
        CASE
            WHEN coluna = alguma_coisa THEN retorno
            WHEN coluna = outra_coisa THEN retorno
            .
            .
            .
            ELSE retorno
        END
    FROM Tabela;
    ```


  - **Forma "simplificada" (avalia primeiro uma expressão e depois a compara com cada valor da cláusula `WHEN`):**

    ```postgresql
    CASE expressão
    	WHEN valor_1 THEN retorno/resultado
    	WHEN valor_2 THEN retorno/resultado
    	.
    	.
    	.
    	ELSE retorno/resultado
    END
    
    -- Exemplo:
    SELECT coluna,
        CASE coluna
            WHEN coluna = alguma_coisa THEN retorno
            WHEN coluna = outra_coisa THEN retorno
            .
            .
            .
            ELSE retorno
        END
    FROM Tabela;
    ```


- **Existem situações que ambas as formas, nos levam ao mesmo resultado;**

- **Podemos colocar as condições entre parênteses;**

- **Podemos passar um `alias` para a coluna que será gerada, após o `END`:**

  ```postgresql
  END AS nome_da_coluna
  ```

  

#### COALESCE()

- **Aceita um número ilimitado de argumentos e retorna o primeiro argumento que não apresenta o valor `null` (o primeiro argumento não nulo);**

- **Se todos os argumentos apresentam o valor `null`, o próprio valor `null` é retornado**

- ```postgresql
  COALESCE(argumento_1, argumento_2, argumento_3, ..., argumento_N)
  ```

  -  **Avalia os argumentos da esquerda para direita, até encontrar um argumento não nulo. Quando esse argumento não nulo é encontrado, os demais argumentos da lista de argumentos, não são mais avaliados;**

  - **Normalmente, os argumentos passados são as colunas;**

  - **Essa função se torna útil quando estamos consultando uma tabela que possui valores nulos e queremos substituir esses valores;**

    - **Substituindo um valor nulo e aplicando um novo valor:**

      ```postgresql
      COALESCE(valor_nulo, novo_valor)
      
      -- Se o valor não for nulo, ele próprio seŕa retornado;
      ```

      


#### CAST

- **Função/Operador de conversão, que permite a conversão de um tipo em outro;**

  - **OBS.: Nem todas as instâncias de um tipo de dado, poderão ser convertidas em qualquer outro tipo de dado;**

- **Existem dus formas de conversão:**

  - **Podemos utilizar a função. Ex.:**

    ```postgresql
    -- Realizando a conversão de um texto para número
    
    SELECT CAST('2' AS INTEGER)
    ```

  - **Ou podemos utilizar o operador (exclusivo do PostgreSQL):**

    ```postgresql
    -- Realizando a conversão de um texto para número
    
    SELECT '5'::INTEGER
    
    SELECT coluna::TIPO_DE_DADO FROM Tabela;
    ```
    
    

#### NULLIF()

- **Função que recebe dois argumentos e, se os argumentos forem iguais, retorna `null` ou, se forem diferentes, retorna o primeiro argumento que foi passado:**

  ```postgresql
  NULLIF(argumento_1, argumento_2)
  
  -- argumento_1 == argumento_2: retorna NULL
  -- argumento_1 != argumento_2: retorna argumento_1
  ```
  
- **O uso dessa função se torna útil para casos onde o valor `null` causaria um erro ou resultado indesejado;**
  - **OBS.: ao realizar uma divisão de um valor númerico por zero, obteremos um erro e, ao realizar essa mesma divisão, só que agora por `null`, o próprio valor `null` é retornado;**



#### VIEWS

- **Objeto de banco de dados que consiste em uma consulta armazenada e pode ser acessada como uma tabela virtual (ela não armazena dados fisicamente, ela simplesmente armazena a consulta);**

- **Uma `VIEW` é basicamente, uma `query` nomeada (uma nova forma de apresentar os dados de uma tabela). Ao invés de sempre termos de "escrever manualmente" a mesma `query`, que por muitas vezes, são complexas, podemos cirar uma `VIEW`, que nos permite a realização de uma `query`, a partir de uma chamada simples;**

  - **Isso é útil para consultas que realizamos repetidamente;**

- **Sintaxe:**

  ```postgresql
  CREATE VIEW nome_da_view AS consulta;
  ```

- **Exemplo:**

  ```postgresql
  -- Criando a VIEW
  CREATE VIEW nome_da_view AS
  SELECT coluna_1, coluna_2, coluna_3 
  FROM Tabela_A
  JOIN Tabela_B
  ON Tabela_A.id = Tabela_B.id;
  
  -- Utilizando a VIEW
  SELECT * FROM nome_da_view;
  ```

  **Se a `VIEW` for criada com sucesso, veremos as mensagems: `CREATE VIEW` e `Query returned successfully...`;**

- **Se precisarmos alterar a `VIEW`, utilizamos a clásula `CREATE OR REPLACE`:**

  ```postgresql
  CREATE OR REPLACE VIEW nome_da_view AS
  SELECT coluna_1, coluna_2, coluna_3, coluna_4 
  FROM Tabela_A
  JOIN Tabela_B
  ON Tabela_A.id = Tabela_B.id;
  ```

- **Renomeando uma `VIEW`:**

  ```postgresql
  ALTER VIEW nome_da_view RENAME TO novo_nome;
  ```

- **Para remover/deletar uma `VIEW`, utilizamos a cláusula `DROP VIEW`:**

  ```postgresql
  DROP VIEW nome_da_view;
  
  -- Removendo uma VIEW com a cláusula IF EXISTS (assim evitamos erros)
  DROP VIEW IF EXISTS nome_da_view;
  ```

- **Para saber mais:**

  **https://www.postgresql.org/docs/current/sql-createview.html**



#### Import e Export

- **Nem todos os arquivos externos, poderão ser importados;**

- **Se o arquivo não for compatível, devemos editá-lo/alterá-lo, a fim de corrigir problemas como por exemplo, o alinhamento corretor dos dados;**

- **Para saber mais (sobre os arquivos compatíveis):**

  **https://www.postgresql.org/docs/current/sql-copy.html**

- **Outra observação a ser feita é que devemos fornecer o caminho correto para o arquivo, ou então o comando `import` falhará;**
- **Além disso, o comando `import` não cria uma tabela. A importação dos dados funcionará sobre a suposição de que a tabela já está criada;**
- **A tabela criada deve apresentar correspondência com a estrutura do arquivo dos dados a serem importados;**
- **Agora, para realizar a importação/exportação, acessamos a tabela (`Database` > `Schemas` > `public` > `Tables` > `Tabela`), com o botão direito do mouse, clicamos na opção `Import/Export Data...`. Uma espécie de modal seŕá aberto. Lá poderemos realizar a importação/exportação (podemos ainda, ver as colunas que estão sendo importadas/exportadas e decidir quais serão utilizadas no processos);**
  - **Na importação/exportação, devemos informar que a tabela possui um cabeçalho (`Header=Yes`) e informar o delimitador;**
  - **Em caso de erros na realização da importação, o mais provável é que se tenha fornecido o caminho para o arquivo, de forma errada;**
