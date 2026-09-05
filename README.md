# Vetly — Documentação

Vetly é uma plataforma marketplace que conecta tutores de pets a veterinários/clínicas independentes: busca, agendamento, pagamento com split, fidelidade e um prontuário que pertence ao animal (não à clínica), portável entre clínicas parceiras ("mente colmeia").

Esta pasta é a fonte de verdade da arquitetura, regras de negócio e escopo do MVP, usada tanto pelo time quanto por agentes de IA trabalhando nos repositórios `vetly-.net`, `vetly-java`, `vetly-database` e no client app.

## Documentos

| Arquivo | Conteúdo |
|---|---|
| [`vetly-produto.md`](./vetly-produto.md) | Visão de produto: personas, modelo de negócio, fluxos de uso (§8), monetização |
| [`vetly-tech.md`](./vetly-tech.md) | Fonte de verdade das regras de negócio (RN-001–107), mapa de entidades/relacionamentos, modelo de dados |
| [`vetly-engenharia-v2.md`](./vetly-engenharia-v2.md) | Documentação técnica de endpoints, rebaseada no código real de `vetly-.net`: o que já existe, o que falta, conflitos entre produto e código |
| [`flow-vetly-stt.json`](./flow-vetly-stt.json) | Export do fluxo Node-RED de transcrição de áudio (speech-to-text), referenciado em `vetly-engenharia-v2.md` §5.3 |

## Backend ativo

O backend em desenvolvimento é o **`vetly-.net`** (ASP.NET Core). `vetly-java` + `vetly-database` implementam uma versão anterior e mais limitada do produto e estão congelados — ver [`legado/AVISO.md`](./legado/AVISO.md) antes de usar qualquer coisa desses repositórios como referência de regra de negócio atual.

## Convenção de rastreabilidade

Toda regra de negócio tem um código `RN-XXX` estável entre `vetly-tech.md` (definição) e `vetly-engenharia-v2.md` (onde/como está implementada no código). Ao alterar uma regra, atualize os dois documentos.
