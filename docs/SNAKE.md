# Atualização da cobrinha

O workflow `.github/workflows/snake.yml` gera as versões clara e escura e publica na branch `output`. O README usa esses arquivos diretamente.

## Quando executa

- A cada seis horas, no minuto 37: 00:37, 06:37, 12:37 e 18:37 UTC.
- Ao alterar o workflow ou o README na `main`.
- Manualmente em Actions → Generate contribution snake → Run workflow.

Os horários agendados pelo GitHub podem sofrer atraso. O job tem limite de dez minutos e execuções concorrentes são serializadas para não disputar a publicação.

## Diagnóstico

O badge no perfil abre o histórico de execuções. Cada execução bem-sucedida informa a geração/publicação no resumo. Para conferir a imagem efetivamente publicada, consulte o último commit da branch `output`. Sem alteração no SVG, não é criado um commit vazio.

Em 15/10/2026, o arquivo publicado ainda era de 06/08/2026. O histórico disponível tinha duas execuções manuais e nenhuma agendada, embora o workflow estivesse marcado como ativo e possuísse um cron diário. Isso confirma a imagem antiga, mas não comprova a causa da ausência de disparos. Não foi atribuído automaticamente à regra de inatividade.

O agendamento foi atualizado, preservando a geração oficial do Platane/snk e a publicação existente. As ações foram fixadas nos commits correspondentes às versões v3/v5 consultadas, e os dois SVGs são validados antes da publicação. O histórico da branch `output` é mantido. Não é necessário token pessoal: usa-se o `GITHUB_TOKEN` temporário do job.

Se o repositório público ficar sem atividade por 60 dias, o GitHub pode desabilitar agendamentos. Nesse caso, habilite novamente o workflow em Actions. Não se criam commits artificiais na `main` para evitar essa regra.

Referências: [agendamentos](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule), [habilitar workflows](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/disable-and-enable-workflows), [Platane/snk](https://github.com/Platane/snk).
