# Estratégia de integração das fontes

A integração de uma fonte depende de mais do que encontrar uma página pública. O coletor precisa funcionar a partir do ambiente de execução, gerar links úteis e fornecer dados que possam ser normalizados sem depender de uma sessão de candidato.

## Solides — incluída

O projeto adicionou um coletor público da Solides e o integrou ao fluxo do Job Hunter, incluindo busca por palavras-chave. A implementação usa dados do portal público e URLs canônicas no formato `https://vagas.solides.com.br/vaga/{id}`; não foi usada conta de candidato.

**Motivação:** entregar links estáveis e evitar URLs não canônicas que quebram quando a pessoa abre a notificação.

## Recrutei

Em uma configuração anterior, requisições do Cloud Run receberam HTTP 403 e o coletor ficou desativado. A configuração atual voltou a habilitar a fonte. O resultado de cada execução depende da resposta do portal e deve ser acompanhado no diagnóstico por coletor.

## Falhas parciais

O resumo separa avisos, erros e resultados de coleta. Uma falha em uma fonte pode produzir um resultado parcial enquanto as demais continuam sendo processadas.

## Critérios para uma nova fonte

- Pode ser consultada de forma autorizada e sustentável pelo runtime?
- Falhas da fonte podem ser diferenciadas de uma busca válida sem resultados?
- Os dados permitem deduplicação e atualização?
- A URL entregue é canônica e abre a vaga correta?
- O coletor evita credenciais e dados de sessão de candidato?
- Há testes para normalização, falhas HTTP e registros incompletos?

Disponibilidade de portais muda. Os estados acima descrevem as decisões documentadas do projeto e devem ser revalidados antes de qualquer afirmação sobre execução atual.
