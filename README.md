Funcionalidades do Vetly

Fase 1 - ONBOARDING 

1.1 Personas e Estrutura de Acesso

A plataforma atende duas personas distintas no cadastro inicial.

A primeira é o veterinário autônomo, que opera como pessoa jurídica, atende múltiplos tutores e gerencia sua própria agenda de forma independente.

A segunda é a empresa, que pode ser uma clínica, hospital veterinário, pet shop com serviços clínicos ou qualquer estabelecimento com múltiplos profissionais.Nessa persona existe um usuário administrador responsável pela gestão dos veterinários vinculados. Cada veterinário enxerga apenas sua própria agenda e seus próprios pacientes. O administrador tem visibilidade total sobre todos os profissionais, agendas e atendimentos da unidade.

1.2 Cadastro do Perfil Profissional

O veterinário preenche seus dados profissionais: CRMV, UF de atuação, especialidades, espécies atendidas e titulação acadêmica. O sistema realiza a validação automática do CRMV junto ao conselho regional antes de publicar o perfil. Perfis com CRMV inválido ou suspenso não são publicados e o profissional é notificado.

Em seguida configura sua agenda: dias e horários de atendimento, duração média por consulta e intervalo entre atendimentos.

Por fim, cadastra os serviços que oferece: consulta, cirurgia, exame, vacinação, entre outros. Para cada serviço informa o valor e se aceita plano de saúde pet.

1.3 Estrutura Organizacional — Persona Empresa

Administrador

O administrador é o responsável pela gestão da unidade na plataforma e tem acesso irrestrito a todas as informações da clínica ou estabelecimento. Suas responsabilidades incluem cadastrar, editar e desativar veterinários vinculados à unidade. 

Ele visualiza o faturamento consolidado da unidade na plataforma para fins de gestão operacional. Visualiza a agenda de todos os veterinários e pode redistribuir atendimentos quando necessário. Acompanha os KPIs operacionais e financeiros da unidade em tempo real. Gerencia os planos de saúde aceitos e os serviços oferecidos por cada profissional. (Corrigir a parte do financeiro)

Veterinário Vinculado

O veterinário vinculado opera de forma independente dentro da estrutura da empresa, com visibilidade restrita ao seu próprio escopo. Ele visualiza e gerencia apenas sua própria agenda. Acessa apenas os prontuários dos animais que atendeu ou que estão agendados para ele. Realiza o fluxo completo de atendimento dentro das configurações definidas pelo administrador. Não visualiza a agenda nem os pacientes de outros profissionais da unidade. Não acessa dados financeiros de outros veterinários. Não altera regras de repasse nem configurações da clínica. Não cadastra ou remove outros profissionais.

1.4 Offboarding de Veterinário Vinculado

Quando o administrador desativa um veterinário da unidade, o acesso à plataforma é encerrado imediatamente. O histórico de todos os atendimentos realizados por aquele profissional permanece vinculado ao animal e à plataforma, coerente com a filosofia de mente colmeia do Vetly: o prontuário pertence ao animal e estará disponível em qualquer clínica parceira onde o tutor escolher ser atendido.

O veterinário desativado perde acesso aos prontuários, podendo solicitar apenas um extrato dos atendimentos que realizou para fins de comprovação profissional, sem acesso a dados pessoais do tutor ou do animal, respeitando a LGPD.

Os agendamentos futuros daquele veterinário são sinalizados automaticamente para o administrador, que pode redistribuir para outro profissional ou cancelar com notificação ao tutor via WhatsApp.

1.5 Conta Bancária e Regras de Repasse

O veterinário cadastra seus dados bancários para recebimento: banco, agência, conta e CPF ou CNPJ do titular. Informa também sua chave Pix. As regras de repasse são configuradas nesta etapa de acordo com a persona. Para o veterinário autônomo, o split é configurado entre o profissional e a plataforma, com percentual e prazo de liquidação definidos no onboarding. Para a persona empresa, o split é configurado entre o estabelecimento e a plataforma. A remuneração dos veterinários vinculados é uma relação interna da empresa com seus profissionais e está fora do escopo da plataforma. O processamento financeiro é integrado ao gateway Abacate Pay.

1.6 Planos de Assinatura (Pensar nas liberações por planos )

A plataforma oferece três planos progressivos.

Básico: agenda com lembretes, prontuário básico e comunicação com o tutor via WhatsApp.

Profissional: tudo do plano Básico, acrescido de IA assistente durante a consulta, geração automática de documentos e prontuário completo e estruturado.

Enterprise: tudo do plano Profissional, acrescido de analytics avançado, gestão multiprofissional e integrações externas(quais seriam essas integrações?).

1.7 Ativação e Primeiro Acesso

Após concluir o cadastro, validar o CRMV e assinar o plano, o perfil do veterinário é publicado na plataforma e fica visível para tutores na região. A partir desse momento ele está apto a receber agendamentos e realizar atendimentos.

FASE 2 — OPERAÇÃO DIÁRIA

2.1 Dashboard

Ao acessar a plataforma, o veterinário visualiza o dashboard do dia. Essa é a tela central de operação e apresenta a agenda do dia com todos os atendimentos programados, pacientes aguardando atendimento, alertas de animais de alto risco e retornos pendentes. Cada item é clicável e direciona o veterinário diretamente para o painel do paciente correspondente.

O administrador visualiza o dashboard consolidado de toda a unidade, com a agenda de todos os veterinários e os indicadores operacionais do estabelecimento.

2.2 Pré-Consulta — Briefing do Animal

Antes de iniciar o atendimento, o veterinário acessa o painel do animal. O sistema apresenta automaticamente um briefing com as informações relevantes para aquela consulta: histórico clínico completo do animal, medicações em uso, data e resumo da última consulta, alertas clínicos ativos e resultados de exames anteriores registrados na plataforma.

Caso o tutor tenha relatado sintomas via WhatsApp antes da consulta, essas informações chegam ao veterinário neste momento, já organizadas e visíveis no briefing. O veterinário entra na consulta com contexto completo, sem precisar perguntar o que já foi informado.

2.3 Modalidade de Atendimento

A plataforma suporta atendimentos presenciais e remotos. A modalidade é definida no momento do agendamento e leva em consideração a gravidade do caso e a natureza do serviço. Procedimentos que exigem presença física, como cirurgias, vacinações e exames clínicos, são sempre presenciais. Consultas de acompanhamento, retornos e orientações podem ser realizadas de forma remota.

O atendimento remoto acontece por videochamada integrada dentro do próprio app da plataforma. No horário agendado, o tutor recebe uma notificação via WhatsApp com um link que abre diretamente a sala de atendimento no app. O veterinário acessa a sala pelo próprio painel da plataforma. Durante a chamada, o veterinário tem acesso simultâneo ao painel do animal com briefing, histórico e sugestões da IA, sem precisar alternar entre telas. Ao encerrar a chamada, o fluxo pós-consulta de geração de documentos segue normalmente.

2.4 Durante a Consulta — IA Assistente

Durante o atendimento a IA opera em paralelo ao veterinário com duas funções principais.

Sugestão de Diagnóstico: com base nos sintomas relatados pelo tutor, nos dados do briefing e no histórico longitudinal do animal, a IA apresenta hipóteses diagnósticas ordenadas por probabilidade. O veterinário valida ou descarta cada hipótese.

Sugestão de Protocolo: a partir do diagnóstico validado, a IA sugere medicamentos com doses calculadas por espécie e peso, posologia e alertas de interação medicamentosa. O veterinário valida o protocolo antes de qualquer geração de documento.

A IA não toma nenhuma decisão de forma autônoma. Toda sugestão exige validação explícita do profissional.

2.5 Fluxo Financeiro

O pagamento é processado de forma antecipada na maioria dos serviços. Consultas, vacinações, cirurgias e exames são pagos no momento do agendamento via Pix ou cartão de crédito e débito, com processamento via Abacate Pay.

Exceções ao pagamento antecipado: em internações é cobrado um valor de caução no ato da entrada e o saldo restante é apurado ao longo da internação com base nas diárias e procedimentos registrados, sendo pago na saída. Em atendimentos de emergência presencial, quando o animal chega sem agendamento prévio, o pagamento é realizado no ato do atendimento.

O split financeiro é processado automaticamente pelo sistema conforme as regras configuradas no onboarding. Para o veterinário autônomo o repasse é feito diretamente entre o profissional e a plataforma. Para a persona empresa o repasse é feito entre o estabelecimento e a plataforma.

2.6 Fluxo de Internação

O veterinário abre uma ficha de internação vinculada ao animal. A cada dia, o profissional responsável pelo acompanhamento registra na plataforma os procedimentos realizados, medicações administradas e evolução clínica do animal. O sistema apura automaticamente o valor total com base nos itens registrados e no número de diárias. O tutor recebe atualizações diárias via WhatsApp sobre o estado do animal. Ao receber alta, o sistema gera o resumo completo da internação, a nota fiscal com o saldo restante após desconto da caução e encaminha o link de pagamento via Abacate Pay. O prontuário da internação é incorporado ao histórico longitudinal do animal.

2.7 Fluxo de Exames

O veterinário solicita um exame dentro da plataforma durante ou após a consulta. O sistema registra a solicitação no histórico do animal e notifica o tutor via WhatsApp com as orientações necessárias para a realização do exame.

Quando o resultado estiver disponível, ele pode ser inserido na plataforma de duas formas: pelo próprio veterinário via upload, ou pelo laboratório parceiro caso haja integração estabelecida. O resultado é vinculado automaticamente ao histórico do animal e gera um alerta para o veterinário solicitante. A IA processa o resultado e o incorpora ao contexto clínico do animal para consultas futuras. O tutor recebe o resultado via WhatsApp assim que o veterinário liberar o acesso.

2.8 Pós-Consulta — Geração de Documentos

Após a validação do diagnóstico e do protocolo pelo veterinário, o sistema gera automaticamente os documentos do atendimento.

Prontuário: gerado de forma completa e estruturada, registrando todos os dados clínicos da consulta e alimentando o histórico longitudinal do animal.

Atestado: gerado conforme a necessidade do atendimento, podendo ser de saúde, óbito ou transporte.

Receita Veterinária: preenchida automaticamente pela IA com medicamento, dose, posologia e orientações com base no protocolo validado. O veterinário assina digitalmente com certificado vinculado ao seu CRMV. Essa etapa é obrigatória e não pode ser suprimida por exigência legal.

Nota Fiscal: gerada automaticamente com split financeiro processado em seguida, respeitando as regras de repasse configuradas.

2.9 Correção Pós-Geração de Documentos

O veterinário pode solicitar correção de um documento já gerado dentro de um prazo de 24 horas após a consulta. O sistema não sobrescreve o documento original. Em vez disso, gera uma versão corrigida vinculada ao registro original, com anotação automática de data, hora e CRMV do profissional que solicitou a correção. Ambas as versões ficam armazenadas no histórico do animal.

Para correções fora do prazo de 24 horas, o sistema exige uma justificativa registrada antes de liberar a edição. Esse fluxo está em conformidade com as diretrizes do CFMV para alteração de prontuários clínicos.

2.10 Distribuição de Documentos

Após a geração, os documentos são distribuídos automaticamente por dois caminhos.

O tutor recebe via WhatsApp os documentos pertinentes ao seu animal: receita, atestado quando aplicável, resumo da consulta e orientações pós-atendimento geradas pela IA.

O Vetly recebe todos os dados do atendimento para consolidação do histórico clínico e financeiro. O Vetly opera como mente colmeia: independentemente de onde o animal for atendido dentro da rede de clínicas parceiras, o histórico completo estará disponível para o profissional que realizar o próximo atendimento.

FASE 3 — WHATSAPP

3.1 Visão Geral do Canal

O WhatsApp é o canal principal de relacionamento entre a plataforma e o tutor. O canal é bidirecional: o sistema dispara mensagens proativas e o tutor pode interagir ativamente com a IA a qualquer momento. Toda a experiência é projetada para funcionar integralmente dentro do WhatsApp, sem exigir que o tutor instale um aplicativo ou acesse outro ambiente, exceto no momento do pagamento, que redireciona para o checkout do Abacate Pay.

3.2 Primeiro Acesso do Tutor

O primeiro acesso do tutor ao canal pode acontecer por três caminhos. O veterinário ou recepcionista envia um link de ativação via WhatsApp ou SMS diretamente pelo painel da plataforma após o primeiro atendimento presencial. Um QR code disponível no consultório ou recepção da clínica direciona o tutor para o canal ao ser escaneado. O próprio tutor encontra o veterinário na plataforma e inicia o contato pelo canal diretamente.

Em qualquer um dos três caminhos, ao iniciar a conversa o tutor passa por um fluxo de boas-vindas onde confirma seus dados, cadastra o pet e registra o consentimento LGPD antes de qualquer outra interação.

3.3 Agendamento de Consulta

O tutor inicia o agendamento enviando uma mensagem para o número da plataforma. A IA conduz o fluxo conversacional coletando as informações necessárias: identificação do pet, tipo de serviço desejado, preferência de data e horário e nome do veterinário caso o tutor já tenha um de preferência.

Com base nas informações coletadas, a IA apresenta as opções de horários disponíveis. Após a escolha do tutor, o sistema gera o link de pagamento via Abacate Pay e encaminha pelo próprio WhatsApp. A consulta é confirmada somente após a confirmação do pagamento. O tutor recebe uma mensagem de confirmação com data, horário, nome do veterinário e modalidade do atendimento.

Quando não há horários disponíveis para o serviço ou veterinário solicitado, a IA apresenta três opções ao tutor: entrar em lista de espera para aquele veterinário específico, ver horários disponíveis com outros veterinários da mesma clínica ou região, ou escolher uma data futura disponível. Se o tutor entrar na lista de espera e um horário abrir, ele recebe notificação imediata via WhatsApp com a opção de confirmar em um clique.

3.4 Cancelamento e Remarcação

O tutor pode cancelar ou remarcar uma consulta diretamente pelo WhatsApp. A IA identifica a solicitação, localiza o agendamento ativo e apresenta as opções disponíveis.

Em caso de remarcação, um novo horário é selecionado e o pagamento já realizado é transferido para a nova data. Em caso de cancelamento, a plataforma aplica a política de reembolso configurada pela clínica: cancelamentos com mais de 24 horas de antecedência geram reembolso integral; cancelamentos entre 24 horas e 2 horas antes da consulta geram reembolso parcial, com percentual de retenção configurável pela clínica no onboarding; cancelamentos com menos de 2 horas de antecedência ou no ato do atendimento não geram reembolso.

O tutor é informado da política no momento do agendamento e recebe a confirmação do reembolso via WhatsApp quando aplicável.

3.5 Relato de Sintomas e Triagem

O tutor pode relatar sintomas do seu pet pelo WhatsApp a qualquer momento, independente de ter uma consulta agendada. A IA conduz uma triagem conversacional coletando informações sobre os sintomas, há quanto tempo estão ocorrendo, comportamento do animal e outros dados relevantes.

Com base nas informações coletadas, a IA responde com orientações básicas adequadas à situação. Se a gravidade dos sintomas indicar necessidade de atendimento, a IA recomenda o agendamento de uma consulta e oferece iniciar o fluxo diretamente na conversa. Se o tutor já tiver uma consulta agendada, as informações coletadas na triagem são encaminhadas automaticamente ao veterinário e ficam disponíveis no briefing pré-consulta.

3.6 Recebimento de Documentos

Após cada atendimento, o tutor recebe automaticamente pelo WhatsApp os documentos gerados pela consulta: receita veterinária, atestado quando aplicável, resumo do atendimento com as principais orientações pós-consulta e resultados de exames quando liberados pelo veterinário. Todos os documentos são enviados em formato legível e acessível diretamente na conversa.

3.7 Comunicação Proativa — Lembretes e Alertas

A plataforma monitora continuamente o histórico e o perfil do animal e dispara comunicações proativas para o tutor nos seguintes casos: lembretes de vacinas e vermífugos com antecedência configurável, baseados no protocolo específico da espécie, raça e idade do animal; lembretes de retorno quando o veterinário programa um acompanhamento ao final da consulta; lembretes de medicação quando o animal está em tratamento contínuo, com alertas de dose e orientações de recompra quando o estoque estimado estiver próximo do fim; alertas de check-up preventivo baseados na fase de vida do animal com recomendações específicas por espécie e raça; orientações sazonais relevantes para o pet como cuidados com calor, frio, época de carrapatos e outros fatores ambientais pertinentes à região e à estação do ano; dicas personalizadas de nutrição, higiene e bem-estar geradas com base no perfil individual do animal, não conteúdo genérico.

3.8 Régua de Reenvio de Lembretes

Para cada lembrete programado, o sistema opera com uma régua de até três tentativas. Primeiro envio com 7 dias de antecedência. Segundo envio, sem resposta, com 3 dias de antecedência. Terceiro envio, sem resposta, 1 dia de antecedência. Após três tentativas sem resposta, a clínica ou veterinário responsável recebe um alerta no dashboard sinalizando o tutor como não responsivo para aquele evento. O sistema não realiza novos envios para o mesmo evento, evitando spam e bloqueio do número.

3.9 Tom e Comportamento da IA

A IA se comporta como uma assistente próxima e empática, mas objetiva. Ela nunca substitui o veterinário em orientações clínicas complexas e sempre que necessário encaminha o tutor para um atendimento profissional. A experiência deve transmitir ao tutor a sensação de que nenhum detalhe do cuidado do seu pet será esquecido.

LGPD — FLUXO OPERACIONAL
4.1 Coleta de Consentimento

O consentimento é coletado no primeiro contato do tutor com a plataforma, seja pelo WhatsApp ou presencialmente. O sistema apresenta de forma clara e em linguagem acessível quais dados serão coletados e para quais finalidades: atendimento clínico, lembretes e comunicação proativa, e compartilhamento com clínicas parceiras da rede.

O consentimento é granular. O tutor autoriza separadamente cada finalidade e pode revogar qualquer uma delas a qualquer momento enviando uma mensagem pelo próprio WhatsApp. A revogação é registrada com data e hora no sistema.

4.2 Proteção de Dados

Dados de saúde do animal são classificados como sensíveis dentro da arquitetura do sistema e possuem camada adicional de proteção de acesso. Apenas veterinários que já atenderam ou estão agendados para atender aquele animal têm acesso ao prontuário, salvo consentimento explícito do tutor para compartilhamento amplo na rede.

Dados anonimizados e agregados podem ser utilizados para geração de insights internos e, em fases futuras do roadmap, para parcerias com a indústria, sempre respeitando os limites legais da LGPD.

4.3 Segurança como Argumento de Venda

A conformidade LGPD nativa da plataforma é um diferencial competitivo para as clínicas parceiras: ao adotar a Clyvo Vet, o estabelecimento elimina o risco jurídico de não conformidade sem precisar implementar nenhuma estrutura própria de governança de dados.