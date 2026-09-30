# Blumel

Cliente: **Blumel Performance Educacional**.

## Fluxos

| Fluxo | Descrição | Status |
|---|---|---|
| [Fluxo 1](fluxos/fluxo-1-lead-contrato-projeto.md) | Lead → Contrato → Projeto | Em construção |

## Referência técnica

### n8n

- Instância: `https://primary-production-66b9f.up.railway.app`
- Credenciais já criadas no n8n:
  - `Header Auth account`: autenticação do webhook de entrada (`X-Blumel-Token`)
  - `ClickUp account`: OAuth2, workspace Blumel Performance Educacional

| Workflow | ID | Status | Export |
|---|---|---|---|
| Blumel - Lead Entrada | `o4tN7EPCvbnp6DXo` | Ativo | [`workflows/`](workflows/) (pendente) |

### ClickUp

Workspace **Blumel Performance Educacional**, ID `9013045857`.

| Uso no fluxo | Space | Folder | Lista | ID da lista |
|---|---|---|---|---|
| Task de projeto (hoje, a remover) | Operação Blumel | 1 · Clientes | Cliente Operação | `901328155865` |
| Coleta de dados do cliente | Administrativo | Jurídico | Coleta de Dados | `901328255280` |
| Gestão do contrato | Administrativo | Jurídico | Gestão Contratual | `901328255279` |
| Task de cliente (fim do fluxo) | Comercial | CRM | Gestão de Clientes | `901328255364` |
| Task de projeto (fim do fluxo) | Projetos | Painel de Projetos | Projetos Externos (clientes) | `901328255438` |

Status configurados:

- **Coleta de Dados:** `para coletar` → `preenchido` → `para aprovar dados` → `solicitar dados` → `gerar contrato` → `contrato redigido` → `arquivado`
- **Gestão Contratual:** `inativo` → `para revisar` → `aguardando assinatura` → `em vigor` → `suspenso` → `encerrado` → `arquivado`

### Integrações externas

| Serviço | Uso | Acesso |
|---|---|---|
| UAZAPI | Envio do Form de Coleta por WhatsApp | Token recebido; credencial no n8n (a confirmar) |
| Zapsign | Assinatura do contrato + webhook de retorno | Chave API recebida; credencial no n8n (a confirmar) |
| Google Drive/Docs | Geração do contrato a partir de modelo | Credencial no n8n (a confirmar); modelo de contrato pendente |
| Asaas | Cobrança (fora do Fluxo 1) | Não confirmado |

## Documentos

- [Fluxo 1: resumo e próximos passos (PDF)](docs/fluxo-1-resumo-e-proximos-passos.pdf)
