# 🤖 B2B Lead Enrichment & Salesforce Automation (n8n)

## 📌 Visão Geral
Este projeto é um fluxo avançado de automação desenvolvido no **n8n**. Ele foi criado para capturar diariamente novas empresas que venceram licitações (via SharePoint/Excel), enriquecer seus dados de contato de forma inteligente e criar Leads completos e prontos para prospecção no CRM **Salesforce**.

O grande diferencial deste projeto é o sistema de **Enriquecimento em Duas Camadas (Fallback com IA)**, que garante que a equipe comercial não perca tempo buscando telefones manualmente.

## 🏗️ Arquitetura e Fluxo de Dados

1. **Gatilho Diário:** A automação roda automaticamente todos os dias às 09:00.
2. **Extração de Dados:** Lê as novas linhas de uma planilha hospedada no SharePoint.
3. **Filtro Anti-Duplicidade:** Verifica se a linha já foi processada anteriormente para evitar duplicidade no CRM.
4. **Enriquecimento Nível 1 (API Pública):** Consulta o CNPJ da empresa na API da Receita Federal (ReceitaWS) para buscar telefone e e-mail oficiais.
5. **Enriquecimento Nível 2 (Fallback com IA):** 
   - Se a empresa não tiver contato público, a automação aciona a API do **Serper.dev** para realizar uma busca estruturada no Google.
   - O resultado da busca é enviado para uma **IA Local (Ollama / Qwen2.5)** via RAG.
   - A IA extrai e formata o telefone e o e-mail encontrados na internet, retornando um JSON limpo.
6. **Normalização:** Os dados (seja da Receita ou da IA) são padronizados.
7. **Integração com Salesforce:** Cria o Lead no CRM e o atribui automaticamente ao executivo de vendas.
8. **Atualização de Status:** Carimba a planilha do Excel confirmando o envio.

## 🚀 Tecnologias Utilizadas
- **n8n** (Orquestração de Fluxos)
- **Salesforce API** (CRM B2B)
- **Microsoft SharePoint / Excel Node** (Base de Dados)
- **ReceitaWS API** (Consultas de CNPJ)
- **Serper.dev API** (Web Scraping / Google Search)
- **Ollama / Qwen2.5** (Inteligência Artificial Local / LLM)

## ⚙️ Como utilizar este fluxo
1. Importe o arquivo `automacao_leads.json` para o seu n8n.
2. Configure suas credenciais OAuth2 do Salesforce e Microsoft.
3. Adicione sua chave do Serper no nó de HTTP Request.
4. Aponte o nó da IA para a URL da sua instância do Ollama.
