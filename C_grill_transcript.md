# C_grill_transcript.md — Transcrição do Grill-Me

## Sessão de entrevista metodológica para o Service Blueprint AS-IS

**Artefato de referência:** B_relatorio_assistente_v3.md
**Objeto:** Atendimento ao Seguro-Desemprego pela URA da Caixa Econômica Federal
**Metodologia:** Service Blueprint de Shostack (1984)
**Data:** 27/06/2026

---

## Transcrição

---

**Assistente:**
Li o relatório. Vou te entrevistar sobre o desenho do **Service Blueprint AS-IS** seguindo Shostack, uma pergunta de cada vez.

---

**Pergunta 1 — Escopo e granularidade**

O relatório mapeia 6 etapas (Conexão/IVR → Autenticação → Consulta Self-Service → Fila → Atendente → Encerramento). Você quer diagramar **todas as 6** no mesmo blueprint (visão ponta-a-ponta) ou dividir em blueprints separados por etapa macro (ex: um blueprint só do IVR, outro só do transbordo humano)?

Minha recomendação: um único blueprint de ponta-a-ponta, pois o *fail point* central é justamente a **fragmentação intersetorial** — perder isso num recorte estreito derrotaria o propósito.

**Resposta:** mesmo

---

**Pergunta 2 — Linha de Interação (Line of Interaction): qual é o gatilho inicial?**

O blueprint precisa definir onde o cidadão *cruza* para o frontstage. O relatório começa com "Discar para o 0800". Você considera:

- A **decisão do cidadão de pegar o telefone** como a primeira ação do cidadão (pré-serviço), entrando na linha de interação apenas quando a URA atende?
- Ou a **discagem** já é a primeira ação cruzando a linha?

Minha recomendação: incluir uma *pré-etapa* "Cidadão reúne documentos (CTPS, CPF) e disca 0800" como ação do cidadão *acima* da linha de interação, e a linha de interação é cruzada no momento em que a URA **responde** (início do frontstage). Isso separa o esforço do cidadão do serviço prestado.

**Resposta:** O 0800 que atende tem importância. Assim, o fato da discagem faz parte do front stage.

---

**Pergunta 3 — Linha de Visibilidade (Line of Visibility): o que fica invisível ao cidadão em cada etapa?**

O relatório menciona vários processos de backstage, mas alguns podem estar *acima* da linha de visibilidade (frontstage) na percepção do cidadão. Me ajude a classificar estes elementos — **visível** (frontstage) ou **invisível** (backstage):

1. **Mensagem "Esta ligação será gravada para fins de qualidade"** → visível (o cidadão ouve)
2. **Roteamento ACD da chamada para o próximo servidor disponível** → invisível
3. **Consulta em tempo real à base Dataprev versus leitura de lote estático** → ?
4. **Validação lógica do CPF contra o banco cadastral** → ?
5. **Gravação do áudio da chamada para auditoria** → ?
6. **Registro de log de navegação nos menus (quais opções o cidadão apertou)** → ?
7. **Monitoria de qualidade por supervisor (escuta ao vivo ou gravada)** → ?

Minha recomendação: tudo que o cidadão **ouve, vê ou experimenta sensorialmente** fica acima da linha (frontstage). Tudo que é processamento, registro, validação ou auditoria *sem interação sensorial direta* fica abaixo (backstage). Os itens 1 é frontstage; 2, 5, 6, 7 são backstage. Os itens 3 e 4 são backstage — com uma ressalva: se a validação falha e o sistema *responde* com uma mensagem de erro, a *mensagem* (consequência) é frontstage, mas o *processamento* permanece backstage. Concorda?

**Resposta:** concordo

---

**Pergunta 4 — Linha de Interação Interna (Line of Internal Interaction): quem são os atores de suporte e onde eles entram?**

Shostack separa **backstage** (ações que os frontline workers fazem sem o cidadão ver) de **processos de suporte** (feitos por outras áreas/pessoas que não interagem com o cidadão). Olhando o relatório, identifiquei estes candidatos a *suporte*:

| Ator / Sistema | Está em backstage ou suporte? |
|---|---|
| Dataprev (homologação de lotes mensais) | ? |
| CODEFAT (edição de normas que mudam regras do SD) | ? |
| Equipe de QA da terceirizada (escuta gravações) | ? |
| Diretoria de Canais da Caixa (atualiza árvore IVR) | ? |
| Concessionária de Telecom (infra de rede) | ? |
| Comitê de atualização de mensagens (Caixa + MTE) | ? |
| Receita Federal (base de CPF) | ? |

Minha recomendação:
- **Backstage**: QA da terceirizada, Diretoria de Canais (se eles interagem diretamente com o sistema de URA que está sendo usado no momento do atendimento).
- **Suporte**: Dataprev, CODEFAT, Concessionária, Receita Federal — são entidades externas cujos processos rodam *independentemente* do atendimento em tempo real. O Comitê de atualização seria suporte, porque age em ciclo mensal, não em tempo real.

Você enxerga algum desses atores como fazendo parte do backstage (por interagir com o frontline worker durante ou imediatamente antes do atendimento)?

**Resposta:** Backstage apenas o QA da terceirizada, os demais suporte.

---

**Pergunta 5 — Evidências Físicas (Physical Evidence): o relatório lista várias, mas qual é a linha do tempo delas?**

Shostack define Evidências Físicas como tudo que o cidadão **toca, recebe, vê ou experimenta** como prova material da passagem pelo serviço. O relatório cita:

1. Mensagem de gravação da ligação ("esta chamada será gravada")
2. Mensagem informativa de conformidade com a LGPD
3. Tons de ocupado / congestionamento
4. Mensagem vocal "Digite o número do seu CPF"
5. Leitura sintetizada das parcelas
6. Mensagens de indisponibilidade programada
7. Música de espera padronizada
8. Número de protocolo eletrônico
9. Anotações em papel feitas pelo cidadão
10. Registro de protocolo histórico
11. Logs de reclamações na Ouvidoria/CGU/Consumidor.gov

**Pergunta:** além desses 11, você identifica alguma outra evidência física? Por exemplo:
- **Silêncio** (tempo ocioso sem música, sem mensagem) — é uma evidência pela ausência?
- O **tom de "chamada em espera"** (o *ring* ou clique antes do operador atender)?

Minha recomendação: incluir **silêncio / tempo oco** como evidência física negativa nas etapas 4 (fila) e 5 (pausa entre fala do cidadão e resposta do operador), porque ele *sinaliza* ao cidadão que "algo pode estar errado" sem que o sistema intencionalmente comunique.

**Resposta:** Vamos simplificar, não inclua.

---

**Pergunta 6 — Linha de Interação (detalhamento): o cidadão interage apenas com o sistema URA até a Etapa 4, certo?**

No blueprint AS-IS, as Etapas 1-3 são puramente **máquina → cidadão** (URA). A linha de interação, portanto, conecta:

- **Etapas 1-3**: Cidadão ↔ Plataforma URA (sistema)
- **Etapa 4**: Cidadão ↔ Fila ACD (transição, ainda sem humano)
- **Etapas 5-6**: Cidadão ↔ Atendente terceirizado (humano)

Ou seja, o touchpoint humano só aparece na Etapa 5.

**Pergunta:** no blueprint, você quer representar a URA e o Atendente Humano como dois **frontstage actors distintos** (um "Frontstage Tecnológico" e um "Frontstage Humano"), ou tudo como uma única camada de frontstage, apenas diferenciando por ícone/legenda?

Minha recomendação: manter camada única de frontstage, mas rotular claramente "URA" vs "Atendente" nas ações. Isso evita complicar o diagrama com subcamadas desnecessárias e preserva a clareza.

**Resposta:** Camada única.

---

**Pergunta 7 — Fail Points: priorização e conexão causal**

O relatório lista 11 failure points distribuídos nas 6 etapas. Alguns são **causa** e outros são **consequência** (reação em cadeia). Exemplo clássico:

- FP1: *Congestionamento cai a linha no início* (Etapa 1) → *Cidadão redisca* (Etapa 1 novamente) → *Aumenta fila ACD* (Etapa 4)
- FP2: *Timeout na API de validação* (Etapa 2) → *Cidadão redigita CPF* → *Sistema bloqueia por erro repetitivo* → *Cidadão liga de novo* (Etapa 1) → *Retorna à fila* (Etapa 4)

**Pergunta:** você quer que o blueprint:
a) Liste os 11 FPs pontualmente em cada etapa (como está no relatório), OU
b) Desenhe **setas de retroalimentação** conectando FPs entre etapas (ex: FP da Etapa 2 → Falha na Etapa 4) para mostrar o *failure demand* cruzado?

Minha recomendação: **(b)** — se você só lista por etapa, perde o principal problema do sistema: a **circularidade** (cidadão que é jogado de volta ao início). O relatório já cita "Failure Demand Cruzada Federativa" na Etapa 6; o blueprint ganha muito ao mostrar visualmente esses loops.

**Resposta:** b

---

**Pergunta 8 — A representação dos loops de Failure Demand**

No blueprint AS-IS, os loops de retroalimentação cruzam etapas. O mais crítico é o **ciclo vicioso federativo** (Etapa 6 → Etapa 1):

```
Etapa 6: "Procure o MTE" → Cidadão liga pra Central 158 → não resolve → Etapa 1: redisca 0800 Caixa
```

Outro loop importante:

```
Etapa 2: CPF rejeitado por timeout → Etapa 1: cidadão redisca → Etapa 4: sobrecarga na fila ACD
```

**Pergunta:** como você quer representar esses loops? Especificamente:

a) **Setas tracejadas vermelhas** saindo do fail point de origem e apontando para a etapa de destino (ex: seta da Etapa 6 "Redirecionamento verbal" → Etapa 1 "Discagem"), com legenda "Failure Demand Loop"
b) **Uma camada adicional abaixo do blueprint** chamada "Failure Demand Circulation", com linhas do tempo conectando os FPs
c) **Notações de "goto"** ao lado de cada fail point (ex: *"→ retorna à Etapa 1 — 42% dos casos"*)

Minha recomendação: **(a)** — setas tracejadas vermelhas visíveis na própria camada do blueprint, porque (b) cria uma camada extra que pode confundir e (c) é texto demais num diagrama que deveria ser visual.

**Resposta:** a. Somente mais uma pergunta e encerre o grill-me.

---

**Pergunta 9 (final) — O blueprint é AS-IS documental ou AS-IS hipotético?**

O relatório classifica parte do backstage como **"Lacuna de Arquitetura"** (ex: se a atualização da URA é por API síncrona ou lote batch) e parte do frontstage como **"Hipótese H1 pendente de validação"** (ex: perda de contexto entre URA e CRM do atendente).

No blueprint:
a) Você **marca explicitamente** esses elementos com um selo "Lacuna Documental" ou "Hipótese" (ex: asterisco + legenda)?
b) Você **omite** o que não é fato confirmado e só desenha o que está documentado, deixando espaços em branco?

Minha recomendação: **(a)** — marcar com notação clara. Espaços em branco dão a falsa impressão de que o processo *não existe* (quando ele existe, só não está documentado). Um selo "Hipótese" ou "Lacuna" preserva o rigor metodológico que o relatório busca.

**Resposta:** a, conforme a recomendação. Prefiro que consolide como tabela. Mapeie ≥ 5 etapas na ótica do cidadão, com as cinco camadas e marcando os fail points.

---

**Assistente:**
Segue o Service Blueprint AS-IS consolidado em tabela markdown. [...] O arquivo foi salvo em `C:\Users\cbm\AppData\Local\Temp\opencode\service_blueprint_as_is.md`. Quer que eu ajuste algo?

**Resposta:** Salve no arquivo C_blueprint_asis.md presente na pasta.

---

**Assistente:**
Substituído. [...] Quer que eu ajuste algo?

**Resposta:** Agora suba este arquivo no repositório GH correpondente no arquivo correto.

---

**Assistente:**
Arquivo commitado e enviado ao `main` em `https://github.com/me-mourao-pinto/exercicio-3.1.git` (commit `6e36247`).

**Resposta:** coloque a transcrição de toda a nossa conversa do grill-me no arquivo C_grill_transcript.md tanto na pasta local como no repositório GH correspondente.

---

## Encaminhamento

Transcrição sendo salva neste arquivo e enviada ao repositório.
