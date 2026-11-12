# 🤖 MyClone: Assistente Operacional e Orquestrador de Tarefas

**Resumo Executivo:** Orquestração de um assistente virtual autônomo (clone operacional) desenvolvido para centralizar a captura de demandas, priorizar tarefas e executar ações automaticamente. O sistema atua como um "segundo cérebro", integrando processamento de linguagem natural, gestão de calendário e roteamento de execução entre agentes de IA e humanos.

*(Nota: O código fonte deste projeto é mantido privado por conter dados pessoais e configurações de infraestrutura sensíveis. Este documento detalha a arquitetura do agente e a engenharia da solução focada na trilha de AI Builder.)*

---

## 🚩 O Desafio Operacional (A Dor)
A gestão de demandas diárias sofre de fragmentação crônica: pendências chegam espalhadas por WhatsApp, e-mail, anotações em papel ou ficam apenas na cabeça. O resultado é um acúmulo onde nada é efetivamente perdido, mas também nada avança com agilidade, gerando sobrecarga cognitiva e ineficiência na execução.

## 💡 Arquitetura e Fluxograma da Solução

O sistema foi desenhado como um funil automatizado de produtividade, indo da captura da mensagem à execução final, utilizando um repositório *Markdown* como banco de dados canônico.

<img width="2154" height="702" alt="fluxo" src="https://github.com/user-attachments/assets/1cf47e7b-d8e4-419f-8a8f-ac08813c0753" />


1. **Captura Omnichannel:** O usuário envia texto ou áudio para um bot privado no Telegram. Um serviço no servidor processa o áudio localmente (via *faster-whisper*, sem custos de API) e o converte em uma nota na Caixa de Entrada.
2. **Triagem por IA:** O agente analisa a caixa de entrada e classifica cada demanda: define a área, estipula prazo, avalia o esforço e decide "Quem executa?" (humano, agente de chat, agente de desktop ou agente de código).
3. **Orquestração de Agenda (Briefing):** O sistema cruza as pendências com o Google Calendar. Ele aloca inteligentemente tarefas longas nos espaços livres maiores da agenda, e tarefas rápidas nos intervalos curtos, entregando um planejamento diário viável.
4. **Priorização Dinâmica:** Geração automatizada de uma página diária baseada na Matriz GUT (Gravidade × Urgência × Tendência), que se atualiza em tempo real a cada item concluído.
5. **Execução e Commit:** A fila delegada ao agente de IA roda de forma autônoma. Ao finalizar, o agente apenas adiciona o resultado ao final da nota e atualiza os metadados. Um *hook* de segurança garante que a IA nunca sobrescreva dados já existentes. 

## 🛠️ Infraestrutura e Governança de Dados

A arquitetura foge do padrão de bancos de dados tradicionais, apostando na portabilidade e segurança:

* **Fonte da Verdade (Vault em Obsidian):** O banco de dados é um *vault* de arquivos Markdown com *frontmatter* YAML. Ele é versionado via Git e sincronizado continuamente entre o servidor e o ambiente local.
* **Servidor e Painel Web:** Hospedado em um VPS Linux próprio executando os serviços via `systemd`. O controle visual é feito por um painel web construído em Flask (PWA), onde cada ação no painel gera automaticamente um *commit* no Git.
* **Segurança (*Zero Trust*):** Acesso à web protegido por túneis do **Cloudflare Access**. Nenhum segredo ou `.env` está versionado. Os agentes possuem diretrizes rígidas que os impedem de executar comandos vindos de conteúdos externos.
* **Previsibilidade Financeira:** Transcrição e hospedagem rodam em hardware próprio e serviços com assinatura fixa, sem surpresas de faturamento por chamadas de API avulsas.

---

## 📈 Metodologia e Rastreabilidade
Desenvolvido com **Claude Code** utilizando a metodologia TDD (*Test-Driven Development*). A evolução do sistema é guiada por especificações (*specs*) rigorosas antes da codificação.

* **Confiabilidade:** +1.300 testes automatizados validados a cada alteração.
* **Maturidade de Código:** +400 commits rastreáveis.
* **Rastreabilidade de Decisões:** Toda alteração estrutural notável é registrada em um *Decision Log*, elevando a versão da especificação e mantendo o histórico de engenharia documentado.

