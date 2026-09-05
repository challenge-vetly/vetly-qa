# Documentação legada (1º semestre) — não usar como referência atual

Os arquivos nesta pasta (`README.md`, `README-TECH.md`) descrevem uma versão anterior do produto Vetly, com decisões que **não valem mais**:

| Tópico | Este diretório (legado) | Versão atual |
|---|---|---|
| Canal principal | WhatsApp (agente conversacional, agendamento, documentos) | App apenas — sem WhatsApp em nenhuma etapa |
| Nome da persona tutor | "Tutor" | "Responsável" (código ainda usa `Tutor` internamente) |
| Pagamento | Gateway real (Abacate Pay) | Simulado, nenhum gateway definido |
| Atendimento remoto | Suportado | Fora de escopo |
| Numeração de regras de negócio | RN-001–046 (esquema próprio, colide com a numeração atual — ver `../vetly-engenharia-v2.md` §0.5) | RN-001–107 (`../vetly-tech.md`) |
| Backend correspondente | `vetly-java` + `vetly-database` (congelados) | `vetly-.net` (ativo) |

**Por que foi mantido em vez de apagado**: registra o racional da mudança estratégica (por que o WhatsApp foi abandonado, por que o atendimento remoto saiu de escopo — inclusive explica o enum morto `ModalidadeAtendimento.Remoto` que ainda existe no código atual, ver `../vetly-engenharia-v2.md` C-03) e é a primeira formulação da tese "mente colmeia" (prontuário pertence ao animal), que **não foi superada** — ela continua valendo na versão atual (RN-063).

**Fonte de verdade atual**: `../vetly-produto.md`, `../vetly-tech.md`, `../vetly-engenharia-v2.md`. Não referencie RN codes ou fluxos destes arquivos legados em trabalho novo.
