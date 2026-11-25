# CT-G01-02 — Cancelar Chamado

## Identificação

| Campo | Valor |
| --- | --- |
| **ID** | CT-G01-02 |
| **Requisito rastreado** | RF-G01-13 |
| **Caso de uso** | UC-G01-04 |
| **Tipo** | funcional |
| **Prioridade** | alta |

## Objetivo

Verificar que um chamado em estado permitido pode ser cancelado com motivo, registrando o responsável e emitindo o evento correspondente.

## Pré-condições

- O chamado `CH-002` está registrado com status `Aberto`.
- O ator `TASI-001` está identificado no sistema.

## Dados de entrada

| Campo | Valor |
| --- | --- |
| `idChamado` | CH-002 |
| `statusAtual` | Aberto |
| `motivoCancelamento` | Solicitação de cancelamento pelo solicitante. |
| `responsavel` | TASI-001 |

## Passos

1. Selecionar o chamado `CH-002`.
2. Informar o motivo do cancelamento.
3. Confirmar o cancelamento.
4. Verificar o status do chamado, os dados registrados e o evento de auditoria.

## Resultado esperado

- O cancelamento é aceito.
- O status do chamado passa para `Cancelado`.
- O motivo, o responsável e a data/hora são registrados.
- O evento `CHAMADO_CANCELADO` é emitido.

## Resultado obtido

não executado

## Critério de aceite

Passou se o status for alterado para `Cancelado`, o motivo, responsável e data/hora forem registrados e o evento `CHAMADO_CANCELADO` for emitido; caso contrário, falhou.

## Observações

Teste ainda não executado. O cenário usa o status `Aberto`, permitido pelo RF-G01-13.
