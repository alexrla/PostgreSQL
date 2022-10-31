# PostgreSQL



### Fundamentos

#### SELECT

- **Instrução que nos possibilita a recuperação de informações de uma tabela existente em um banco de dados;**

- **Sintaxe:**

  ```postgresql
  SELECT nome_da_coluna FROM nome_da_tabela;
  ```

- **Selecionando mais de uma coluna:**

  ```postgresql
  SELECT coluna1, coluna2, ..., coluna_n FROM nome_da_tabela;
  ```

  **As colunas, na consulta, não precisam ser colocadas na ordem em que aparecem na tabela;**

- **Selecionando todas as colunas:**

  ```postgresql
  SELECT * FROM nome_da_tabela;
  ```

- **OBSs.:**

  - **A linguagem SQL não é uma linguagem _case-sensitive_, mas por padrão, as instruções acabam sendo escritas com letras maiúsculas;**
  - **E o uso do ponto e vírgula, indica o final da consulta (o final da query);**



#### DISTINCT

- **Utilizado quando temos valores duplicados em uma tabela e queremos apenas listar os valores únicos/distintos, dessa tabela;**

  - **Pode ser utilizado com várias colunas;**

- **Sintaxe:**

  ```postgresql
  SELECT DISTINCT nome_da_coluna FROM nome_da_tabela;
  ```

- **Para maior clareza, podemos declarar o nome da coluna entre parênteses:**

  ```postgresql
  SELECT DISTINCT(nome_da_coluna) FROM nome_da_tabela;
  ```



#### COUNT

- **Retorna a quantidade de linhas (seja de toda a tabela/de uma determinada coluna, que no final  deve acabar dando no mesmo resultado);**

- **Sintaxe:**

  ```postgresql
  SELECT COUNT(nome_da_coluna) FROM nome_da_tabela;
  ```

  **O nome da coluna tem de estar entre parênteses;**

- **Retornando a quantidade de linhas de toda a tabela:**

  ```postgresql
  SELECT COUNT(*) FROM nome_da_tabela;
  ```

- **Combinando instruções (verificando quantos resultados distintos, existem em uma coluna):**

  ```postgresql
  SELECT COUNT(DISTINCT nome_da_coluna) FROM nome_da_tabela;
  ```



#### WHERE

- **Utilizada para realizarmos consultas, baseadas em uma ou mais, condições;**

- **Sintaxe:**

  ```postgresql
  SELECT nome da coluna
  FROM nome_da_tabela
  WHERE condição;
  ```

- **Operadores que podemos utilizar na construção de condições:**

  - **Operadores de comparação:**

    - **`=` (igual a)**

    - **`<> ou !=` (diferente de)**

    - **`>` (maior que)**

    - **`<` (menor que)**

    - **`>=` (maior ou igual a)**

    - **`<=` (menor ou igual a)**

      **Os operadores de comparação (`>`, `<`, `>=` e `<=`) podem ser utilizados com textos e datas (que também podemos as considerar como texto, já que as passamos entre apóstrofos/"aspas simples");**

      **Com textos, a comparação é feita utilizando-se a ordem alfabética;**

  - **Opeadores lógicos:**

    - **`AND` (E lógico)**

    - **`OR` (OU lógico)**

    - **`NOT` (NÂO lógico)**

      **Através dos operadores lógicos, podemos combinar várias comparações em uma única condição;**

- **OBS.:**

  ```postgresql
  -- Texto
  WHERE nome='Alex'
  
  -- Número
  WHERE id=2
  ```



#### ORDER BY

- **Ajuda a exibir os resultados da consulta, em uma determinada ordem (crescente ou decrescente);**

  - **Válidos para ordenação numérica e alfabética (incluindo datas);**

- **Sintaxe:**

  ```postgresql
  SELECT nome_da_coluna 
  FROM nome_da_tabela
  ORDER BY nome_da_coluna ASC/DESC;
  ```

  - **Podemos consultar diferentes colunas e fazer a ordenação a partir de uma coluna específica (ou podemos realizar a ordenação a partir de mais de uma coluna);**
  - **ASC (ascending - ascendente):  ordenação feita de forma crescente;**
    - **É o valor padrão (não precisa ser declarado);**
  - **DESC (descending - descendente): ordenação feita na forma decrescente;**

- **OBS_1.: em uma query (consulta), a instrução `ORDER BY`, é uma das últimas a serem executadas (deve vir após o `SELECT` e o `WHERE`, por exemplo);**

- **OBS_2.: podemos especificar um tipo de ordenação para cada coluna:**

  ```postgresql
  SELECT coluna1, coluna2,...
  FROM Tabela
  ORDER BY coluna1 ASC, coluna2 DESC, ...;
  ```

- **OBS_3.: podemos realizar a ordenação (de forma crescente ou decrescente), por colunas que não estamos solicitando na nossa consulta:**

  ```postgresql
  SELECT coluna2,coluna3
  FROM Tabela
  ORDER BY coluna1 ASC, coluna2 DESC;
  ```



#### LIMIT

- **Nos permite limitar o número de linhas retornadas, em uma consulta;**

  - **Última instrução a ser executada (deve vir após o `SELECT`, o `WHERE` e o `ORDER BY`);**

- **Exemplo:**

  ```postgresql
  SELECT nome_da_coluna 
  FROM nome_da_tabela
  ORDER BY nome_da_coluna ASC
  LIMIT número_de_linhas_a_serem_retornadas;
  ```


- **Podemos utilizar ainda, em conjunto com a cláusula `LIMIT`, a cláusula `OFFSET` , na qual passamos um valor, que corresponde a quantidade de linhas que serão puladas/ignoradas, antes de haver o retorno:**

  ```postgresql
  SELECT nome_da_coluna 
  FROM nome_da_tabela
  ORDER BY nome_da_coluna ASC
  LIMIT número_de_linhas_a_serem_retornadas OFFSET número_de_linhas_a_serem_puladas;
  
  --  Retorna o mesmo número de linhas que foi especificado, mas
  -- pula as linhas da sequência (de acordo com o valor passado
  -- no OFFSET) e exibe as próximas;
  ```



#### FETCH

- **A cláusula `LIMIT` não é um comando padrão do SQL (da linguagem SQL). E o PostgreSQL fornece uma forma padrão (que está em conformidade com o padrão SQL) para limitar o número de linhas a serem retornados em uma consulta:**

  ```postgresql
  -- Exemplo
  
  SELECT coluna FROM Tabela
  FETCH FIRST/NEXT número_de_linhas ROW/ROWS ONLY;
  
  --  O número de linhas por padrão é 1, ou seja, se ele não for
  -- especificado, uma linha será retornada;
  ```

- **A cláusula `FETCH` também pode ser utilizada em conjunto com a cláusula `OFFSET`:**

  ```postgresql
  SELECT coluna FROM Tabela
  OFFSET valor ROWS
  FETCH FIRST/NEXT número_de_linhas ROW/ROWS ONLY;
  
  -- Vejamos que a cláusula OFFSET vem antes, da cláusula FETCH;
  ```

  

#### BETWEEN

- **Utilizado para a realização de consultas baseadas em valores que estão contidos em um intervalo de valores (aplicado na condição do `WHERE`);**

  - **Seria o mesmo que:**

    ```postgresql
    -- Operador AND
    valor >= valor_mínimo AND valor <= valor_máximo
    
    -- BETWEEN
    valor BETWEEN valor_mínimo AND valor_máximo
    ```

  - **Também podemos combiná-lo com o operador `NOT`:**

    ```postgresql
    valor NOT BETWEEN valor_mínimo AND valor_máximo
    ```

  - **O que acaba sendo o mesmo que:**

    ```postgresql
    valor < valor_mínimo OR valor > valor_máximo
    ```

  - **Além disso, ele também pode ser utilizado com datas (nesse caso, devemos formatar as datas para o padrão ISO 8601):**

    ```postgresql
    data BETWEEN '1900-01-01' AND '2100-12-31'
    ```

  
  - **Com textos (strings), por exemplo (dada uma tabela com a coluna nomes):**
  
    ```postgresql
    SELECT * FROM Tabela
    WHERE nomes BETWEEN 'A' AND 'J';
    
    --  Os valores são retornados com base na ordem alfabética;
    --  No exemplo acima, são retornados os nomes que começam com 
    -- as letras entre 'A' e 'J', incluindo o nomes que se iniciam 
    -- pelas próprias letras 'A' e 'J';
    ```
  
    

#### IN

- **Utilizado para a realização de consultas, com base em valores que estão contidos em uma "lista de opções" (o valor da coluna, deve ser igual a um dos valores passados na "lista de opções", para ser retornado);**

  - **Também utilizado , assim como o `BETWEEN`, na condição do `WHERE`;**
  - **Um alternativa as consultas onde queremos os dados onde o valor de uma coluna pode ser igual a mais de um valor (utilizamos o operador `IN`, ao invés do operador  `=`);**

- **Exemplo:**

  ```postgresql
  valor IN (opção_1, opção_2, opção_3, ..., opção_n)
  ```

- **Também podemos combiná-lo com outros operadores:**

  ```postgresql
  valor NOT IN (opção_1, opção_2, opção_3, ..., opção_n)
  ```

- **OBS.:  tanto para o `BETWEEN`, como para o `IN`, o `valor` utilizado nos exemplos, correspondem aos nomes de colunas, que queremos utilizar nas nossas consultas (`queries`);** 

  

#### LIKE e ILIKE

- **Utilizados para encontrar determinados valores textuais, a partir da correpondência de padrões, que nós mesmos estabelecemos;**

- **Para isso, fazemos uso de dois "caracteres coringas especiais":**

  - **`underline/underscore ( _ )`: corresponde a qualquer caractere único;**
  - **`símbolo de porcentagem ( % )`: corresponde a qualquer sequência de caracteres (qualquer quantidade);**

- **Os comandos `LIKE`e `ILIKE`, se diferenciam pelo fato do `LIKE` ser case-sensitive ('A' é diferente de 'a') e o `ILIKE` não;**

- **Exemplos:**

  ```postgresql
  -- Todos os nomes que se iniciam com a letra 'A'
  WHERE nome LIKE 'A%'
  
  -- Todos os nomes que terminam com a letra 'a'
  WHERE nome LIKE '%a'
  
  --  Buscando pelo filme 'Missão Impossível', sem nos preocuparmos
  -- com o número da sequência
  WHERE título LIKE 'Missão Impossível _'
  
  -- Podemos utilizar múltiplos underlines
  WHERE valor LIKE 'Versão __'
  
  -- Podemos combinar os caracteres coringas
  WHERE nome LIKE '_le%'
  
  -- Exemplos:
  --         Alex
  --         Alexandre
  --         Alexsandro
  --         Alexsander
  ```

- **Podemos utilizar os dois operadores, em conjuntos com outros operadores. Por exemplo:**

  ```postgresql
  NOT LIKE
  NOT ILIKE
  
  LIKE AND LIKE
  ILIKE AND ILIKE
  LIKE AND NOT LIKE
  ILIKE AND NOT ILIKE
  
  LIKE OR LIKE
  ILIKE OR ILIKE
  LIKE OR NOT LIKE
  ILIKE OR NOT ILIKE
  ```

- **E ainda, podemos colocar uma expressão entre os símbolos de porcentagem:**

  ```postgresql
  WHERE nome LIKE/ILIKE '%le%'
  
  --  Assim podemos pegar qualquer nome que tenha a expressão 'le', seja
  -- no ínico, no meio ou no final do nome (em qualquer posição)
  ```




#### IS NULL e IS NOT NULL

- **O `NULL` significa informação ausente ou não aplicável;**

  - **O `NULL` não é um valor. Logo, não podemos compará-lo como nenhum outro valor, de nenhum tipo de dado;**
  - **Além disso, `NULL != NULL`. Essa expressão retorna `NULL`;**

- **Para retornarmos as colunas, onde os valores não são nulos, fazemos:**

  ```postgresql
  SELECT * FROM Tabela
  WHERE coluna IS NOT NULL;
  ```

- **Do contrário, para verificarmos onde temos informações ausentes, fazemos:**

  ```postgresql
  SELECT * FROM Tabela
  WHERE coluna IS NULL;
  ```




#### Concatenação

- **Pode ser utilizada com textos (strings) ou colunas;**

- **Para concantenar strings, utilizamos o operador de concatenação: `||` (dois pipes):**

  ```postgresql
  'string1' || 'string_2'  || ... || 'string_n'
  ```

- **Para "concatenar colunas" (concatenar os valores das colunas), utilizamos a função `CONCAT`:**

  ```postgresql
  SELECT CONCAT(coluna_1, coluna_2) AS novo_nome
  FROM Tabela;
  ```

- **Outro exemplo (concatenar os valores das colunas de nome e sobrenome, de uma tabela de usuários):**

  ```postgresql
  SELECT CONCAT(nome, ' ', sobrenome) AS nome_completo
  FROM usuarios;
  ```

- **Também temos a função `CONCAT_WS`, que também é utilizada para a operação de concatenação. A diferença dela para a função `CONCAT`, é que nela o "primeiro parâmetro", deve ser um separador e em seguida informamos as colunas:**

  ```postgresql
  SELECT CONCAT_WS(separador, coluna_1, coluna_2, ..., coluna_N) AS nome
  FROM Tabela;
  
  -- WS (With Separator);
  -- O separador é uma string;
  ```

  
