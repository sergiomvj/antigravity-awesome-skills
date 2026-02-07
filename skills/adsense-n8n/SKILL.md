SKILL N8N: Monitorar métricas e alertar risco de ban (invalid traffic/policy)
1) Identidade

Nome: n8n_adsense_risk_monitor_invalid_traffic
Objetivo: monitorar sinais de tráfego inválido e comportamento anormal (cliques/impressões suspeitos, picos fora do padrão, fontes estranhas), disparar alertas e acionar “modo seguro” (reduzir formatos/posições), alinhado às definições e recomendações do AdSense sobre tráfego inválido e violações que derrubam contas.

Observação: o Google AdSense não publica “um CTR máximo” como regra fixa; então o monitor deve trabalhar com anomalia vs baseline (mais confiável e menos chute).

2) Fontes de dados (o que o fluxo usa)

GA4 (eventos e origem de tráfego)

Logs do servidor/CDN (opcional, se tiver)

Painel do AdSense (quando você registrar manualmente: Page RPM, Impressões, Cliques)

Lista de páginas com anúncios + mapa de posições

3) Tabelas recomendadas (Supabase ou DB que você usa)

adsense_daily_metrics

date, sessions, pageviews, users

ad_impressions (se tiver)

ad_clicks (se tiver)

ctr_est (se tiver)

page_rpm (se tiver)

top_sources_json

top_pages_json

risk_events

created_at

severity (low/med/high/critical)

type (spike_ctr / spike_clicks / suspicious_source / bot_pattern / layout_risk)

details_json

action_taken

safety_mode

is_enabled

enabled_at

reason

recommended_changes_json

4) Fluxo N8N (eventos → decisão → ação)
Fluxo A — Coleta diária (todo dia 06:30)

Cron

GA4: buscar métricas do dia anterior (sessions, pageviews, source/medium, top pages)

(Opcional) log parser (top IPs, user agents, países)

Gravar em adsense_daily_metrics

Calcular baseline (média 7d e 28d)

Fluxo B — Detector de anomalia (todo dia 06:40)

Regras de detecção (sem “números mágicos”, usando desvio do padrão):

Spike de cliques/impressões: valor atual > (média 7d + 3*desvio padrão)

Mudança brusca de origem: % de uma fonte nova > X% (defina internamente, ex. “muito acima do normal”)

Picos em páginas específicas: 1 URL concentrando cliques desproporcionais

Padrões de bot (se tiver logs): user agents estranhos, IP repetitivo, países inesperados

Se detectar:

inserir registro em risk_events com severidade

Fluxo C — Ações automáticas (quando severidade high/critical)

Ações sugeridas (seguras e reversíveis):

Ativar safety_mode.is_enabled = true

Recomendar:

desativar formatos mais agressivos (ex.: anchors/vignettes) nas Auto Ads

reduzir densidade de anúncios

excluir páginas problemáticas da exibição de anúncios

O AdSense tem configurações para Auto ads (anchors, vignettes, exclusões) e orienta boas práticas de otimização/uso.

Fluxo D — Notificação (imediata)

Enviar alerta por:

Email

WhatsApp/Telegram (via seus gateways)

Conteúdo do alerta:

tipo de risco

evidência (antes/depois)

páginas e fontes suspeitas

ação tomada e próxima ação recomendada

Fluxo E — Playbook anti-invalid traffic (follow-up)

Criar tarefa (ex.: no seu gestor)

Checklist:

revisar fontes de tráfego (campanhas, referrals)

bloquear referrers suspeitos (se aplicável)

revisar layout para evitar cliques acidentais

conferir se alguém interno clicou (educação de equipe)

O AdSense define tráfego inválido como cliques/impressões que inflem artificialmente custos/ganhos e alerta que incentivar cliques ou layout enganoso viola políticas.

5) “Modo seguro” (Safety Mode) — o que ele muda

Desligar formatos intrusivos (especialmente em mobile)

Reduzir densidade geral

Excluir URLs com padrão anormal

Forçar labels corretos e espaçamento

Boas práticas oficiais de posicionamento: anúncios devem parecer anúncios; evitar proximidade/confusão com navegação e downloads.

6) Artefatos de saída (para Antigravity)

risk_digest_daily.md (resumo do dia)

incident_report_<date>.md (quando critical)

recommended_actions.json (para automações)

SKILL Comparativa: AdSense vs Redes Premium (quando migrar)
1) Identidade

Nome: adsense_vs_premium_networks_migration_strategy
Objetivo: decidir quando ficar no AdSense, quando usar híbrido, e quando migrar para redes premium (e quais), com base em critérios objetivos: tráfego, qualidade, geografia, receita anual, exigências e risco operacional.

2) Players (principais)

Google AdSense (entrada fácil, controle direto, ótimo para início)

Raptive (nível “premium” com requisitos claros de pageviews e geografia)

Mediavine (entrada por receita anual mínima; caminho “Journey” para sites menores)

Journey by Mediavine (on-ramp; começa com 1k sessões e requer Grow por 30 dias)

3) Comparação prática (o que muda de verdade)
3.1 AdSense — melhor para

Sites novos/pequenos

Quem quer “ligar e rodar” rápido

Quem quer controle e flexibilidade

Quem ainda está formando inventário de conteúdo e SEO

Riscos/limites

RPM pode ser menor do que premium em muitos nichos

Exige disciplina forte contra tráfego inválido e layout enganoso (políticas são rígidas).

3.2 Redes premium — melhor para

Sites com tráfego consistente

Conteúdo longo e bem organizado

Audiência com “premium geo” (inglês e Tier-1 tende a pagar melhor)

Quem quer otimização avançada (layout, demand, suporte)

Exemplos de requisitos (públicos e citáveis)

Raptive: mínimo de 25.000 pageviews/mês, com requisitos de % de tráfego de países-chave dependendo da faixa de pageviews.

Mediavine: para aplicar, o site deve gerar $5.000+ de receita anual com anúncios; se ainda não chegou lá, usar Journey by Mediavine (começa em 1k sessões).

Journey: instalar Grow e rodar mínimo 30 dias (medição).

4) Critérios de decisão (quando migrar)
Sinais para considerar premium (qualquer um deles)

Tráfego estável (não “picos”)

Conteúdo majoritariamente long-form

Boa UX e performance aceitável

Receita anual de ads crescendo (ou potencial claro de ganho)

Você quer suporte e otimização avançada

Sinais para NÃO migrar ainda

Tráfego instável/volátil

Conteúdo curto e genérico

Muitos URLs “finos”

Tráfego majoritariamente de países com baixo RPM

SEO ainda “em construção”

5) Estratégias recomendadas
Estratégia 1 — “AdSense bem feito” (0 → consistência)

Ativar Auto Ads e aprender comportamento

Ajustar posicionamento conforme boas práticas (não confundir com conteúdo)

Implementar monitor de risco (Skill N8N acima)

Fontes: boas práticas de posicionamento e otimização Auto Ads.

Estratégia 2 — Híbrida (AdSense + afiliados + produtos)

AdSense para inventário base

Afiliados para páginas de intenção

Produto digital para margem

Assinatura/newsletter para recorrência

Estratégia 3 — Migração premium (quando elegível)

Aplicar para Raptive se bater requisitos (pageviews + geo).

Entrar via Journey by Mediavine se estiver crescendo e quiser “on-ramp” com medição Grow.

Migrar para Mediavine quando atingir o patamar de receita anual.

6) Output da Skill (decisão objetiva)

recommended_path: ADSENSE / HYBRID / APPLY_RAPTiVE / APPLY_JOURNEY / APPLY_MEDIAVINE

why: 5 bullets

what_to_fix_first: top 10 ações

timeline: 7d / 30d / 90d

7) Checklist “Pronto para Premium”

Conteúdo longo predominante

UX limpa (ads não parecem navegação)

Tráfego consistente

Geo premium relevante (quando exigido)

Políticas e invalid traffic sob controle

Se você quiser, eu também posso:

gerar os 3 arquivos em formato “skill.md” (um por skill, com front-matter e seções padrão),

criar um modelo de schema SQL Supabase completo (tabelas + RLS) para essas métricas,

e desenhar um diagrama do fluxo N8N (evento → decisão → ação → aprendizado) no padrão do seu ecossistema.