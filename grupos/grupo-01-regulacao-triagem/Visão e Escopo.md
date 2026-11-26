# Escopo e Visão

**Qual o problema que este modulo resolve:** O módulo busca solucionar as dificuldades no registro e na organização das ocorrências de emergência, cujas informações podem ser anotadas em papel ou transmitidas por rádio, o que dificulta sua centralização, rastreabilidade e auditoria.
Além disso, a ausência de um processo formalizado de triagem e classificação de risco pode comprometer a identificação da gravidade dos casos e levar a decisões inadequadas no direcionamento dos recursos de atendimento, com possível atraso em casos críticos.
Para isso, o módulo(G1) registra cada ocorrência como um chamado único, com identificador e histórico, e padroniza a classificação de risco pelo Protocolo de Manchester, decidida pelo Médico Regulador.


### FRONTEIRA DO SISTEMA

```
.......... FRONTEIRA DO SISTEMA ..........

+----------------+     +--------------------+     +--------------------+
|    ENTRADAS    |---->|   PROCESSAMENTO    |---->|       SAÍDAS       |
| Ocorrência     |     | Médico Regulador   |     | Chamado            |
| Acolhimento    |     | faz a triagem      |     | classificado       |
| Abre o chamado |     | (Manchester)       |     | enviado ao G2      |
| (via TASI)     |     +--------------------+     +--------------------+
+----------------+                                          |
        ^           +--------------------+                  |
        |           |      FEEDBACK      |                  |
        |           | Novas informações, |                  |
        |           | agravamento ou     |                  |
        +- - - - - -| cancelamento       |<- - - - - - - - -+
                    +--------------------+
```



**Fronteira de entrada:** quando a ocorrência é recebida, é feito o acolhimento, que registra os dados minimos e vincula o chamado a um TASI. O TASI abre o chamado e, a partir daí, esses dados, vindos do ambiente externo (solicitante), cruzam a fronteira e passam a ser tratados pelo sistema. Em termos de software, equivale a uma requisição à API.

**Processamento:** o Médico Regulador analisa os dados, realiza a triagem e classifica o risco pelo protocolo de Manchester. Equivale ao backend, onde ficam as regras de negócio.

**Fronteira de saída:** o chamado classificado, com sua prioridade Manchester, é entregue ao módulo de Despacho e Frota (G2), que decide qual viatura enviar. Equivale à resposta da API (ou a um evento enviado ao G2).

**Feedback:** durante o atendimento, podem surgir novas informações ou o agravamento da condição do paciente, e o processo retorna à fronteira de entrada. Ao chegar ao processamento, o Médico Regulador pode reclassificar o risco, se necessário, mantendo um ciclo de retroalimentação conforme a evolução do caso. Equivale a uma nova requisição que atualiza o mesmo chamado.


# **O que está fora do escopo:**
- Despacho e gerenciamento de viaturas.
- Acompanhamento do deslocamento das equipes.
- Prontuário e telemedicina.
- Gerenciamento de leitos hospitalares.
- Controle de estoque e medicamentos.

#  Premissas:
- O acolhimento coleta os dados mínimos (solicitante, localização e queixa) e vincula o chamado a um TASI, que abre o chamado.
-  O TASI realiza o registro inicial da ocorrência e não decide conduta clínica nem atribui prioridade.
-   O Médico Regulador é o único responsável pela triagem (classificação de risco) e pela reclassificação, segundo o Protocolo de Manchester.
- Todo chamado classificado possui identificador, prioridade, profissional responsável e data/hora da classificação registrados.
-  Cada reclassificação gera um novo registro de triagem, preservando o histórico do mesmo chamado.
- O sistema emite eventos para comunicar classificação, cancelamento e reclassificação aos demais módulos, como o Despacho e Frota (G2), que só despacha chamado classificado.
