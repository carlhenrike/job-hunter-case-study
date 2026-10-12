# Qualidade e validação

## Evidências disponíveis

Os valores abaixo vêm de uma avaliação offline exploratória registrada em outubro de 2026. O conjunto tem apenas 20 casos e não é publicado.

| Abordagem | Resultado observado | Leitura correta |
|---|---:|---|
| Regras offline (baseline) | 14/20 | Referência inicial para comparação |
| Prompt Gemini v2 | 12/20 | Abaixo do baseline nesse conjunto |
| Prompt v3 + perfil v2: respostas estruturalmente válidas | 15/20 | Formato correto não significa classificação correta |
| Prompt v3 + perfil v2: concordância humana | 11/20 | Concordância limitada na amostra |

Essa amostra é pequena e pode não representar vagas novas, diferentes fontes, idiomas ou alterações futuras nos anúncios. Não use esses números como promessa de precisão.

## Como avaliar

1. Fixar um conjunto de casos e registrar os critérios antes de testar.
2. Comparar a classificação com uma avaliação humana independente.
3. Validar se cada justificativa aponta evidência presente no anúncio.
4. Testar casos novos fora do conjunto usado para ajustar o prompt.
5. Separar respostas inválidas, desacordo e falha de evidência.
6. Manter o baseline disponível e não substituir o filtro de produção sem validação.

## Estado

O experimento ajudou a identificar que consistência estrutural e concordância semântica são medidas diferentes. O filtro de produção permanece baseado em comportamento validado separadamente; o experimento com IA não é apresentado como ganho de precisão.

Dados de vagas, prompts completos e arquivos de avaliação permanecem privados. Nenhuma informação de candidatos ou dados pessoais é incluída neste repositório.
