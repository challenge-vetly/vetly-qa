README-TECH — Vetly
Documento técnico derivado exclusivamente do README.md. Não contém informações adicionais, sugestões de arquitetura ou decisões de implementação.

---

SEÇÃO 1 — MAPA DE ENTIDADES E RELACIONAMENTOS

---

ENTIDADE: Veterinário
Atributos: CRMV, UF de atuação, especialidades, espécies atendidas, titulação acadêmica, dias e horários de atendimento, duração média por consulta, intervalo entre atendimentos, serviços oferecidos (tipo, valor, aceita plano de saúde pet), dados bancários (banco, agência, conta, CPF ou CNPJ do titular), chave Pix, plano de assinatura (Básico / Profissional / Enterprise), persona (autônomo ou vinculado)
Relacionamentos:
- Realiza Consulta (1:N) — conduz os atendimentos registrados na plataforma
- Atende Animal (N:N via Consulta) — cada veterinário atende múltiplos animais; cada animal pode ser atendido por múltiplos veterinários
- Solicita Exame (1:N) — registra solicitações de exame durante ou após a consulta
- Abre Internação (1:N) — vincula ficha de internação ao animal
- Assina Documento (1:N) — assina digitalmente a receita veterinária com certificado vinculado ao CRMV
- Pertence a Empresa (N:1) — quando na modalidade veterinário vinculado
- Recebe repasse via split (N:1) — processado automaticamente conforme regras do onboarding

---

ENTIDADE: Empresa
Atributos: tipo (clínica, hospital veterinário, pet shop com serviços clínicos ou estabelecimento com múltiplos profissionais), planos de saúde aceitos, serviços oferecidos por profissional
Relacionamentos:
- Tem Administrador (1:1) — usuário responsável pela gestão da unidade
- Emprega Veterinário vinculado (1:N) — cada empresa pode ter múltiplos veterinários
- Processa split com Plataforma (1:1) — percentual e prazo de liquidação configurados no onboarding

---

ENTIDADE: Administrador
Atributos: acesso irrestrito a todas as informações da unidade
Relacionamentos:
- Gerencia Empresa (1:1) — representa a unidade na plataforma
- Gerencia Veterinário vinculado (1:N) — cadastra, edita e desativa profissionais
- Recebe alerta de agendamentos futuros de veterinário desativado (1:N) — para redistribuição ou cancelamento

---

ENTIDADE: Animal
Atributos: espécie, raça, idade, histórico clínico completo, medicações em uso, alertas clínicos ativos
Relacionamentos:
- Pertence a Tutor (N:1) — cadastrado pelo tutor no primeiro acesso
- Tem Prontuário (1:N) — histórico longitudinal vinculado ao animal, não ao veterinário
- Tem Consulta (1:N) — cada atendimento registrado na plataforma
- Tem Internação (1:N) — fichas de internação vinculadas ao animal
- Tem Exame (1:N) — solicitações e resultados vinculados ao histórico do animal

---

ENTIDADE: Tutor
Atributos: dados pessoais confirmados no primeiro acesso, consentimento LGPD granular por finalidade (data e hora de registro e de eventual revogação)
Relacionamentos:
- Possui Animal (1:N) — um tutor pode ter múltiplos pets cadastrados
- Agenda Consulta (1:N) — via WhatsApp com fluxo conduzido pela IA
- Realiza Pagamento (1:N) — via Pix ou cartão, processado pelo Abacate Pay
- Recebe Documento (1:N) — receita, atestado, resumo de consulta e resultados de exame via WhatsApp

---

ENTIDADE: Consulta
Atributos: data, horário, modalidade (presencial ou remota), nome do veterinário, diagnóstico validado, protocolo validado, status de pagamento
Relacionamentos:
- Realizada por Veterinário (N:1) — cada consulta tem um veterinário responsável
- Sobre Animal (N:1) — cada consulta está vinculada a um animal
- Agendada por Tutor (N:1) — solicitada pelo tutor via WhatsApp
- Gera Documento (1:N) — prontuário, atestado quando aplicável, receita veterinária, nota fiscal
- Tem Pagamento (1:1) — pago no ato do agendamento, salvo exceções previstas

---

ENTIDADE: Prontuário
Atributos: dados clínicos completos da consulta, histórico longitudinal do animal, versão original, versão corrigida (quando aplicável) com data, hora e CRMV do profissional que solicitou a correção
Relacionamentos:
- Pertence a Animal (N:1) — o prontuário é do animal, não do veterinário ou da clínica
- Gerado por Consulta (N:1) — cada consulta gera um prontuário
- Gerado por Internação (N:1) — o prontuário da internação é incorporado ao histórico do animal
- Tem versão corrigida vinculada (1:0..1) — versão original e corrigida coexistem no histórico

---

ENTIDADE: Exame
Atributos: tipo de solicitação, resultado (inserido por upload do veterinário ou via laboratório parceiro), status de liberação ao tutor
Relacionamentos:
- Solicitado por Veterinário (N:1) — durante ou após a consulta
- Vinculado a Animal (N:1) — resultado incorporado ao histórico longitudinal
- Inserido por Laboratório parceiro (0:1) — quando há integração estabelecida

---

ENTIDADE: Internação
Atributos: procedimentos diários registrados, medicações administradas, evolução clínica diária, valor total apurado automaticamente (diárias + procedimentos), valor de caução de entrada, saldo restante pago na saída
Relacionamentos:
- Vinculada a Animal (N:1) — ficha aberta e associada ao animal
- Acompanhada por Veterinário (N:1) — profissional responsável pelo registro diário
- Gera Documento (1:N) — resumo completo da internação e nota fiscal com saldo restante
- Tem Pagamento (1:N) — caução na entrada e saldo pago na saída

---

ENTIDADE: Documento
Atributos: tipo (Prontuário, Atestado de saúde / óbito / transporte, Receita Veterinária, Nota Fiscal), versão (original ou corrigida), data e hora de geração, CRMV do profissional signatário (receita), assinatura digital vinculada ao CRMV (apenas receita)
Relacionamentos:
- Gerado em Consulta (N:1) ou Internação (N:1) — cada atendimento gera documentos associados
- Assinado por Veterinário (N:1) — obrigatório apenas para receita veterinária
- Recebido por Tutor via WhatsApp (N:N) — receita, atestado quando aplicável, resumo e resultados de exame
- Armazenado no Vetly (N:1) — consolidação do histórico clínico e financeiro

---

ENTIDADE: Pagamento
Atributos: meio (Pix, cartão de crédito, cartão de débito), momento (antecipado no agendamento / no ato do atendimento / caução + saldo na saída), processador (Abacate Pay), percentual e prazo de liquidação do split
Relacionamentos:
- Realizado por Tutor (N:1) — tutor é o pagador em todos os cenários
- Associado a Consulta, Internação ou serviço avulso (N:1) — cada pagamento vinculado a um evento
- Processado via Abacate Pay (N:1) — gateway único de processamento
- Split aplicado entre Veterinário autônomo e Plataforma, ou entre Empresa e Plataforma (N:1) — conforme regras configuradas no onboarding

---

SEÇÃO 2 — FLUXOS

---

FLUXO 1 — Onboarding do Veterinário Autônomo
Origem: Seções 1.2, 1.5, 1.6, 1.7

1. Veterinário preenche dados profissionais (CRMV, UF, especialidades, espécies atendidas, titulação) → dados registrados na plataforma
2. Sistema valida CRMV junto ao conselho regional → perfil aprovado ou rejeitado; profissional notificado em caso de CRMV inválido ou suspenso
3. Veterinário configura agenda (dias, horários de atendimento, duração média por consulta, intervalo entre atendimentos) → agenda disponível na plataforma
4. Veterinário cadastra serviços (tipo, valor, aceita plano de saúde pet) → catálogo de serviços criado
5. Veterinário cadastra dados bancários (banco, agência, conta, CPF ou CNPJ do titular) e chave Pix → dados de recebimento registrados
6. Veterinário configura split (percentual e prazo de liquidação entre profissional e plataforma) → regras de repasse definidas
7. Veterinário assina plano de assinatura → plano ativado
8. Sistema publica perfil na plataforma → veterinário visível para tutores na região e apto a receber agendamentos

---

FLUXO 2 — Onboarding da Empresa
Origem: Seções 1.1, 1.3, 1.5, 1.6, 1.7

1. Administrador realiza cadastro da empresa → empresa registrada na plataforma
2. Administrador cadastra veterinários vinculados à unidade → profissionais associados à empresa
3. Sistema valida CRMV de cada veterinário junto ao conselho regional → perfil aprovado ou rejeitado; profissional notificado em caso de CRMV inválido ou suspenso
4. Administrador configura agenda de cada veterinário → agendas disponíveis na plataforma
5. Administrador configura serviços oferecidos e planos de saúde aceitos por profissional → catálogo configurado por unidade
6. Administrador cadastra dados bancários da empresa e chave Pix → dados de recebimento registrados
7. Administrador configura split (percentual e prazo de liquidação entre estabelecimento e plataforma) → regras de repasse definidas
8. Administrador assina plano de assinatura → plano ativado
9. Sistema publica perfis dos veterinários vinculados na plataforma → profissionais visíveis para tutores na região

---

FLUXO 3 — Operação Diária
Origem: Seções 2.1, 2.2, 2.3, 2.4, 2.8, 2.10

1. Veterinário acessa a plataforma → dashboard do dia exibido com agenda, pacientes aguardando atendimento, alertas de animais de alto risco e retornos pendentes
2. Veterinário seleciona atendimento → painel do animal aberto com briefing pré-consulta (histórico clínico, medicações em uso, data e resumo da última consulta, alertas clínicos ativos, exames anteriores e sintomas relatados via WhatsApp)
3. Veterinário inicia consulta (presencial ou remota) → sala de atendimento aberta com acesso simultâneo ao painel do animal
4. IA apresenta hipóteses diagnósticas ordenadas por probabilidade → veterinário valida ou descarta cada hipótese
5. IA sugere protocolo (medicamentos, doses por espécie e peso, posologia, alertas de interação medicamentosa) → veterinário valida o protocolo
6. Sistema gera documentos automaticamente após validação (prontuário, atestado quando aplicável, receita veterinária, nota fiscal) → documentos gerados e preparados para distribuição
7. Veterinário assina digitalmente a receita veterinária com certificado vinculado ao CRMV → etapa obrigatória concluída
8. Sistema distribui documentos: tutor recebe via WhatsApp; Vetly consolida histórico clínico e financeiro → distribuição concluída
9. Sistema processa split financeiro automaticamente conforme regras do onboarding → repasse efetuado via Abacate Pay

---

FLUXO 4 — Agendamento via WhatsApp
Origem: Seções 3.2, 3.3

1. Tutor envia mensagem para o número da plataforma → IA inicia fluxo conversacional de agendamento
2. [Primeiro acesso] IA conduz fluxo de boas-vindas: tutor confirma dados, cadastra pet e registra consentimento LGPD → cadastro concluído antes de qualquer outra interação
3. IA coleta informações (identificação do pet, tipo de serviço, preferência de data e horário, nome do veterinário quando houver) → dados de agendamento registrados
4. IA apresenta horários disponíveis → tutor escolhe horário
5. Sistema gera link de pagamento via Abacate Pay e encaminha pelo WhatsApp → tutor realiza pagamento
6. Sistema confirma pagamento → consulta confirmada; tutor recebe mensagem com data, horário, nome do veterinário e modalidade do atendimento
7. [Sem horários disponíveis] IA apresenta três opções: lista de espera para o veterinário solicitado, horários com outros veterinários da mesma clínica ou região, ou data futura disponível → tutor escolhe opção
8. [Tutor entra na lista de espera e horário abre] Sistema notifica tutor via WhatsApp → tutor confirma o agendamento em um clique

---

FLUXO 5 — Cancelamento e Remarcação
Origem: Seção 3.4

1. Tutor solicita cancelamento ou remarcação pelo WhatsApp → IA identifica o agendamento ativo e apresenta as opções disponíveis
2. [Remarcação] Tutor seleciona novo horário → pagamento anterior transferido para a nova data; consulta confirmada
3. [Cancelamento com mais de 24h de antecedência] Sistema aplica política → reembolso integral processado; tutor notificado via WhatsApp
4. [Cancelamento entre 24h e 2h de antecedência] Sistema aplica política → reembolso parcial processado com percentual de retenção configurado pela clínica no onboarding; tutor notificado via WhatsApp
5. [Cancelamento com menos de 2h ou no ato do atendimento] Sistema aplica política → sem reembolso; tutor notificado via WhatsApp

---

FLUXO 6 — Triagem de Sintomas
Origem: Seções 2.2, 3.5

1. Tutor relata sintomas do pet pelo WhatsApp → IA inicia triagem conversacional
2. IA coleta informações (sintomas, duração, comportamento do animal e outros dados relevantes) → dados de triagem registrados
3. IA avalia gravidade e responde com orientações básicas adequadas à situação → tutor recebe orientação
4. [Gravidade indica necessidade de atendimento] IA recomenda agendamento de consulta e oferece iniciar o fluxo diretamente na conversa → tutor pode agendar sem sair do WhatsApp
5. [Tutor já tem consulta agendada] Sistema encaminha informações da triagem ao veterinário → dados disponíveis no briefing pré-consulta

---

FLUXO 7 — Geração de Documentos
Origem: Seções 2.8, 2.9, 2.10

1. Veterinário valida diagnóstico e protocolo → sistema inicia geração automática de documentos
2. Sistema gera prontuário completo e estruturado → histórico longitudinal do animal atualizado
3. Sistema gera atestado conforme necessidade do atendimento (saúde, óbito ou transporte) → documento disponível para distribuição
4. Sistema gera receita veterinária preenchida pela IA → veterinário assina digitalmente com certificado vinculado ao CRMV; etapa obrigatória por exigência legal
5. Sistema gera nota fiscal → split financeiro processado conforme regras configuradas
6. [Correção dentro de 24h] Veterinário solicita correção → sistema gera versão corrigida vinculada ao original com registro automático de data, hora e CRMV; ambas as versões armazenadas no histórico do animal
7. [Correção após 24h] Veterinário registra justificativa → sistema libera edição; versão corrigida vinculada ao original com o mesmo registro
8. Sistema distribui documentos: tutor recebe receita, atestado quando aplicável, resumo da consulta e orientações pós-atendimento via WhatsApp; Vetly recebe todos os dados para consolidação do histórico

---

FLUXO 8 — Fluxo de Internação
Origem: Seções 2.5, 2.6

1. Veterinário abre ficha de internação vinculada ao animal → internação registrada na plataforma
2. Sistema cobra valor de caução no ato da entrada → pagamento processado via Abacate Pay
3. Veterinário responsável registra diariamente procedimentos realizados, medicações administradas e evolução clínica → ficha atualizada; valor total apurado automaticamente com base nos itens e no número de diárias
4. Sistema envia atualização diária ao tutor via WhatsApp → tutor informado sobre o estado do animal
5. Veterinário dá alta ao animal → sistema gera resumo completo da internação e nota fiscal com saldo restante após desconto da caução
6. Sistema encaminha link de pagamento do saldo ao tutor via Abacate Pay → tutor realiza pagamento
7. Sistema incorpora prontuário da internação ao histórico longitudinal do animal → histórico atualizado

---

FLUXO 9 — Fluxo de Exames
Origem: Seção 2.7

1. Veterinário solicita exame dentro da plataforma durante ou após a consulta → solicitação registrada no histórico do animal
2. Sistema notifica tutor via WhatsApp com orientações para realização do exame → tutor recebe instruções
3. [Resultado via veterinário] Veterinário realiza upload do resultado → resultado vinculado automaticamente ao histórico do animal
4. [Resultado via laboratório parceiro] Laboratório insere resultado pela integração → resultado vinculado automaticamente ao histórico do animal
5. Sistema gera alerta para o veterinário solicitante → veterinário notificado da disponibilidade do resultado
6. IA processa resultado e incorpora ao contexto clínico do animal → dado disponível para consultas futuras
7. Veterinário libera acesso ao resultado → tutor recebe o resultado via WhatsApp

---

FLUXO 10 — Pagamento
Origem: Seções 1.5, 2.5

1. [Consulta, vacinação, cirurgia ou exame] Tutor realiza pagamento no momento do agendamento via Pix ou cartão de crédito ou débito → sistema confirma pagamento; consulta confirmada
2. [Internação — entrada] Sistema cobra caução no ato da entrada → pagamento processado via Abacate Pay; saldo apurado ao longo da internação com base em diárias e procedimentos registrados
3. [Internação — alta] Sistema apura saldo restante → nota fiscal gerada; link de pagamento enviado ao tutor via WhatsApp; tutor realiza pagamento
4. [Emergência presencial sem agendamento prévio] Tutor realiza pagamento no ato do atendimento → pagamento processado via Abacate Pay
5. Sistema processa split financeiro automaticamente conforme regras configuradas no onboarding → para veterinário autônomo: repasse entre profissional e plataforma; para empresa: repasse entre estabelecimento e plataforma

---

SEÇÃO 3 — REGRAS DE NEGÓCIO

---

ACESSO E PERMISSÕES

RN-001 | O veterinário vinculado visualiza e gerencia apenas sua própria agenda. | Origem: 1.3
RN-002 | O veterinário vinculado acessa apenas os prontuários dos animais que atendeu ou que estão agendados para ele. | Origem: 1.3
RN-003 | O veterinário vinculado não visualiza a agenda nem os pacientes de outros profissionais da unidade. | Origem: 1.3
RN-004 | O veterinário vinculado não acessa dados financeiros de outros veterinários. | Origem: 1.3
RN-005 | O veterinário vinculado não altera regras de repasse nem configurações da clínica. | Origem: 1.3
RN-006 | O veterinário vinculado não cadastra ou remove outros profissionais. | Origem: 1.3
RN-007 | O administrador tem acesso irrestrito a todas as informações da clínica ou estabelecimento. | Origem: 1.3
RN-008 | Ao ser desativado pelo administrador, o veterinário perde acesso à plataforma imediatamente. | Origem: 1.4
RN-009 | O veterinário desativado pode solicitar apenas um extrato dos atendimentos que realizou, sem acesso a dados pessoais do tutor ou do animal. | Origem: 1.4
RN-010 | Apenas veterinários que já atenderam ou estão agendados para atender um animal têm acesso ao prontuário, salvo consentimento explícito do tutor para compartilhamento amplo na rede. | Origem: 4.2
RN-011 | Perfis com CRMV inválido ou suspenso não são publicados na plataforma e o profissional é notificado. | Origem: 1.2

---

FINANCEIRO

RN-012 | Para o veterinário autônomo, o split é configurado entre o profissional e a plataforma, com percentual e prazo de liquidação definidos no onboarding. | Origem: 1.5
RN-013 | Para a persona empresa, o split é configurado entre o estabelecimento e a plataforma; a remuneração dos veterinários vinculados é uma relação interna da empresa e está fora do escopo da plataforma. | Origem: 1.5
RN-014 | O processamento financeiro é integrado ao gateway Abacate Pay. | Origem: 1.5
RN-015 | Consultas, vacinações, cirurgias e exames são pagos no momento do agendamento via Pix ou cartão de crédito e débito. | Origem: 2.5
RN-016 | Em internações é cobrado um valor de caução no ato da entrada; o saldo restante é apurado ao longo da internação e pago na saída. | Origem: 2.5
RN-017 | Em atendimentos de emergência presencial sem agendamento prévio, o pagamento é realizado no ato do atendimento. | Origem: 2.5
RN-018 | O split financeiro é processado automaticamente pelo sistema conforme as regras configuradas no onboarding. | Origem: 2.5
RN-019 | Cancelamentos com mais de 24 horas de antecedência geram reembolso integral. | Origem: 3.4
RN-020 | Cancelamentos entre 24 horas e 2 horas antes da consulta geram reembolso parcial, com percentual de retenção configurável pela clínica no onboarding. | Origem: 3.4
RN-021 | Cancelamentos com menos de 2 horas de antecedência ou no ato do atendimento não geram reembolso. | Origem: 3.4
RN-022 | Em caso de remarcação, o pagamento já realizado é transferido para a nova data. | Origem: 3.4
RN-023 | O tutor é informado da política de reembolso no momento do agendamento. | Origem: 3.4

---

CLÍNICO

RN-024 | A IA não toma nenhuma decisão de forma autônoma; toda sugestão exige validação explícita do profissional. | Origem: 2.4
RN-025 | Procedimentos que exigem presença física — cirurgias, vacinações e exames clínicos — são sempre presenciais. | Origem: 2.3
RN-026 | O prontuário pertence ao animal e estará disponível em qualquer clínica parceira onde o tutor escolher ser atendido. | Origem: 1.4
RN-027 | O histórico de atendimentos de um veterinário desativado permanece vinculado ao animal e à plataforma. | Origem: 1.4
RN-028 | Os agendamentos futuros de um veterinário desativado são sinalizados automaticamente ao administrador para redistribuição ou cancelamento com notificação ao tutor via WhatsApp. | Origem: 1.4
RN-029 | A régua de reenvio de lembretes opera com no máximo três tentativas (7 dias, 3 dias e 1 dia de antecedência); após três tentativas sem resposta, nenhum novo envio é realizado para o mesmo evento. | Origem: 3.8
RN-030 | Após três tentativas sem resposta, o veterinário ou clínica responsável recebe um alerta no dashboard sinalizando o tutor como não responsivo para aquele evento. | Origem: 3.8

---

DOCUMENTOS

RN-031 | A assinatura digital da receita veterinária pelo profissional é obrigatória e não pode ser suprimida por exigência legal. | Origem: 2.8
RN-032 | O sistema não sobrescreve o documento original; gera uma versão corrigida vinculada ao registro original com anotação automática de data, hora e CRMV do profissional que solicitou a correção. | Origem: 2.9
RN-033 | Correções de documentos podem ser solicitadas pelo veterinário dentro de um prazo de 24 horas após a consulta sem necessidade de justificativa. | Origem: 2.9
RN-034 | Correções fora do prazo de 24 horas exigem justificativa registrada antes da liberação da edição. | Origem: 2.9
RN-035 | Ambas as versões — original e corrigida — ficam armazenadas no histórico do animal. | Origem: 2.9
RN-036 | O resultado de exame é enviado ao tutor via WhatsApp somente após o veterinário liberar o acesso. | Origem: 2.7

---

WHATSAPP

RN-037 | A consulta é confirmada somente após a confirmação do pagamento. | Origem: 3.3
RN-038 | Quando não há horários disponíveis, a IA apresenta ao tutor três opções: lista de espera, outros veterinários da mesma clínica ou região, ou data futura disponível. | Origem: 3.3
RN-039 | Sintomas relatados pelo tutor via WhatsApp antes da consulta são encaminhados ao veterinário e ficam disponíveis no briefing pré-consulta. | Origem: 2.2 / 3.5
RN-040 | A IA nunca substitui o veterinário em orientações clínicas complexas e sempre encaminha o tutor para atendimento profissional quando necessário. | Origem: 3.9
RN-041 | O primeiro acesso do tutor exige a conclusão do fluxo de boas-vindas — confirmação de dados, cadastro do pet e consentimento LGPD — antes de qualquer outra interação. | Origem: 3.2

---

LGPD

RN-042 | O consentimento é coletado no primeiro contato do tutor com a plataforma, antes de qualquer outra interação. | Origem: 3.2 / 4.1
RN-043 | O consentimento é granular: o tutor autoriza separadamente cada finalidade (atendimento clínico, lembretes e comunicação proativa, compartilhamento com clínicas parceiras da rede). | Origem: 4.1
RN-044 | O tutor pode revogar qualquer consentimento a qualquer momento pelo WhatsApp; a revogação é registrada com data e hora no sistema. | Origem: 4.1
RN-045 | Dados de saúde do animal são classificados como sensíveis e possuem camada adicional de proteção de acesso. | Origem: 4.2
RN-046 | Dados anonimizados e agregados podem ser utilizados para geração de insights internos, respeitando os limites legais da LGPD. | Origem: 4.2
