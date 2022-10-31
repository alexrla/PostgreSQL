# PostgreSQL



### Comandos Avançados



#### TIMESTAMPS e EXTRACT

- **Tipos de data e hora (tipos de dados):**

  - **`TIME`: apresenta o "tempo" (hora, minutos e segundos);**
  - **`DATE`: apresenta a data (com mês, dia e ano);**
  - **`TIMESTAMP`: combina "tempo" e data;**
  - **`TIMESTAMPTZ`: combina "tempo" e data (apresenta o fuso horário);**

- **Funções e operações relacionadas aos tipos de dados que correspodem a data e hora:**

  - **`TIMEZONE`**
  - **`NOW()`: retorna data e hora, atuais/correntes;**
  - **`TIMEOFDAY()`: retorna data e hora atuais (no formato de texto);**
  - **`CURRENT_TIME`: retorna a hora atual com informações do fuso horário;**
  - **`CURRENT_DATE`: retorna a data atual;**

- **Funções utilizadas para extrairmos informações dos tipos de dados baseados em data e hora:**

  - **`EXTRACT()`: utilizada para recuperarmos "subcampos" de uma data/hora, como por exemplo, o ano, a hora, etc.:**

    ```postgresql
    EXTRACT(YEAR FROM data/coluna_de_datas)
    
    -- Com alias
    EXTRACT(YEAR FROM coluna_de_datas) AS ano
    ```

    - **Para saber mais:**

      **https://www.postgresql.org/docs/current/functions-datetime.html**

  - **`AGE()`: retorna o número de anos, meses e dias, de diferença, entre duas datas:**

    ```postgresql
    AGE(data_1, data_2)
    
    --  Se as datas contiverem informações de hora, a diferença 
    -- entre elas também retornadas
    ```

  - **`TO_CHAR()`: converte números, datas e horas, para um valor de texto**:

    ```postgresql
    TO_CHAR(data, 'mm-dd-yyyy')
    
    -- 'mm-dd-yyyy' corresponde a formatação
    ```

    - **Para saber mais:**

      **https://www.postgresql.org/docs/current/functions-formatting.html**


- **Função `EXTRACT()`**
  - **"Subcampos" que podem ser extraídos (existem mais, além dos que são exibidos abaixo):**
    - **YEAR (ano)**
    - **MONTH (mês)**
    - **DAY (dia)**
    - **WEEK (semana)**
    - **QUARTER (trimestre do ano)**

- **OBS.: se gravarmos informações de data/hora e mais tarde, também precisarmos gravar informações relacionadas ao fuso horário, não poderemos adicioná-las, já que nunca a gravamos;**



#### Funções e operadores matemáticos

- **Operadores matemáticos:**

  ```postgresql
  -- Adição
  SELECT valor_1 + valor_2 AS adição;
  
  -- Subtração
  SELECT valor_1 - valor_2 AS subtração;
  
  -- Divisão (retorna o resultado inteiro)
  SELECT valor_1 / valor_2 AS divisão;
  
  -- Multiplicação
  SELECT valor_1 * valor_2 AS multiplicação;
  
  -- Resto/Módulo (resto da divisão)
  SELECT valor_1 % valor_2 AS módulo;
  
  --  Ao realizar operações onde um dos operandos é nulo (null), o
  -- resultado da operação será nulo (null)
  ```

- **Para saber mais:**

  **https://www.postgresql.org/docs/9.5/functions-math.html**



#### Funções e operadores de string

- **Funções que podem ser aplicadas em colunas definidas com o tipo de dado de texto (`VARCHAR`, `STRING`, `CHAR`);**

  - **Funções de `string`**

    - **`UPPER`: converte `strings` e expressões/valores textuais de colunas, para letras maiúsculas:**

      ```postgresql
      SELECT UPPER('string');
      
      SELECT UPPER(coluna) FROM Tabela;
      ```

    - **`LOWER`: converte `strings` e expressões/valores textuais de colunas, para letras minúsculas:**

      ```postgresql
      SELECT LOWER('string');
      
      SELECT LOWER(coluna) FROM Tabela;
      ```

    - **`INITCAP`: aplica a capitalização nas expressões/valores textuais (a primeira letra de cada palavra fica em maiúscula e as demais em minúscula):**

      ```postgresql
      SELECT INITCAP('string');
      
      SELECT INITCAP(coluna) FROM Tabela;
      ```

    - **`LEFT`: extrai um número `n` de caracteres de uma determinada `string`:**

      ```postgresql
      SELECT LEFT('string', n)
      
      SELECT LEFT(coluna, n) FROM Tabela;
      
      --  n: um número inteiro;
      --  n: especifica o número de caracteres à esquerda, que devem ser extraídos;
      --  n: se for negativo, a extração se da a partir da "direita para à esquerda" (os 
      -- 'n' caracteres finais, são removidos);
      
      -- Exemplos
      SELECT LEFT('ABC', 1); -- Resultado: A
      SELECT LEFT('ABC', 2); -- Resultado: AB
      SELECT LEFT('ABC', 3); -- Resultado: ABC
      SELECT LEFT('ABC', -2); -- Resultado: A
      ```

    - **`RIGHT`: extrai um número `n` de caracteres de uma determinada `string`:**

      ```postgresql
      SELECT RIGHT('string', n)
      
      SELECT RIGHT(coluna, n) FROM Tabela;
      
      --  n: um número inteiro;
      --  n: especifica o número de caracteres à direita, que devem ser extraídos;
      --  n: se for negativo, a extração se da a partir da "esquerda para à direita" (os 
      -- 'n' caracteres iniciais, são removidos);
      
      -- Exemplos
      SELECT RIGHT('XYZ', 1); -- Resultado: Z
      SELECT RIGHT('XYZ', 2); -- Resultado: YZ
      SELECT RIGHT('XYZ', 3); -- Resultado: XYZ
      SELECT RIGHT('XYZ', -1); -- Resultado: YZ
      ```

    - **`REVERSE`: inverte a ordem da string:**

      ```postgresql
      SELECT REVERSE('string');
      
      SELECT REVERSE(coluna) FROM Tabela;
      ```

    - **`SUBSTRING`: retorna uma parte de uma `string`:**

      ```postgresql
      SELECT SUBSTRING('string', posição_inicial, n);
      
      SELECT SUBSTRING(coluna, posição_inicial, n) FROM Tabela;
      
      --  posição_inicial: valor inteiro, que indica a posição da string, 
      -- onde irá se iniciar a extração;
      --  n: valor inteiro que determina o número de caracteres a serem 
      -- extraídos (contando com o caractere da posição inicial);
      ```

      **- Se: `posição_inicial + n > string`, será retornada toda `string`, a partir da posição_inicial;**

      **- Se: `posição_inicial = 0`, a extração começará a partir do primeiro caractere da `string`;**

      **- O parâmetro `posição_inicial` é opcional (se ele não for informado, a extração se inicia no primeiro caractere);**

      **- O parâmetro `n` também é opcional (se ele for omitido, será retornada toda a `string` a partir da posição_inicial);**

      **- A posição_inicial não pode ser um valor negativo;**

      **- OBS.: o primeiro caractere tem índice 1;**

    - **`REPLACE`: utilizada para pesquisar e substituir todas as ocorrências de uma `string`, por outra (uma nova):**

      ```postgresql
      SELECT REPLACE(target, 'string', 'nova_string');
      
      --  target: string na qual desejamos realizar uma substituição;
      --  'string': texto que desejamos pesquisar e substituir no target 
      -- (caso existam várias ocorrências, todas serão substituídas);
      --  'nova_string': texto que substituirá o antigo;
      
      -- Outros exemplos:
      SELECT REPLACE(coluna, 'string', 'nova_string') FROM Tabela;
      
      UPDATE Tabela 
      SET coluna = REPLACE(coluna, 'string', 'nova_string')
      WHERE coluna = valor ;
      ```

    - **`SPLIT_PART`: divide uma `string`, com base em um delimitador:**

      ```postgresql
      SELECT SPLIT_PART('string', 'delimitador', n);
      
      --  'string': texto a ser dividido;
      --  'delimitador': utilizado na divisão da string;
      --  n: índice do campo a ser retornado (a string é dividida em diferentes 
      -- campos/partes e o 'n' indica o campo/parte a ser retornada);
      
      -- Outro exemplo:
      SELECT SPLIT_PART(coluna, 'delimitador', n) FROM Tabela;
      
      -- Recuperando campos de uma data (precisamos convertê-la para um tipo de dado de de texto, para )
      SELECT SPLIT_PART(data::TEXT, '-', 1) FROM TABELA;
      ```
      
      **- Se `n` for maior que o número de campos (resultado da divisão da `string`), será retornado uma `string` vazia;**

- **Para saber mais:**

  **https://www.postgresql.org/docs/9.1/functions-string.html**



#### Subqueries (Subconsultas)

- **Subqueries permitem a construção de consultas complexas, essencialmente executando uma consulta, nos resultados de outra consulta, ou somente usando os resultados de outra consulta;**

- **As `subqueries` às vezes são chamadas de consultas aninhadas (já que permite a criação de consultas aninhadas: teremos um `SELECT` dentro de outro `SELECT`);**

- **Exemplo: temos uma tabela contendo as colunas de alunos e de notas. Se quisermos retornar os alunos e as notas, faríamos:**

  ```postgresql
  SELECT alunos, notas
  FROM provas;
  ```

- **Agora, se quisermos retornar a média dessas notas, poderíamos fazer:**

  ```postgresql
  SELECT AVG(notas)
  FROM provas;
  ```

- **E se por um acaso, quiséssemos saber os alunos que obtiveram notas superiores a média da turma? Diante dessa situação, poderíamos realizar uma subquery, a fim de resgatar a média da turma e depois verificaríamos os alunos com notas acima da média da turma:**

  ```postgresql
  SELECT alunos, notas
  FROM provas
  WHERE notas > (SELECT AVG(notas) FROM provas);
  
  -- Primeiro é realizado a operação entre parênteses
  ```

- **O operador `EXISTS`, é utilizado para verificar a existência de linhas em alguma subconsulta;**

  - **Normalmente, subconsultas são passadas em uma função `EXISTS()`, para verificar se alguma linha é retornada ou não (retorna true/false):**

    ```postgresql
    SELECT nome_da_coluna
    FROM nome_da_tabela
    WHERE EXISTS(
    	SELECT nome_da_coluna
        FROM nome_da_tabela
        WHERE condição
    );
    ```

- **OBSs.:**

  - **As subqueries são realizadas primeiro, desde que estejam entre parênteses;**

  - **Utilizando o operador `IN`, também estamos criando subconsultas;**

  - **Se a subconsulta retorna `null`, o resultado retornado pela função `EXISTS`, será `true`;**

  - **Para verificar a não existência de linhas, aplicamos o operador `NOT`:**

    ```postgresql
    NOT EXISTS (subquery)
    ```



##### Subqueries Correlated (subconsultas correlacionadas)

- **São `subqueries` dependentes (a consulta interna depende da consulta externa, para ser executada);**

- **A consulta interna referencia uma tabela da consulta externa:**

  ```postgresql
  SELECT T1.coluna_1, T1.coluna_2, T1.coluna_3
  FROM Tabela_1 T1
  WHERE T1.coluna_3 = (
  	SELECT MIN(coluna_3) FROM Tabela_2 T2
      WHERE T2.coluna_4 = T1.coluna_4
  );
  
  -- Vejamos que não podemos executar a subquery de forma isolada
  SELECT MIN(coluna_3) FROM Tabela_2 T2
  WHERE T2.coluna_4 = T1.coluna_4;
  ```

  

##### Subqueries Uncorrelated (subconsultas não-correlacionadas)

- **São `subqueries` independentes (a consulta interna que não depende da consulta externa, a principal/a maior, para ser executada);**

- **Os resultados de uma subconsulta não correlacionada, são retornados e utilizados pela consulta externa, uma vez:**

  ```postgresql
  SELECT coluna_1, coluna_2 FROM Tabela_1 
  WHERE coluna_1 > (SELECT AVG(coluna_2) FROM Tabela_1);
  
  --  Subconsultas não-correlacionadas, podem ser executadas isoladamente:
  SELECT AVG(coluna_2) FROM Tabela_1;
  ```

- **Para passarmos vários valores para a consulta externa, utilizamos o operador `IN`:**

  ```postgresql
  SELECT coluna_1 FROM Tabela_1
  WHERE coluna_2 IN
  (
      SELECT coluna_3 FROM Tabela_2
      WHERE coluna_4 > coluna_5
  );
  ```



#### Self-Join (Auto-Junção/Junção Automática)

- **Consulta na qual uma tabela é unida a si mesma;**

- **São úteis para comparar valores entre colunas da mesma tabela;**

- **Não há uma palavra-chave para podermos definir auto-junções, elas são semelhantes as junções padrões, possuem a mesma sintaxe. Porém, é necessário o uso de um `alias` para a tabela, pois caso contrário, os nomes das tabelas seriam ambíguos em operações de `self-join`**:

  ```postgresql
  SELECT Tabela_A.coluna, Tabela_B.coluna
  FROM Tabela AS Tabela_A
  JOIN Tabela AS Tabela_B
  ON Tabela_A.alguma_coluna = Tabela_B.outra_coluna;
  ```





