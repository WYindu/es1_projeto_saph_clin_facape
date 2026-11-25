# UC-G01-04 — Cancelar Chamado

## Identificação

| Campo | Valor |
| --- | --- |
| **ID** | UC-G01-04 |
| **Módulo** | G01 — Regulação e Triagem |
| **Ator primário** | TASI ou Médico Regulador |
| **Atores secundários** | G6 — Auditoria |
| **Objetivo** | Cancelar um chamado que esteja em estado permitido, registrando o motivo e o responsável pela ação. |
| **Requisitos rastreados** | RF-G01-13 |
| **Prioridade** | essencial |

## Pré-condições

- O chamado está registrado no sistema.
- O chamado encontra-se com status `Aberto` ou `Em Triagem`.
- O ator está identificado no sistema.

## Pós-condições

- **Sucesso:** o chamado passa para o status `Cancelado`, com motivo, responsável e data/hora registrados.
- **Falha:** o status do chamado permanece inalterado e o cancelamento não é efetivado.

## Fluxo principal

1. O ator seleciona o chamado que deseja cancelar.
2. O sistema apresenta os dados e o status atual do chamado.
3. O sistema verifica se o chamado está em estado permitido para cancelamento.
4. O ator informa o motivo do cancelamento.
5. O sistema identifica o ator responsável.
6. O sistema valida as informações do cancelamento.
7. O sistema registra o motivo, o responsável e a data/hora.
8. O sistema altera o status do chamado para `Cancelado`.
9. O sistema emite o evento `CHAMADO_CANCELADO`.
10. O sistema confirma o cancelamento.

## Fluxos alternativos

### FA-01 — Cancelamento solicitado pelo ator

- **Condição:** o ator solicita o cancelamento de um chamado que está em estado permitido.
- **Passos:** o ator informa o motivo do cancelamento e confirma a operação.
- **Reconvergência:** retorna ao passo 5 do fluxo principal.

## Fluxos de exceção

### FE-01 — Motivo não informado

- **Condição:** o ator tenta confirmar o cancelamento sem informar o motivo.
- **Passos:** o sistema rejeita a operação e informa que o motivo é obrigatório.
- **Estado final:** o chamado mantém o status anterior e não é cancelado.

### FE-02 — Status não permite cancelamento

- **Condição:** o chamado está em estado diferente de `Aberto` ou `Em Triagem`.
- **Passos:** o sistema rejeita o cancelamento e informa que o status atual não permite a operação.
- **Estado final:** o chamado mantém seu status atual e o cancelamento não é efetivado.

## Regras de negócio aplicadas

| Regra | Descrição |
| --- | --- |
| RN-09 | Ações sensíveis devem gerar evento de auditoria com autor, ação, entidade e data/hora. |

## Relacionamentos

- Não possui relacionamentos `<<include>>` ou `<<extend>>` identificados.

## Dados manipulados

| Entidade | Dado | Origem |
| --- | --- | --- |
| Chamado | `idChamado` | G1 |
| Chamado | status atual | Sistema |
| Chamado | motivo do cancelamento | Ator |
| Chamado | responsável pelo cancelamento | Sistema |
| Chamado | data/hora do cancelamento | Sistema |

## Critérios de aceite

- [ ] Permite cancelar somente chamados com status `Aberto` ou `Em Triagem`.
- [ ] Exige motivo para o cancelamento.
- [ ] Registra a identificação do responsável.
- [ ] Altera o status do chamado para `Cancelado`.
- [ ] Emite o evento `CHAMADO_CANCELADO`.
- [ ] Rejeita cancelamentos em status não permitido sem alterar o estado do chamado.

## Rastreabilidade

| RF | Classe | Sequência | Teste |
| --- | --- | --- | --- |
| RF-G01-13 | TelaGerenciamentoChamado, ControladorChamado, Chamado | Pendente | CT-G01-02 |
