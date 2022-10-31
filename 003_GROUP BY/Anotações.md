# PostgreSQL



### GROUP BY

#### Funções de agregação (Funções agregadoras)

- **Basicamente, recebe várias entradas (executa um cálculo nesse conjunto de valores recebidos) e retornar uma única saída;**

- **Funções de agregação, mais comuns:**

  - **`AVG()`: retorna a média de dos valores (um valor de ponto flutuante - com casas decimais);**

    ```postgresql
    SELECT AVG(coluna) FROM Tabela;
    ```

    - **Podemos utilizar a função `ROUND()`, para especificar a precisão de casa decimais (ela aceita dois parâmetros: o valor a ser arredondado e o número de casas decimais que desejamos que esse valor tenha):**

      ```postgresql
      ROUND(AVG(coluna), número_de_casas_decimais)
      ```

  - **`COUNT()`: retorna o número de registros/linhas :**

    ```postgresql
    SELECT COUNT(coluna) FROM Tabela;
    SELECT COUNT(*) FROM Tabela;
    
    -- Não conta valores nulos;
    ```

  - **`MAX()`: retorna o maior valor (o valor máximo);**

    ```postgresql
    SELECT MAX(coluna) FROM Tabela;
    
    --  Pode ser utilizado com datas (retorna a data mais recente);
    --  Também funciona com textos (retorna de acordo com a ordem 
    -- alfabética - mais próximo ao início do alfabeto/o final 
    -- do alfabeto);
    ```

  - **`MIN()`: retorna o menor valor (o valor mínimo);**

    ```postgresql
    SELECT MIN(coluna) FROM Tabela;
    
    --  Pode ser utilizado com datas (retorna a data mais antiga);
    --  Também funciona com textos (retorna de acordo com a ordem 
    -- alfabética - mais próximo ao início do alfabeto/o início 
    -- do alfabeto);
    ```

  - **`SUM()`: retorna a soma total dos valores:**

    ```postgresql
    SELECT SUM(coluna) FROM Tabela;
    
    -- ERROR
    SELECT SUM(*) FROM Tabela;
    
    -- Não usamos a função SUM com textos e nem com datas;
    ```

- **As funções de agregação são utilizadas em conjunto com a instruções (cláusulas) `SELECT`/`HAVING`;**

- **OBS.: podemos utilizar mais de uma função agregadora em uma consulta, mas não podemos utilizar uma função agregadora (que retorna um único valor) e querer buscar por dados de uma coluna (que geralmente retorna mais de um valor), em uma única consulta;**



#### GROUP BY

- **Permite a agregação/agrupamento de linhas (que pertencem a uma mesma categoria),  trazendo um resumo dos seus dados (a partir de uma função de agregação);**

- **Sintaxe "geral":**

  ```postgresql
  SELECT coluna_a_ser_agrupada, FUNÇÂO_AGREGADORA(coluna)
  FROM Tabela
  GROUP BY coluna_a_ser_agrupada;
  
  -- Outro exemplo
  SELECT
  	coluna_1,
  	coluna_2,
  	...,
  	FUNÇÂO_AGREGADORA(coluna)
  FROM
  	Tabela
  GROUP BY
  	coluna_1,
  	coluna_2,
  	...;
  ```

- **OBS_1.: a cláusula `GROUP BY` deve vir após uma cláusula `FROM`, ou após uma cláusula `WHERE`;**

- **OBS_2.: podemos utilizar o `GROUP BY`, com diferentes colunas;**

- **OBS_3.: ordem das cláusulas em uma `query`:**

  ```postgresql
  SELECT ...
  FROM ...
  WHERE ...
  GROUP BY ...
  ORDER BY ...
  LIMIT ...
  ```

   

#### HAVING

- **Nos possibilita a aplicação de filtros, após a agregação já ter ocorrido;**

  ```postgresql
  SELECT coluna_1, FUNÇÂO_AGREGADORA(coluna_2)
  FROM Tabela
  GROUP BY coluna_1
  HAVING condição;
  ```
  
  - **Na cláusula `HAVING`, inclusive, podemos aplicar o filtro em funções agregadoras:**
  
    ```postgresql
    SELECT coluna_1, FUNÇÂO_AGREGADORA(coluna_2)
    FROM Tabela
    GROUP BY coluna_1
    HAVING FUNÇÂO_AGREGADORA(coluna_2) = valor;
    ```

  - **Em uma mesma `query`, podemos ter as cláusulas `HAVING` (aplicada a grupos de linhas) e `WHERE` (aplicada a linhas);**

  - ```postgresql
    SELECT ...
    FROM ...
    WHERE ...
    GROUP BY ...
    HAVING ...
    ORDER BY ...
    LIMIT ...
    ```
  
    
  
  
  
  
