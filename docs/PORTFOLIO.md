# Guia dos projetos

[Voltar ao perfil](https://github.com/daniel-moulaz)

Um caminho curto para conhecer cada produto e, se quiser aprofundar, chegar às decisões e aos testes que estão no repositório. As demonstrações e os testes têm escopos diferentes: uma interface navegável não comprova o funcionamento de toda a infraestrutura.

## Aresta

**Problema:** acompanhar uma integração quando o destino falha, a origem reenvia um evento ou o payload precisa ser transformado.

**Para conhecer:** abra a [demo](https://aresta.daniel-moulaz.workers.dev), filtre os eventos por falha e consulte os detalhes de uma execução. O painel público usa dados demonstrativos; o [modo Docker](https://github.com/daniel-moulaz/aresta#rodando-o-projeto) conecta a API Python e o PostgreSQL.

| Escolha | Onde conferir |
| --- | --- |
| Chave de idempotência por fluxo para identificar reenvios | [Constraint no modelo](https://github.com/daniel-moulaz/aresta/blob/main/services/api/app/models.py) e [teste da API](https://github.com/daniel-moulaz/aresta/blob/main/services/api/tests/test_api.py) |
| Separação entre erro definitivo e recuperável antes de permitir retry | [ADR](https://github.com/daniel-moulaz/aresta/blob/main/docs/decisions/0001-idempotency-and-retries.md) e [motor de processamento](https://github.com/daniel-moulaz/aresta/blob/main/services/api/app/services/engine.py) |
| Verificação de assinatura de payloads | [Implementação HMAC](https://github.com/daniel-moulaz/aresta/blob/main/services/api/app/services/signatures.py) e [testes](https://github.com/daniel-moulaz/aresta/blob/main/services/api/tests/test_signatures.py) |

**Limite atual:** autenticação por organização, adaptadores de destino configuráveis e fila de retry com backoff estão no roadmap. A demo não deve ser usada para enviar dados sensíveis. [Arquitetura](https://github.com/daniel-moulaz/aresta/blob/main/docs/architecture.md) · [CI](https://github.com/daniel-moulaz/aresta/actions/workflows/ci.yml).

## SEPTEM Cinemas

**Problema:** reservar assentos e validar ingressos sem permitir duas alocações ativas para o mesmo lugar ou dois usos do mesmo ingresso.

**Para conhecer:** veja o [roteiro com telas](https://github.com/daniel-moulaz/septem-cinemas/blob/main/docs/DEMO.md) e a [demo online](https://septem-cinemas.vercel.app). A programação pública depende das sessões cadastradas; o roteiro permanece disponível quando não há sessões futuras.

| Escolha | Onde conferir |
| --- | --- |
| Locks, revalidação transacional e índice único parcial para assentos | [Prova de concorrência](https://github.com/daniel-moulaz/septem-cinemas/blob/main/docs/CONCURRENCY-PROOF.md) e [teste de reservas](https://github.com/daniel-moulaz/septem-cinemas/blob/main/apps/api/tests/reservations.integration.test.ts) |
| Consumo do ingresso condicionado ao estado no banco | [Testes de portaria](https://github.com/daniel-moulaz/septem-cinemas/blob/main/apps/api/tests/gate.integration.test.ts) |
| SSE para avisar mudanças, com banco como autoridade sobre a reserva | [Arquitetura](https://github.com/daniel-moulaz/septem-cinemas/blob/main/docs/ARCHITECTURE.md) e [testes de eventos](https://github.com/daniel-moulaz/septem-cinemas/blob/main/apps/api/tests/session-events.integration.test.ts) |

**Limite atual:** pagamentos são simulados, a demo compartilha dados entre visitantes e não há benchmark de carga publicado. A prova de concorrência cobre cenários definidos, não todos os casos possíveis. [Limitações](https://github.com/daniel-moulaz/septem-cinemas/blob/main/docs/KNOWN-LIMITATIONS.md) · [CI](https://github.com/daniel-moulaz/septem-cinemas/actions/workflows/ci.yml).

## Nexo

**Problema:** entender saldo, consumo e compromissos futuros sem confundir transferência ou pagamento de fatura com uma nova despesa.

**Para conhecer:** veja as [telas com dados fictícios](https://github.com/daniel-moulaz/Nexo#telas) e as [instruções para executar](https://github.com/daniel-moulaz/Nexo#executar-o-projeto). O aplicativo é local; não há demo web hospedada.

| Escolha | Onde conferir |
| --- | --- |
| Regras financeiras compartilhadas, com persistência SQLite por plataforma | [Regras](https://github.com/daniel-moulaz/Nexo/blob/main/lib/services/finance_rules.dart), [plataforma do banco](https://github.com/daniel-moulaz/Nexo/blob/main/lib/database/database_platform.dart) e [testes contábeis](https://github.com/daniel-moulaz/Nexo/blob/main/test/ledger_test.dart) |
| Previsão baseada em compromissos cadastrados, sem inventar renda futura | [Linha do tempo](https://github.com/daniel-moulaz/Nexo/blob/main/lib/services/cash_timeline.dart) e [testes](https://github.com/daniel-moulaz/Nexo/blob/main/test/cash_timeline_test.dart) |
| Sugestões (inclusive do Open Finance) passam pela caixa "Para revisar" antes de virar lançamento | [Caixa de sugestões](https://github.com/daniel-moulaz/Nexo/blob/main/lib/services/notification_inbox.dart), [testes de revisão](https://github.com/daniel-moulaz/Nexo/blob/main/test/notification_review_screen_test.dart) e [testes da conciliação](https://github.com/daniel-moulaz/Nexo/blob/main/test/inbox_reconciler_test.dart) |

**Limite atual:** Android em uso; homologação completa do Windows em andamento. A sincronização entre celular e PC é opcional, usa um projeto Supabase próprio e não tem criptografia de ponta a ponta ([revisão de segurança](https://github.com/daniel-moulaz/Nexo/blob/main/docs/SEGURANCA.md)).

O Nexo foi construído a partir de uma base open source sob licença MIT, o [Expensy de Mina Android](https://github.com/mina-android/Expensy), com os créditos na [LICENSE](https://github.com/daniel-moulaz/Nexo/blob/main/LICENSE). Mais detalhes no [README](https://github.com/daniel-moulaz/Nexo#desenvolvimento).

## Merid

**Problema:** separar o prazo de uma tarefa do horário reservado para executá-la e enxergar o que cabe no dia.

**Para conhecer:** siga o [fluxo principal](https://github.com/daniel-moulaz/merid#experimente-o-fluxo-principal) em um ambiente local. O piloto documentado foi validado em Android físico; o código usa Expo para Android, iOS e Web, sem uma demo pública hospedada anunciada no repositório.

| Escolha | Onde conferir |
| --- | --- |
| Tarefa e bloco de agenda são entidades separadas | [Modelo e FK composta](https://github.com/daniel-moulaz/merid/blob/main/supabase/migrations/20260829160000_schedule_blocks.sql) e [testes de integração](https://github.com/daniel-moulaz/merid/blob/main/tests/rls/schedule-blocks.rls.test.ts) |
| Isolamento por usuário no PostgreSQL, além da validação do formulário | [Testes de RLS](https://github.com/daniel-moulaz/merid/tree/main/tests/rls) e [arquitetura](https://github.com/daniel-moulaz/merid/blob/main/docs/ARCHITECTURE.md) |
| Datas civis e instantes têm semânticas distintas; o dia depende do fuso do perfil | [Testes de timezone](https://github.com/daniel-moulaz/merid/blob/main/src/lib/time/time-zone.test.ts) |
| Blocos que só encostam não conflitam; sobreposições geram aviso | [Testes de intervalos](https://github.com/daniel-moulaz/merid/blob/main/src/modules/schedule/domain/overlap.test.ts) e [política de conflito](https://github.com/daniel-moulaz/merid/blob/main/src/modules/schedule/domain/conflict-policy.ts) |

**Limite atual:** recorrência, notificações, colaboração e sincronização com calendários externos estão fora do primeiro piloto. [Escopo do produto](https://github.com/daniel-moulaz/merid/blob/main/docs/PRODUCT.md) · [Como executar os testes](https://github.com/daniel-moulaz/merid#scripts).

## Contato

[LinkedIn](https://www.linkedin.com/in/daniel-moulaz/) · [E-mail](mailto:danielmoulaz7@gmail.com)
