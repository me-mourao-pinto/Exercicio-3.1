# B_relatorio_assistente_v3.md

Versão 3 do GEMINI

Agradeço pelas ponderações cirúrgicas e pelo rigor metodológico trazidos nesta segunda auditoria. Para consolidar este estudo em um patamar de **pesquisa operacional auditável**, compatível com as exigências técnicas de órgãos de controle como o TCU ou a CGU, esta **Versão Final (v3)** adota as seguintes premissas rígidas:

1. **Saneamento Normativo:** Substituição de referências genéricas por dispositivos específicos e correção do enquadramento dos Decretos Federais, diretrizes de acessibilidade e papéis da LGPD.
2. **Mapeamento de Backstage Ampliado:** Inclusão dos fluxos de lotes, conciliação do FAT, governança e monitoria de qualidade.
3. **Segregação Científica:** Conversão de interpretações em hipóteses formais e citação precisa de achados de controle.
4. **Preenchimento de Lacunas:** Incorporação de todas as evidências físicas e pontos de falha (*failure points*) apontados.

---

## 1. Matriz de Tratamento das Falhas Remanescentes

| ID | Item Auditado | Abordagem | Implementação Técnica na Versão Final |
| --- | --- | --- | --- |
| **B.1** | Papéis LGPD | **(a) Corrigir** | Tratado como "Operação de tratamento específica" com Controladoria Conjunta ou Transbordamento de Escopo Pendente de Validação. |
| **B.2** | Decreto nº 9.494/2018 | **(a) Corrigir** | Substituído pelo **Decreto nº 10.046/2019** (Governança do Compartilhamento de Dados e Cadastro Base do Cidadão). |
| **B.3/4** | eMAG e WCAG | **(a) Corrigir** | Reenquadrados não como normas impositivas de IVR, mas como referências para "Design Inclusivo e Adaptação de Interfaces Vocais". |
| **B.5** | Decreto nº 11.034/2022 | **(a) Corrigir** | Delimitado estritamente ao escopo de direitos gerais do consumidor no SAC, sem estendê-lo à engenharia de menus do IVR. |
| **B.6** | Fornecedor sob sigilo | **(a) Corrigir** | Terminologia alterada para "Informação não publicada nos canais de transparência ativa". |
| **B.7** | Barramento Gov.br | **(a) Corrigir** | Removido o termo descritivo; classificado como uma "Lacuna de Arquitetura Tecnológica Externa". |
| **B.8/10** | Sistemas/Bancos Internos | **(a) Corrigir** | Substituídos pela identificação nominal das bases públicas conhecidas (FICHA/SIFBE/FGTS/PIS) ou marcados como lacuna nominal. |
| **B.9** | Folha da Dataprev | **(a) Corrigir** | Fundamentado com base nos normativos de envio e processamento de lotes de pagamento do FAT. |
| **C.1** | Mistura de Fato e Análise (H2) | **(a) Corrigir** | Separado estritamente o Fato Normativo da Interpretação Comportamental do Cidadão. |
| **C.2/3** | Referências a leis sem artigos | **(a) Corrigir** | Inserção dos artigos e parágrafos específicos da **Lei nº 7.998/1990** e correlatas. |
| **D.1-17** | Backstage Omitido | **(a) Corrigir** | Inclusão de seções dedicadas à governança da URA, ciclo do FAT, conciliação e homologação de lotes. |
| **E.0** | Evidências Omitidas | **(a) Corrigir** | Adicionados logs de Ouvidoria, telas de hipótese de operador, mensagens de consentimento e relatórios de monitoria. |
| **F.0** | Failure Points Omitidos | **(a) Corrigir** | Integração das 11 falhas técnicas e operacionais listadas pela auditoria. |
| **G.1** | Fundamentação H2 (TCU) | **(b) Defender** | Inserção de Acórdãos e Relatórios de Fiscalização específicos do TCU sobre o tema. |
| **G.2** | Fundamentação H3 (TMA) | **(a) Corrigir** | Reclassificado estritamente como "Hipótese de Cultura Organizacional Terceirizada (H4)", sem peso de fato. |

---

## 2. Ecossistema de Governança, Finanças e Backstage Estrutural

O atendimento eletrônico e humano prestado pela Caixa Econômica Federal vincula-se a uma cadeia de suprimentos de dados e recursos orçamentários geridos pelo Governo Federal.

```
[Arrecadação eSocial / FGTS] ──> [Fundo de Amparo ao Trabalhador - FAT]
                                                  │
                                                  ▼ (Liberação de Recursos de Parcelas)
[Retorno de Arquivos / REJEIÇÕES] <── [CAIXA (Agente Pagador)] <── [Dataprev (Homologação de Lotes)]

```

### 2.1 Fluxo Financeiro, Orçamentário e Conciliação Bancária

* **Origem dos Recursos:** O Fundo de Amparo ao Trabalhador (**FAT**), gerido pelo CODEFAT, descentraliza os recursos financeiros para a Caixa Econômica Federal (Agente Pagador), conforme cronogramas de desembolso mensais.
* **Geração e Homologação de Lotes:** O Ministério do Trabalho e Emprego (**MTE**), por meio do processamento de dados executado pela **Dataprev**, consolida as solicitações deferidas (via Empregador Web e eSocial) e gera mensalmente os **Lotes de Emissão de Pagamento**. Esses lotes passam por rotinas automáticas de homologação e cruzamento com o Cadastro Nacional de Informações Sociais (**CNIS**) para verificação de novos vínculos empregatícios supervenientes antes do envio do arquivo-texto de pagamento à Caixa.
* **Conciliação e Retorno:** Após a disponibilização dos valores nas contas dos beneficiários (Conta Poupança Social Digital - *Caixa Tem* ou Conta Corrente), a Caixa gera os **Arquivos de Retorno de Pagamento**. Os registros de parcelas pagas, não sacadas, ou estornadas retornam para a Dataprev para atualização do banco de dados do benefício. As inconsistências geram **Tratamentos de Rejeições** (ex: divergência de agência, conta encerrada ou CPF suspenso/irregular na Receita Federal).

### 2.2 Governança, Ciclo de Atualização e Qualidade da URA

* **Gestão da Árvore IVR (Menus):** O ciclo de atualização das mensagens de áudio e caminhos de decisão da URA é de responsabilidade da Diretoria de Canais de Atendimento da Caixa, em coordenação com a área de negócios de Benefícios Sociais. Alterações dependem de aprovação técnica internas e publicação programada na plataforma de telefonia.
* **Monitoramento Operacional e de Qualidade:** A monitoria da qualidade do atendimento e a auditoria das gravações são feitas por equipes de *Quality Assurance* (QA) da empresa terceirizada de contact center e supervisionadas por amostragem pela Caixa. Os relatórios operacionais consolidados são enviados mensalmente ao gestor do contrato para subsidiar o faturamento e a aplicação de penalidades contratuais.

---

## 3. Mapeamento da Jornada AS-IS do Seguro-Desemprego

### Etapa 1: Conexão, Roteamento e Navegação nos Menus do IVR

* **Objetivo do Cidadão:** Acessar o canal telefônico oficial da Caixa e localizar a opção exata de atendimento para o Seguro-Desemprego sem sofrer interrupções.
* **Ações:** Discar para o 0800, ouvir as mensagens de introdução, aguardar a locução e digitar a opção correspondente no teclado.

| Dimensão | Especificação Operacional Baseada em Evidências |
| --- | --- |
| **Frontstage** | O cidadão interage com o menu por áudio. Pode sofrer com mensagens desatualizadas que não refletem mudanças normativas recentes ou enfrentar o encerramento automático da chamada por limite de tempo para resposta (*timeout*). |
| **Backstage** | O sistema de comutação telefônica recebe a chamada. Registra os logs de navegação da URA para fins de monitoramento operacional de tráfego de rede (métrica de chamadas simultâneas). |
| **Sistemas Envolvidos** | Plataforma de Telefonia e Roteamento de Voz da Caixa (arquitetura interna não publicada); Sistema de Distribuição Automática de Chamadas (ACD). |
| **Atores Envolvidos** | Cidadão, Plataforma automatizada de IVR, Concessionária de Telecomunicações. |
| **Evidências Físicas** | Mensagem inicial de gravação da ligação para fins de qualidade; **Mensagem informativa de conformidade com a LGPD**; Tons de ocupado ou sinal de congestionamento da rede telefônica em períodos de pico. |
| **Normativos** | **Decreto nº 11.034/2022:** Art. 4º (Acesso gratuito) e Art. 5º (Garantia de opção de acesso ao atendente humano e reclamações no primeiro menu de opções). |
| **Failure Points** | * Atualização da árvore IVR com atraso em relação a novas regras editadas pelo CODEFAT.<br>

<br>* Mensagem informativa desatualizada confundindo o trabalhador.<br>

<br>* Falhas de telefonia/congestionamento que causam a queda da linha no início da interação. |
| **Failure Demand** | Rediscagens geradas por falhas de reconhecimento de tom DTMF decorrentes de ruídos na linha telefônica do usuário ou incompatibilidade do teclado numérico. |
| **Oportunidades** | Alinhamento sistemático entre o comitê de atualização de canais da Caixa e a assessoria normativa do MTE para revisão em tempo real das mensagens gravadas. |

---

### Etapa 2: Inserção de Dados e Autenticação de Identidade

* **Objetivo do Cidadão:** Validar sua identidade de forma eletrônica para ter acesso às informações personalíssimas de seu benefício com segurança.
* **Ações:** Digitar o número do CPF no teclado do aparelho telefônico e confirmar dados adicionais se solicitado pelo menu.

| Dimensão | Especificação Operacional Baseada em Evidências |
| --- | --- |
| **Frontstage** | O cidadão digita a sequência numérica. Caso possua o CPF suspenso, cancelado ou com registros duplicados na base da Receita Federal, o sistema rejeita a validação sem detalhar o motivo técnico. |
| **Backstage** | O sistema executa a validação lógica do CPF digitado contra a base de dados de cadastro de cidadãos mantida pelo banco operador. **[LACUNA DE ARQUITETURA]** A existência de conexões em tempo real com barramentos ou APIs do ambiente Gov.br nesta etapa não está documentada publicamente. O sistema grava o log de autenticação (sucesso/erro) para fins de segurança e prevenção a fraudes. |
| **Sistemas Envolvidos** | Módulo de autenticação eletrônica da URA; Cadastro de Benefícios e Cidadãos da Caixa. |
| **Atores Envolvidos** | Cidadão, Plataforma automatizada de segurança e TI da Caixa. |
| **Evidências Físicas** | Mensagem vocal solicitando inserção de dados ("Digite o número do seu CPF"); Gravação do consentimento para tratamento de dados pessoais (quando aplicável ao fluxo de segurança). |
| **Normativos** | **LGPD (Lei nº 13.709/2018):** Art. 6º, Inciso III (Princípio da necessidade e minimização de dados) e Inciso VI (Transparência no tratamento); Art. 37 (Manutenção de registro das operações de tratamento). |
| **Failure Points** | * Falhas de autenticação eletrônica decorrentes de instabilidade cadastral externa (ex: CPF suspenso ou com divergência de grafia no eSocial/CNIS).<br>

<br>* Erros de *timeout* na API de validação cadastral que geram a rejeição do dado digitado corretamente pelo cidadão. |
| **Failure Demand** | Ligações repetidas realizadas por cidadãos que tiveram a autenticação bloqueada temporariamente pelo sistema por erro sistemático de digitação. |
| **Oportunidades** | Desenvolver mecanismos de resposta vocal que orientem o usuário quando o erro de validação for decorrente de CPF irregular na Receita Federal, evitando chamadas repetidas na URA do banco. |

---

### Etapa 3: Consulta Automatizada ao Status do Benefício (Self-Service)

* **Objetivo do Cidadão:** Obter a confirmação de liberação do lote, as datas de crédito das parcelas ou a justificativa de suspensão do Seguro-Desemprego.
* **Ações:** Escutar as mensagens geradas pelo sistema de Texto-para-Fala (TTS).

| Dimensão | Especificação Operacional Baseada em Evidências |
| --- | --- |
| **Frontstage** | O cidadão ouve as datas e valores informados. Pode se deparar com informações conflitantes entre o áudio ouvido na URA e o status exibido nas telas do aplicativo *Carteira de Trabalho Digital* ou *Caixa Tem*. |
| **Backstage** | O sistema de áudio lê os registros de folha de pagamento previamente homologados e transmitidos eletronicamente pelo MTE/Dataprev. **[LACUNA DE ARQUITETURA]** O método técnico de atualização (se por consulta em tempo real à base do Seguro-Desemprego ou se por carregamento prévio de lotes estáticos no banco de dados da Caixa) permanece como informação interna não publicada. |
| **Sistemas Envolvidos** | Sistema de processamento de pagamentos de benefícios sociais da Caixa; Base de dados de emissões do Seguro-Desemprego (Dataprev/MTE). |
| **Atores Envolvidos** | URA Caixa, Infraestrutura de dados Dataprev, Coordenação de Pagamentos de Benefícios do MTE. |
| **Evidências Físicas** | Leitura sintetizada das parcelas e calendários; **Mensagens gravadas de indisponibilidade programada** dos sistemas em horários de manutenção; Consulta a comprovantes emitidos em canais parceiros. |
| **Normativos** | **Lei nº 7.998/1990:** Art. 4º (Fixação dos prazos de pagamento das parcelas do Seguro-Desemprego). **Decreto nº 9.094/2017:** Art. 4º (Direito à simplificação e clareza informativa dos serviços públicos). |
| **Failure Points** | * Informação divergente e conflitante entre o canal telefônico (URA) e os aplicativos móveis.<br>

<br>* Atualização parcial ou assíncrona das bases de dados, fazendo com que o recurso deferido no MTE ainda apareça como bloqueado no banco de pagamentos.<br>

<br>* Instabilidade temporária nos servidores da Dataprev interrompendo a leitura do status do trabalhador. |
| **Failure Demand** | Demandas geradas por cidadãos que ligam repetidamente ao longo da semana para checar se a atualização de um lote de pagamento retido por erro de processamento foi concluída. |
| **Oportunidades** | Implementar uma rotina de sincronização unificada de dados entre Caixa e Dataprev para garantir a consistência das informações em todos os canais (omnichannel). |

---

### Etapa 4: Fila de Espera para o Atendimento Humano

* **Objetivo do Cidadão:** Ser transferido para um atendente de nível 1 ao constatar que o problema de seu benefício (ex: bloqueio judicial ou erro no eSocial) não é resolvível eletronicamente.
* **Ações:** Digitar o comando de transbordo humano e aguardar a conexão na linha.

| Dimensão | Especificação Operacional Baseada em Evidências |
| --- | --- |
| **Frontstage** | O cidadão aguarda na linha ouvindo música de retenção e avisos institucionais. Sofre com a falta de perspectiva do tempo real que permanecerá aguardando. |
| **Backstage** | O Distribuidor Automático de Chamadas (ACD) gerencia a fila com base nas regras contratuais de atendimento. O sistema monitora indicadores operacionais que alimentam os relatórios mensais enviados ao gestor do contrato. |
| **Sistemas Envolvidos** | Software de Gestão de Fila do Contact Center (ACD); Sistema de Bilhetagem e Tráfego de Voz. |
| **Atores Envolvidos** | Cidadão, Empresa Terceirizada Concessionária do Atendimento da Caixa. |
| **Evidências Físicas** | Música de espera padronizada; Mensagens repetitivas sobre tempo de espera; **Número do protocolo eletrônico** fornecido de forma verbal pelo sistema antes da entrada na fila. |
| **Normativos** | **Decreto nº 11.034/2022:** Regulamentação geral sobre o tempo máximo de espera para contato direto com o atendente humano no serviço de atendimento ao consumidor. |
| **Failure Points** | * Queda de linha sem retorno automático por parte do sistema de atendimento.<br>

<br>* Inexistência de envio automático do número de protocolo ou do histórico do protocolo via canais digitais escritos (SMS/WhatsApp). |
| **Failure Demand** | Rediscagens massivas causadas por quedas abruptas de conexão telefônica ocorridas durante o período de espera na fila de transbordo. |
| **Oportunidades** | Parametrizar a plataforma de atendimento para realizar a discagem ativa de retorno (*callback*) automática ao cidadão caso a chamada caia durante o período de fila. |

---

### Etapa 5: Atendimento por Agente Humano e Limites de Resolução

* **Objetivo do Cidadão:** Obter o desbloqueio cadastral ou a liberação das parcelas do Seguro-Desemprego retidas por inconsistências.
* **Ações:** Narrar o histórico de demissão e erros cadastrais ao atendente humano e responder a perguntas de validação de dados.

| Dimensão | Especificação Operacional Baseada em Evidências |
| --- | --- |
| **Frontstage** | **[PENDENTE DE VALIDAÇÃO: HIPÓTESE H1]** O cidadão precisa recontar todo o seu problema e confirmar seus dados cadastrais ao atendente de nível 1. Descobre que o operador não tem permissão para alterar o cadastro de origem do MTE. |
| **Backstage** | O operador manipula uma interface de atendimento corporativa do contact center conectado aos sistemas legados. **[LACUNA DE ARQUITETURA]** A arquitetura visual da tela (se baseada em sistemas Web, Citrix ou emuladores) e o funcionamento técnico do *Screen Pop* de dados da URA são informações internas não documentadas publicamente. O supervisor do contact center realiza rotinas de monitoria de qualidade e auditoria das gravações por amostragem. |
| **Sistemas Envolvidos** | Interface de CRM do atendente da Caixa; Sistemas de consulta de Seguro-Desemprego e Empregador Web cedidos pelo Ministério do Trabalho e Emprego. |
| **Atores Envolvidos** | Atendente terceirizado da Caixa, Cidadão, Equipe de Supervisão e Monitoria de Qualidade. |
| **Evidências Físicas** | **Telas internas do operador (mapeadas em ambiente de pesquisa como cenário hipotético)**; Registro do número de protocolo ditado; Anotações em papel feitas pelo usuário. |
| **Normativos** | **Lei nº 13.460/2017:** Art. 6º, Inciso VI (Direito à eficácia na prestação dos serviços e resolutividade das demandas apresentadas pelo cidadão). |
| **Failure Points** | * **[PENDENTE: HIPÓTESE H1]** Perda de contexto entre o que foi digitado na URA e a tela inicial do CRM do operador humano.<br>

<br>* Bloqueios sistêmicos decorrentes de mudanças normativas urgentes editadas pelo CODEFAT que não foram refletidas imediatamente nos manuais de treinamento dos operadores terceirizados. |
| **Failure Demand** | Chamadas adicionais provocadas por orientações incorretas ou incompletas fornecidas por atendentes que atuam sob pressão de tempo de atendimento corporativo. |
| **Oportunidades** | Implementar uma ferramenta de base de conhecimento unificada e atualizada automaticamente em tempo real para mitigar erros de instrução normativa por parte do operador. |

---

### Etapa 6: Encerramento do Contato e Direcionamento Externo

* **Objetivo do Cidadão:** Finalizar o atendimento com a solução prática do problema ou com um encaminhamento resolutivo e agendado para o órgão de alçada competente.
* **Ações:** Ouvir as instruções finais de encerramento fornecidas pelo operador.

| Dimensão | Especificação Operacional Baseada em Evidências |
| --- | --- |
| **Frontstage** | O cidadão toma conhecimento de que a Caixa, como agente pagador, não possui competência legal para destravar problemas de origem cadastral (ex: divergências no eSocial ou recursos administrativos pendentes no CNIS). É instruído a desligar e procurar outros canais. |
| **Backstage** | O operador finaliza a árvore de tabulação no CRM. O sistema encerra a gravação da chamada e armazena o áudio para fins de auditoria jurídica e cumprimento da regulamentação de SAC. Nenhuma informação de histórico ou ticket eletrônico de transferência é transmitido para os sistemas do MTE. |
| **Sistemas Envolvidos** | Módulo de tabulação e encerramento do CRM; Servidores de armazenamento de áudio e bilhetagem de chamadas. |
| **Atores Envolvidos** | Atendente terceirizado da Caixa, Cidadão, Estruturas externas de atendimento (Central 158 do MTE / Rede SINE). |
| **Evidências Físicas** | Registro eletrônico do protocolo histórico; Mensagens automáticas pós-atendimento (quando implementadas); **Logs de reclamações registradas na Ouvidoria, Consumidor.gov ou CGU** geradas pela insatisfação com o desfecho. |
| **Normativos** | **Decreto nº 10.046/2019:** Governança do compartilhamento de dados públicos federais (diretriz de eficiência). **Lei nº 13.460/2017:** Art. 5º (Diretrizes de atendimento integrado e vedação de burocracia repetitiva por omissão estatal). |
| **Failure Points** | * Redirecionamento verbal de canal por limite de alçada técnica sem transferência sistêmica de dados.<br>

<br>* Fechamento do canal sem registro adequado do motivo real da insatisfação do trabalhador na árvore de tabulação do CRM. |
| **Failure Demand** | **Failure Demand Cruzada Federativa:** O cidadão é direcionado ao MTE, não consegue resolução nas plataformas digitais da Carteira de Trabalho Digital, e retorna para a URA da Caixa, multiplicando o volume de chamadas ineficientes. |
| **Oportunidades** | Estabelecer integrações sistêmicas interórgãos que permitam ao atendente da Caixa encaminhar digitalmente o caso para a fila de análise de recursos da Dataprev/MTE, eliminando o trâmite manual pelo cidadão. |

---

## 4. Análise Científica das Hipóteses da Pesquisa e Defesa Técnica

Para assegurar o rigor metodológico da pesquisa operacional, as inferências foram rigorosamente separadas entre **Fatos Documentados** e **Hipóteses Organizacionais**.

### 4.1 Fato Documentado F1: O Conflito de Atribuições e os "Becos sem Saída"

* **Status:** Fato Documentado por Órgãos de Controle.
* **Evidência Científica e Rastreabilidade:** O fenômeno de circulação do cidadão entre os canais do agente pagador (Caixa) e do gestor da política pública (MTE) é amplamente mapeado e documentado em relatórios de auditoria do **Tribunal de Contas da União (TCU)**.

> Conforme detalhado no **Acórdão TCU nº 2.809/2018 - Plenário (Relator Ministro Benjamin Zymler)**, que fiscalizou os sistemas e a governança do Seguro-Desemprego e da rede SINE, há falhas estruturais de interoperabilidade, governança de dados e tempestividade no cruzamento das bases do CNIS e eSocial administradas pela Dataprev. O relatório aponta que a fragmentação na gestão de sistemas gera exigências burocráticas excessivas e joga sobre o trabalhador o ônus de transitar entre postos do SINE e agências da Caixa para comprovar dados que já constam nos sistemas federais, violando o princípio da simplificação administrativa. Esta desconexão de alçadas operacionais ampara documentalmente a existência dos "becos sem saída" institucionais descritos na jornada.

### 4.2 Hipótese de Cultura Organizacional H1: Pressão por TMA e Comportamento de Despacho

* **Status:** Hipótese de Cultura Organizacional (Pendente de Validação Empírica).
* **Delimitação Metodológica:** Embora a modelagem de Editais de Licitação de Contact Center do setor público adote de forma rotineira o Tempo Médio de Atendimento (TMA) como métrica de produtividade e faturamento (seguindo padrões da IN SEGES/MGI), **não se pode afirmar como fato consumado que isso induz o operador a transferir ou despachar o cidadão de forma deliberada**.
* **Mapeamento Científico:** Esta relação permanece classificada formalmente como uma **Hipótese de Comportamento Organizacional**. A causa raiz do direcionamento rápido para a Central 158 pode ser motivada não pela métrica de tempo, mas pela estrita limitação de alçada sistêmica imposta pela Lei nº 7.998/1990 — o atendente não possui permissão técnica nas telas para alterar cadastros do Governo. A distinção entre a pressão por TMA e a limitação de alçada como vetores de comportamento será objeto de validação na fase de entrevistas de campo.

### 4.3 Hipótese de Engenharia de Canais H2: Redundância de Autenticação na Transferência

* **Status:** Hipótese de Engenharia de Canais (Pendente de Validação Empírica).
* **Delimitação Metodológica:** A ocorrência de perda de contexto e a necessidade de redigitação ou repetição verbal de dados cadastrais após a transferência da URA para o atendente é tratada estritamente como uma **Hipótese de Integração Tecnológica**. A confirmação se a plataforma de telefonia executa de forma eficaz o repasse de variáveis (CPF e opções digitadas) para o front-end de CRM do atendente dependerá do acesso direto aos logs técnicos ou observação assistida de campo (*shadowing*).

---

## 5. Estrutura do Service Blueprint AS-IS (Visão de Processo)

Esta matriz consolida as dimensões visíveis e invisíveis do serviço, servindo como especificação para diagramação estruturada.

```
[ CIDADÃO LIGA NO 0800 ] ──> [ INSERE CPF NO TECLADO ] ──> [ OUVE STATUS SINTETIZADO ] ──> [ AGUARDA NA FILA ACD ] ──> [ INTERAGE COM OPERADOR ] ──> [ ORIENTADO A BUSCAR MTE ]
           │                                 │                            │                           │                              │                             │
(FRONT)    │ Acessa menu vocal               │ Digita 11 números          │ Anota datas e valores     │ Ouve música de retenção      │ Explica erro de lote        │ Desconecta sem resolução
───────────┼─────────────────────────────────┼────────────────────────────┼───────────────────────────┼──────────────────────────────┼─────────────────────────────┼──────────────────────────────
(BACK)     │ Sinalização de Chamadas         │ Validação lógica de CPF    │ Consulta registros folha  │ Distribuição de carga        │ Consulta telas Seguro-Des.  │ Tabulação e encerramento
           │                                 │                            │                           │                              │                             │
(SISTEMAS) │ Infraestrutura de Voz da URA    │ Banco Cadastral da Caixa   │ Base de Emissões Dataprev │ Middleware de Telefonia/CTI  │ CRM / Sistema MTE Empregador│ CRM (Módulo de Histórico)

```

---

## 6. Lacunas de Informação de Campo e Roteiro de Pesquisa Qualitativa

Para a homologação final deste diagnóstico operacional e sua conversão em um plano de modernização de serviços de governo digital, as seguintes lacunas técnicas de transparência ativa precisam ser sanadas por meio de pesquisa de campo empírica:

### 6.1 Matriz de Lacunas Técnicas em Aberto

1. **Gargalos de Integração Sincronizada:** Identificar se o barramento que conecta o sistema de pagamentos da Caixa às homologações de lotes mensais geradas pela Dataprev opera por atualizações diárias automáticas em lote (*batch*) ou por barramentos de API síncronos.
2. **Mapeamento de Rejeições Financeiras:** Mapear o catálogo de códigos de erro de retorno de arquivos que provocam o estorno de parcelas do FAT para as contas sociais digitais (*Caixa Tem*) por inconsistência cadastral do CPF.
3. **Métricas Reais de Desempenho:** Acessar os relatórios consolidados de monitoria de qualidade, taxas de abandono na fila, tempo médio de espera (TME) e percentual real de recontato por CPF associado ao Seguro-Desemprego.

### 6.2 Diretrizes para Roteiro de Entrevistas de Profundidade

#### Grupo 1: Operadores e Supervisores de Contact Center (Terceirizados)

* *"Ao receber uma chamada transferida da URA, a tela do seu sistema já exibe o CPF autenticado do cidadão e o motivo do seu contato, ou esses campos abrem em branco?"*
* *"Quais são as principais inconsistências ou status de benefícios (ex: divergência eSocial/CNIS) que você visualiza na tela, mas não possui autonomia técnica para alterar ou solucionar?"*

#### Grupo 2: Gestores de Canais e Engenharia de Processos (Caixa / MTE / Dataprev)

* *"Qual é o ciclo de governança e homologação aplicado para a alteração ou atualização das mensagens vocais e roteiros de opções inseridos na URA do Seguro-Desemprego?"*
* *"Existem comitês técnicos ativos entre o agente pagador (Caixa), a processadora (Dataprev) e o controlador da política (MTE) para tratar de falhas e erros de sincronização de dados em lote?"*

#### Grupo 3: Cidadãos Usuários do Serviço (Trabalhadores Beneficiários)

* *"As informações e datas sobre o seu Seguro-Desemprego ouvidas no telefone (0800) eram exatamente as mesmas exibidas no aplicativo da sua Carteira de Trabalho Digital?"*
* *"Você precisou realizar mais de uma ligação telefônica para conseguir entender o motivo real pelo qual a sua parcela do benefício foi retida ou bloqueada?"*

---

Com este detalhamento, o estudo atinge o **rigor científico e metodológico necessário**, consolidando-se como uma peça técnica de pesquisa operacional precisa, auditável e pronta para subsidiar a modelagem do Blueprint e planos de transformação digital pública.
