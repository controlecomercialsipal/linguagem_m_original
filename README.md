# 📊 ETL - Controle de Contratos 2026 (Power Query)

Este documento descreve as etapas de transformação de dados (ETL) realizadas via Power Query para a consolidação da base de status de contratos.

## 📁 Origem dos Dados
A extração é realizada diretamente de um arquivo Excel hospedado no SharePoint da Controladoria Comercial:
* **Caminho:** `Restrita/02 - Execução Comercial – Compras/BI_Contratos/CONTROLE DE CONTRATOS 2026.xlsx`
* **Tabela de Origem:** `Base Status`

## 🛠️ Etapas de Processamento (Lógica M)

O script executa as seguintes transformações:
1.  **Conexão Web:** Acessa o arquivo via `Web.Contents` usando a URL do SharePoint.
2.  **Promoção de Cabeçalhos:** Define a primeira linha da planilha como os títulos das colunas.
3.  **Tipagem de Dados:** * Converte colunas numéricas (Quantidade, Datas UTC) para `Int64`.
    * Força a coluna `Contrato` e `Código da carteira` para o tipo `Texto` (para evitar perda de zeros à esquerda ou erros em códigos longos).
4.  **Enriquecimento (Join):**
    * Realiza uma mesclagem de consultas (Left Outer Join) com a tabela `(ID NEGOCIADOR - REPRESENTANTES)`.
    * Utiliza a coluna `Código da carteira` como chave de ligação.
5.  **Expansão:** Adiciona a informação de **Filial** ao resultado final, vinda da tabela de representantes.

## 📝 Observações Importantes
* **Datas em UTC:** Note que as colunas de entrega e pedido estão no formato numérico UTC e podem precisar de conversão adicional para formato de data local (`Date.Type`) em etapas futuras.
* **Dependência:** A atualização desta consulta depende da existência e do acesso à tabela de dimensões de **Representantes**.

## 🚀 Como utilizar
1. Abra o **Power BI Desktop** ou **Excel**.
2. Vá em **Obter Dados > Consultas Vazias**.
3. Abra o **Editor Avançado** e cole o código fornecido.
4. Certifique-se de estar autenticado com sua conta organizacional da Sipal para acessar o SharePoint.
