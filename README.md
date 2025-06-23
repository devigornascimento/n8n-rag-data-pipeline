
# Pipeline de Dados para RAG com n8n, Supabase e OpenAI

## Descrição

Este projeto implementa um pipeline de dados robusto e automatizado utilizando a plataforma low-code **n8n**. Sua principal função é ler dados de uma planilha no Google Sheets, processá-los, gerar embeddings (vetores) através da API da OpenAI e, por fim, armazená-los em um Vector Store no Supabase.

O objetivo é manter uma base de conhecimento vetorial sempre sincronizada com uma fonte de dados externa, servindo como a fundação para aplicações de Busca Semântica e RAG (Retrieval-Augmented Generation).

## Arquitetura e Fluxo de Dados

O workflow opera em uma sequência lógica para garantir a integridade e atualização dos dados:

```
[Google Sheet]
      |
      '--> 1. Gatilho (Modificação no arquivo)
      |
      v
+------------------------------------------------------+
|                                                      |
|                   [n8n Workflow]                     |
|                                                      |
|  2. Limpeza: Deleta vetores antigos no Supabase.     |
|  3. Extração: Baixa e lê a planilha.                 |
|  4. Vetorização: Gera embeddings com OpenAI.         |
|  5. Armazenamento: Insere novos vetores no Supabase. |
|                                                      |
+------------------------------------------------------+
      |
      v
[Supabase Vector Store]
```

## Funcionalidades Principais

* **Sincronização Automática**: O workflow é acionado a cada minuto para verificar se há atualizações na planilha do Google Sheets.
* **Processo de Upsert**: Garante que a base de dados vetorial não contenha dados desatualizados ou duplicados. Antes de inserir novos dados, ele apaga os registros antigos associados ao arquivo modificado.
* **Vetorização com OpenAI**: Utiliza o nó `Embeddings OpenAI` para converter dados textuais em vetores numéricos de alta dimensão, permitindo a busca por similaridade semântica.
* **Armazenamento Otimizado**: Insere os dados processados e vetorizados em um Supabase Vector Store, ideal para aplicações de IA que necessitam de buscas rápidas e eficientes.

## Tecnologias Utilizadas

* **Plataforma de Automação:** n8n
* **Fonte de Dados:** Google Drive / Google Sheets
* **Vetorização:** OpenAI
* **Banco de Dados Vetorial:** Supabase (Vector Store)

## Como Configurar e Usar

Para executar este projeto, siga os passos abaixo:

1.  **Pré-requisitos:**
    * Uma instância do n8n (Cloud ou auto-hospedada).
    * Conta no Google Cloud Platform com a API do Google Drive habilitada.
    * Conta na OpenAI com créditos de API disponíveis.
    * Conta no Supabase com um projeto criado.

2.  **Importar o Workflow:**
    * Faça o download do arquivo `.json` deste repositório.
    * Na sua instância do n8n, importe o arquivo através da opção "Import from File...".

3.  **Configurar Credenciais:**
    * **Google Drive**: No nó `Google Drive Trigger`, configure ou crie novas credenciais do tipo "Google Drive OAuth2 API".
    * **Supabase**: Nos nós `Supabase` e `Supabase Vector Store`, configure ou crie novas credenciais do tipo "Supabase API".
    * **OpenAI**: No nó `Embeddings OpenAI`, configure ou crie novas credenciais do tipo "OpenAI API".

4.  **Configurar Nós:**
    * **`Google Drive Trigger`**: No campo "File to Watch", insira o ID da sua planilha Google Sheets.
    * **`Google Drive`**: No campo "File ID", utilize uma expressão para pegar o ID do nó de gatilho ou insira-o manualmente.
    * **`Supabase` e `Supabase Vector Store`**: Verifique se o nome da tabela (`products`) está correto de acordo com a sua configuração no Supabase.

5.  **Ativar o Workflow:**
    * Salve todas as alterações.
    * Ative o workflow para iniciar o processo de sincronização automática.
