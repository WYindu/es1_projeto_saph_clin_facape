# CT-G01-01 — Reclassificar Chamado

## Identificação

| Campo | Valor |
| --- | --- |
| **ID** | CT-G01-01 |
| **Requisito rastreado** | RF-G01-11 |
| **Caso de uso** | UC-G01-03 |
| **Tipo** | funcional |
| **Prioridade** | alta |

## Objetivo

Verificar que um chamado já classificado pode ser reclassificado com justificativa, registrando a nova triagem e preservando o histórico anterior.

## Pré-condições

- O chamado `CH-001` está registrado e classificado como `AMARELO`.
- O ator Médico Regulador `MED-001` está identificado no sistema.

## Dados de entrada

| Campo | Valor |
| --- | --- |
| `idChamado` | CH-001 |
| `prioridadeAtual` | AMARELO |
| `novaPrioridade` | LARANJA |
| `justificativa` | Agravamento do quadro clínico identificado durante nova avaliação. |
| `responsavel` | MED-001 |

## Passos

1. Selecionar o chamado `CH-001`.
2. Informar a nova prioridade `LARANJA`.
3. Informar a justificativa clínica.
4. Confirmar a reclassificação.
5. Verificar o registro da nova triagem, a preservação da classificação anterior e o evento de auditoria.

## Resultado esperado

- A reclassificação é aceita.
- Uma nova triagem é registrada com a nova prioridade, justificativa, responsável e data/hora.
- A classificação anterior permanece no histórico.
- O evento `PRIORIDADE_RECLASSIFICADA` é emitido.

## Resultado obtido

não executado

## Critério de aceite

Passou se a nova triagem e seus dados forem registrados, a classificação anterior permanecer no histórico e o evento `PRIORIDADE_RECLASSIFICADA` for emitido; caso contrário, falhou.

## Observações

Os dados são ilustrativos para teste e devem respeitar os critérios clínicos do protocolo Manchester. Teste ainda não executado.
