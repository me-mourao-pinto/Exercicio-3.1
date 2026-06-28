# C_blueprint_asis.md — Service Blueprint AS-IS

## Atendimento ao Seguro-Desemprego pela URA da Caixa Econômica Federal

Metodologia: Shostack (1984) — 5 camadas, 3 linhas divisórias, fail points.
Versão consolidada em tabela a partir de B_relatorio_assistente_v3.md e sessão de entrevista metodológica (grill-me).

---

## Blueprint AS-IS (Visão Matricial)

| Camada | Etapa 1: Acesso e IVR | Etapa 2: Autenticação CPF | Etapa 3: Consulta Automatizada | Etapa 4: Fila ACD | Etapa 5: Atendente Humano | Etapa 6: Encerramento |
|---|---|---|---|---|---|---|
| **Evidências Físicas** | Mensagem "chamada gravada"; Mensagem LGPD; Tom de congestionamento/ocupação | Mensagem vocal "Digite o CPF"; Gravação de consentimento LGPD | Voz TTS com parcelas/datas; Mensagem de indisponibilidade programada | Música de espera; Número de protocolo verbal | [Hipótese H1] Telas internas do operador; Protocolo ditado; Anotações em papel | Registro de protocolo histórico; Logs de Ouvidoria/CGU/Consumidor.gov |
| **Ações do Cidadão** | Disca 0800; Ouve menu; Digita opção SD no teclado | Digita 11 dígitos do CPF; Confirma dados se solicitado | Escuta TTS; Anota datas e valores | Aguarda na linha com música de retenção | Narra histórico demissão; Confirma dados; Responde perguntas | Ouve instruções finais; Desliga; Procura MTE/Central 158 |
| ▲ — **Linha de Interação** ▲ — | | | | | | |
| **Frontstage** | URA executa menu em áudio; Locução das opções; Mensagens gravadas | URA valida CPF eletronicamente; Rejeição sem detalhar motivo técnico | URA lê registros de folha; **[FP3]** Informação conflitante com app Carteira de Trabalho Digital | ACD gerencia fila; [Lacuna] Sem estimativa de tempo real de espera | Atendente interage verbalmente; [Hipótese H1] Perda de contexto URA→CRM; Operador sem alçada para alterar cadastro MTE | Atendente informa que Caixa não resolve causa raiz; **[FP7]** Redirecionamento verbal sem transferência sistêmica |
| ▲ — **Linha de Visibilidade** ▲ — | | | | | | |
| **Backstage** | Sistema de comutação telefônica; Registro de logs de navegação; [Lacuna] Arquitetura de roteamento não publicada | Validação lógica contra base cadastral Caixa; Log de autenticação; [Lacuna] Conexão Gov.br não documentada | Consulta a base de emissões Dataprev; [Lacuna] Método de atualização (batch vs API) não documentado | Distribuição ACD; Monitoria de indicadores (TME, abandono) | Operador manipula CRM/sistemas legados; [Lacuna] Arquitetura de tela não documentada; QA monitora gravações por amostragem | Finalização da tabulação no CRM; Armazenamento do áudio para auditoria; **[FP7]** Nenhum ticket transferido ao MTE |
| ▲ — **Linha de Interação Interna** ▲ — | | | | | | |
| **Processos de Suporte** | Concessionária de Telecom (rede); Diretoria de Canais Caixa (manutenção IVR); Comitê de atualização de mensagens | Base cadastral Receita Federal (CPF) | Dataprev: homologação de lotes mensais de pagamento; CODEFAT: edição de normas do SD | Sistema de bilhetagem e tráfego de voz | [Hipótese H1] Base de conhecimento SD / manual de treinamento defasado por normas CODEFAT | Sistemas MTE (Central 158 / SINE); Rede SINE de atendimento presencial |
| **Failure Points / Failure Demand Loops** | **FP1:** Congestionamento/queda no início → 🔁 loop p/ Etapa 1; **FP2:** Árvore IVR desatualizada vs regras CODEFAT | **FP3:** Timeout na API validação rejeita CPF correto; CPF suspenso sem orientação → 🔁 loop p/ Etapa 1 (rediscagem) e Etapa 4 (sobrecarga fila) | **FP4:** Divergência URA vs app móvel; Atualização assíncrona (deferido no MTE aparece bloqueado) → 🔁 loop p/ Etapas 2 e 1 | **FP5:** Queda de linha sem retorno automático → 🔁 loop p/ Etapa 1; Inexistência de envio de protocolo por SMS/WhatsApp | **FP6:** Perda de contexto URA→CRM → repetição de dados; Bloqueio normativo sem atualização de treinamento → orientação incorreta | **FP7:** Redirecionamento verbal sem transferência sistêmica → 🔁 **Failure Demand Cruzado Federativo** p/ Etapa 1 (cidadão redisca 0800 após circular MTE → SINE) |

---

## Legenda

| Notação | Significado |
|---|---|
| [Lacuna] | Processo existente mas sem documentação pública disponível |
| [Hipótese H1] | Comportamento inferido, pendente de validação empírica em campo |
| [FPn] | Failure point identificado (numerado por tipo, não por etapa) |
| 🔁 | Failure demand loop — retroalimentação que faz o cidadão retornar a etapas anteriores |

## Mapa de Loops de Failure Demand

```
FP3 (Timeout CPF) ──🔁──► Etapa 1 (redisca) ──► Etapa 4 (sobrecarga fila)
FP1 (Queda linha) ──🔁──► Etapa 1 (redisca)
FP4 (Divergência) ──🔁──► Etapa 2 (tenta de novo) ──► Etapa 1
FP5 (Queda na fila) ──🔁──► Etapa 1 (redisca)
FP7 (Redirecionamento) ──🔁──► MTE/Central 158 ──► SINE ──🔁──► Etapa 1 (Failure Demand Cruzado Federativo)
```

---

## Notas Metodológicas

- **Linha de Interação** (▲): separa as Ações do Cidadão do Frontstage. Tudo acima é de responsabilidade do cidadão; tudo abaixo é de responsabilidade do provedor do serviço.
- **Linha de Visibilidade** (▲): separa o Frontstage (visível ao cidadão) do Backstage (invisível). O cidadão interage sensorialmente com os elementos de Frontstage.
- **Linha de Interação Interna** (▲): separa o Backstage (ações de frontline workers sem o cidadão ver) dos Processos de Suporte (terceiros, sistemas legados, órgãos externos).
- As **lacunas de arquitetura** identificadas (conexão Gov.br, método de atualização de bases, Screen Pop do CRM) estão marcadas como pendentes de pesquisa de campo (vide Seção 6 do relatório B_relatorio_assistente_v3.md).
- As **hipóteses H1 (perda de contexto / pressão por TMA)** estão classificadas como hipóteses organizacionais pendentes de validação empírica.
- As **Evidências Físicas** de bastidor (telas de operador, relatórios de monitoria) requerem acesso a ambientes controlados de pesquisa.
