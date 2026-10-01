# Fluxo 1: Lead → Contrato → Projeto

Automatiza a passagem de um lead recém-fechado no CRM externo do Blumel até a
criação da task de projeto no ClickUp, passando por coleta de dados e
assinatura de contrato.

> **Regra de lógica:** nenhuma task de cliente nasce no início. A primeira task
> real só aparece quando o cliente responde ao formulário de coleta de dados. A
> task de cliente e a task de projeto só nascem no fim, quando o contrato entra
> em vigor.

## Visão geral

```
CRM externo
  │  POST /webhook/blumel/lead-entrada  (X-Blumel-Token)
  ▼
[1] Lead Entrada ── valida payload ──► 400 se inválido
  │
  ▼
[2] Envia link do Form de Coleta ao cliente (WhatsApp / UAZAPI)
  │
  ▼  cliente responde o Form
[3] Task em Coleta de Dados (901328255280)
  │
  ▼  status "gerar contrato"
[4] Task em Gestão Contratual (901328255279) + contrato gerado no Google Docs
  │
  ▼  status "aguardando assinatura"
[5] Envio do documento ao Zapsign
  │
  ▼  webhook Zapsign: assinatura completa
[6] Task de contrato → status "em vigor"
  │
  ▼  status "em vigor"
[7] Task em Gestão de Clientes (901328255364)
    + Task em Projetos Externos (clientes) (901328255438)   ◄── fim do Fluxo 1
```

## O que já está construído

Workflow n8n **Blumel - Lead Entrada** (`o4tN7EPCvbnp6DXo`, ativo):

1. Webhook recebe o payload do CRM em `POST /webhook/blumel/lead-entrada`,
   autenticado pelo header `X-Blumel-Token`.
2. Nó IF **Payload Válido?** confere os campos obrigatórios:
   - nome e CPF do responsável legal
   - razão social e CNPJ da empresa
   - produtos/serviços contratados
3. Se inválido → responde `400` com o motivo.
4. Se válido → **hoje cria uma task em Cliente Operação** (`901328155865`).
   ⚠️ Esse passo deve ser removido (ver passo 1 abaixo).
5. Responde `200` confirmando o recebimento.

## Próximos passos

Legenda: 🟢 feito · 🟠 a construir, sem bloqueio · 🔴 bloqueado por acesso externo

| # | Passo | Depende de | Status |
|---|---|---|---|
| 1 | Remover o nó "Criar Task Projeto" do workflow Lead Entrada | — | 🟠 |
| 2 | Confirmar ou criar o Form nativo do ClickUp apontado para a lista Coleta de Dados | — | 🟠 |
| 3 | Substituir o nó removido por um passo que envia o link do Form ao cliente | Credencial UAZAPI | 🔴 |
| 4 | Status `gerar contrato` (Coleta de Dados) → cria task em Gestão Contratual + gera doc a partir do modelo, linkado na task | Modelo de contrato + credencial Google | 🔴 |
| 5 | Status `aguardando assinatura` → envia o doc ao Zapsign | API Zapsign | 🔴 |
| 6 | Webhook receptor do Zapsign (assinatura completa) → task de contrato para `em vigor` | API Zapsign | 🔴 |
| 7 | Status `em vigor` → cria task em Gestão de Clientes + task em Projetos Externos (clientes) | — | 🟠 |
| 8 | Teste ponta a ponta com lead de teste antes de liberar em produção | Passos 1–7 | 🟠 |

## Pendências

| O que falta | Quem resolve | Status |
|---|---|---|
| Credencial UAZAPI (URL base + token da instância conectada ao SIM) | Time Blumel (Gabriel) | Recebida; salva no n8n (a confirmar) |
| Acesso à API do Zapsign | Time Blumel | Chave recebida e testada (01/10); salva no n8n (a confirmar) |
| Acesso à API do Asaas | Time Blumel | Não confirmado (fora do Fluxo 1) |
| Modelo de contrato no Google Docs + credencial Google | Amplio / Blumel | Credencial Drive no n8n (a confirmar); modelo pendente |
| Instância n8n + chave de API | Amplio | Aguardando confirmação da instância |
| Canal de envio do Form de Coleta | Amplio | Decidido: WhatsApp via UAZAPI |
