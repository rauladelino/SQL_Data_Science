# Pré-processamento de Dados de Câncer de Mama com SQL: Label Encoding, Concatenação e One-Hot Encoding

Este repositório demonstra técnicas de pré-processamento de dados aplicadas ao dataset de câncer de mama utilizando SQL. O objetivo é transformar e preparar os dados para análises subsequentes e modelagem de machine learning. As técnicas utilizadas incluem Label Encoding, concatenação de colunas para criação de novas features e One-Hot Encoding.

## Dataset Utilizado

O projeto utiliza o dataset *Breast Cancer* disponível no UCI Machine Learning Repository: [https://archive.ics.uci.edu/dataset/14/breast+cancer](https://archive.ics.uci.edu/dataset/14/breast+cancer).
Este conjunto de dados contém informações sobre pacientes com câncer de mama e características relacionadas ao tumor e ao histórico da paciente.

## Transformações Aplicadas

O script SQL (`transformations.sql` ou similar) realiza as seguintes transformações nos dados:

1.  **Label Encoding na Variável `menopausa`:** A coluna `menopausa`, que contém valores categóricos como 'premeno', 'ge40' e 'lt40', é transformada em uma representação numérica usando Label Encoding. Os valores são mapeados para 1, 2 e 3, respectivamente.

2.  **Criação da Coluna `posição_tumor`:** Uma nova coluna chamada `posição_tumor` é criada concatenando as colunas `inv_nodes` (número de linfonodos axilares envolvidos) e `quadrante` (localização do tumor). Um hífen é usado como separador. Esta coluna combina informações sobre a extensão do tumor e sua localização.

3.  **One-Hot Encoding na Variável `deg_malig`:** A coluna `deg_malig` (grau de malignidade do tumor), que contém valores 1, 2 e 3, é transformada usando One-Hot Encoding. Três novas colunas são criadas: `deg_malig_cat1`, `deg_malig_cat2` e `deg_malig_cat3`. Cada coluna representa uma categoria de `deg_malig`, recebendo o valor 1 se a amostra pertencer àquela categoria e 0 caso contrário.

## Script SQL

O script SQL principal (`transformations.sql`) realiza as seguintes operações:

1.  **Criação da Tabela `tb_dados6`:** Uma nova tabela chamada `tb_dados6` é criada a partir da tabela `tb_dados2`. Esta tabela `tb_dados2` deve existir previamente e conter os dados originais (ou já pré-processados em etapas anteriores).

2.  **Transformação das Variáveis:**
    *   A coluna `menopausa` é transformada usando Label Encoding com a expressão `CASE`.
    *   A coluna `posição_tumor` é criada concatenando as colunas `inv_nodes` e `quadrante` usando a função `CONCAT`.
    *   As colunas `deg_malig_cat1`, `deg_malig_cat2` e `deg_malig_cat3` são criadas usando One-Hot Encoding com a expressão `CASE`.

3.  **Seleção das Colunas:** As colunas originais (ou transformadas em etapas anteriores) são selecionadas e incluídas na nova tabela `tb_dados6`.

A estrutura do script SQL é apresentada abaixo:

```sql
CREATE TABLE cap03.tb_dados6
AS
SELECT
  classe,
  idade,
  CASE
    WHEN menopausa = 'premeno' THEN 1
    WHEN menopausa = 'ge40' THEN 2
    WHEN menopausa = 'lt40' THEN 3
  END AS menopausa,
  tamanho_tumor,
  CONCAT(inv_nodes, '-', quadrante) AS posição_tumor,
  node_caps,
  CASE
    WHEN deg_malig = 1 THEN 1
    ELSE 0
  END AS deg_malig_cat1,
  CASE
    WHEN deg_malig = 2 THEN 1
    ELSE 0
  END AS deg_malig_cat2,
  CASE
    WHEN deg_malig = 3 THEN 1
    ELSE 0
  END AS deg_malig_cat3,
  seio,
  quadrante,
  iarradiando
FROM
  cap03.tb_dados2;