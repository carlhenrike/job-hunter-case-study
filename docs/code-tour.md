# Tour pelo código

Este guia mostra a organização real do projeto privado e trechos representativos para explicar o fluxo. Ele não reproduz o aplicativo completo nem contém configurações de produção. Os exemplos são simplificados; não são um pacote executável.

## Mapa de módulos

```text
src/job_hunter/
├── collectors/          # adaptadores de coleta por fonte
├── notifications/       # canais de notificação
├── api.py               # API HTTP e endpoints de consulta
├── cli.py               # comandos e coordenação das execuções
├── config.py            # leitura e validação da configuração local
├── dashboard.py         # interface de consulta
├── filter_evaluation.py # avaliação offline do filtro
├── job_quality.py       # canonicalização e deduplicação entre fontes
├── models.py            # modelos Job e ScoreResult
├── scoring.py           # regras explicáveis de relevância
├── service.py           # análise, deduplicação e persistência
├── storage.py           # contrato Repository, SQLite e Firestore
└── text.py              # normalização e tratamento de texto

tests/                   # testes unitários e de integração com dependências simuladas
scripts/                 # utilitários de avaliação e manutenção
infra/terraform/         # recursos e permissões de infraestrutura
```

A lista omite configurações reais e arquivos de dados. O perfil de busca — por exemplo, termos, localização e faixa salarial — fica fora deste repositório público.

## Modelo de domínio

O modelo transforma anúncios de diferentes fontes em uma estrutura comum. Exemplo representativo com dados fictícios:

```python
from dataclasses import dataclass, field


@dataclass(slots=True)
class Job:
    external_id: str
    title: str
    company: str
    location: str
    description: str
    url: str
    source: str
    workplace_type: str = "unspecified"
    match_score: int = 0
    score_reasons: list[str] = field(default_factory=list)
    score_warnings: list[str] = field(default_factory=list)
```

O coletor preenche os campos da fonte; as etapas seguintes trabalham com o modelo comum. Identificadores e links acima são campos do domínio, não valores de produção.

## Fluxo de processamento

Versão resumida do fluxo existente:

```python
inserted, updated, analyzed, new_jobs = analyze_and_save(
    collected_jobs, search_profile, repository
)
notify([job for job in new_jobs if job.match_score >= alert_threshold])
```

A implementação real trata falhas por fonte, registros repetidos, atualizações e histórico de execução. O trecho representa a chamada do serviço existente, mas omite a coleta, as regras de alerta e detalhes de infraestrutura de propósito.

## Limite de persistência

O serviço depende de um contrato de repositório. Isso permite usar SQLite no desenvolvimento local e nos testes e Firestore na execução em nuvem, sem espalhar consultas específicas de banco pelo fluxo de negócio.

```python
class Repository(Protocol):
    def initialize(self) -> None: ...
    def existing_keys(self) -> set[tuple[str, str]]: ...
    def upsert_many(self, jobs: Iterable[Job]) -> tuple[int, int]: ...
    def list_jobs(self, min_score: int = 0, limit: int = 20) -> list[dict]: ...
```

## Como os testes ajudam

Os testes verificam comportamentos isolados, como normalização de uma fonte, cálculo do score, deduplicação, persistência, tratamento de falhas e respostas da API. Serviços externos e banco podem ser substituídos por objetos simulados nos testes, evitando credenciais e efeitos reais.

## O que foi deliberadamente omitido

- Perfil pessoal de busca e regras com valores reais.
- URLs de execução, IDs de projeto, nomes de recursos e valores de ambiente.
- Tokens, credenciais OAuth, dados de vagas e resultados associados a anúncios identificáveis.
- Arquivos de avaliação e exemplos extraídos de dados privados.
