# D_diagrama_asis.md — Diagrama de Relações AS-IS

## Atendimento ao Seguro-Desemprego pela URA da Caixa Econômica Federal

Diagrama de relações entre etapas e atores, derivado do Service Blueprint (C_blueprint_asis.md).

```mermaid
flowchart LR
    %% Styles by layer (actor group)
    classDef citizen fill:#d4e6f1,stroke:#2980b9,stroke-width:2px,color:#1a5276
    classDef front fill:#fdebd0,stroke:#e67e22,stroke-width:2px,color:#7e5109
    classDef back fill:#d5f5e3,stroke:#27ae60,stroke-width:2px,color:#1e8449
    classDef support fill:#fadbd8,stroke:#e74c3c,stroke-width:2px,color:#922b21
    classDef fail fill:#f5b7b1,stroke:#c0392b,stroke-width:2px,color:#7b241c
    classDef gap fill:#fef9e7,stroke:#f1c40f,stroke-width:2px,stroke-dasharray:5 5,color:#7d6608
    classDef loop fill:none,stroke:#c0392b,stroke-width:2px,stroke-dasharray:8 4

    %% ============================================================
    %% COLUMN 1 — Etapa 1: Acesso e IVR
    %% ============================================================
    subgraph E1["① Acesso e Navegação no IVR"]
        direction TB
        C1["🧑 Disca 0800<br/>Digita opção SD"]
        F1["🤖 URA: Menu de áudio<br/>Reconhecimento DTMF"]
        B1["⚙️ Comutação telefônica<br/>Registro de logs"]
        S1["🏛️ Concessionária Telecom<br/>Diretoria Canais Caixa"]
        FP1{{"⚠️ FP1: Congestionamento/queda<br/>⚠️ FP2: Árvore desatualizada"}}
    end

    %% ============================================================
    %% COLUMN 2 — Etapa 2: Autenticação CPF
    %% ============================================================
    subgraph E2["② Autenticação e Validação de CPF"]
        direction TB
        C2["🧑 Digita 11 dígitos<br/>do CPF no teclado"]
        F2["🤖 URA: Validação<br/>eletrônica do CPF"]
        B2["⚙️ Validação contra base<br/>cadastral Caixa<br/>[?] Lacuna: conexão Gov.br"]
        S2["🏛️ Base da Receita<br/>Federal (CPF)"]
        FP3{{"⚠️ FP3: Timeout na API<br/>CPF suspenso sem detalhe"}}
    end

    %% ============================================================
    %% COLUMN 3 — Etapa 3: Consulta Self-Service
    %% ============================================================
    subgraph E3["③ Consulta Automatizada ao Status"]
        direction TB
        C3["🧑 Escuta TTS<br/>e anota informações"]
        F3["🤖 URA: TTS lê parcelas,<br/>datas e calendários"]
        B3["⚙️ Consulta a base de<br/>emissões Dataprev<br/>[?] Lacuna: batch vs API"]
        S3["🏛️ Dataprev: homologação<br/>de lotes mensais<br/>CODEFAT: edição normas"]
        FP4{{"⚠️ FP4: Divergência<br/>URA vs app móvel"}}
    end

    %% ============================================================
    %% COLUMN 4 — Etapa 4: Fila de Espera
    %% ============================================================
    subgraph E4["④ Fila de Espera para Atendente"]
        direction TB
        C4["🧑 Aguarda na linha<br/>Ouve música de retenção"]
        F4["🤖 ACD: Fila de espera<br/>Distribuição automática"]
        B4["⚙️ Distribuição ACD<br/>Monitoria de indicadores"]
        S4["🏛️ Sistema de Bilhetagem<br/>e Tráfego de Voz"]
        FP5{{"⚠️ FP5: Queda de linha<br/>sem retorno automático"}}
    end

    %% ============================================================
    %% COLUMN 5 — Etapa 5: Atendente Humano
    %% ============================================================
    subgraph E5["⑤ Atendimento por Agente Humano"]
        direction TB
        C5["🧑 Narra histórico<br/>Confirma dados verbalmente"]
        F5["🤖 Atendente: Interação<br/>e consulta a sistemas<br/>[?] Hipótese H1: perda contexto"]
        B5["⚙️ CRM / Sistemas legados<br/>QA monitora gravações<br/>por amostragem"]
        S5["🏛️ Base de conhecimento<br/>Manuais de treinamento<br/>[?] Hipótese H1: defasados"]
        FP6{{"⚠️ FP6: Perda de contexto<br/>URA→CRM e normativo<br/>defasado"}}
    end

    %% ============================================================
    %% COLUMN 6 — Etapa 6: Encerramento
    %% ============================================================
    subgraph E6["⑥ Encerramento e Direcionamento"]
        direction TB
        C6["🧑 Ouve instruções<br/>Desliga / busca MTE"]
        F6["🤖 Atendente: Redireciona<br/>verbalmente sem dados"]
        B6["⚙️ Tabulação no CRM<br/>Armazenamento de áudio<br/>[?] Nenhum ticket ao MTE"]
        S6["🏛️ MTE / Central 158<br/>Rede SINE de atendimento"]
        FP7{{"⚠️ FP7: Redirecionamento<br/>sem transferência<br/>sistêmica de dados"}}
    end

    %% ============================================================
    %% HORIZONTAL CONNECTIONS (main flow by layer)
    %% ============================================================
    C1 --> C2 --> C3 --> C4 --> C5 --> C6
    F1 --> F2 --> F3 --> F4 --> F5 --> F6
    B1 --> B2 --> B3 --> B4 --> B5 --> B6
    S1 --> S2 --> S3 --> S4 --> S5 --> S6

    %% ============================================================
    %% FAILURE DEMAND LOOPS (dashed red)
    %% ============================================================
    FP3 -.->|🔁 Redisca| C1
    FP3 -.->|🔁 Sobrecarrega fila| F4
    FP5 -.->|🔁 Redisca| C1
    FP4 -.->|🔁 Tenta de novo| C2
    FP4 -.->|🔁 Redisca| C1
    FP7 -.->|🔁 Failure Demand<br/>Cruzado Federativo| C1
    C6 -.->|"🚶 Cidadão circula<br/>MTE → SINE → volta"| C1
```

## Legenda

| Ícone | Significado |
|---|---|
| 🧑 | Ações do Cidadão — acima da Linha de Interação |
| 🤖 | Frontstage — visível ao cidadão (URA, ACD, Atendente) |
| ⚙️ | Backstage — invisível ao cidadão (sistemas, logs, QA) |
| 🏛️ | Processos de Suporte — terceiros, órgãos externos |
| ⚠️ FPn | Failure point identificado |
| [?] Lacuna / Hipótese | Processo existente sem documentação pública ou pendente de validação |
| 🔁 | Failure demand loop — retroalimentação que gera rediscagens |
| ---> | Fluxo principal do atendimento (etapas sequenciais) |
| -.->
