# Decisões técnicas

## Execução em lote no Cloud Run Jobs

A coleta é um workload finito: inicia, processa fontes, grava resultados e encerra. Cloud Run Jobs atende esse formato sem manter um processo de coleta ligado continuamente. O Cloud Scheduler inicia as execuções conforme a frequência configurada.

**Trade-off:** o Job precisa controlar timeout, tentativas, idempotência e falhas parciais. Uma execução bem-sucedida não deve mascarar a falha de uma fonte.

## Firestore para estado e histórico

O Firestore oferece persistência gerenciada e integra-se ao runtime no Google Cloud. O estado armazenado permite reconhecer anúncios novos e alterações posteriores.

**Trade-off:** consultas e retenção precisam ser planejadas. Leituras históricas sem limite aumentam custo e podem degradar a execução. A evolução do esquema precisa considerar migrações e compatibilidade.

## Notificações com revisão humana

Telegram e e-mail reduzem o tempo entre encontrar uma vaga e tomar conhecimento dela. A notificação é uma fila de atenção, não uma candidatura automática.

**Trade-off:** tokens e credenciais OAuth devem ficar em armazenamento de segredos; erros na entrega precisam ser observáveis e recuperáveis.

## Classificação híbrida

Regras explícitas dão um baseline previsível. A classificação assistida por IA foi avaliada como experimento para interpretar textos variados e evidências do anúncio.

**Trade-off:** respostas plausíveis ainda podem ser inválidas, e a concordância em uma amostra pequena não demonstra precisão real. Por isso, a avaliação inclui estrutura da resposta, evidências e comparação humana; os testes offline não alteram automaticamente o filtro de produção.

## CI/CD com federação de identidade

O desenho de deploy usa GitHub Actions com OIDC/Workload Identity Federation, evitando manter uma chave de conta de serviço de longa duração no repositório.

**Trade-off:** as permissões concedidas ao workflow precisam ser restritas por repositório, branch e ambiente. Workflows de pull request não confiáveis não devem receber acesso de escrita ao estado de produção.

## Imagem imutável, smoke test e rollback

A imagem deve ser referenciada por digest para que promoção e rollback apontem para o mesmo artefato. Após o deploy, smoke tests devem validar os serviços afetados.

**Ponto de atenção:** o Job e a API são serviços separados. Atualização e rollback precisam cobrir ambos; validar somente o Job pode deixar a API executando uma imagem antiga.
