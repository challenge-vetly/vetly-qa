# Vetly — Regras de Negócio, Entidades e Relacionamentos

> Documento técnico do Vetly. Fonte de verdade das **regras de negócio (RN-001 a RN-107)**, do **mapa de entidades e relacionamentos** e do **modelo de dados**. Os fluxos da plataforma e a visão de produto estão em **[vetly-produto.md](./vetly-produto.md)**, que referencia estas RNs.
>
> **Convenções:** a persona dona do pet é **Responsável**. A operação do Responsável acontece **inteiramente no app** — não há canal WhatsApp em nenhuma etapa. Onde uma regra é **alvo de produção** e não entra no MVP, isso está sinalizado na própria regra, sem removê-la.
>
> **Parâmetros:** todos os valores numéricos citados pelas RNs estão fechados na §1. A referência de mercado que embasa cada um está na §7, para que qualquer recalibração futura seja discutida contra a origem do número.

---

## 1. Parâmetros de Lançamento

Fonte única dos números citados pelas RNs. Ajustáveis com dados reais.

Todos os valores estão definidos. A referência de mercado que embasa cada um está na §7.

| Domínio | Parâmetro | Valor | RN |
|---|---|---|---|
| Matching | Raio de busca | 10 km default, expansível a 25 km pelo Responsável | RN-028 |
| Matching | Pesos do score | distância 40% · avaliação 30% · disponibilidade 30% | RN-030 |
| Matching | Mínimo de avaliações para nota pública | 3 avaliações | RN-057 |
| Matching | Selo de perfil novo | 30 dias a partir da publicação do perfil | RN-033 |
| Agendamento | Lock de checkout | 10 minutos | RN-035 |
| Lista de espera | Prioridade do 1º da fila | 15 minutos | RN-037 |
| Agendamento | Limite de remarcações por consulta | 2 remarcações | RN-043 |
| Cancelamento | Reembolso integral | > 24h de antecedência | RN-041 |
| Cancelamento | Reembolso parcial | 24h a 2h, retenção configurável pela clínica | RN-041/042 |
| Cancelamento | Sem reembolso | < 2h ou no ato | RN-041 |
| Documentos | Janela de correção livre | 24 horas | RN-088 |
| Fidelidade | Ganho por serviço pago | **1 ponto por R$ 1 gasto** | RN-047 |
| Fidelidade | Ganho por obrigação cumprida no prazo | **50 pontos por obrigação** | RN-047 |
| Fidelidade | Tiers (acúmulo em 12 meses) | Bronze 0–999 · Prata 1.000–2.999 · Ouro 3.000+ | RN-048 |
| Fidelidade | Multiplicador de ganho | Bronze 1,0× · Prata 1,25× · Ouro 1,5× | RN-048 |
| Fidelidade | Conversão | **100 pontos = R$ 3** | RN-049 |
| Fidelidade | Expiração de pontos | 12 meses (FIFO) | RN-050 |
| Fidelidade | Financiamento do desconto | ≤ R$ 10: 100% Vetly · R$ 10,01–30: 60/40 · > R$ 30: 30/70 | RN-051 |
| Fidelidade | Validade do cupom QR | 30 dias, sem retorno de pontos | RN-053 |
| Avaliação | Janela para avaliar | 14 dias após "realizada" | RN-055 |
| Comissão | Take rate de serviço | **15% Básico / 12% Profissional / 10% Enterprise** | RN-070 |
| Planos | Assinatura Básico | **R$ 0 (freemium)** — monetizado só pelo take rate | RN-072 |
| Planos | Assinatura Profissional | **R$ 249/mês** | RN-072 |
| Planos | Enterprise por faixa de vets | R$ 599 (1–5) · R$ 999 (6–10) · R$ 1.699 (11–20) · +R$ 70/vet acima de 20 | RN-072 |
| Marketplace | Taxa de listagem mensal por item | Alimentação R$ 150 · Medicamentos R$ 250 · Higiene R$ 100 | RN-073 |
| Dados | k-anonimato Nível 1 | k ≥ 100 por célula | RN-075 |
| Notificações | Régua de reenvio | 3 tentativas (7 / 3 / 1 dias antes), push + in-app | RN-094 |
| Colmeia | Expiração do acesso concedido | consulta + 24h + retornos vinculados | RN-065 |

---

## 2. Mapa de Entidades e Relacionamentos

Cada entidade lista atributos principais e relacionamentos com cardinalidade. Atributos completos e status de MVP estão na §5; esta seção é o mapa relacional.

### ENTIDADE: Responsável
Atributos: dados de conta, consentimento LGPD granular por finalidade (data/hora de registro e de revogação), dispositivos/push tokens, geolocalização autorizada (fallback por CEP/bairro — RN-027), preferências de notificação por categoria, opt-in de promoções, tier de fidelidade, saldo de pontos.
Relacionamentos:
- Possui **Animal** (1:N) — múltiplos pets; 1 Responsável por pet
- Agenda **Consulta** (1:N) — no app, com pré-sintomas anexados (RN-005/RN-036)
- Realiza **Pagamento** (1:N) — no MVP registro simulado, sem liquidação (RN-071)
- Recebe **Documento** (1:N) — no board do pet, dentro do app (RN-090)
- Escreve **Avaliação** (1:N) — uma por consulta realizada (RN-056)
- Acumula **PontosFidelidade** (1:N) — por obrigações cumpridas e serviços pagos (RN-047)
- Gera **CupomResgate** (1:N) — troca de pontos por item do marketplace (RN-053)
- Recebe **Notificação** (1:N) — push + inbox in-app (RN-092)

### ENTIDADE: Veterinário
Atributos: CRMV, UF de atuação, especialidades, espécies atendidas, titulação acadêmica, agenda (dias/horários, duração média, intervalo), serviços oferecidos (tipo, valor, aceita plano de saúde pet), dados bancários (banco, agência, conta, CPF/CNPJ do titular), chave Pix, plano de assinatura (Básico/Profissional/Enterprise), persona (autônomo ou vinculado). **Campos de endereço embutidos** (CEP, logradouro, número, complemento, bairro, cidade, UF, latitude/longitude derivadas do endereço — RN-026; modelo 1:1, sem tabela separada). Métricas de matching: nota média, nº de avaliações, status no matching (ativo/suspenso).
Relacionamentos:
- Realiza **Consulta** (1:N)
- Solicita **Exame** (1:N)
- Acompanha **Internação** (1:N)
- Assina **Documento** (1:N) — receita; no MVP a assinatura é o nome digitado (RN-087)
- Recebe **Avaliação** (1:N)
- Pertence a **Empresa** (N:1) — quando vinculado
- Recebe **Pagamento** (1:N) — repasse persona autônoma; no MVP registrado, não liquidado
- Gera **LogDeAuditoriaIA** (1:N) e gera **LogDeAcessoProntuário** (1:N)

### ENTIDADE: Empresa
Atributos: tipo (clínica, hospital veterinário, pet shop com serviços clínicos, estabelecimento com múltiplos profissionais), planos de saúde aceitos, serviços por profissional, faixa de assinatura Enterprise por nº de vets (RN-072), política de retenção em cancelamento parcial (RN-042). Endereço e coordenadas nos mesmos moldes do Veterinário (RN-026).
Relacionamentos:
- Tem **Administrador** (1:1)
- Emprega **Veterinário** vinculado (1:N)
- Designa **Veterinário** para **Consulta** (1:N) — agendamento feito com a clínica (RN-003)
- Origina **Pagamento** (1:N) — repasse da unidade; no MVP registrado, não liquidado

### ENTIDADE: Administrador
Atributos: escopo de acesso **consolidado da unidade com vedações financeiras** (RN-106) — sem dados bancários pessoais de vets, sem remuneração interna dos vinculados, sem dados de outros estabelecimentos (vetly-produto.md §7.3).
Relacionamentos:
- Gerencia **Empresa** (1:1)
- Gerencia **Veterinário** vinculado (1:N) — cadastra, edita, desativa (RN-022)
- Redistribui **Consulta** (1:N) — agendamentos de vet desativado (RN-025)

### ENTIDADE: Animal
Atributos: nome, espécie, raça, sexo, nascimento/idade, **peso** (obrigatório para a IA sugerir dose — RN-081), foto, castrado, condições pré-existentes, alergias, carteira de vacinação, histórico clínico longitudinal, medicações em uso, alertas clínicos ativos, flags de registros ocultados pelo Responsável (exceto alertas de segurança — RN-068).
Relacionamentos:
- Pertence a **Responsável** (N:1)
- Tem **Prontuário** (1:N) — vinculado ao animal, não ao vet (RN-063)
- Tem **Consulta** (1:N), **Internação** (1:N), **Exame** (1:N)
- Tem **ObrigaçãoDoPet** (1:N) — calendário derivado de espécie/raça/idade (RN-046)
- Tem **AvatarPet** (1:1) — mockado no MVP, só cachorro (RN-096)
- Tem **LogDeAcessoProntuário** (1:N) — visível ao Responsável (RN-067)

### ENTIDADE: Consulta
Atributos: data, horário, serviço/necessidade, vet responsável, pré-sintomas anexados (estruturado + mídia — RN-036), **estado do slot/consulta** (livre / em checkout / confirmada / realizada / cancelada / no-show — RN-035/RN-038), lock de checkout, contador de remarcações, timestamps de início e encerramento da consulta pelo vet (RN-008), diagnóstico aprovado, protocolo aprovado, referência à **Avaliação**.
Relacionamentos:
- Realizada por **Veterinário** (N:1)
- Sobre **Animal** (N:1)
- Agendada por **Responsável** (N:1) — no app (RN-001 a RN-006)
- Gera **Documento** (1:N) — do estado final aprovado pelo vet (RN-083)
- Tem **Pagamento** (1:1) — simulado; confirma a consulta e encerra o lock (RN-006/RN-035)
- Gera **Avaliação** (1:0..1) — disparada quando o vet marca "realizada" (RN-055)
- Gera **PontosFidelidade** (1:N) — só se confirmada e realizada (RN-052)
- Gera **LogDeAuditoriaIA** (1:N) — cada decisão da IA/vet (RN-084)
- Cumpre **ObrigaçãoDoPet** (0..1) — quando realizada até a data-limite (RN-047)

### ENTIDADE: Prontuário
Atributos: dados clínicos completos e estruturados, versão original, versão corrigida (data, hora, CRMV do solicitante — RN-088), justificativa quando fora do prazo (RN-089).
Relacionamentos:
- Pertence a **Animal** (N:1) — do animal, não do vet/clínica (RN-063)
- Gerado por **Consulta** (N:1) ou por **Internação** (N:1)
- Referencia **Prontuário** (1:0..1) — auto-relacionamento original × versão corrigida; ambas coexistem

### ENTIDADE: Exame
Atributos: tipo de solicitação, orientações ao Responsável, resultado (upload pelo vet — MVP; recebimento por laboratório parceiro — alvo), status de liberação ao Responsável.
Relacionamentos:
- Solicitado por **Veterinário** (N:1)
- Vinculado a **Animal** (N:1) — resultado incorporado ao histórico (RN-103/RN-104)

### ENTIDADE: Internação
Atributos: procedimentos diários, medicações administradas, evolução clínica diária, nº de diárias, valor total apurado, caução de entrada, saldo restante na saída. No MVP os valores são registrados, não liquidados.
Relacionamentos:
- Vinculada a **Animal** (N:1)
- Acompanhada por **Veterinário** (N:1)
- Gera **Documento** (1:N) — resumo da internação e NF do saldo (RN-102)
- Tem **Pagamento** (1:N) — caução + saldo (RN-101)

### ENTIDADE: Documento
Atributos: tipo (Prontuário, Atestado saúde/óbito/transporte, Receita Veterinária, Nota Fiscal), versão (original/corrigida), data/hora de geração, CRMV signatário (receita), **assinatura — MVP: nome digitado; produção: certificado ICP-Brasil (RN-087)**.
Relacionamentos:
- Gerado por **Consulta** (N:1) — do estado final aprovado ou corrigido pelo vet (RN-083)
- Gerado por **Internação** (N:1)
- Assinado por **Veterinário** (N:1) — obrigatório para receita
- Recebido por **Responsável** (N:N) — destino: board do pet no app (RN-090)

### ENTIDADE: Pagamento
Atributos: **no MVP é registro simulado**, sem gateway de pagamento integrado (status, valor, comissão calculada por RN-070, incidência do desconto de fidelidade por RN-051); tipos: consulta, caução/saldo de internação; meio-alvo de produção Pix/cartão, fornecedor a definir; momento (antecipado no agendamento / caução + saldo na internação / no ato em emergência — RN-040). Nenhum valor liquidado no MVP.
Relacionamentos:
- Realizado por **Responsável** (N:1) — pagador em todos os cenários
- Associado a **Consulta** (N:1) ou a **Internação** (N:1)
- Vinculado a **Veterinário** (N:1) — repasse persona autônoma (registrado)
- Vinculado a **Empresa** (N:1) — repasse persona empresa (registrado)

### ENTIDADE: Avaliação
Atributos: nota geral (1–5, obrigatória — única obrigatória no MVP — RN-056), comentário opcional, data, status de moderação.
Relacionamentos:
- Sobre **Consulta** (N:1) — única por consulta realizada (RN-056)
- Escrita por **Responsável** (N:1)
- Recebida por **Veterinário** (N:1) — nota alimenta a busca (RN-057)

### ENTIDADE: ObrigaçãoDoPet
Atributos: tipo (vacina, vermífugo, retorno, check-up, higienização), data-limite, status (em dia / a vencer / atrasado).
Relacionamentos:
- Pertence a **Animal** (N:1) — gerada no cadastro do pet (RN-046)
- Cumprida por **Consulta** (0..1) — serviço realizado até a data-limite
- Gera **Notificação** (1:N) e gera **PontosFidelidade** (1:N)
- Alimenta **AvatarPet** (N:1) — em produção; mockado no MVP (RN-096)

### ENTIDADE: PontosFidelidade
Atributos: evento de origem, pontos brutos, multiplicador de tier aplicado, pontos creditados, data, expiração (12m, FIFO — RN-050), estorno. No MVP acúmulo e tier são reais; o desconto em reais é calculado/exibido, sem abatimento (RN-051).
Relacionamentos:
- Acumulados por **Responsável** (N:1)
- Originados de **Consulta** ou **ObrigaçãoDoPet** (N:1)
- Consumidos por **CupomResgate** (N:1)

### ENTIDADE: CupomResgate
Atributos: código QR, item resgatado, pontos debitados, valor do desconto em reais, divisão da incidência (parte Vetly / parte vet — RN-051), data de emissão, validade, status (emitido / resgatado / expirado).
Relacionamentos:
- Emitido para **Responsável** (N:1)
- Referencia **ItemMarketplace** (N:1) — mockado no MVP
- Debita **PontosFidelidade** (1:N)

### ENTIDADE: Notificação
Atributos: evento, classificação (aviso × ação/conteúdo — RN-091), canal (in-app, push), tentativa da régua (RN-094), status de leitura, exigência de opt-in (promoções — RN-093).
Relacionamentos:
- Recebida por **Responsável** ou **Veterinário** (N:1)
- Originada de um evento (N:1)

### ENTIDADE: LogDeAuditoriaIA
Atributos: timestamp, versão do modelo, trecho capturado, conteúdo sugerido, decisão do vet (aprova/rejeita/corrige), **conteúdo final**, CRMV. Log **imutável**, retido junto ao Prontuário (RN-084).
Relacionamentos:
- Vinculado a **Consulta** (N:1) e a **Veterinário** (N:1)

### ENTIDADE: LogDeAcessoProntuário
Atributos: quem, quando, contexto (consulta X), base de acesso (consentimento de rede × atendimento direto). **Visível ao Responsável** (RN-067).
Relacionamentos:
- Vinculado a **Animal** (N:1) e a **Veterinário** (N:1)

### ENTIDADE: AvatarPet *(mockado no MVP)*
Atributos: estado visual derivado das obrigações (saudável, vacina atrasada, higiene atrasada), espécie suportada (só cachorro no MVP — RN-096).
Relacionamentos:
- Pertence a **Animal** (1:1)
- Deriva de **ObrigaçãoDoPet** (1:N) — em produção; no MVP os estados são fixos no front (RN-097)

### ENTIDADE: EmpresaParceira *(alvo, não MVP)*
Atributos: razão social, categorias de atuação, taxa de listagem contratada por categoria (RN-073), status de contrato.
Relacionamentos:
- Oferece **ItemMarketplace** (1:N)
- Honra **CupomResgate** (1:N)

### ENTIDADE: ItemMarketplace *(alvo, não MVP)*
Atributos: nome, categoria (alimentação / medicamentos / higiene), preço de referência, custo em pontos, status de listagem. Catálogo em memória no front durante o MVP (RN-098).
Relacionamentos:
- Ofertado por **EmpresaParceira** (N:1)
- Resgatado via **CupomResgate** (1:N)

---

## 3. Regras de Negócio — Base (RN-001 a RN-025)

Regras diretamente vinculadas aos fluxos da §8 do README. A coluna **Escopo** indica o grau de implementação no MVP.

| RN | Regra | Escopo |
|---|---|---|
| RN-001 | Com a geolocalização concedida, o app lista clínicas e vets autônomos por proximidade e serviço oferecido. | MVP |
| RN-002 | O Responsável filtra por necessidade (banho, tosa, emergência, rotina); o resultado é ordenado por score de distância, avaliação e disponibilidade (RN-030). | MVP |
| RN-003 | Ao escolher uma **clínica**, a consulta é atribuída ao profissional que a clínica designar; ao escolher um **vet autônomo**, a consulta é agendada diretamente com ele. | MVP |
| RN-004 | Sem horário disponível, o app oferece três saídas: lista de espera para aquele vet, horários com outros vets da mesma clínica/região, ou data futura. | MVP |
| RN-005 | Os pré-sintomas informados no agendamento são anexados ao briefing pré-consulta e servem de entrada à IA da consulta (RN-078). | MVP |
| RN-006 | A consulta só é confirmada após o pagamento. **No MVP o pagamento é etapa simulada** que retorna sucesso e dispara a confirmação; NF e split são calculados e gravados, não liquidados. | MVP (simulado) |
| RN-007 | Todo evento operacional dirigido ao Responsável é notificado por **in-app + push**; não existe canal WhatsApp. | MVP |
| RN-008 | A consulta **inicia** quando o vet aciona "iniciar consulta" e **encerra** quando o vet aciona "encerrar consulta"; esses dois eventos delimitam a janela de captura da IA e disparam os processos pós-consulta. | MVP |
| RN-009 | Durante a janela aberta, a IA recebe as falas do médico e captura os dados clínicos do atendimento (RN-080). | MVP (Profissional+) |
| RN-010 | Encerrada a consulta, o sistema gera prontuário, atestado (quando aplicável), receita e NF a partir do estado final aprovado pelo vet (RN-082/RN-083). | MVP (Profissional+) |
| RN-011 | Uma automação publica os documentos gerados no app e os incorpora ao histórico de vida do animal. | MVP |
| RN-012 | Sobre um agendamento ativo, o Responsável pode cancelar ou remarcar pelo app. | MVP |
| RN-013 | Na remarcação, o pagamento já realizado é transferido para a nova data, sem nova cobrança. | MVP (registrado) |
| RN-014 | Janelas de reembolso: > 24h integral; 24h–2h parcial com retenção configurável pela clínica; < 2h ou no ato, sem reembolso. No MVP o valor é calculado e exibido, não liquidado. | MVP (registrado) |
| RN-015 | Obrigação cumprida no prazo ou serviço pago credita pontos, aplicando o multiplicador do tier vigente (RN-048). | MVP |
| RN-016 | A cada crédito, o sistema recalcula o acúmulo de 12 meses e reavalia o tier do Responsável, notificando mudanças. | MVP |
| RN-017 | Ao selecionar um item, o sistema calcula e exibe o desconto em reais e a faixa de financiamento Vetly × vet aplicável (RN-051). | MVP (calculado/exibido) |
| RN-018 | Confirmada a troca, os pontos são debitados (FIFO), o cupom QR é emitido e a redução de comissão e de repasse é registrada conforme a faixa. | MVP (registrado) |
| RN-019 | O cupom QR é validado no estabelecimento e o desconto é aplicado ao item escolhido. **No MVP não há validação real nem movimentação de valores.** | Mock |
| RN-020 | O cadastro do pet gera o calendário de ObrigaçõesDoPet a partir de espécie, raça e idade, e define o estado inicial do avatar. | Calendário: MVP · Avatar: mock |
| RN-021 | O avatar reflete o estado das obrigações: vencida sem cumprimento altera o estado visual; cumprida devolve ao estado saudável. **No MVP os estados são mockados e só há cachorro.** | Mock |
| RN-022 | A desativação de um vet vinculado pelo administrador encerra o acesso à plataforma imediatamente. | MVP |
| RN-023 | O histórico dos atendimentos produzidos permanece vinculado ao animal, independentemente do desligamento do profissional. | MVP |
| RN-024 | O vet desativado perde acesso aos prontuários e pode solicitar apenas extrato dos atendimentos que realizou, sem dados pessoais do Responsável ou do animal. | MVP |
| RN-025 | Agendamentos futuros do vet desativado são sinalizados ao administrador para redistribuição ou cancelamento, com notificação in-app + push ao Responsável. | MVP |

---

## 4. Regras de Negócio — Plataforma (RN-026 a RN-107)

Formato: **enunciado | justificativa**. O status de MVP, quando relevante, está no próprio enunciado.

### A. Matching e Geolocalização
- **RN-026** | Todo vet/unidade tem **endereço persistido em banco** nos campos do próprio registro (modelo 1:1, sem tabela separada), em bloco obrigatório do cadastro; a **latitude/longitude é derivada desse endereço** e é a fonte usada na busca e no cálculo de proximidade — nunca dado mockado no front. | Localização é dado de negócio auditável e reaproveitável; não pode viver só no cliente.
- **RN-027** | A distância é calculada entre a **posição do Responsável** (permissão de localização do dispositivo) e a **coordenada do vet** (RN-026). Negada a permissão, o fallback ordena por bairro/CEP informado pelo Responsável. | Sem fallback o fluxo de busca trava quando a permissão é negada.
- **RN-028** | A busca lista vets/clínicas dentro de um raio default de **10 km**, expansível pelo Responsável até **25 km**. | Cobre a densidade de um centro urbano sem devolver opções inviáveis; a expansão manual resolve regiões de baixa densidade sem precisar de raio dinâmico.
- **RN-029** | Espécie atendida compatível com o pet é **filtro eliminatório**: vet que não atende a espécie nunca aparece no resultado. | Evita matching clinicamente inválido.
- **RN-030** | A ordenação combina três fatores numa nota única: **distância, avaliação e disponibilidade**, com pesos **40 / 30 / 30**. Vet sem o mínimo de avaliações (RN-057) é ordenado apenas por distância e disponibilidade. | Traduz a promessa do README §3.2 (escolher por qualidade, disponibilidade e distância) em critério computável.
- **RN-031** | Desempate: maior nota → menor distância → maior disponibilidade nas próximas 48h. | Critério determinístico e auditável.
- **RN-032** | Filtros disponíveis: tipo de serviço/necessidade, especialidade, faixa de preço e "atende hoje". | Definição de produto (README §3.2).
- **RN-033** | Vet sem o mínimo de avaliações (RN-057) recebe o selo **"Novo na Vetly" por 30 dias** e permanece elegível ao resultado, ordenado apenas por distância e disponibilidade — sem nota atribuída e sem boost artificial. | Resolve o cold start sem punir entrantes nem inventar reputação que o vet ainda não tem.

### B. Agendamento e Máquina de Estados
- **RN-034** | O agendamento parte de **pet + serviço/necessidade** e exibe disponibilidade em tempo real por vet ou clínica. | Definição de produto.
- **RN-035** | **Máquina de estados do slot:** `livre → em checkout (lock de 10 min) → confirmado`, ou `→ liberado` se o lock expira. O checkout inicia ao entrar no pagamento simulado; o retorno de sucesso transiciona para `confirmado`. | Impede overbooking sem gateway real e torna o lock testável.
- **RN-036** | Pré-sintomas são coletados como **texto guiado + mídia opcional** e vinculados à consulta; alimentam o briefing (RN-005) e a IA (RN-078). | Qualifica a demanda e é a única fonte de contexto prévio do Responsável.
- **RN-037** | Lista de espera: liberado um slot, o 1º da fila tem **prioridade por 15 min**, com push e confirmação em um toque; expirada, passa ao próximo. | Ordena a fila sem intervenção manual.
- **RN-038** | Estados da consulta: `confirmada → realizada` (o vet marca) `| cancelada | no-show`. "Realizada" dispara avaliação (RN-055) e pontuação (RN-052). | Fonte única do ciclo de vida da consulta.
- **RN-039** | Todo atendimento é **presencial** no MVP; a modalidade é registrada no agendamento. Procedimentos que exigem presença física (cirurgia, vacinação, exame clínico) são sempre presenciais. Atendimento remoto é alvo de produção. | Segurança clínica + escopo declarado no README §5.9.
- **RN-040** | Em emergência presencial sem agendamento prévio, o pagamento ocorre no ato do atendimento, e não de forma antecipada. | Exceção prevista no README §5.4.

### C. Cancelamento, Reembolso e No-show
- **RN-041** | O cancelamento pelo Responsável aplica as janelas de reembolso (RN-014); no MVP a consequência é calculada e exibida, não liquidada. | Coerência com o pagamento simulado.
- **RN-042** | O **percentual de retenção** da faixa parcial (24h–2h) é configurado pela clínica/vet no onboarding e exibido ao Responsável no momento do agendamento. | A política é da clínica; a plataforma só a aplica e a torna transparente.
- **RN-043** | A remarcação transfere o pagamento sem nova cobrança e incrementa o contador da consulta, limitado a **2 remarcações**; esgotado o limite, resta cancelar sob a política vigente (RN-041). | Duas remarcações cobrem imprevisto legítimo; acima disso, remarcar vira burla à janela de reembolso.
- **RN-044** | O no-show do Responsável é registrado na consulta e não gera reembolso, seguindo a faixa "< 2h ou no ato" (RN-014). | Reaproveita a política já definida, sem criar penalidade nova.
- **RN-045** | Cancelamento pelo vet ou pela clínica notifica o Responsável por in-app + push e libera o slot; quando decorrente de offboarding, segue a RN-025. | Simetria de tratamento e coerência com o fluxo 6.

### D. Fidelidade, Pontos e Resgate
- **RN-046** | "Obrigações do pet" são os eventos do protocolo por espécie/raça/idade (vacinas, vermífugos, retornos, check-ups, higienização), gerados no cadastro do pet e atualizados a cada consulta. | Define o objeto do programa e a fonte do avatar (RN-096).
- **RN-047** | **Fato gerador e volume de pontos:** (a) serviço pago na plataforma rende **1 ponto por R$ 1 gasto**, arredondado para baixo; (b) obrigação do pet cumprida **no prazo** via Vetly rende **50 pontos fixos**, independentemente do valor. Ambos passam pelo multiplicador de tier (RN-048). | 1 ponto por real é o padrão universal do varejo pet brasileiro (Petz Clubz, Petios), imediatamente compreensível. O bônus fixo por obrigação é o que diferencia a Vetly: recompensa **comportamento de cuidado**, não só gasto — um Responsável que mantém a vacinação em dia pontua mesmo em ano de baixa despesa.
- **RN-048** | **Tiers e multiplicador:** Bronze (0–999 pts/12m, 1,0×), Prata (1.000–2.999, 1,25×), Ouro (3.000+, 1,5×). O multiplicador incide sobre os pontos brutos no momento do crédito. | Faixas calibradas contra o ganho da RN-047 para serem alcançáveis: Responsável de 1 pet com uso ocasional fica em Bronze; 1 pet com calendário em dia e uso regular chega a Prata no primeiro ano; multi-pet ou uso intenso chega a Ouro. Multiplicador crescente no topo é o padrão dos programas de fidelidade pet que sustentam recorrência.
- **RN-049** | **Conversão:** **100 pontos = R$ 3** de desconto. | Calibrado sobre o retorno efetivo praticado em programas de saúde pet consolidados (Petco Pals devolve ~3% do gasto). Com o ganho da RN-047, o programa devolve cerca de **3% do valor gasto mais um bônus de comportamento**, o que cabe dentro de um take rate de 10–15%. Uma conversão mais generosa devolveria mais do que a comissão inteira da transação.
- **RN-050** | Pontos expiram em **12 meses** a partir do crédito, consumidos em **FIFO**; o tier é reavaliado sobre a janela móvel de 12 meses. | Estimula recorrência e evita passivo eterno de pontos.
- **RN-051** | **Financiamento do desconto:** o valor resgatado é absorvido dentro do split da transação, por faixa — até R$ 10: **100% Vetly**; de R$ 10,01 a R$ 30: **60% Vetly / 40% vet**; acima de R$ 30: **30% Vetly / 70% vet**. No MVP a divisão é calculada, gravada e exibida, **sem abatimento financeiro real**. | Descontos pequenos não oneram o vet (adesão); resgates grandes são co-financiados por quem captura a recorrência.
- **RN-052** | Só pontuam eventos com consulta/serviço **confirmado e realizado**; cancelamento e reembolso estornam os pontos correspondentes. | Antifraude básica do programa.
- **RN-053** | A troca de pontos emite um **CupomResgate** com código QR, item vinculado, valor do desconto, divisão da incidência e **validade de 30 dias**; expirado o cupom, os pontos **não** retornam ao saldo. | Trinta dias é a validade padrão de cupom no maior marketplace de consumo do país (iFood) e cria urgência de resgate. O não retorno dos pontos evita passivo perpétuo e resgate especulativo — o Responsável é avisado da validade na emissão e por push antes do vencimento.
- **RN-054** | Um cupom vale para **um item e uma transação**; não é acumulável com outro cupom na mesma compra. | Evita empilhamento de descontos sobre a mesma margem.

### E. Avaliação
- **RN-055** | **Gatilho:** o pop-up de avaliação dispara quando o vet marca a consulta como "realizada" (RN-038); o Responsável tem **14 dias** para responder. | Definição de produto + antifraude estrutural (só quem foi atendido avalia).
- **RN-056** | **Relação 1 avaliação : 1 consulta realizada.** A **nota geral (1–5) é a única obrigatória no MVP**; comentário é opcional. | Nota simples para alimentar o ranking sem fricção.
- **RN-057** | A nota do vet só é exibida publicamente e só entra no score de matching a partir de **3 avaliações**; antes disso vale a RN-033. | Três é o mínimo adotado por marketplaces de reputação para publicar média: abaixo disso uma única nota extrema define o perfil inteiro.
- **RN-058** | Comentários com dados pessoais, ofensas ou conteúdo fora de escopo são ocultados pela moderação; **a nota permanece**, salvo fraude comprovada. | Modera o texto sem permitir gestão de nota via denúncia.
- **RN-059** | Avaliações de consultas canceladas ou reembolsadas são invalidadas e removidas do cálculo. | Protege o ativo central de notoriedade.

### F. Colmeia Clínica e LGPD
- **RN-060** | No primeiro acesso ao app, o Responsável confirma dados, cadastra o pet e registra o **consentimento LGPD antes de qualquer outra ação**. | Base legal precede o tratamento de dados.
- **RN-061** | O consentimento é **granular por finalidade**: atendimento clínico, lembretes e comunicação proativa, compartilhamento com clínicas parceiras da rede, promoções (opt-in — RN-093) e dados agregados (RN-077). | LGPD: finalidade específica, informada e destacável.
- **RN-062** | A revogação é feita no próprio app, a qualquer momento, com registro de **data e hora**; cessa concessões futuras sem apagar registros clínicos já produzidos. | Concilia direito do titular com a guarda regulatória do prontuário.
- **RN-063** | O prontuário **pertence ao animal** e o acompanha entre vets e unidades; nenhuma clínica retém o histórico. | Tese central do produto (README §1).
- **RN-064** | **Colmeia por evento clínico:** ao agendar com um vet, se o consentimento de rede está ativo, o acesso ao histórico completo é concedido **automaticamente** àquele vet. | Portabilidade sem ação manual do Responsável.
- **RN-065** | O acesso concedido por agendamento **expira ao fim do ciclo (consulta + 24h + retornos vinculados)**; depois o vet mantém apenas o extrato do que produziu. | Minimização de acesso.
- **RN-066** | Sem consentimento de rede, vale o **acesso restrito**: o vet vê apenas o que produziu e o que o Responsável liberar explicitamente. | Consentimento é a chave que abre a colmeia, não o default silencioso.
- **RN-067** | Todo acesso a prontuário gera entrada no **LogDeAcessoProntuário** (quem, quando, contexto, base de acesso), **visível ao Responsável**. | Transparência é o que torna a colmeia sustentável juridicamente.
- **RN-068** | O Responsável pode ocultar registros do histórico, **exceto alertas de segurança (alergias e interações), que nunca são ocultáveis**. | Trava de segurança clínica acima da preferência de privacidade.
- **RN-069** | Dados de saúde do animal são tratados como **sensíveis**, com camada adicional de proteção de acesso. | Classificação declarada no README §7.2.

### G. Monetização
- **RN-070** | Take rate por transação conforme o plano do vet: **15% Básico / 12% Profissional / 10% Enterprise**. A maior comissão pertence ao menor plano. No MVP o valor é calculado e registrado, não liquidado. | Core financeiro; fonte de verdade do split.
- **RN-071** | Toda transação gera **NF automática** com o split aplicado, respeitando as regras do plano. No MVP a NF é gerada como registro, sem liquidação nem emissão fiscal real. | Mantém a contabilidade do MVP fiel sem fingir emissão.
- **RN-072** | Assinaturas dos vets: **Básico R$ 0** (freemium — o vet entra sem custo fixo e a Vetly monetiza só pelo take rate de 15%), **Profissional R$ 249/mês** (take rate cai a 12% e libera a IA na consulta) e **Enterprise por faixa de nº de vets** (valor-base cobre os 5 primeiros e sobe por degraus — §1), com troca de faixa automática ao cruzar o limite. | A escada troca **assinatura por comissão**: quanto mais o vet paga de fixo, menos paga por transação. O Básico gratuito remove a barreira de entrada e povoa o matching — sem oferta o marketplace não existe. O Profissional a R$ 249 fica na faixa dos ERPs veterinários consolidados do mercado brasileiro (R$ 199–250/mês), com a vantagem de já incluir captação de demanda. O ponto de equilíbrio é explícito para o vendedor: a economia de 3 p.p. de comissão paga a assinatura a partir de ~R$ 8.300 de faturamento mensal na plataforma.
- **RN-073** | **Taxa de listagem no marketplace por categoria**, cobrada da empresa fornecedora por item listado ao mês: Alimentação R$ 150 · Medicamentos R$ 250 · Higiene R$ 100. Não é comissão sobre a venda. Não incide no MVP (marketplace mockado — RN-098). | Terceira linha de receita, no modelo de taxa por categoria usado por marketplaces de varejo.
- **RN-074** | Serviços clínicos oferecidos pelo próprio vet/clínica **não pagam taxa de listagem** — já são monetizados pela assinatura e pelo split. | Evita cobrança dupla sobre o mesmo parceiro.
- **RN-075** | **Nível 1 — Audience Insights:** venda apenas de dados agregados e anonimizados, com **k ≥ 100 por célula** e contrato vedando reidentificação. Não é dado pessoal; não exige consentimento individual. Não exercitado no MVP. | Salvaguarda técnica da monetização de dados.
- **RN-076** | **Nível 2 — Lead Qualificado:** compartilhamento de dado individual identificado só com **opt-in destacado** e **opt-out em um toque**; cada compartilhamento é logado. Não exercitado no MVP. | Onde está o valor alto — e só é legal com consentimento explícito.
- **RN-077** | O Responsável tem opt-out específico da finalidade "dados agregados", **sem perda de funcionalidade** no app. | LGPD: consentimento não pode ser condição de uso quando não é essencial ao serviço.

### H. IA na Consulta — Responsabilidade e Limites
- **RN-078** | **Entradas da IA:** (a) histórico acessível ao vet **naquele atendimento** (colmeia — RN-064/065; a IA nunca amplia o acesso do vet), (b) pré-sintomas (RN-005/RN-036) e (c) espécie, raça, idade e peso do animal. | A IA assiste; o vet responde; o acesso da IA é o mesmo do vet, nunca maior.
- **RN-079** | A janela de captura é delimitada por "iniciar consulta" e "encerrar consulta" (RN-008); fora dela a IA não captura áudio nem produz conteúdo clínico. | Limite explícito de gravação, auditável e sob controle do profissional.
- **RN-080** | A IA converte a fala do médico em **dados clínicos estruturados** (queixa, achados, diagnóstico, protocolo, orientações), que compõem o rascunho do atendimento. | Função central da IA nesta arquitetura: reduzir a fricção de digitação do vet.
- **RN-081** | Para qualquer sugestão de **dose**, o **peso do animal é obrigatório**; sem peso cadastrado, a IA exige o dado antes de sugerir posologia. | É onde mora o erro clínico: dose sem peso não pode ser sugerida.
- **RN-082** | **Decisão do vet — Aprovar / Corrigir / Não aprovar.** *Aprovar:* o rascunho vira estado final. *Corrigir:* o vet edita integralmente o conteúdo e o texto que ele deixa é o **estado final autoritativo** — a IA **não re-infere nada clínico**, no máximo reformata para os documentos. *Não aprovar:* o ciclo encerra sem emitir documentos. | O que o vet escreve é o que vale; a IA não sobrescreve nem reinterpreta a decisão clínica.
- **RN-083** | A geração de receita, prontuário e atestado parte **do estado final** (RN-082) e é **formatação/estruturação, não nova inferência clínica**. | Documentos refletem exatamente o que o vet deixou como final.
- **RN-084** | **Trilha de auditoria (LogDeAuditoriaIA):** cada decisão grava conteúdo sugerido, decisão do vet, **conteúdo final**, versão do modelo, CRMV e timestamp. Log **imutável**, retido junto ao prontuário. | Defesa jurídica da plataforma e do vet, e insumo de melhoria do modelo.
- **RN-085** | A IA na consulta e a geração de documentos por IA são exclusivas dos planos **Profissional e Enterprise**; no Básico o prontuário é preenchido manualmente pelo vet. | Alinha o produto à matriz de features (README §4.2).

### I. Documentos e Prontuário
- **RN-086** | Tipos de documento emitidos: **Prontuário**, **Atestado** (saúde, óbito ou transporte), **Receita Veterinária** e **Nota Fiscal**. | Escopo fechado de artefatos do atendimento.
- **RN-087** | A receita exige assinatura do vet. **No MVP a assinatura é o nome digitado** e não habilita dispensação externa de controlados; em produção, certificado ICP-Brasil vinculado ao CRMV. Etapa obrigatória por exigência legal, não suprimível. | Compliance sanitário com escopo de MVP declarado.
- **RN-088** | Correção dentro de **24 horas** gera **versão corrigida vinculada ao original**, com data, hora e CRMV do solicitante; o original **não é sobrescrito** e ambas as versões ficam no histórico. | Conformidade com as diretrizes do CFMV para alteração de prontuários.
- **RN-089** | Correção **após 24 horas** exige justificativa registrada antes de liberar a edição. | Mesma diretriz do CFMV, com trilha reforçada fora do prazo.
- **RN-090** | Documentos e resultados de exame residem no **board do pet dentro do app**; o Responsável recebe push avisando da disponibilidade, e o conteúdo nunca trafega por canal externo. | Regra-mãe da arquitetura app-only.

### J. Notificações
- **RN-091** | Todo evento é classificado como **aviso** (notificável por push) ou **ação/conteúdo** (residente no app); a matriz do README §6.2 é a fonte de verdade. | Separa o que pode ser empurrado do que exige o app aberto.
- **RN-092** | Os canais do MVP são **inbox in-app + push**. Não há canal WhatsApp em nenhuma etapa do produto. | Decisão de arquitetura do README §6.1.
- **RN-093** | Promoções exigem **opt-in específico** de marketing e têm opt-out em um toque. | LGPD + saúde do canal.
- **RN-094** | **Régua de reenvio de lembretes:** até 3 tentativas — 7 dias, 3 dias e 1 dia antes da data-limite —, interrompida na primeira resposta. | Cobertura suficiente sem fadiga de notificação.
- **RN-095** | Após a 3ª tentativa sem resposta, o vet/clínica recebe alerta no dashboard sinalizando o Responsável como **não responsivo** para aquele evento, e o sistema não reenvia. | Fecha o ciclo do lembrete e transfere a decisão ao profissional.

### K. Avatar Digital e Marketplace (mock no MVP)
- **RN-096** | O **AvatarPet** deriva do calendário de obrigações (RN-046) e reflete o estado de saúde/cuidado do animal. **No MVP é totalmente mockado e suporta apenas cachorro**: os estados são fixos no front e não reagem a dados reais. | Feature de engajamento; entra como vitrine antes de virar sistema.
- **RN-097** | Estados do avatar em produção: obrigação vencida altera a representação (vacina atrasada → estado adoentado; higienização atrasada → pelo longo), e o cumprimento devolve ao estado saudável. | Especifica o comportamento-alvo para que o mock já nasça na forma certa.
- **RN-098** | O **marketplace é mockado no MVP**: catálogo em memória no front, sem EmpresaParceira nem ItemMarketplace reais, sem cobrança de taxa de listagem e sem movimentação de valores; a ação de compra leva a "em breve". | Evita construir cadastro de parceiros e logística antes de validar o engajamento.
- **RN-099** | Mesmo mockado, o catálogo respeita a **taxonomia de categorias** (alimentação, medicamentos, higiene) que sustenta a taxa de listagem (RN-073). | Garante que o mock produza dados compatíveis com o modelo real.

### L. Operação Clínica — Internação e Exames
- **RN-100** | A internação abre uma ficha vinculada ao animal; a cada dia o profissional registra procedimentos, medicações e evolução clínica, e o sistema apura o valor total por diárias + itens. | Núcleo operacional da internação.
- **RN-101** | A internação cobra **caução na entrada** e apura o **saldo restante para pagamento na saída** — exceção ao pagamento antecipado (RN-006). No MVP ambos são registros simulados. | Exceção declarada no README §5.4.
- **RN-102** | Na alta, o sistema gera o **resumo da internação** e a **NF do saldo** após desconto da caução, e incorpora o prontuário da internação ao histórico longitudinal do animal. | Fecha o ciclo e alimenta a colmeia.
- **RN-103** | O exame é solicitado dentro da plataforma, registrado no histórico do animal, e o Responsável é notificado com as orientações de preparo. | Rastreabilidade da solicitação.
- **RN-104** | O resultado é inserido por **upload do vet** (MVP) ou recebido de laboratório parceiro (alvo); é vinculado ao histórico, alerta o solicitante, entra no contexto clínico da IA e só chega ao Responsável **após liberação explícita do vet**. | O vet controla a divulgação de resultado clínico ao Responsável.

### M. Acesso e Estrutura Organizacional
- **RN-105** | O **veterinário vinculado** opera restrito ao próprio escopo: vê e gerencia apenas a própria agenda, acessa apenas prontuários de animais que atendeu ou que estão agendados para ele, e **não** vê agenda, pacientes ou dados financeiros de outros profissionais, **não** altera regras de repasse ou configurações da clínica e **não** cadastra ou remove profissionais. | Isolamento de escopo dentro da unidade.
- **RN-106** | O **Administrador** tem visão consolidada da unidade — faturamento bruto por período/serviço/vet, comissões e repasses com status, NFs, reembolsos, retenções e KPIs (ticket médio, ocupação, cancelamento/no-show, receita por especialidade) — e é **vedado** de acessar dados bancários pessoais de vets, movimentações fora da unidade, remuneração interna dos vinculados (a plataforma mostra **produção**, nunca "salário") e dados de outros estabelecimentos. | Gestão da unidade sem vazamento de dado pessoal ou de terceiros.
- **RN-107** | O **CRMV é validado junto ao conselho regional** no onboarding; perfil com registro inválido ou suspenso **não é publicado** no matching e o profissional é notificado. | Barreira de entrada regulatória do marketplace clínico.

---

## 5. Modelo de Dados

### 5.1 Entidades com atributos de estado
- **Responsável** — credenciais, dispositivos/push tokens, geolocalização (com fallback CEP/bairro — RN-027), preferências de notificação por categoria, opt-in de promoções, consentimentos granulares por finalidade (data/hora e revogações), tier, saldo de pontos.
- **Veterinário** — endereço embutido (CEP…UF + lat/long derivadas — RN-026, modelo 1:1); nota média, nº de avaliações, status no matching, plano vigente. *(Multi-unidade no futuro extrai Endereço para entidade 1:N; adiado por simplicidade.)*
- **Consulta** — pré-sintomas (estruturado + mídia), estado do slot/consulta (RN-035/RN-038), lock de checkout, contador de remarcações, timestamps de início/encerramento (RN-008), estado final aprovado pelo vet.
- **Animal** — calendário de ObrigaçõesDoPet; **peso** (obrigatório para dose — RN-081); flags de registros ocultados (exceto alertas de segurança — RN-068).
- **Documento** — destino board do pet no app; versão original × corrigida; assinatura (MVP: nome digitado; produção: ICP-Brasil — RN-087).
- **Pagamento** — registro simulado no MVP (status, valor, comissão RN-070, incidência do desconto RN-051); tipos consulta / caução / saldo de internação. Nenhum valor liquidado.

### 5.2 Entidades novas
| Entidade | Atributos principais | Relacionamentos | Escopo |
|---|---|---|---|
| **Avaliação** | nota geral (obrigatória), comentário opcional, data, status de moderação | N:1 Consulta (única por consulta realizada — RN-056); N:1 Responsável; N:1 Veterinário | MVP |
| **ObrigaçãoDoPet** | tipo, data-limite, status | N:1 Animal; 0..1 Consulta que a cumpriu; gera Notificação e PontosFidelidade | MVP |
| **PontosFidelidade** | evento de origem, pontos brutos, multiplicador aplicado, pontos creditados, expiração (12m FIFO), estorno | N:1 Responsável; N:1 evento (Consulta/Obrigação); N:1 CupomResgate | MVP (acúmulo/tier reais; desconto calculado — RN-051) |
| **CupomResgate** | código QR, item, pontos debitados, desconto em R$, divisão da incidência, validade, status | N:1 Responsável; N:1 ItemMarketplace; 1:N PontosFidelidade | MVP parcial (emissão real, validação mockada — RN-019) |
| **Notificação** | evento, classificação (aviso × ação/conteúdo), canal, tentativa da régua, status de leitura | N:1 Responsável (ou Vet); N:1 evento | MVP (in-app + push) |
| **LogDeAuditoriaIA** | timestamp, versão do modelo, conteúdo sugerido, decisão, conteúdo final, CRMV | N:1 Consulta; N:1 Veterinário; imutável | MVP (IA real, Profissional+) |
| **LogDeAcessoProntuário** | quem, quando, contexto, base de acesso | N:1 Animal; N:1 Veterinário; visível ao Responsável | MVP |
| **AvatarPet** | estado visual, espécie suportada | 1:1 Animal; deriva de ObrigaçãoDoPet (produção) | Mock (só cachorro) |
| **EmpresaParceira** | razão social, categorias, taxa de listagem contratada, status | 1:N ItemMarketplace; 1:N CupomResgate | Alvo (não existe no MVP) |
| **ItemMarketplace** | nome, categoria, preço de referência, custo em pontos | N:1 EmpresaParceira; 1:N CupomResgate | Alvo (catálogo mockado no front) |

---

## 6. Escopo do MVP — Mock vs. Real

Nenhuma regra é abandonada; os itens abaixo são **alvo de produção**, com a RN preservada.

**Totalmente mockado (sem lógica de backend)**
- **Avatar digital** — RN-096/097: estados fixos no front, apenas cachorro.
- **Catálogo do marketplace** — RN-098/099: itens em memória, compra leva a "em breve".
- **Validação do cupom QR no estabelecimento** — RN-019: cupom é emitido de verdade, mas não há validação no ponto físico nem movimentação de valor.
- **Taxa de listagem** — RN-073: regra de preço definida, nenhuma cobrança executada.

**Real, com dinheiro simulado (fluxo e dados verdadeiros, sem liquidação)**
- **Pagamento e confirmação de agendamento** — RN-006/RN-035.
- **Split e comissão por plano** — RN-070: calculado e gravado, não repassado.
- **NF** — RN-071: gerada como registro, sem emissão fiscal.
- **Reembolso e remarcação** — RN-014/RN-041: consequência calculada e exibida.
- **Desconto de fidelidade e sua incidência** — RN-051: calculado, gravado e exibido, sem abatimento.
- **Assinatura da receita** — RN-087: nome digitado; ICP-Brasil é alvo.

**Real e completo**
- Validação de CRMV (RN-107), matching e geolocalização (RN-026 a RN-033), agendamento e máquina de estados (RN-034 a RN-038), briefing e pré-sintomas (RN-005/RN-036), IA de captura e geração de documentos (RN-078 a RN-085), correção de documentos (RN-088/089), internação e exames (RN-100 a RN-104), avaliação (RN-055 a RN-059), fidelidade — acúmulo, tier e expiração (RN-046 a RN-052), notificações in-app + push (RN-091 a RN-095), consentimento, colmeia e logs de acesso (RN-060 a RN-069).

**Fora de escopo desta fase**
- **Atendimento remoto por videochamada** — RN-039.
- **Integrações externas e API para terceiros** — README §4.3; inclui o recebimento automático de resultado por laboratório parceiro (RN-104).
- **Monetização de dados, Níveis 1 e 2** — RN-075/RN-076: salvaguardas definidas, operação não exercitada.
- **Canal WhatsApp** — descontinuado do produto; não é alvo (RN-092).

---

## 7. Origem dos Parâmetros — Referências de Mercado

Todos os parâmetros da §1 estão definidos. Esta seção registra **de onde veio cada número**, para que a calibração futura seja discutida contra a referência e não contra intuição.

### 7.1 Fidelidade

| Decisão | Valor | Referência de mercado |
|---|---|---|
| Ganho por gasto (RN-047) | 1 ponto por R$ 1 | Padrão praticamente universal do varejo pet brasileiro: Petz Clubz e Petios creditam 1 ponto por real gasto. É a mecânica que o Responsável já conhece de outros programas. |
| Ganho por obrigação (RN-047) | 50 pontos fixos | Magnitude equivalente ao bônus de entrada usado por programas pet para ativar o cadastro. Aqui cumpre função diferente e mais estratégica: paga **comportamento de cuidado**, não gasto. |
| Conversão (RN-049) | 100 pontos = R$ 3 | Programas de saúde pet consolidados devolvem na ordem de 3% do gasto (Petco Pals converte pontos em crédito nessa faixa). É o teto que cabe dentro de um take rate de 10–15%. |
| Tiers (RN-048) | 0–999 / 1.000–2.999 / 3.000+ | Estrutura de três faixas com multiplicador crescente no topo, padrão de programas pet com componente de serviço. Faixas calibradas para alcançabilidade (ver 7.4). |
| Expiração (RN-050) | 12 meses, FIFO | Expiração anual é o default de programas de pontos; alguns operam sem expirar, mas então o passivo cresce indefinidamente. |
| Validade do cupom (RN-053) | 30 dias | Validade padrão de cupom no iFood; cria urgência de resgate sem parecer arbitrária. |
| Financiamento do desconto (RN-051) | faixas 100/0 · 60/40 · 30/70 | Modelo de subsídio compartilhado de promoção usado por marketplaces de delivery, em que plataforma e parceiro dividem o custo do desconto conforme a campanha. |

### 7.2 Planos e Comissão

| Decisão | Valor | Referência de mercado |
|---|---|---|
| Básico (RN-072) | R$ 0 | Modelo freemium de marketplace: listar é grátis, a monetização vem da transação. Sem oferta no matching o produto não existe — a barreira de entrada precisa ser zero. |
| Profissional (RN-072) | R$ 249/mês | Faixa dos ERPs veterinários consolidados no Brasil (Vetus opera na casa de R$ 200–250/mês; entrada de outras plataformas fica entre R$ 157 e R$ 359). A Vetly entrega isso **mais** captação de demanda. |
| Enterprise (RN-072) | R$ 599 a R$ 1.699+ | Faixa dos planos de clínica/hospital do mercado (que chegam a R$ 979/mês em gestão pura), justificada pela gestão multiprofissional e pelo menor take rate. |
| Take rate (RN-070) | 15 / 12 / 10% | Escada inversa à assinatura. Referência de intermediação de serviços com captação de demanda inclusa. |

**Ponto de equilíbrio do Profissional:** a diferença de 3 p.p. de comissão (15% → 12%) paga a assinatura de R$ 249 a partir de **R$ 8.300/mês** de faturamento na plataforma. Abaixo disso o Básico é objetivamente melhor para o vet — e isso deve ser dito na venda, não escondido.

### 7.3 Operação

| Decisão | Valor | Referência |
|---|---|---|
| Lock de checkout (RN-035) | 10 minutos | Janela padrão de reserva temporária em plataformas de venda com estoque limitado; longa o bastante para concluir o pagamento, curta o bastante para não travar a agenda. |
| Prioridade na lista de espera (RN-037) | 15 minutos | Tempo hábil para reagir a um push sem imobilizar o slot. |
| Janela de avaliação (RN-055) | 14 dias | Mesma janela do Airbnb, referência de marketplace bilateral com reputação. |
| Mínimo para nota pública (RN-057) | 3 avaliações | Piso adotado por marketplaces de reputação antes de publicar média. |
| Raio de busca (RN-028) | 10 km, até 25 km | Densidade típica de capital brasileira. |
| Limite de remarcações (RN-043) | 2 por consulta | Cobre imprevisto legítimo sem virar burla à janela de reembolso. |

### 7.4 Verificação de coerência do programa de fidelidade

Simulação com os parâmetros fechados, para o time validar o cálculo na implementação:

| Perfil | Gasto/ano | Obrigações/ano | Pontos | Tier | Valor em desconto | % do gasto |
|---|---|---|---|---|---|---|
| 1 pet, uso ocasional | R$ 300 | 4 | 300 + 200 = **500** | Bronze | R$ 15 | 5,0% |
| 1 pet, calendário em dia | R$ 800 | 9 | 800 + 450 = **1.250** | Prata | R$ 37,50 | 4,7% |
| Multi-pet / uso intenso | R$ 2.000 | 20 | 2.000 + 1.000 = **3.000** | Ouro | R$ 90 | 4,5% |

O retorno fica em torno de 4,5–5% do gasto, acima dos ~3% de referência porque o bônus por obrigação é somado deliberadamente — é o incentivo que sustenta a tese do produto (cuidado preventivo e recorrência). Como a maior parte dos resgates cai na faixa de até R$ 10, **a Vetly absorve sozinha a maior parte desse custo** (RN-051), tratando-o como investimento em retenção e não como desconto operacional do vet. Se o custo se mostrar alto na operação real, a alavanca a ajustar primeiro é o **bônus por obrigação** (RN-047), não a conversão — mexer na conversão quebra a legibilidade do programa para o Responsável.

### 7.5 Contrato das integrações de produção

O documento não pode especificar a API de terceiros, mas define o **contrato que a Vetly expõe internamente**, para que o MVP já nasça com a fronteira certa e a troca de fornecedor não vire refatoração.

**Gateway de pagamento — RN-006/RN-070/RN-071.** Nenhum gateway está contratado ou integrado nesta fase; a feature de pagamento real foi descartada do MVP. O núcleo do produto nunca chama gateway algum diretamente: conversa com um adaptador de pagamento com quatro operações — `criarCobranca`, `consultarStatus`, `estornar`, `receberWebhookDeStatus`. No MVP, **as quatro operações são simuladas** (retorno de sucesso determinístico, sem chamada externa). Requisitos que a interface já respeita, mesmo mockada, para que a troca por um fornecedor real no futuro seja substituição de implementação e não refatoração de fluxo:
- **Idempotência** por chave da transação: reenvio da mesma cobrança não gera duplicidade.
- **Estado autoritativo no webhook**, nunca na resposta síncrona: a confirmação da consulta (RN-006) reage ao evento de status simulado, não ao retorno da chamada.
- **Split e desconto calculados pela Vetly** (RN-051/RN-070), nunca pelo gateway.
- A escolha do fornecedor real (Pix/cartão) é decisão de produto em aberto, fora do escopo desta fase.

**Validação de CRMV (RN-107).** Adaptador com uma operação — `validarRegistro(crmv, uf)` — retornando `válido | inválido | suspenso | indisponível`. O estado `indisponível` é obrigatório no contrato: quando o conselho não responde, o perfil fica **pendente de validação** e não é publicado no matching, em vez de ser aprovado por omissão.

---

*Documento técnico do Vetly. Companheiro de [vetly-produto.md](./vetly-produto.md) (produto e fluxos). Toda RN é rastreável; onde é alvo de produção e não MVP, está sinalizado sem apagar a regra.*
