<img width="1465" height="408" alt="Captura de tela 2026-05-21 191524" src="https://github.com/user-attachments/assets/3e8c4391-bc45-4b20-bbe2-c23dc93a90f7" />


# 🚀 Automação de Captação e Enriquecimento de Leads (SharePoint ➔ Salesforce) via n8n

Este repositório contém um fluxo avançado construído no **n8n** para automação de processos comerciais. O robô lê dados de licitações em uma planilha no SharePoint/Excel Online, enriquece o cadastro da empresa via APIs e Inteligência Artificial, e envia as informações tratadas para o Salesforce, garantindo um banco de dados limpo, sem duplicidades e com histórico de recorrência.

## 🌟 Principais Funcionalidades

* **Leitura Inteligente em Lote:** O fluxo varre a base de dados lendo apenas registros novos (onde a coluna de status está vazia).
* **Validação de CNPJ:** O sistema verifica se o CNPJ possui os 14 dígitos corretos. CNPJs inválidos são bloqueados e recebem a tag de erro direto na planilha, impedindo que o CRM seja poluído.
* **Dossiê Completo (ReceitaWS):** Integração via API que puxa automaticamente os dados públicos da Receita Federal: CNAE, Porte da Empresa, Endereço completo, E-mail e Telefone societários.
* **Web Scraping + IA (Serper + LLM):** Se a empresa não possuir contato direto na Receita, o robô faz uma busca no Google usando a API do Serper e utiliza um modelo de Inteligência Artificial para ler os resultados e extrair telefones e e-mails de forma contextual.
* **Módulo Anti-Duplicidade (Salesforce):** Antes de inserir qualquer dado, o robô consulta o banco do Salesforce buscando pelo CNPJ.
    * 🟢 **Novo Lead:** Cria o registro com os dados de contato completos, formatados e higienizados.
    * 🟡 **Lead Existente (Update Inteligente):** Não cria duplicidade! Ele acessa o lead já existente e atualiza o histórico de licitações.
* **Contador e Histórico de Licitações:** Um grande diferencial estratégico! O sistema soma `+1` na quantidade de licitações daquela empresa e empilha um log formatado (1° Licitação, 2° Licitação...) com links, permitindo ao time comercial identificar "peixes grandes" e recorrência.
* **Tratamento de Erros e Sanitização de Dados:** Bloqueio de quebras no envio corrigindo e-mails inválidos automaticamente (ex: substituindo vírgula por ponto) e limitando o número de caracteres em campos rigorosos do CRM.

## 🛠️ Tecnologias Utilizadas

* [n8n](https://n8n.io/) - Orquestração do Workflow
* [Microsoft Graph API](https://learn.microsoft.com/en-us/graph/) - Leitura/Escrita no Excel Online (SharePoint)
* [Salesforce API](https://developer.salesforce.com/) - Gestão de Leads
* [ReceitaWS](https://receitaws.com.br/) - Consulta de CNPJ
* [Serper.dev](https://serper.dev/) - Busca no Google
* [RunPod / Ollama (LLM)] - Extração inteligente de dados não-estruturados

## ⚙️ Pré-requisitos e Configuração (Salesforce)

Para que o fluxo funcione corretamente, é necessário criar os seguintes **Campos Personalizados (Custom Fields)** no objeto `Lead` do seu Salesforce:

1.  `CNPJ__c` (Texto)
2.  `Quantidade_de_Licita_es__c` (Número)
3.  `Hist_rico_de_Licita_es__c` (Área de Texto Longo)
4.  `Observacao__c` (Área de Texto Longo)

## 🚀 Como instalar e usar

1. Faça o clone deste repositório ou o download do arquivo `.json`.
2. No seu painel do n8n, vá em `Workflows` > `Add Workflow` e clique em **Import from File** (ou apenas cole o conteúdo do JSON).
3. **Configure as suas Credenciais:** Você precisará autenticar os nós do Microsoft Excel, Salesforce e Serper.dev com suas próprias contas.
4. Ajuste o ID da Planilha (`Workbook`) e da Tabela (`Table`) no nó "Ler Excel SharePoint" para apontar para o seu arquivo.
5. Ative o fluxo! Ele pode ser acionado por cronograma (ex: rodar todos os dias às 09:00) ou manualmente.

> ⚠️ **Aviso de Segurança Importante:** O arquivo `workflow.json` disponibilizado neste repositório está limpo e **não contém credenciais**. Se você for fazer um fork ou exportar suas próprias alterações, certifique-se de que a opção de exportar credenciais do n8n esteja DESATIVADA para não vazar dados sensíveis do seu CRM ou banco de dados.

---
*Desenvolvido com foco em eficiência comercial e higienização de banco de dados.*
