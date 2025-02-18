# Pré-processamento de Dados de Câncer de Mama com SQL

Este repositório demonstra técnicas de pré-processamento de dados utilizando SQL, com foco em categorização, codificação e binarização de variáveis categóricas. O objetivo é preparar dados para análise e modelagem em projetos de Data Science, convertendo informações qualitativas em formatos numéricos que algoritmos de Machine Learning podem processar de forma eficiente.

## Dataset Utilizado

O projeto utiliza o dataset "Breast Cancer" disponível no UCI Machine Learning Repository: [https://archive.ics.uci.edu/dataset/14/breast+cancer](https://archive.ics.uci.edu/dataset/14/breast+cancer). Este conjunto de dados contém informações sobre pacientes com câncer de mama e características relacionadas ao tumor e ao histórico da paciente.

## Técnicas Implementadas

O script SQL (`script.sql` ou nome similar) demonstra as seguintes técnicas:

*   **Codificação (Label Encoding):** Conversão de variáveis categóricas ordinais em representações numéricas. Exemplo: mapeamento da variável `quadrante` (left_low, right_up, etc.) para valores de 1 a 5.

*   **Binarização:** Transformação de variáveis categóricas binárias em valores 0 e 1. Exemplo: conversão das variáveis `classe` e `irradiando` para representações binárias.

*   **Categorização:** Agrupamento de valores de uma variável contínua ou ordinal em categorias distintas. Exemplo: criação da variável `tamanho_tumor` com categorias como 'Muito Pequeno', 'Pequeno', 'Médio', etc., a partir da variável original.

## Script SQL

O script SQL principal (`script.sql`) realiza as seguintes operações:

1.  **Criação da Tabela `tb_dados2`:** Uma nova tabela (`tb_dados2`) é criada a partir de uma tabela existente (`tb_dados`), aplicando as transformações de categorização, codificação e binarização.

2.  **Transformação das Variáveis:** As variáveis categóricas são transformadas em representações numéricas utilizando a expressão `CASE` do SQL. A seguir, o código SQL utilizado:

    ```sql
    CREATE TABLE cap03.tb_dados2 AS
    SELECT
        CASE
            WHEN classe = 'no-recurrence-events' THEN 0
            WHEN classe = 'recurrence-events' THEN 1
        END as classe,
        idade,
        menopausa,
        CASE
            WHEN tamanho_tumor = '0-4' OR tamanho_tumor = '5-9' THEN 'Muito Pequeno'
            WHEN tamanho_tumor = '10-14' OR tamanho_tumor = '15-19' THEN 'Pequeno'
            WHEN tamanho_tumor = '20-24' OR tamanho_tumor = '25-29' THEN 'Medio'
            WHEN tamanho_tumor = '30-34' OR tamanho_tumor = '35-39' THEN 'Grande'
            WHEN tamanho_tumor = '40-44' OR tamanho_tumor = '45-49' THEN 'Muito Grande'
            WHEN tamanho_tumor = '50-54' OR tamanho_tumor = '55-59' THEN 'Tratamento Urgente'
        END as tamanho_tumor,
        inv_nodes,
        CASE
            WHEN node_caps = 'no' THEN 0
            WHEN node_caps = 'yes' THEN 1
            ELSE 2
        END as node_caps,
        deg_malig,
        CASE
            WHEN seio = 'left' THEN 'E'
            WHEN seio = 'right' THEN 'D'
        END as seio,
        CASE
            WHEN quadrante = 'left_low' THEN 1
            WHEN quadrante = 'right_up' THEN 2
            WHEN quadrante = 'left_up' THEN 3
            WHEN quadrante = 'right_low' THEN 4
            WHEN quadrante = 'central' THEN 5
            ELSE 0
        END as quadrante,
        CASE
            WHEN irradiando = 'no' THEN 0
            WHEN irradiando = 'yes' THEN 1
        END as iarradiando
    FROM cap03.tb_dados;
    ```

3.  **Contagem de Registros:** (Opcional) Uma consulta `SELECT COUNT(*)` pode ser executada para verificar o número total de registros na tabela resultante.

## Como Usar

1.  **Importe o Dataset:** Importe o dataset "Breast Cancer" para o seu sistema de gerenciamento de banco de dados (MySQL, PostgreSQL, etc.).

2.  **Crie a Tabela Original:** Crie a tabela `tb_dados` (ou ajuste o nome no script) com a estrutura correspondente ao dataset importado.

3.  **Execute o Script SQL:** Execute o script SQL para criar a tabela `tb_dados2` com as variáveis transformadas.

4.  **Analise os Resultados:** Utilize a tabela `tb_dados2` como entrada para seus modelos de Machine Learning ou outras análises de dados.

## Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir uma issue ou enviar um pull request com sugestões de melhorias, novas técnicas de pré-processamento ou correções de bugs.
