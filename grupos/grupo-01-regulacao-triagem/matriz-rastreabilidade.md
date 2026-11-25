# Matriz de Rastreabilidade — G01

> Preencha conforme o modelo em `docs/templates/matriz-rastreabilidade.md`.
> Célula vazia é pendência: use `pendente` explicitamente.

## Requisitos funcionais

| RF | Descrição curta | Caso de uso | Classe(s) | Sequência | Caso de teste | Status |
| --- | --- | --- | --- | --- | --- | --- |
| RF-G01-01 | Registrar chamado com solicitante, localização e queixa principal | UC-G01-01 | `TelaAcolhimento`, `ControladorChamado`, `Chamado`, `Solicitante`|[SEQUENCIA-G01-01](./requisitos/rf.md#L8) | | pendente |
| RF-G01-02 | Classificar risco nas cinco cores do Protocolo de Manchester | UC-G01-02 | `TelaTriagem`, `ControladorTriagem`, `Chamado`, `Triagem`, `ClassificacaoRisco` |[SEQUENCIA-G01-02](./requisitos/rf.md#L28) | | pendente |
| RF-G01-03 | Registrar autor e justificativa em toda reclassificação | UC-G01-03 | `TelaTriagem`, `ControladorTriagem`, `Chamado`, `Triagem`|[SEQUENCIA-G01-03](./requisitos/rf.md#L46) | | pendente |
| RF-G01-04 | Detectar chamado duplicado da mesma ocorrência | UC-G01-01 | `TelaAcolhimento`, `ControladorChamado`, `Chamado`|[SEQUENCIA-G01-04](./requisitos/rf.md#L63) | | pendente |
| RF-G01-05 | Cancelar chamado com motivo obrigatório | UC-G01-04 | `TelaGerenciamentoChamado`, `ControladorChamado`, `Chamado`|[SEQUENCIA-G01-05](./requisitos/rf.md#L84) | | pendente |
| RF-G01-06 | Manter histórico completo de classificações do chamado | UC-G01-02 | `TelaConsultaHistorico`, `ControladorConsulta`, `Chamado`, `Triagem`|[SEQUENCIA-G01-06](./requisitos/rf.md#L101) | | pendente |
| RF-G01-07 | Alertar regulador quando triagem exceder 4 minutos (RN-01) | UC-G01-02 | `PainelAlertasRegulador`, `ServicoMonitoramentoTempo`, `Chamado`|[SEQUENCIA-G01-07](./requisitos/rf.md#L119) | | pendente |
| RF-G01-08 | Publicar evento `CHAMADO_CLASSIFICADO` para G6 | UC-G01-02 | `BarramentoEventosG6`, `ControladorTriagem`, `Chamado`, `Triagem`|[SEQUENCIA-G01-08](./requisitos/rf.md#L136) | | pendente |


## Requisitos não funcionais

| RNF | Métrica e alvo | Como é verificado | Status |
| --- | --- | --- | --- |
| RNF-G01-01 | Acolhimento concluído em ≤ 90 s no fluxo principal | Executar testes de usabilidade E2E cronometrados com operadores e medir o tempo de resposta da API (p95 ≤ 120s) | pendente |
grupo-01/requisitos
| RNF-G01-02 | Triagem disponível mesmo com falha de rede local (registro offline) | Simular desconexão de rede durante a operação, validando o salvamento local e a sincronização posterior idempotente ao reconectar. | pendente |
| RNF-G01-03 | Dado do solicitante protegido por perfil de acesso (LGPD) | Executar testes automatizados de autorização na API por perfil (RBAC) garantindo mascaramento de dados e ausência de informações sensíveis nos logs. | pendente |
| RNF-G01-04 | Reclassificação visível no histórico em ≤ 3 s | Aplicar testes de carga (k6/JMeter) na API e medir a atualização em tempo real no painel do cliente via WebSockets. | pendente |
| RNF-G01-05 | Vocabulário de queixa alinhado ao glossário do domínio | Forçar validação de entrada por dicionário controlado na interface e realizar auditoria estática de conformidade na base de dados. | pendente |

## Contratos consumidos

| Contrato | Origem | Campo crítico |
| --- | --- | --- |
| — | Ligação do solicitante (fora do sistema) | — |

## Contratos fornecidos

| Contrato | Destino | Campo crítico |
| --- | --- | --- |
| — | Contrato C1 `chamado_classificado` para G2 | — |

## Pendências

| Item | Impacto | Prazo interno |
| --- | --- | --- |
| | | |
