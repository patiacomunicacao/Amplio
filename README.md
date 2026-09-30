# Amplio

Automações e integrações desenvolvidas pela Amplio para seus clientes.

## Estrutura

```
clientes/
  <cliente>/
    README.md      visão geral, referências técnicas e status
    fluxos/        especificação de cada fluxo
    workflows/     exports JSON dos workflows n8n
    docs/          documentos de referência recebidos
```

## Credenciais

Nenhum segredo (tokens, API keys, senhas) é versionado. Credenciais ficam no
n8n; valores locais vão em arquivos `.env`, que são ignorados pelo git. Use o
`.env.example` de cada cliente como referência.

## Clientes

- [Blumel](clientes/blumel/README.md)
