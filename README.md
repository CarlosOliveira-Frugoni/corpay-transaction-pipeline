# corpay-transaction-pipeline
Pipeline de dados e telemetria veicular (IoT) para monitoramento e gestão inteligente de frotas. Desenvolvido sob a perspectiva de otimização de custos logísticos, telemetria de alta frequência e geração de valor de negócio
# 🚚 Fleet Telemetry Pipeline

> Estudo técnico sobre a arquitetura de pipelines de dados e telemetria veicular (IoT) voltado para a otimização de frotas corporativas, análise de consumo e redução de custos logísticos. Projeto desenvolvido com base em engenharia de prompts utilizando o [NotebookLM](https://notebooklm.google/).

![Status](https://img.shields.io/badge/status-conclu%C3%ADdo-green)
![DIO](https://img.shields.io/badge/DIO-Bootcamp-blueviolet)
![Tema](https://img.shields.io/badge/tema-Telemetria%20%2B%20IoT%20%2B%20Dados-blue)

---

## 📌 Contexto e Objetivos

**Tema escolhido:** Pipeline de Dados e Telemetria para Otimização de Frotas Corporativas.

**Por que esse tema?**
Durante minha atuação no setor agroindustrial, vivi de perto o desafio de atuar na interface entre a engenharia de aplicação e o cliente final, traduzindo requisitos de projetos em eficiência real de campo. Com a minha transição de carreira para a área de dados, escolhi este tema para entender como arquitetar fluxos de dados de alta frequência capazes de monitorar a saúde veicular e o consumo de combustível, transformando a telemetria bruta em decisões financeiras estratégicas para gestores de frotas.

**Objetivos de estudo:**
- [x] Entender a captura de dados de telemetria veicular via rede CAN/OBD-II
- [x] Analisar o uso de protocolos leves de comunicação (como MQTT) no cenário de rodovias brasileiras
- [x] Estruturar o fluxo de um pipeline de dados (Ingestão -> Processamento -> Armazenamento -> Análise)
- [x] Avaliar como os dados de telemetria se convertem em inteligência financeira para frotas (redução de combustível, comportamento de condução)

---

## 📚 Curadoria de Fontes

Foram selecionadas as seguintes fontes abertas para upload no NotebookLM:

| # | Título | Tipo | Link |
|---|--------|------|------|
| 1 | Da Coleta Manual à Telemetria Automática: Integração de OBD-II em Gestão de Frotas | Artigo técnico | [Pesquisar no Acadêmico](https://scholar.google.com.br/) |
| 2 | Desenvolvimento de sistema embarcado para telemetria (UFPE) | Trabalho acadêmico | [Pesquisar no Acadêmico](https://scholar.google.com.br/) |
| 3 | Implementação de sistema de telemetria e comunicação sem fio (UFAM) | Trabalho acadêmico | [Pesquisar no Acadêmico](https://scholar.google.com.br/) |
| 4 | Acelerando o Futuro - Pipeline IA–IoT (eduCAPES) | Documento técnico | [Acessar CAPES](https://educapes.capes.gov.br/) |

---

## 🧪 Engenharia de Prompts e "Cicatrizes"

### Prompt 1 — Conceitos Técnicos de Telemetria

**Prompt utilizado:**
> *Com base nos documentos, quais são os dados mais comuns capturados da rede CAN/OBD-II dos veículos (como velocidade, RPM, consumo de combustível) e como funciona o protocolo MQTT para enviar esses dados de forma leve?*

**Resposta obtida (resumo):**
O NotebookLM mapeou os principais dados capturados de barramentos CAN veiculares (velocidade, RPM, consumo, nível de combustível, dinâmica de frenagem/pedal de aceleração e temperaturas vitais do motor/transmissão). 
Em relação ao protocolo **MQTT**, a IA detalhou os 3 pilares que o tornam ideal para IoT:
1. **Cabeçalho minimalista de apenas 2 bytes** (reduzindo em até 90% o consumo em relação ao HTTP).
2. **Arquitetura Publish/Subscribe desacoplada** via Broker central.
3. **Mecanismos de sessão persistente e Keep-Alive binários** (pacotes de ping de 2 bytes).

---

### Prompt 2 — Regra de Negócio & ROI

**Prompt utilizado:**
> *Agindo como um Arquiteto de Soluções de IoT e Telemetria, explique como o pipeline de dados de telemetria ajuda empresas a reduzirem custos operacionais com combustível e manutenção de frotas. Quais são os principais alarmes que o sistema pode disparar em tempo real para o gestor da frota com base nesses dados?*

**Resposta obtida (resumo):**
A IA relacionou o tratamento de dados a benefícios de negócios mensuráveis:
- **Redução de Combustível:** Análise de acelerações bruscas, ociosidade do motor e RPM fora da faixa ideal de torque, além da otimização de rotas.
- **Redução de Manutenção:** Algoritmos preditivos e preventivos para detectar falhas mecânicas antecipadamente e diminuição do desgaste de peças como freios e pneus através de condução segura.
- **Alarmes em Tempo Real:** Disparos imediatos para excesso de velocidade, comportamento de direção perigosa, desvio de rotas programadas (geofencing), superaquecimento de óleo/motor, e detecção precoce de anomalias preditas.

---

### Prompt 3 — Refinamento e "Cicatriz" (Troubleshooting)

**❌ Primeira tentativa (prompt genérico):**
> *Como funciona a telemetria?*

**Problema encontrado:** A resposta inicial foi vaga e focada em aspectos superficiais (como a história do GPS), sem trazer a arquitetura interna do fluxo de dados das fontes fornecidas.

**✅ Prompt refinado (foco em resiliência de rede):**
> *Como o pipeline de dados lida com a perda temporária de sinal de internet em rodovias brasileiras (latência/desconexão)? Quais técnicas de buffer local ou retransmissão são sugeridas nos artigos?*

**Resultado após refinamento:**
A IA detalhou três estratégias de engenharia presentes nos textos:
1. **Buffer de Hardware local:** Armazenamento em cartões SD com microcontroladores separados (um para ler os dados da rede CAN e outro dedicado exclusivamente à gravação, evitando concorrência e perdas).
2. **Resiliência do MQTT:** Persistência de mensagens usando flags de QoS (Qualidade de Serviço) e retenção, além do mecanismo de emergência *Last Will* (Testamento) que avisa o Broker que o dispositivo ficou offline.
3. **Confirmação confiável de recebimento (Reliable ACKs):** Estrutura (semelhante à utilizada pela Tesla) onde o carro só apaga o dado local do buffer após receber uma confirmação assinada pelo servidor na nuvem.

**💡 Lição aprendida ("Cicatriz"):**
Modelos de IA aplicados a materiais técnicos exigem especificidade contextual. Prompts genéricos de telemetria ignoram os detalhes estruturais da rede móvel instável. Ao refinar a pergunta focando em "zonas de sombra", "buffer" e "protocolos", extraímos soluções reais de engenharia aplicáveis às rodovias do Brasil.

---

## 📖 Miniguia de Estudo (Entrega Final)

### Resumos Estruturados

*   **Rede CAN e Leitura OBD-II:** A telemetria moderna se conecta diretamente à rede do veículo para colher dados físicos convertidos em informações digitais através da porta padrão de diagnóstico OBD-II.
*   **Protocolo de Ingestão (MQTT):** O MQTT atua abrindo uma conexão única persistente para enviar rajadas pequenas de dados no formato Publish/Subscribe, gerando enorme economia de banda.
*   **Tratamento de Perdas (Resiliência):** O pipeline de dados é projetado prevendo a desconexão temporária. O sistema a bordo acumula os dados no buffer do cartão SD e só descarta após receber o "Reliable ACK" do servidor ao reestabelecer o sinal móvel.

### Glossário de Conceitos

| Termo | Definição | Importância na Gestão de Frotas |
|---|---|---|
| **Rede CAN / OBD-II** | Barramento de dados interno do veículo que conecta todos os módulos eletrônicos. | Permite acesso direto aos sensores nativos do motor e do chassi. |
| **MQTT** | Protocolo leve de mensagem do tipo Pub/Sub voltado para redes móveis instáveis. | Reduz severamente o custo de franquia de dados celulares do rastreador. |
| **Broker MQTT** | Servidor centralizado intermediário que recebe as mensagens do veículo. | Garante o desacoplamento entre o dispositivo veicular e a aplicação final. |
| **Buffer Local** | Armazenamento de dados local temporário no próprio hardware do carro. | Evita a perda de informações cruciais de telemetria em áreas sem sinal. |
| **Time-Series Database** | Banco de dados otimizado para salvar registros marcados com data e hora. | Essencial para rastrear a evolução temporal do consumo e falhas de cada placa. |
| **Reliable ACKs** | Confirmação de recebimento que o servidor envia de volta ao veículo. | Mecanismo de segurança que impede a exclusão de dados do buffer sem garantia de gravação na nuvem. |

### Prompts Reutilizáveis

```text
Prompt 1: "Com base nos artigos de telemetria de frotas, como a técnica de windowing (janelamento de tempo) pode ser aplicada no processamento de dados de sensores de aceleração (frenagens bruscas) para evitar sobrecarregar o banco de dados principal?"
```

```text
Prompt 2: "Explique como uma arquitetura orientada a microsserviços gerencia o consumo simultâneo de pacotes de dados MQTT gerados por frotas contendo milhares de veículos em tempo real."
```

---

## 🛠️ Tecnologias e Ferramentas

- **NotebookLM** — Pesquisa grounded e engenharia de prompts.
- **GitHub** — Versionamento e portfólio.

---

## 👤 Autor

**Carlos Oliveira Frugoni**

[![GitHub](https://img.shields.io/badge/GitHub-Perfil-181717?logo=github)](https://github.com/CarlosOliveira-Frugoni)

---

> 📝 Projeto desenvolvido como parte do Bootcamp da Sem Parar Corpay na [Digital Innovation One (DIO)](https://dio.me).
