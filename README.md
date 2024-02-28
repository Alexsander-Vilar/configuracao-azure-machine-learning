
# Configuração do Azure Machine Learning para Previsão de Aluguel de Bicicletas

Este guia fornece um passo a passo para configurar um espaço de trabalho no Azure Machine Learning e treinar um modelo automatizado para prever o aluguel de bicicletas. Após o treinamento, o melhor modelo será implantado como um serviço web para previsão em tempo real.

## Configuração do Azure Machine Learning

1. Acesse o [Portal Azure](https://portal.azure.com) utilizando suas credenciais da Microsoft.

2. Selecione "+ Criar um recurso" e pesquise por "Machine Learning". Crie um novo recurso do Azure Machine Learning com as seguintes configurações:
   - **Assinatura:** Sua assinatura do Azure.
   - **Grupo de recursos:** Crie ou selecione um grupo de recursos.
   - **Nome:** Insira um nome exclusivo para o espaço de trabalho.
   - **Região:** Selecione a região geográfica mais próxima.
   - **Conta de armazenamento:** Observe a nova conta de armazenamento padrão.
   - **Cofre de chaves:** Observe o novo cofre de chaves padrão.
   - **Insights de aplicativo:** Observe o novo recurso padrão de insights de aplicativo.
   - **Registro de contêiner:** Nenhum (será criado automaticamente na primeira implantação do modelo).

3. Selecione "Revisar + criar" e, em seguida, "Criar". Aguarde a criação do espaço de trabalho.

4. Após a criação, vá para o recurso implantado e selecione "Launch Studio" ou acesse [Azure Machine Learning Studio](https://ml.azure.com).

5. No Azure Machine Learning Studio, certifique-se de ver seu espaço de trabalho recém-criado. Caso contrário, selecione "Todos os espaços de trabalho" no menu à esquerda e escolha o espaço de trabalho criado.

## Treinamento Automatizado do Modelo

1. Na página "Automated ML" (em Authoring), crie um novo trabalho de ML automatizado com as seguintes configurações:
   - **Configurações básicas:**
     - **Nome do trabalho:** mslearn-bike-automl
     - **Novo nome do experimento:** mslearn-bike-rental
     - **Descrição:** Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
     - **Tipo de tarefa:** Regressão

   - **Tipo de tarefa e dados:**
     - Selecione o tipo de tarefa: Regressão
     - Selecionar conjunto de dados: Crie um novo conjunto de dados "aluguel de bicicletas" com as configurações fornecidas.

   - **Configurações de tarefa:**
     - **Tipo de tarefa:** Regressão
     - **Conjunto de dados:** aluguel de bicicletas
     - **Coluna de destino:** Aluguéis (inteiro)
     - Configurações adicionais conforme especificado.

   - **Calcular:**
     - Selecione o tipo de computação: sem servidor
     - Tipo de máquina virtual: CPU
     - Camada de máquina virtual: Dedicada
     - Tamanho da máquina virtual: Standard_DS3_V2 (ou escolha disponível)

2. Envie o trabalho de treinamento e aguarde a conclusão.

## Avaliação do Melhor Modelo

1. Na guia "Visão geral do trabalho automatizado de aprendizado de máquina", observe o melhor resumo do modelo. Confira o nome do algoritmo.

2. Selecione o texto no "Nome do algoritmo do melhor modelo" para visualizar seus detalhes.

3. Na guia "Métricas", analise os gráficos residuais e predito_true para avaliar o desempenho do modelo treinado.

## Implantação e Teste do Modelo

1. Na guia "Modelo" do melhor modelo treinado, selecione "Implantar" e use a opção de "Serviço Web" com as configurações fornecidas.

2. Aguarde a conclusão da implantação. O status será indicado como "Running".

3. Aguarde até que o status mude para "Succeeded" (5 a 10 minutos).

## Teste do Serviço Implantado

1. No Azure Machine Learning Studio, selecione "Endpoints" no menu esquerdo e abra o endpoint em tempo real de "previsão de alugueres".

2. Na página do endpoint, acesse a guia "Teste".

3. No painel de "Dados de entrada para testar o endpoint", substitua o modelo JSON pelos dados fornecidos:
   ```json
   {
     "Inputs": { 
       "data": [
         {
           "day": 1,
           "mnth": 1,   
           "year": 2022,
           "season": 2,
           "holiday": 0,
           "weekday": 1,
           "workingday": 1,
           "weathersit": 2, 
           "temp": 0.3, 
           "atemp": 0.3,
           "hum": 0.3,
           "windspeed": 0.3 
         }
       ]    
     },   
     "GlobalParameters": 1.0
   }
   ```

4. Clique no botão "Testar". 
   **Resultado Obtido:**
   ```json
   {
     "Results": [
       352.2406516281595
     ]
   }
   ```

Agora seu modelo está implantado e pronto para prever o número de aluguéis de bicicletas com base nos dados de entrada fornecidos.
```
