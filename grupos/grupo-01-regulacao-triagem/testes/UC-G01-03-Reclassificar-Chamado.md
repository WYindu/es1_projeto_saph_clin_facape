# UC-G01-03 — Reclassificar Chamado

## Identificação

| Campo | Valor |
| --- | --- |
| **ID** | UC-G01-03 |
| **Módulo** | G01 — Regulação e Triagem |
| **Ator primário** | Médico Regulador |
| **Atores secundários** | G6 — Auditoria |
| **Objetivo** | Reclassificar o risco de um chamado já classificado, registrando a nova prioridade e preservando o histórico de triagens. |
| **Requisitos rastreados** | RF-G01-11 |
| **Prioridade** | essencial |

## Pré-condições

- O chamado está registrado e já possui classificação de risco.
- O Médico Regulador está identificado no sistema.

## Pós-condições

- **Sucesso:** uma nova triagem é registrada com a nova classificação, justificativa, responsável e data/hora, mantendo as classificações anteriores no histórico.
- **Falha:** nenhuma nova triagem é registrada e a classificação anterior permanece válida.

## Fluxo principal

1. O Médico Regulador seleciona um chamado já classificado.
2. O sistema apresenta os dados do chamado e sua classificação atual.
3. O Médico Regulador informa a nova classificação de risco conforme o protocolo Manchester.
4. O Médico Regulador informa a justificativa clínica para a reclassificação.
5. O sistema identifica o Médico Regulador responsável.
6. O sistema valida os dados informados.
7. O sistema registra uma nova triagem vinculada ao chamado.
8. O sistema preserva a triagem e a classificação anteriores no histórico.
9. O sistema registra a data e hora da reclassificação.
10. O sistema emite o evento `PRIORIDADE_RECLASSIFICADA`.
11. O sistema confirma a reclassificação.

## Fluxos alternativos

### FA-01 — Reclassificação para uma prioridade diferente

- **Condição:** o Médico Regulador identifica alteração no quadro clínico que exige mudança de prioridade.
- **Passos:** o Médico Regulador seleciona a nova prioridade, informa a justificativa e confirma a reclassificação.
- **Reconvergência:** retorna ao passo 6 do fluxo principal.

## Fluxos de exceção

### FE-01 — Justificativa não informada

- **Condição:** o Médico Regulador tenta confirmar a reclassificação sem informar justificativa.
- **Passos:** o sistema rejeita a operação e informa que a justificativa é obrigatória.
- **Estado final:** a classificação anterior permanece válida e nenhuma nova triagem é registrada.

### FE-02 — Dados inválidos

- **Condição:** os dados informados não atendem às validações do sistema.
- **Passos:** o sistema informa a inconsistência e não efetiva a reclassificação.
- **Estado final:** o chamado mantém sua classificação anterior e seu histórico.

## Regras de negócio aplicadas

| Regra | Descrição |
| --- | --- |
| RN-02 | A prioridade do chamado não pode ser sobrescrita silenciosamente; a reclassificação exige justificativa e identificação do responsável. |
| RN-09 | Ações sensíveis devem gerar evento de auditoria com autor, ação, entidade e data/hora. |

## Relacionamentos

- `<<extend>>` UC-G01-02 — Classificar Risco: a reclassificação é uma extensão do processo de classificação e ocorre quando um chamado já classificado necessita de nova avaliação.

## Dados manipulados

| Entidade | Dado | Origem |
| --- | --- | --- |
| Chamado | `idChamado` | G1 |
| Triagem | classificação de risco atual | G1 |
| Triagem | nova classificação de risco | Médico Regulador |
| Triagem | justificativa clínica | Médico Regulador |
| Triagem | responsável | Sistema |
| Triagem | data/hora | Sistema |

## Critérios de aceite

- [ ] Permite reclassificar somente chamado que já possui classificação.
- [ ] Exige justificativa clínica para a reclassificação.
- [ ] Registra a identificação do Médico Regulador responsável.
- [ ] Cria nova triagem sem apagar as classificações anteriores.
- [ ] Emite o evento `PRIORIDADE_RECLASSIFICADA`.
- [ ] O histórico da reclassificação fica disponível conforme o RNF aplicável.

## Rastreabilidade

| RF | Classe | Sequência | Teste |
| --- | --- | --- | --- |
| RF-G01-11 | TelaTriagem, ControladorTriagem, Chamado, Triagem | Pendente | CT-G01-01 |
