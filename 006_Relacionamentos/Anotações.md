# PostgreSQL



### Relacionamentos



#### O que são relacionamentos

- **Associações/conexões entre tabelas;**
- **Essas conexões são criadas através das chaves primárias, que fazem referência as chaves primárias de outrastabelas;**



#### Tipos de relacionamento

- **1 para 1 (1:1)**
  - **A chave primária de uma tabela, é referenciada no máximo, uma vez (0 ou 1), em uma coluna de chave estrangeira, em uma outra tabela (a chave primária de uma tabela, pode aparecer somente em um único registro de uma outra tabela);**
  - **Tipo de relacionamento raro de acontecer (é muito mais simples e eficiente, agrupar os registros em uma única tabela);**

- **1 para muitos (1:N)**
  - **A chave primária de uma tabela, pode aparecer em vários registros de uma outra tabela;**
  - **Tipo mais comum de relacionamento;**

- **Muitos para muitos (N:N)**
  - **A chave primária de uma tabela A, pode ser referenciada em muitos registros de uma tabela B, assim como a chave primária dessa mesma tabela B, pode ser referenciada em vários registros da tabela A;**
  - **Porém, diante desse tipo de relacionamento, não podemos utilizar apenas chaves primárias e estrangeiras, pois isso viola a exclusividade das colunas de chave primária. Isso acontece porque ao termos uma coluna de chave estrangeira, teríamos valores repetidos  para as chaves primárias (o que não pode, já que as chaves primárias devem ser únicas);**
  - **Logo, devemos ter uma terceira tabela (uma tabela de junção),a tabela C. Essa tabela C deverá conter chaves estrangeiras que fazem referência as chaves primárias das tabelas A e B;**
  - **Por fim, essa tabela C poderá possuir como chave primária, as colunas que correspondem as chaves estrangeiras (a chave primária pode ser uma combinação de colunas);**

