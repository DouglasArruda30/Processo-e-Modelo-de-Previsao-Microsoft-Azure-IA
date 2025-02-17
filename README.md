# Processo-e-Modelo-de-Previsao-Microsoft-Azure-IA
DIO Bootcamp Microsoft AI 900

# Modelo de Previsão no Azure Machine Learning

Este guia descreve como criar, treinar e implantar um modelo de previsão diretamente pelo Portal do Azure utilizando o **Azure Machine Learning**. O exercício foi realizado como parte do **DIO Bootcamp Microsoft IA-900**.

## 1. Configurar o Workspace do Azure ML

### Acesse o Portal do Azure:
- Faça login no [Portal do Azure](https://portal.azure.com).

### Crie um Workspace do Azure ML:
1. No menu esquerdo, clique em **Criar um recurso**.
2. Busque por **Machine Learning** e selecione **Azure Machine Learning**.
3. Escolha a **Assinatura** e crie um **Grupo de Recursos**.
4. Preencha os detalhes:
   - **Nome**: `workspace-ml-predicao`
   - **Grupo de Recursos**: Crie um novo ou escolha um existente.
   - **Região**: Escolha a mesma região dos seus dados.
5. Clique em **Revisar + Criar** e confirme.

## 2. Criar e Configurar um Experimento

### Abra o Workspace:
- Após a criação, acesse o workspace e clique em **Azure Machine Learning Studio** para abrir o estúdio no navegador.

### Prepare os Dados:
1. Vá até a aba **Dados** e clique em **+ Criar**.
2. Insira **Nome** e **Descrição** nos respectivos campos.
3. Escolha o tipo de dados (ex: `urifile`, `mltable`, `Tabular`, `Arquivo`, `JSON`, `CSV`) e clique em **Avançar**.
4. Faça o upload do arquivo de entrada (ex: JSON fornecido), conecte um armazenamento existente ou forneça um link da Web.
5. Verifique e registre os dados com um nome, como `bike-rentals`.

### Use o Aprendizado de Máquina Automatizado para Treinar um Modelo:
1. No **Azure Machine Learning Studio**, visualize a página **ML automatizada** (em **Criação**).
2. Crie um novo trabalho de ML automatizado com as seguintes configurações:
   - **Nome do trabalho**: O campo será preenchido automaticamente com um nome exclusivo.
   - **Novo nome do experimento**: `mslearn-bike-rental`.
   - **Descrição**: Aprendizado de máquina automatizada para previsão de aluguel de bicicletas.
   - **Tags**: Nenhuma.
   
3. **Tipo de Tarefa e Dados**:
   - **Tipo de tarefa**: **Regressão**.
   - **Selecionar conjunto de dados**: Crie um novo conjunto de dados:
     - **Nome**: `aluguel-de-bicicletas`
     - **Descrição**: Dados históricos de aluguel de bicicletas.
     - **Tipo**: Tabela (`mltable or table`).
   - **Fonte de dados**: Selecione **De arquivos locais**.
   - **Tipo de armazenamento de destino**: **Azure Blob Storage**.
   - **Nome**: `workspaceblobstore`.
   - **Seleção de MLtable**:`https://aka.ms/bike-rentals` Pasta de upload – Baixe e descompacte a pasta que contém os arquivos. 
   - Selecione **Criar** e depois selecione o conjunto de dados `bike-rentals` para continuar o trabalho de ML automatizado.

## 3. Avaliar Modelo

1. No guia **Visão geral** do trabalho de aprendizado de máquina automatizada, observe o resumo do modelo.
2. Selecione o texto em **Nome do algoritmo** para visualizar os detalhes do modelo.
3. Selecione a guia **Métricas** e visualize os gráficos **Residuais** e **Prediction_true**.

## 4. Implantar o Modelo

### Crie um Endpoint:
1. Na aba **Implantações**, clique em **+ Criar Ponto de Extremidade**.
2. Escolha **Ação de Inferência em Tempo Real**.
3. Preencha os detalhes:
   - **Máquina virtual**: `Standard_DS3_v2`.
   - **Contagem de instâncias**: 3.
   - **Ponto final**: Novo.
   - **Nome do ponto de extremidade**: Deixe o padrão (garanta que seja globalmente exclusivo).
   - **Nome da implantação**: Deixe o padrão.
   - **Inferência de coleta de dados**: Desativado.
   - **Modelo de pacote**: Desativado.

### Implante:
- Aguarde até que o status da implantação mude para **Succeeded**. Isso pode levar de 5 a 10 minutos.

## 5. Testar o Endpoint

1. No **Azure Machine Learning Studio**, no menu à esquerda, selecione **Endpoints** e abra o endpoint em tempo real `predict-rentals`.
2. Na página do endpoint, selecione a guia **Teste**.
3. No painel de dados de entrada, substitua o JSON do modelo pelos seguintes dados:

```json
"input_data": {
    "columns": [
      "day", "mnth", "year", "season", "holiday", "weekday", "workingday", "weathersit", "temp", "atemp", "hum", "windspeed"
    ],
    "index": [0],
    "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
  }
``` 
Veja o Resultado:

O resultado esperado será algo como: [361.41500445154400].

Teste sua aplicação:
Utilize uma URL e uma chave para realizar requisições via cURL , Postman ou outro cliente HTTP.
6. Monitorar e Atualizar


Acesse o Monitoramento:
Na aba Implantações , visualize os logs e o desempenho do endpoint.
Atualize o Modelo:

Para melhorar o desempenho, treine o modelo com novos dados e publique uma nova versão.


7. Limpar
Se você não pretende mais usar o serviço web, pode excluir o ponto de extremidade para evitar custos desnecessários. No Azure Machine Learning Studio , acesse o guia de ponto de extremidade e clique em Excluir , confirmando a ação.



