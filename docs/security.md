# Segurança e privacidade

Este repositório é público e foi separado do projeto de produção. Ele contém documentação de arquitetura e decisões; não contém o código completo nem configurações do ambiente.

## O que fica fora do repositório

- Tokens de Telegram, credenciais OAuth e segredos do aplicativo.
- Chaves de contas de serviço, arquivos de credenciais e variáveis de ambiente reais.
- IDs de projeto, nomes de recursos privados e endereços internos.
- Banco de vagas, dados de avaliação e conteúdo não autorizado das fontes.
- Dados pessoais de candidatos ou recrutadores.

## Controles previstos no desenho

- Guardar segredos de runtime no Secret Manager.
- Autenticar o CI/CD por OIDC/Workload Identity Federation, evitando chaves JSON de longa duração.
- Limitar permissões do workflow ao recurso e ambiente necessários.
- Restringir credenciais de produção a execuções confiáveis de branch/ambiente protegido.
- Revisar mudanças de infraestrutura e dependências antes de promover uma imagem.
- Implantar imagens por digest e validar os serviços após a mudança.

## Divulgação

Antes de publicar novos screenshots, logs ou exemplos, remover tokens, e-mails, IDs de recursos, URLs de administração e dados identificáveis. Use apenas dados fictícios ou evidências sanitizadas.
