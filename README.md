# Integração CS Ops: HubSpot ↔ Google Sheets

Automação que lê tickets e contatos do HubSpot (CRM) via API REST e escreve os dados automaticamente numa planilha do Google Sheets — eliminando a exportação manual repetida de relatórios.

## O problema que resolve

Times de Customer Success Operations precisam manter planilhas de acompanhamento (status de tickets, dados de contato) sempre atualizadas, sem depender de exportação manual repetida do CRM.

## Como funciona

1. **Autenticação** via Private App Token do HubSpot (Bearer token), com escopos restritos apenas ao necessário (`crm.objects.contacts.read/write`, `tickets`).
2. **Busca de pipelines/etapas** dos tickets, para traduzir o status interno em um nome legível.
3. **Leitura de tickets** via API (`crm/v3/objects/tickets`).
4. **Busca do contato associado** a cada ticket (`crm/v4/objects/tickets/{id}/associations/contacts`).
5. **Escrita automática** dos dados (ID, assunto, status, nome e e-mail do contato) na planilha ativa do Google Sheets, via Apps Script.

## Estrutura do repositório

```
testar_conexao_hubspot.gs      # valida se a chave de API está ativa antes de qualquer operação
puxar_tickets_hubspot.gs       # lê tickets + contato associado do HubSpot e escreve na planilha
```

## Tecnologias

- Google Apps Script (JavaScript)
- HubSpot API v3 (CRM Objects) e v4 (Associations)
- REST / JSON / autenticação Bearer token

## Conceitos técnicos aplicados

- Requisições HTTP (GET) e tratamento de status code (200, 400, 401)
- Autenticação via token com escopo restrito (least privilege)
- Associação entre objetos via API (ticket ↔ contato)
- Tratamento de erro por requisição (`muteHttpExceptions`, verificação de `responseCode`)

## Próximos passos

- Mover a chave de API para `PropertiesService` do Apps Script, em vez de deixá-la escrita diretamente no código.
- Adicionar lógica de atualização incremental (identificar registros já existentes na planilha e atualizar só o que mudou, sem apagar anotações manuais da equipe).
- Adicionar um indicador de health score por contato, calculado a partir de propriedades customizadas do HubSpot.

## Contexto

Venho de um background em desenvolvimento de software e estou migrando para a área de Customer Success Operations / Melhoria de Processos. Este projeto é um exercício prático de como aplicar lógica de programação — autenticação, consumo de API, estruturação de dados — para resolver um problema real de operação, em vez de depender de processos manuais e planilhas desatualizadas.

## Autor

Pedro Henrique Ferreira Bastos · [LinkedIn](https://linkedin.com/in/pedrinbastos) · [GitHub](https://github.com/pedrohbastos94)
