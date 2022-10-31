# PostgreSQL



### JOINS



#### AS

- **Cláusula que possibilita a criação de um `alias` para uma coluna (um nome alternativo para a coluna, uma forma de renomear a coluna);**

- **Exemplos:**

  ```postgresql
  SELECT nome_da_coluna AS novo_nome_da_coluna FROM nome_da_tabela;
  
  -- Usando com funções agregadoras
  SELECT FUNÇÂO_AGREGADORA(coluna) AS novo_nome FROM tabela;
  ```

  - **A cláusula `AS`, só pode ser utilizadas na instrução `SELECT`;**
  - **OBS_1.: não podemos utilizar o "novo nome" da coluna, em outras cláusulas como o `WHERE` e o `HAVING`, isso porque aquele nome só irá se referir a coluna na saída dos dados e não na consulta (na `query`):**
  
    ```postgresql
    -- ERROR
    SELECT coluna AS novo_nome FROM Tabela
    WHERE novo_nome = valor;
    ```
  - **OBS_2.: agora, podemos usar esse "novo nome", na cláusula `ORDER BY`:**
  
    ```postgresql
    SELECT coluna AS novo_nome FROM Tabela
    WHERE coluna = valor
    ORDER BY novo_nome;
    ```
  
    

#### JOIN

- **Operações de JOIN consiste basicamente na combinação de várias tabelas, para a consulta de dados;**
- **E essa consulta só é possível, devido a colunas de dados, que apresentam algum tipo de relação, entre as tabelas (essas colunas geralmente são as colunas que representam a chave estrangeira em uma tabela, que faz referência a chave primária de outra tabela);**
- **O principal motivo pela existência de diferentes operações de JOIN, é decidir como lidar com as informações presentes em uma tabela, que será combinada com uma outra tabela;**



#### Tipos de operação:

##### INNER JOIN

- **Utilizada quando procuramos por registros que aparecem em duas ou mais tabelas;**

- **Tipo mais comum;**

- **Basicamente, procura por correspondências (chaves primárias e estrangeiras) em duas ou mais tabelas:**

  ```postgresql
  SELECT * FROM Tabela_A
  INNER JOIN Tabela_B
  ON Tabela_A.coluna_de_correspondência=Tabela_B.coluna_de_correspondência;
  
  -- Outro exemplo
  SELECT Tabela_1.coluna_1, Tabela_1.coluna_2, Tabela_2.coluna_1
  FROM Tabela_1
  INNER JOIN Tabela_2 ON Tabela_1.coluna_3 = Tabela_2.coluna_3;
  ```

- **As "colunas" utilizadas (no `ON`), devem possuir dados  que se correspondem (se equivalem);**

- **Apenas os registros com valores correspondentes em ambas as tabelas, são retornados;**

- **Não importa a ordem em que as tabelas aparecem na consulta, o resultado de uma operação de `INNER JOIN` acaba sendo o mesmo:**

  ```postgresql
  SELECT * FROM Tabela_A
  INNER JOIN Tabela_B
  ON Tabela_A.coluna=Tabela_B.coluna;
  
  -- OU (tanto faz)
  
  SELECT * FROM Tabela_B
  INNER JOIN Tabela_A
  ON Tabela_B.coluna=Tabela_A.coluna;
  ```

- **Nos exemplos dados acima, todos os dados de ambas as tabelas acabam sendo retornados, mas podemos alterar isso, especificando as colunas que queremos que sejam retornadas:**

  ```postgresql
  SELECT coluna_1, coluna_2, ..., coluna_n
  FROM Tabela_A
  INNER JOIN Tabela_B
  ON Tabela_A.coluna=Tabela_B.coluna;
  ```

- **No caso de estarmos especificado as colunas que serão retornadas: se uma das colunas possui o mesmo nome, em ambas as tabelas (ou em pleo menos duas), devemos seleciona uma das tabela e especificar a coluna. **

  **Por exemplo:`Tabela_A.coluna`;**

- **Do contrário, se aquela coluna (o nome dela), existir apenas em uma das tabela, podemos passamos apenas a coluna: `nome_da_coluna` (poderíamos passar a tabela seguida da coluna, que o resultado seria o mesmo, mas dessa forma acaba sendo mais simples) ;**

- **Também podemos colocar apenas o `JOIN`, sem o `INNER` (o PostgreSQL acaba tratando/reconhece esses casos, como um `INNER JOIN`):**

  ```postgresql
  SELECT * FROM Tabela_A
  INNER JOIN Tabela_B
  ON Tabela_A.coluna=Tabela_B.coluna;
  
  -- O mesmo que
  
  SELECT * FROM Tabela_A
  JOIN Tabela_B
  ON Tabela_A.coluna=Tabela_B.coluna;
  ```

- **Escrevendo  consultas de forma mais curta:**

  ```postgresql
  SELECT T1.coluna_1, T1.coluna_2, T2.coluna_1
  FROM Tabela_1 T1
  JOIN Tabela_2 T2 ON T1.coluna_3 = T2.coluna_3;
  
  -- Podemos aplicar outras cláusulas na consulta
  SELECT T1.coluna_1, T1.coluna_2, T2.coluna_1
  FROM Tabela_1 T1
  JOIN Tabela_2 T2 ON T1.coluna_3 = T2.coluna_3
  WHERE condição
  ORDER BY coluna;
  ```

- **Realizando consultas em mais de duas tabelas:**

  ```postgresql
  -- Exemplo 1
  SELECT T1.coluna, T2.coluna, T3.coluna
  FROM Tabela_1 T1
  JOIN Tabela_2 T2 ON T1.coluna = T2.coluna
  JOIN Tabela_3 T3 ON T3.coluna = T2.coluna;
  
  -- Exemplo 2
  SELECT T1.coluna, T2.coluna, T3.coluna, T4.coluna, T5.coluna
  FROM Tabela_1 T1
  JOIN Tabela_2 T2 ON T1.coluna = T2.coluna
  JOIN Tabela_3 T3 ON T3.coluna = T2.coluna
  JOIN Tabela_4 T4 ON T4.coluna = T3.coluna
  JOIN Tabela_5 T5 ON T5.coluna = T2.coluna;
  ```

  

  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/SQL_Join_-_07_A_Inner_Join_B.svg/2560px-SQL_Join_-_07_A_Inner_Join_B.svg.png" alt="img" style="zoom:15%;" />



##### OUTER JOINS

- **Nos permite especificar como lidar com valores que estão presentes em apenas uma das tabelas, que estão sendo "unidas";**
- **Tipos de `OUTER JOINS`:**
  - **`FULL OUTER JOIN`**
  - **`LEFT OUTER JOIN`**
  - **`RIGHT OUTER JOIN`**



###### FULL OUTER JOIN

- **Utilizada para pegar todos os registros correspondentes, de todas as tabelas envolvidas na operação (claro, podemos especificar as colunas que serão retornadas):**

  ```postgresql
  SELECT * FROM Tabela_A
  FULL OUTER JOIN Tabela_B
  ON Tabela_A.coluna_de_correspondência=Tabela_B.coluna_de_correspondência;
  
  -- Outro exemplo
  SELECT T1.coluna_1, T1.coluna_2, T2.coluna_1
  FROM Tabela_1 T1
  FULL JOIN Tabela_2 T2 ON T1.coluna_3 = T2.coluna_3;
  ```

  

  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/SQL_Join_-_05b_A_Full_Join_B.svg/640px-SQL_Join_-_05b_A_Full_Join_B.svg.png" alt="img" style="zoom:50%;" />

  

- **Assim como ocorre na operação de `INNER JOIN`, a ordem em que as tabelas estão dispostas na consulta, não faz diferença;**

- **Como a operação de `FULL OUTER JOIN`, acaba retornando todos os registros, os campos dos registros que aparecem em uma tabela, mas não aparecem na outra, também são retornardos, mas acabam sendo preenchidos com o valor `null`;**

- **Agora, quanto trabalhamos com operações de `FULL OUTER JOIN`, podemos aplicar a cláusula `WHERE` e assim podemos obter "registros exclusivos" das tabelas, ou seja, registros que não aparecem em ambas as tabelas ao mesmo tempo:**

  ```postgresql
  SELECT * FROM Tabela_A
  FULL OUTER JOIN Tabela_B
  ON Tabela_A.coluna_de_correspondência=Tabela_B.coluna_de_correspondência
  WHERE Tabela_A.campo_vazio IS NULL OR Tabela_B.campo_vazio IS NULL;
  
  -- Mais um vez, não importa a ordem em que as tabelas são dispostas na consulta
  ```

  

  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ac/SQL_Join_-_06_A_Full_Join_B_Where_A.key_%3D_null_Or_B.key_%3D_null.svg/2560px-SQL_Join_-_06_A_Full_Join_B_Where_A.key_%3D_null_Or_B.key_%3D_null.svg.png" alt="img" style="zoom:12%;" />

  

  **\*campo_vazio: alguma coluna onde o campo tenha sido preenchido como valor `null`;**



###### LEFT OUTER JOIN

- **Retorna todos os registros que estão na tabela à esquerda:**

  ```postgresql
  SELECT * FROM Tabela_A
  LEFT OUTER JOIN Tabela_B
  ON Tabela_A.coluna_de_correspondência=Tabela_B.coluna_de_correspondência;
  
  -- Outro exemplo
  SELECT T1.coluna_1, T1.coluna_2, T2.coluna_1
  FROM Tabela_1 T1
  LEFT JOIN Tabela_2 T2 ON T1.coluna_3 = T2.coluna_3;
  ```

  

  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f6/SQL_Join_-_01_A_Left_Join_B.svg/1200px-SQL_Join_-_01_A_Left_Join_B.svg.png" alt="img" style="zoom:25%;" />

  

- **No exemplo acima, estamos pegando todos os registros da _Tabela_A_, buscando uma correspondência entre "algo" da _Tabela_A_, com "algo" da _Tabela_B_ ;**

- **Se esse "algo" for encontrado somente na _Tabela_B_, não será retornado;**

- **No caso de não haver correspondência entre a _Tabela_A_ e a _Tabela_B_, os campos dos registros relacionados a _Tabela_B_, serão retornados como nulos (`null`);**

- **E dessa vez, a ordem das tabelas importam ... devemos especificar qual é a "tabela à esquerda" (a tabela à esquerda, deve ser a primeira a ser mencionada/declarada na consulta);**

- **Podemos utilizar tanto a `LEFT OUTER JOIN`, como `LEFT JOIN` (forma encurtada), que o `PostgreSQL` acaba "interpretando" como a mesma coisa (a mesma operação);**

- **Aplicando a cláusula `WHERE`, quando trabalhamos com operações de `LEFT OUTER JOIN`, podemos retornar registros exclusivos da "tabela à esquerda" (registros encontrados apenas na tabela à esquerda):**

  ```postgresql
  SELECT * FROM Tabela_A
  LEFT OUTER JOIN Tabela_B
  ON Tabela_A.coluna_de_correspondência=Tabela_B.coluna_de_correspondência
  WHERE Tabela_B.campo_vazio IS NULL;
  ```

  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a1/SQL_Join_-_02_A_Left_Join_B_Where_B.key_%3D_null.svg/2560px-SQL_Join_-_02_A_Left_Join_B_Where_B.key_%3D_null.svg.png" alt="img" style="zoom:12%;" />



###### RIGHT OUTER JOIN

- **Semelhante ao `LEFT OUTER JOIN`, se diferenciando pelo fato de retornar todos os registros da tabela à direita (a ordem de retorno das tabelas é alterada):**

  ```postgresql
  SELECT * FROM Tabela_A
  RIGHT OUTER JOIN Tabela_B
  ON Tabela_A.coluna_de_correspondência=Tabela_B.coluna_de_correspondência;
  
  -- Outro exemplo
  SELECT T1.coluna_1, T1.coluna_2, T2.coluna_1
  FROM Tabela_1 T1
  RIGHT JOIN Tabela_2 T2 ON T1.coluna_3 = T2.coluna_3;
  ```

  

  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/SQL_Join_-_03_A_Right_Join_B.svg/1200px-SQL_Join_-_03_A_Right_Join_B.svg.png" alt="img" style="zoom:25%;" />

- **A "tabela à direita", deve ser a última tabela a ser especificada/declarada na consulta (a ordem em que as tabelas estão dispostas na consulta, importa);**

- **Podemos utilizar a forma encurtada: `RIGHT JOIN`;**

- **Também podemos aplicar uma cláusula `WHERE`, a fim de pegar registros exclusivos da tabela à direita (resgistros contidos somente na tabela à direita):**

  ```postgresql
  SELECT * FROM Tabela_A
  RIGHT OUTER JOIN Tabela_B
  ON Tabela_A.coluna_de_correspondência=Tabela_B.coluna_de_correspondência
  WHERE Tabela_A.campo_vazio IS NULL;
  ```

  

  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/SQL_Join_-_04_A_Right_Join_B_Where_A.key_%3D_null.svg/1200px-SQL_Join_-_04_A_Right_Join_B_Where_A.key_%3D_null.svg.png" alt="img" style="zoom:25%;" />

  

- **Vejamos que, dependendo da ordem das tabelas na consulta, podemos fazer a operação de `LEFT OUTER JOIN` ou a operação de `RIGHT OUTER JOIN`, e ainda assim, obter os mesmos resultados;**



##### USING

- **Pode ser utilizado no lugar do `ON`:**

  ```postgresql
  SELECT T1.coluna_1, T1.coluna_2, T2.coluna_1
  FROM Tabela_1 T1
  JOIN Tabela_2 T2 USING (coluna_3);
  ```

  - **Para isso, a coluna deve possuir o mesmo nome, em ambas as tabelas (ou seja, a chave primária de uma tabela, deve possuir o mesmo nome  da chave estrangeira de outra tabela);**



#### UNION

- **Utilizado para combinar o conjunto de resultados (o retorno dos registros) de duas ou mais instruções de seleção (`SELECT`);**

- **Sintaxe:**

  ```postgresql
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_1
  UNION
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_2
  UNION
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_3
  .
  .
  .
  UNION
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_n;
  ```

- **As tabelas devem possuir alguma correspondência;**
- **Os resultados são colocados uns sobre os outros;**
- **Devemos selecionar o mesmo número de colunas das tabelas, em cada `SELECT`;**
- **E essas colunas devem ser de tipos de dados compatíveis (por exemplo, no caso dado acima, podemos ter todas as "_coluna_1_" do tipo texto, enquanto todas as "_coluna_2_" devem ser do tipo numérico);**
- **Outros exemplo:**

  ```postgresql
  -- Com WHERE
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_1
  WHERE condição
  UNION
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_2
  WHERE condição;
  
  -- Com ORDER BY
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_1
  WHERE condição
  ORDER BY coluna
  UNION
  SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_2
  WHERE condição
  ORDER BY coluna;
  ```
- **`UNION ALL`**

  - **Semelhante ao `UNION`, só que se diferencia pelo fato do `UNION` remover todas as linhas duplicadas;**
  - **Ou seja, se quisermos registros duplicados, devemos utilizar o `UNION ALL`:**

    ```postgresql
    SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_1
    UNION ALL
    SELECT coluna_1, coluna_2, ..., coluna_n FROM Tabela_2;
    ```




#### INTERSECT

- **Semelhante ao `UNION`e ao `UNION ALL`, só que agora, ao invés de retornar todos os registros de ambas as instruções `SELECT` (todos os conjuntos de resultados obtidos a partir das instruções `SELECT`), o `INTERSECT` retorna apenas os registros que  estão disponíveis em ambas as instruções `SELECT` (os registros disponíveis em ambos os conjuntos de resultados obtidos a partir das instruções `SELECT`)**:

  ```postgresql
  SELECT coluna FROM Tabela_1
  INTERSECT
  SELECT coluna FROM Tabela_2;
  ```



#### EXCEPT

- **Semelhante aos operadores `UNION` e `INTERSECT`, com o porém de que, retorna os registros distintos que aparecem na primeira tabela (da primeira consulta, do primeiro `SELECT`), mas que não aparecem na segunda tabela (do segundo `SELECT`):**

  ```postgresql
  SELECT coluna FROM Tabela_1
  EXCEPT
  SELECT coluna FROM Tabela_2;
  ```

  
