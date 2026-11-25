### **RF-G01-01: Registrar Chamado**
* **Descrição curta**: Registrar uma nova ocorrência de emergência no acolhimento, coletando obrigatoriamente os dados do solicitante, a localização física do fato e a queixa principal.
* **Caso de uso**: `CU-01: Registrar Chamado` (Ator Primário: `TASI / Atendente`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `TelaAcolhimento`
  * *Controle (Control)*: `ControladorChamado`
  * *Entidade (Entity)*: `Chamado`, `Solicitante`
* **Sequência**:
  1. `TASI` → `TelaAcolhimento`: `registrarChamado(dadosSolicitante, localizacao, queixa)`
  2. `TelaAcolhimento` → `ControladorChamado`: `criarChamado(dados)`
  3. `ControladorChamado` → `Solicitante`: `new Solicitante(dadosSolicitante)`
  4. `ControladorChamado` → `Chamado`: `new Chamado(solicitante, localizacao, queixa)`
  5. `Chamado` → `ControladorChamado`: `chamadoCriado(idChamado)`
  6. `ControladorChamado` → `TelaAcolhimento`: `confirmarRegistro(idChamado)`
* **Funcionamento**:
  * É a porta de entrada do sistema. O TASI (Atendente) recebe a chamada telefônica da emergência e preenche os **dados mínimos essenciais**: *quem liga (solicitante)*, *onde é o fato (localização)* e *o que está acontecendo (queixa principal)*. O sistema valida se nada ficou em branco, grava o chamado com o status inicial **"Aberto"** e o disponibiliza na fila para a análise médica.
 

---

### **RF-G01-02: Classificar Risco pelo Protocolo de Manchester**
* **Descrição curta**: Permitir que o Médico Regulador analise os dados clínicos do chamado e atribua o nível de gravidade padronizado em uma das cinco cores do Protocolo de Manchester (Vermelho, Laranja, Amarelo, Verde, Azul).
* **Caso de uso**: `CU-02: Classificar Risco` (Ator Primário: `Médico Regulador`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `TelaTriagem`
  * *Controle (Control)*: `ControladorTriagem`
  * *Entidade (Entity)*: `Chamado`, `Triagem`, `ClassificacaoRisco`
* **Sequência**:
  1. `MedicoRegulador` → `TelaTriagem`: `classificarRisco(idChamado, corManchester)`
  2. `TelaTriagem` → `ControladorTriagem`: `atribuirClassificacao(idChamado, corManchester, idMedico)`
  3. `ControladorTriagem` → `Triagem`: `new Triagem(corManchester, idMedico)`
  4. `ControladorTriagem` → `Chamado`: `adicionarTriagem(triagem)`
  5. `Chamado` → `Chamado`: `setStatus("Classificado")`
  6. `ControladorTriagem` → `TelaTriagem`: `confirmarClassificacao()`
* **Funcionamento**: 
  * O Médico Regulador seleciona um chamado da fila de espera e analisa a queixa clínica. Ele atribui a gravidade baseada nas 5 cores do Protocolo de Manchester (**Vermelho** \- Emergência, **Laranja** \- Muito Urgente, **Amarelo** \- Urgente, **Verde** \- Pouco Urgente, **Azul** \- Não Urgente). O sistema instancia um objeto de `Triagem`, vincula ao `Chamado` e altera o estado do chamado para **"Classificado"**.
---

### **RF-G01-03: Registrar Autor e Justificativa em Reclassificação**
* **Descrição curta**: Exigir a identificação do Médico Regulador autor e o registro da justificativa clínica detalhada em todas as operações de alteração do nível de risco de um chamado já triado.
* **Caso de uso**: `CU-03: Reclassificar Chamado` (`<<extend>>` de `CU-02`) (Ator Primário: `Médico Regulador`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `TelaTriagem`
  * *Controle (Control)*: `ControladorTriagem`
  * *Entidade (Entity)*: `Chamado`, `Triagem`
* **Sequência**:
  1. `MedicoRegulador` → `TelaTriagem`: `reclassificarRisco(idChamado, novaCor, justificativa)`
  2. `TelaTriagem` → `ControladorTriagem`: `processarReclassificacao(idChamado, novaCor, justificativa, idMedico)`
  3. `ControladorTriagem` → `Triagem`: `new Triagem(novaCor, justificativa, idMedico)`
  4. `ControladorTriagem` → `Chamado`: `adicionarNovaTriagem(triagem)`
  5. `ControladorTriagem` → `TelaTriagem`: `confirmarReclassificacao()`
* **Funcionamento**: 
  * Durante a espera ou atendimento, o quadro do paciente pode se agravar ou melhorar (ex.: paciente que era Amarelo sofre parada cardiorrespiratória e vira Vermelho). Ao alterar a cor, o sistema **bloqueia a ação caso o médico não digite a justificativa clínica**. A operação registra o ID do Médico Regulador e cria um **novo registro de triagem**, sem apagar a classificação anterior.
---

### **RF-G01-04: Detectar Chamado Duplicado**
* **Descrição curta**: Identificar a existência de chamados em andamento para a mesma localização e permitir a vinculação da nova ligação ao chamado principal como ocorrência duplicada.
* **Caso de uso**: `CU-04: Detectar Chamado Duplicado` (Ator Primário: `TASI / Sistema`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `TelaAcolhimento`
  * *Controle (Control)*: `ControladorChamado`
  * *Entidade (Entity)*: `Chamado`
* **Sequência**:
  1. `TASI` → `TelaAcolhimento`: `verificarDuplicidade(localizacao)`
  2. `TelaAcolhimento` → `ControladorChamado`: `buscarChamadosAtivosEm(localizacao)`
  3. `ControladorChamado` → `Chamado`: `findAtivosByLocalizacao(localizacao)`
  4. `Chamado` → `ControladorChamado`: `listaChamadosSimilares`
  5. `ControladorChamado` → `TelaAcolhimento`: `exibirChamadosSimilares()`
  6. `TASI` → `TelaAcolhimento`: `vincularAoPrincipal(idChamadoPrincipal)`
  7. `TelaAcolhimento` → `ControladorChamado`: `encerrarPorDuplicidade(idNovoChamado, idPrincipal)`
  8. `ControladorChamado` → `Chamado`: `setChamadoPrincipal(idPrincipal)`
* **Funcionamento**: 
  * Quando várias pessoas ligam para reportar o mesmo acidente (ex.: engavetamento em uma avenida), o sistema busca automaticamente chamados ativos no mesmo endereço. O TASI visualiza a lista de chamados similares e vincula a nova ligação como "duplicada" ao **Chamado Principal**. O novo chamado é encerrado por duplicidade, evitando a criação de ocorrências paralelas e o envio desnecessário de múltiplas frotas de viaturas.

---

### **RF-G01-05: Cancelar Chamado com Motivo Obrigatório**
* **Descrição curta**: Permitir o cancelamento de um chamado aberto ou em triagem mediante o preenchimento obrigatório da justificativa de cancelamento no sistema.
* **Caso de uso**: `CU-05: Cancelar Chamado` (Ator Primário: `TASI / Médico Regulador`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `TelaGerenciamentoChamado`
  * *Controle (Control)*: `ControladorChamado`
  * *Entidade (Entity)*: `Chamado`
* **Sequência**:
  1. `Usuario` → `TelaGerenciamentoChamado`: `cancelarChamado(idChamado, motivo)`
  2. `TelaGerenciamentoChamado` → `ControladorChamado`: `solicitarCancelamento(idChamado, motivo, idUsuario)`
  3. `ControladorChamado` → `Chamado`: `cancelar(motivo, idUsuario)`
  4. `Chamado` → `Chamado`: `setStatus("Cancelado")`
  5. `ControladorChamado` → `TelaGerenciamentoChamado`: `confirmarCancelamento()`
* **Funcionamento**: 
  * Usado em situações de trote, desistência do solicitante ou quando a vítima é socorrida por meios próprios antes da chegada da equipe. O usuário aciona o cancelamento, mas o sistema obriga o preenchimento do **motivo do cancelamento**. O chamado muda para o estado **"Cancelado"** e sai da esteira operacional de atendimento.
---

### **RF-G01-06: Manter Histórico Completo de Classificações**
* **Descrição curta**: Armazenar e disponibilizar para consulta a linha do tempo imutável com todos os registros de triagem e reclassificações associados a um chamado.
* **Caso de uso**: `CU-06: Consultar Histórico do Chamado` (Ator Primário: `Médico Regulador / Auditor`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `TelaConsultaHistorico`
  * *Controle (Control)*: `ControladorConsulta`
  * *Entidade (Entity)*: `Chamado`, `Triagem`
* **Sequência**:
  1. `Usuario` → `TelaConsultaHistorico`: `solicitarHistorico(idChamado)`
  2. `TelaConsultaHistorico` → `ControladorConsulta`: `getHistoricoTriagens(idChamado)`
  3. `ControladorConsulta` → `Chamado`: `getListaTriagens()`
  4. `Chamado` → `ControladorConsulta`: `listaDeTriagens`
  5. `ControladorConsulta` → `TelaConsultaHistorico`: `renderizarLinhaDoTempo(listaDeTriagens)`
* **Funcionamento**:
 * O sistema mantém uma lista imutável de todas as triagens (1..*) que um chamado recebeu ao longo do tempo. Quando um médico ou auditor aciona a consulta, o sistema recupera e exibe a linha do tempo completa: Cor inicial $\rightarrow$ Reclassificação 1 (com justificativa e horário) $\rightarrow$ Reclassificação 2. Nenhuma alteração apaga dados passados.

---

### **RF-G01-07: Alertar Tempo MÁXIMO de Triagem (RN-01)**
* **Descrição curta**: Disparar um alerta visual e sonoro no painel do regulador quando um chamado permanecer mais de 4 minutos no estado "Em Triagem" sem uma classificação atribuída.
* **Caso de uso**: `CU-07: Monitorar Tempo de Triagem` (Ator Primário: `Sistema / Temporizador`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `PainelAlertasRegulador`
  * *Controle (Control)*: `ServicoMonitoramentoTempo`
  * *Entidade (Entity)*: `Chamado`
* **Sequência**:
  1. `Temporizador` → `ServicoMonitoramentoTempo`: `verificarChamadosEmTriagem()`
  2. `ServicoMonitoramentoTempo` → `Chamado`: `getChamadosSemClassificacao()`
  3. `Chamado` → `ServicoMonitoramentoTempo`: `listaChamadosEmAberto`
  4. `ServicoMonitoramentoTempo` → `ServicoMonitoramentoTempo`: `validarTempoExcedido(tempoDecorrido > 4min)`
  5. `ServicoMonitoramentoTempo` → `PainelAlertasRegulador`: `notificarAlertaTriagemExcedida(idChamado, tempoDecorrido)`
* **Funcionamento**:
  * Um serviço automatizado em segundo plano (*timer*) checa continuamente os chamados que estão no estado "Em Triagem". Se um chamado ultrapassar **4 minutos** sem que o médico atribua uma cor de risco, o sistema gera um **alerta visual e sonoro destacado no painel do regulador**, chamando a atenção para casos críticos que não podem aguardar na fila.
---

### **RF-G01-08: Publicar Evento CHAMADO_CLASSIFICADO para G6**
* **Descrição curta**: Notificar assincronamente o Módulo G6 enviando o evento `CHAMADO_CLASSIFICADO` contendo o ID e a prioridade do chamado logo após a conclusão da triagem ou reclassificação.
* **Caso de uso**: `CU-08: Notificar Classificação de Chamado` (Ator Primário: `Sistema / Barramento`)
* **Classe(s)**:
  * *Fronteira (Boundary)*: `BarramentoEventosG6`
  * *Controle (Control)*: `ControladorTriagem`
  * *Entidade (Entity)*: `Chamado`, `Triagem`
* **Sequência**:
  1. `ControladorTriagem` → `Chamado`: `getUltimaTriagem()`
  2. `Chamado` → `ControladorTriagem`: `dadosTriagem`
  3. `ControladorTriagem` → `BarramentoEventosG6`: `publicarEventoChamadoClassificado(idChamado, corManchester, prioridade)`
  4. `BarramentoEventosG6` → `MóduloG6`: `emitirEvento("CHAMADO_CLASSIFICADO", payload)`
* **Funcionamento**:
  *  Assim que o Médico Regulador conclui a triagem (ou reclassificação), o backend do Módulo G1 emite uma mensagem assíncrona (`CHAMADO_CLASSIFICADO`) para o barramento do sistema. O **Módulo G6 (Despacho e Frota)** consome esse evento com a prioridade Manchester calculada para decidir qual viatura (USA - Suporte Avançado ou USB - Suporte Básico) enviar para o local.
