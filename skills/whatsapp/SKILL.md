# WhatsApp Management Skill

**Skill para gestão profissional de WhatsApp Business**

---

## 📋 METADADOS DA SKILL

```yaml
name: whatsapp-management
version: 1.0
description: Sistema completo de gestão WhatsApp Business para monitoramento de grupos, engajamento seletivo de membros, CRM integrado e workflows automatizados
tags: whatsapp, crm, automation, marketing, engagement, analytics
author: Custom Skill
```

---

## 🎯 QUANDO USAR ESTA SKILL

Use esta skill quando o usuário precisar:
- Monitorar e analisar dados de grupos WhatsApp
- Implementar estratégias de engajamento seletivo de membros
- Construir sistemas CRM para contatos WhatsApp
- Automatizar campanhas de mensagens
- Extrair e analisar métricas de conversação
- Gerenciar múltiplos grupos ou contas WhatsApp
- Implementar sistemas de compliance e moderação

---

## 🚀 QUICK START

### Pré-requisitos

```bash
# Instalar dependências
pip install pandas openpyxl selenium whatsapp-web.py --break-system-packages

# Para WhatsApp Business API oficial
pip install requests python-dotenv jwt --break-system-packages
```

### Inicialização Rápida

```python
# 1. Inicializar banco de dados
python -c "from scripts.group_monitor import GroupMonitor; GroupMonitor().init_database()"

# 2. Testar monitoramento
python scripts/group_monitor.py --group "Meu Grupo" --days 7

# 3. Exportar análises
python scripts/group_monitor.py --group "Meu Grupo" --export relatorio.xlsx
```

---

## 📊 CORE WORKFLOWS

### Workflow 1: Monitoramento de Grupos

**Objetivo:** Analisar atividade, identificar padrões, extrair métricas

**Passos:**

1. **Coletar dados do grupo**
```python
monitor = GroupMonitor()
monitor.log_message(phone="+5521999999999", group_name="Sales Team", 
                   content="Mensagem exemplo")
```

2. **Gerar analytics**
```python
analytics = monitor.get_group_analytics("Sales Team", days=7)
# Retorna: total_messages, unique_members, peak_hour, most_active, silent_members
```

3. **Exportar para Excel**
```python
monitor.export_to_excel("Sales Team", "relatorio.xlsx", days=7)
# Sheets: Summary, Most Active, Silent Members, Daily Activity
```

**Métricas Disponíveis:**
- Total de mensagens por período
- Membros únicos ativos
- Média de mensagens/dia
- Horário de pico
- Top 10 membros mais ativos
- Lista de membros silenciosos
- Distribuição diária e por hora

---

### Workflow 2: Segmentação e Engajamento Seletivo

**Objetivo:** Identificar e abordar grupos específicos de membros

**Segmentos Pré-definidos:**

```python
# Membros ativos (>10 mensagens)
--segment active

# Membros moderados (3-10 mensagens)
--segment moderate

# Membros silenciosos (0 mensagens)
--segment silent

# Novos membros (<7 dias)
--segment new_members

# Inativos (>7 dias sem mensagem)
--segment inactive_7_days

# Top 20% mais ativos
--segment top_20_percent
```

**Segmentos Customizados:**

```python
# SQL-like queries
--segment "message_count > 50"
--segment "join_date > date('now', '-7 days')"
--segment "last_active < date('now', '-14 days') AND message_count > 5"
```

**Executar Campanha:**

```python
# 1. Preview (dry-run)
python scripts/selective_outreach.py \
  --segment new_members \
  --template welcome.json \
  --dry-run

# 2. Enviar mensagens
python scripts/selective_outreach.py \
  --segment new_members \
  --template welcome.json

# 3. Exportar segmento para análise
python scripts/selective_outreach.py \
  --segment top_20_percent \
  --export vip_members.csv
```

---

### Workflow 3: CRM e Gestão de Contatos

**Objetivo:** Centralizar dados de contatos e histórico de interações

**Operações Principais:**

1. **Exportar contatos**
```python
python scripts/crm_sync.py --export contatos.xlsx
# Sheets: Contacts (dados principais), Interactions (histórico)
```

2. **Importar/Atualizar contatos**
```python
# Criar Excel com colunas: phone, name, email, company, tags, lifecycle_stage
python scripts/crm_sync.py --import contatos.xlsx --update
```

3. **Enriquecer contatos**
```python
crm = CRMSync()
crm.enrich_contact("+5521999999999", {
    'email': 'cliente@empresa.com',
    'company': 'Tech Corp',
    'lifecycle_stage': 'customer'
})
```

4. **Gestão de Tags**
```python
# Adicionar tag a múltiplos contatos
python scripts/crm_sync.py --tag "+5521999999999,+5521888888888" "vip"

# Buscar por tag
contacts = crm.get_contacts_by_tag("vip")
```

5. **Gerar relatório CRM**
```python
python scripts/crm_sync.py --report crm_report.xlsx
# Sheets: Overview, Tags, Recent Activity
```

6. **Reparar inconsistências**
```python
python scripts/crm_sync.py --repair
# Remove duplicatas e sincroniza members → contacts
```

**Schema do Banco de Dados:**

```sql
-- Contatos principais
contacts (
    id, phone, name, email, company, 
    tags, custom_fields, created_at, updated_at, 
    lifecycle_stage
)

-- Membros dos grupos
members (
    id, phone, name, join_date, 
    last_active, message_count, status
)

-- Mensagens
messages (
    id, member_id, group_name, content, 
    timestamp, media_type
)

-- Interações CRM
interactions (
    id, contact_id, type, content, 
    timestamp, group_id
)

-- Campanhas
outreach (
    id, member_id, campaign_name, 
    message, sent_at
)
```

---

## 🎨 TEMPLATES DE MENSAGENS

### Template: Boas-vindas

```json
{
  "name": "Boas-vindas Padrão",
  "type": "welcome",
  "segment": "new_members",
  "message": "Olá {name}! 👋\n\nSeja muito bem-vindo(a) ao nosso grupo!\n\nEstamos muito felizes em ter você aqui. Este é um espaço para:\n\n✅ Compartilhar conhecimento\n✅ Fazer networking\n✅ Crescer junto com a comunidade\n\nSinta-se à vontade para participar!\n\n---\nPara não receber mensagens, responda SAIR",
  "timing": {
    "delay_hours": 24,
    "send_time": "10:00"
  },
  "options": {
    "personalize": true,
    "rate_limit": 40
  }
}
```

### Template: Reengajamento

```json
{
  "name": "Reengajamento",
  "type": "engagement",
  "segment": "inactive_7_days",
  "message": "Oi {first_name}! 😊\n\nNotamos que você não participa do grupo há um tempo e sentimos sua falta!\n\nTudo bem por aí?\n\nEnquanto você esteve ausente, tivemos discussões interessantes sobre:\n\n🔹 {topic_1}\n🔹 {topic_2}\n🔹 {topic_3}\n\nGostaria de receber um resumo?\n\nResponda SIM! 📬\n\n---\nPara sair, responda SAIR",
  "followup": {
    "if_reply_yes": "Ótimo! 🎉\n\nAqui está o resumo: {summary}",
    "if_no_reply_48h": "mark_as_churned"
  }
}
```

### Template: Oferta Especial

```json
{
  "name": "Oferta VIP",
  "message": "Oi {name}! 💰\n\nTenho uma oportunidade EXCLUSIVA:\n\n🔥 {offer_description}\n💵 De R${original_price} por R${discounted_price}\n⏰ Válido até {deadline}\n\n✅ Link: {offer_link}\n\nSó {slots_left} vagas! 🚀"
}
```

### Template: Pesquisa

```json
{
  "name": "Feedback",
  "message": "Olá {name}! 📊\n\nSua opinião importa!\n\n1️⃣ O que você mais gosta no grupo?\n2️⃣ O que podemos melhorar?\n3️⃣ Nota de 0 a 10?\n\nObrigado! 🙏"
}
```

### Template: Agradecimento

```json
{
  "name": "Agradecimento",
  "message": "Olá {name}! 🏆\n\nObrigado pela participação ativa!\n\nVocê já enviou {message_count} mensagens e é um dos membros mais engajados.\n\nSeu envolvimento faz diferença! 🙌"
}
```

### Tokens de Personalização

```
{name}           - Nome completo
{first_name}     - Primeiro nome
{phone}          - Telefone
{message_count}  - Total de mensagens
{join_date}      - Data de entrada
{last_active}    - Última atividade
{group_name}     - Nome do grupo
{custom_field}   - Campos customizados
```

---

## 🛡️ COMPLIANCE & ANTI-BAN

### Regras CRÍTICAS (NUNCA VIOLAR)

**Rate Limits Obrigatórios:**
```python
# ABSOLUTO (WhatsApp)
- Máximo: 50 msgs/hora
- Máximo: 256 msgs/dia
- Máximo: 1000 msgs/semana

# RECOMENDADO (Seguro)
- 30-40 msgs/hora
- 150-200 msgs/dia
- Distribuído em várias horas
```

**Horários Permitidos:**
```python
# PROIBIDO enviar:
- Antes de 8:00
- Depois de 22:00 (10 PM)

# IDEAL:
- 10:00-12:00 (manhã)
- 14:00-16:00 (tarde)
- 18:00-20:00 (noite)
```

**Conteúdo Proibido:**
```
❌ Palavras spam: FREE, WIN, CLICK, LIMITED
❌ ALL CAPS MESSAGES
❌ Emojis excessivos (>5 por mensagem)
❌ Informações enganosas
❌ Conteúdo adulto/ilegal
```

**Opt-Out OBRIGATÓRIO:**
```python
# Sempre incluir em broadcasts:
"Para não receber mensagens, responda SAIR"

# Keywords de opt-out
OPT_OUT = ['sair', 'parar', 'stop', 'cancelar', 'remover']

def check_opt_out(message):
    return message.lower().strip() in OPT_OUT
```

### Implementação Anti-Ban

**1. Rate Limiter**

```python
class RateLimiter:
    def __init__(self, messages_per_hour=40):
        self.limit = messages_per_hour
        self.sent_times = []
    
    def can_send(self):
        now = datetime.now()
        hour_ago = now - timedelta(hours=1)
        self.sent_times = [t for t in self.sent_times if t > hour_ago]
        return len(self.sent_times) < self.limit
    
    def wait_if_needed(self):
        while not self.can_send():
            time.sleep(60)
```

**2. Randomização de Timing**

```python
import random
import time

def send_with_delay(message, phone):
    # Delay aleatório 5-15 segundos
    delay = random.randint(5, 15)
    time.sleep(delay)
    
    # Personalizar mensagem
    personalized = personalize_message(message, phone)
    send_message(phone, personalized)
```

**3. Personalização Automática**

```python
def personalize_message(template, contact):
    msg = template.replace('{name}', contact['name'])
    
    # Variar saudação
    greetings = ['Olá', 'Oi', 'E aí', 'Tudo bem']
    msg = msg.replace('Olá', random.choice(greetings))
    
    # Variar pontuação
    if random.random() > 0.5:
        msg = msg.replace('!', '.')
    
    return msg
```

**4. Health Monitoring**

```python
def calculate_health_score(metrics):
    score = 100
    
    # Penalidades
    if metrics['delivery_rate'] < 0.9:
        score -= 15
    if metrics['blocks_received'] > 0:
        score -= 30
    if metrics['spam_reports'] > 0:
        score -= 50
    
    return max(0, score)

def should_pause_campaign(health_score):
    if health_score < 50:
        return True, "Critical health score"
    if metrics['blocks_received'] > 5:
        return True, "Too many blocks"
    return False, None
```

**5. Multi-Account Rotation**

```python
class AccountPool:
    def __init__(self, accounts):
        self.accounts = accounts
        self.limiters = {acc: RateLimiter() for acc in accounts}
    
    def get_available_account(self):
        for account in self.accounts:
            if self.limiters[account].can_send():
                return account
        return None  # All at limit
```

### Checklist Pré-Campanha

```
✅ Rate limiter ativo (max 40/hora)
✅ Horário comercial (8h-22h)
✅ Opt-out incluído em todas mensagens
✅ Mensagens personalizadas (sem copy-paste)
✅ Delay aleatório 5-15s entre envios
✅ Sem palavras spam
✅ Conteúdo profissional
✅ Usuários optaram-in
✅ Health score > 70
✅ Conta backup pronta
```

### Resposta a Violações

**Se Banido Temporariamente:**
1. PARE toda atividade imediatamente
2. Aguarde 24-72h
3. Revise o que causou o ban
4. Reduza volume em 50%
5. Implemente delays maiores
6. Monitore health metrics

**Se Banido Permanentemente:**
1. NÃO crie nova conta com mesmo número
2. Use número diferente
3. Comece com volume baixíssimo (<10/dia)
4. Construa reputação em 30 dias
5. Nunca use número banido novamente

---

## 📈 CONFIGURAÇÕES AVANÇADAS

### Monitoramento em Tempo Real

```python
# Stream live de atividades
python scripts/group_monitor.py \
  --group "Support" \
  --live \
  --dashboard

# Acesse: http://localhost:8080
```

### Sentiment Analysis

```python
# Ativar análise de sentimento
python scripts/group_monitor.py \
  --group "Community" \
  --sentiment \
  --days 30

# Retorna: positive_ratio, negative_ratio, neutral_ratio, trends
```

### Topic Extraction

```python
# Identificar tópicos automáticos
python scripts/group_monitor.py \
  --group "Tech Talk" \
  --topics \
  --min-frequency 5

# Retorna: top topics, keyword clusters, trending subjects
```

### Engagement Metrics

```python
# Métricas detalhadas
python scripts/group_monitor.py \
  --group "Sales" \
  --engagement-metrics

# Retorna:
# - Response time averages
# - Thread participation rates
# - Question vs answer ratios
# - Peak hours by member
```

### Automação Completa

**Configuração de Regras:**

```json
{
  "automation": {
    "welcome_sequence": {
      "enabled": true,
      "messages": [
        {"delay_hours": 1, "template": "welcome.json"},
        {"delay_hours": 72, "template": "tips_day_3.json"},
        {"delay_hours": 168, "template": "check_in.json"}
      ]
    },
    "re_engagement": {
      "enabled": true,
      "trigger": "last_active > 7days",
      "template": "engagement.json",
      "max_attempts": 2
    },
    "appreciation": {
      "enabled": true,
      "trigger": "message_count % 50 = 0",
      "template": "thank_you.json"
    }
  }
}
```

**Scoring Automático:**

```json
{
  "scoring": {
    "engagement_score": {
      "formula": "(message_count * 0.4) + (response_rate * 0.3) + (recency * 0.3)",
      "thresholds": {
        "high": ">70",
        "medium": "40-70",
        "low": "<40"
      }
    }
  }
}
```

**Alertas Automáticos:**

```json
{
  "alerts": {
    "sudden_drop": {
      "condition": "active_members_change < -20%",
      "action": "notify_admin"
    },
    "spam_detection": {
      "condition": "same_message_count > 5",
      "action": "flag_for_review"
    }
  }
}
```

### Otimização de Performance

```sql
-- Índices recomendados para grupos grandes (1000+ membros)
CREATE INDEX idx_messages_timestamp ON messages(timestamp);
CREATE INDEX idx_messages_member ON messages(member_id);
CREATE INDEX idx_members_activity ON members(last_active);
CREATE INDEX idx_contacts_phone ON contacts(phone);
CREATE INDEX idx_interactions_contact ON interactions(contact_id);
```

---

## 🔧 SCRIPTS PRINCIPAIS

### Script 1: group_monitor.py

**Funcionalidades:**
- Monitoramento de grupos em tempo real
- Análise de atividade de membros
- Geração de métricas de engajamento
- Exportação para Excel multi-sheet
- Identificação de padrões temporais

**Uso:**

```bash
# Análise básica
python scripts/group_monitor.py --group "Nome do Grupo" --days 7

# Com export
python scripts/group_monitor.py --group "Nome do Grupo" --days 30 --export report.xlsx

# Monitoramento live
python scripts/group_monitor.py --group "Nome do Grupo" --live
```

**Funções Principais:**

```python
class GroupMonitor:
    def add_member(phone, name, join_date)
    def log_message(phone, group_name, content, media_type)
    def get_group_analytics(group_name, days)
    def get_silent_members(group_name, days)
    def export_to_excel(group_name, output_file, days)
```

---

### Script 2: selective_outreach.py

**Funcionalidades:**
- Segmentação avançada de membros
- Campanhas personalizadas
- Rate limiting automático
- Dry-run para testes
- Log de outreach

**Uso:**

```bash
# Preview de campanha
python scripts/selective_outreach.py \
  --segment new_members \
  --template welcome.json \
  --dry-run

# Executar campanha
python scripts/selective_outreach.py \
  --segment inactive_7_days \
  --template engagement.json

# Exportar segmento
python scripts/selective_outreach.py \
  --segment top_20_percent \
  --export vip.csv
```

**Funções Principais:**

```python
class SelectiveOutreach:
    def parse_segment_query(segment)
    def get_target_members(segment)
    def load_template(template_path)
    def personalize_message(template, member)
    def send_campaign(segment, template_path, dry_run)
    def export_segment(segment, output_file)
```

---

### Script 3: crm_sync.py

**Funcionalidades:**
- Sincronização de contatos
- Import/Export Excel
- Enriquecimento de dados
- Gestão de tags
- Relatórios CRM

**Uso:**

```bash
# Exportar contatos
python scripts/crm_sync.py --export contatos.xlsx

# Importar e atualizar
python scripts/crm_sync.py --import contatos.xlsx --update

# Adicionar tags
python scripts/crm_sync.py --tag "+5521999999999,+5521888888888" "vip"

# Gerar relatório
python scripts/crm_sync.py --report crm_report.xlsx

# Reparar database
python scripts/crm_sync.py --repair
```

**Funções Principais:**

```python
class CRMSync:
    def export_contacts(output_file)
    def import_contacts(input_file, update)
    def enrich_contact(phone, data)
    def tag_contacts(phones, tag)
    def get_contacts_by_tag(tag)
    def repair_database()
    def generate_crm_report(output_file)
```

---

## 💼 CASOS DE USO PRÁTICOS

### Caso 1: Comunidade de Torcedores (Flamengo)

**Objetivo:** Engajar torcedores e monetizar comunidade

**Workflow:**

```bash
# 1. Monitorar atividade mensal
python scripts/group_monitor.py --group "Nação Rubro-Negra" --days 30 --export jan_2024.xlsx

# 2. Identificar super-fãs
python scripts/selective_outreach.py \
  --segment "message_count > 100" \
  --export super_fas.csv

# 3. Campanha VIP para super-fãs
python scripts/selective_outreach.py \
  --segment "message_count > 100" \
  --template oferta_vip_flamengo.json

# 4. Reengajar membros inativos
python scripts/selective_outreach.py \
  --segment "last_active > 14days AND message_count > 10" \
  --template volta_pra_nacao.json
```

**Template Específico:**

```json
{
  "name": "Oferta VIP Flamengo",
  "message": "E aí, {first_name}! ⚫🔴\n\nVocê é um dos MAIORES torcedores do grupo!\n\n{message_count} mensagens mostram sua paixão! 🔥\n\nTenho uma OFERTA EXCLUSIVA:\n🎟️ Ingresso Jogo + Camisa Oficial\n💰 R$150 (desconto de 40%!)\n⏰ Só até sexta!\n\nLink: {offer_link}\n\nVAMOS MENGÃO! 🏆"
}
```

---

### Caso 2: SDR / Vendas B2B

**Objetivo:** Qualificar leads e nutrir prospects

**Workflow:**

```bash
# 1. Exportar todos leads
python scripts/crm_sync.py --export leads_whatsapp.xlsx

# 2. Enriquecer com dados (manualmente no Excel)
# Adicionar colunas: company, email, lifecycle_stage, tags

# 3. Reimportar enriquecido
python scripts/crm_sync.py --import leads_whatsapp.xlsx --update

# 4. Segmentar hot leads
python scripts/selective_outreach.py \
  --segment "tag = 'hot_lead'" \
  --export hot_leads.csv

# 5. Campanha de follow-up
python scripts/selective_outreach.py \
  --segment "tag = 'hot_lead' AND last_active < 3days" \
  --template follow_up_vendas.json

# 6. Nurture leads frios
python scripts/selective_outreach.py \
  --segment "lifecycle_stage = 'lead' AND last_active > 7days" \
  --template nurture_conteudo.json
```

**Pipeline CRM:**

```
Lead → Opportunity → Customer
  ↓         ↓            ↓
Tag    Tag + Score   Tag + LTV
```

---

### Caso 3: Community Management

**Objetivo:** Manter engajamento alto e saudável

**Workflow Diário:**

```bash
# Morning - Análise overnight
python scripts/group_monitor.py --group "Tech Community" --days 1

# Afternoon - Identificar em risco
python scripts/selective_outreach.py \
  --segment "last_active BETWEEN 5 AND 7 days" \
  --export em_risco_hoje.csv

# Evening - Campanha reengajamento
python scripts/selective_outreach.py \
  --segment "last_active = 7days" \
  --template engagement.json
```

**Workflow Semanal:**

```bash
# Monday - Relatório semanal
python scripts/group_monitor.py --group "Tech Community" --days 7 --export week_report.xlsx

# Tuesday - Agradecer top contributors
python scripts/selective_outreach.py \
  --segment top_20_percent \
  --template thank_you.json

# Friday - CRM sync
python scripts/crm_sync.py --export community_data.xlsx
python scripts/crm_sync.py --report weekly_crm.xlsx
```

---

### Caso 4: Curso Online / Educação

**Objetivo:** Onboarding de alunos e suporte

**Workflow de Onboarding:**

```bash
# Dia 1 - Boas-vindas automáticas
python scripts/selective_outreach.py \
  --segment "join_date = today" \
  --template welcome_curso.json

# Dia 3 - Check-in
python scripts/selective_outreach.py \
  --segment "join_date = 3days_ago AND message_count = 0" \
  --template check_in_dia3.json

# Dia 7 - Engajamento
python scripts/selective_outreach.py \
  --segment "join_date = 7days_ago" \
  --template feedback_semana1.json

# Dia 30 - Survey conclusão módulo
python scripts/selective_outreach.py \
  --segment "join_date = 30days_ago" \
  --template survey_modulo1.json
```

---

### Caso 5: E-commerce / Loja Online

**Objetivo:** Vendas e pós-venda

**Workflow:**

```bash
# Segmentar por comportamento de compra
python scripts/crm_sync.py --import clientes_loja.xlsx --update

# Campanha novos produtos - compradores ativos
python scripts/selective_outreach.py \
  --segment "tag = 'comprou_ultimos_30d'" \
  --template novo_produto.json

# Recuperar carrinho abandonado
python scripts/selective_outreach.py \
  --segment "tag = 'carrinho_abandonado'" \
  --template recupera_carrinho.json

# Cross-sell baseado em histórico
python scripts/selective_outreach.py \
  --segment "tag = 'comprou_categoria_A'" \
  --template crosssell_categoria_B.json

# Pós-venda e review request
python scripts/selective_outreach.py \
  --segment "tag = 'comprou_7dias_atras'" \
  --template pedido_review.json
```

---

## 🎓 BEST PRACTICES

### Personalização Efetiva

**DO:**
- Use {first_name} em vez de {name} para mais proximidade
- Varie saudações: "Olá", "Oi", "E aí", "Tudo bem"
- Inclua dados específicos: "{message_count} mensagens"
- Contextualize: "Desde {join_date} com a gente"
- Emojis com moderação: 2-3 por mensagem

**DON'T:**
- Copiar/colar mensagens idênticas
- Usar {phone} em mensagens (não é natural)
- Abusar de emojis (>5)
- Mensagens genéricas sem personalização

### Timing Estratégico

**Melhores Horários por Objetivo:**

```
Boas-vindas: 10h-11h (energia positiva)
Vendas/Ofertas: 14h-16h (decisão racional)
Engajamento: 18h-20h (tempo livre)
Educacional: 9h-10h ou 20h-21h
Urgência: 11h-12h ou 17h-18h
```

**Frequência Recomendada:**

```
Welcome: Imediato (1h após join)
Follow-up: 72h após welcome
Check-in: 7 dias
Re-engagement: 14 dias de inatividade
Newsletter: Semanal (mesmo dia/hora)
Promoções: Máximo 2x/semana
```

### A/B Testing

**Elementos para Testar:**

```python
# Teste 1: Saudação
A = "Olá {name}!"
B = "E aí {first_name}!"

# Teste 2: CTA
A = "Clique aqui: {link}"
B = "Acesse agora: {link}"

# Teste 3: Emojis
A = "Oferta imperdível 🔥"
B = "Oferta imperdível"

# Teste 4: Tamanho
A = 150 caracteres
B = 300 caracteres
```

**Métricas de Sucesso:**

```
- Delivery rate (>90%)
- Read rate (>60%)
- Reply rate (>5%)
- Click rate (>10%)
- Conversion rate (>2%)
- Opt-out rate (<1%)
```

### Otimização de Engajamento

**Gatilhos de Engajamento:**

```
✅ Perguntas diretas
✅ Escolhas múltiplas (A, B, C)
✅ Feedback requests
✅ Urgência genuína
✅ Exclusividade
✅ Valor claro
✅ Social proof
```

**Evitar:**

```
❌ Mensagens muito longas (>400 chars)
❌ Múltiplos CTAs
❌ Linguagem técnica demais
❌ Pressão excessiva
❌ Promessas irreais
```

---

## 🚨 TROUBLESHOOTING

### Erro: "Phone not connected"

**Causa:** WhatsApp Web não está ativo

**Solução:**
```
1. Abra WhatsApp Web no navegador
2. Escaneie QR code
3. Mantenha aba aberta
4. Tente novamente
```

---

### Erro: "Database locked"

**Causa:** Múltiplas conexões simultâneas

**Solução:**
```bash
# Fechar processos
pkill -f group_monitor.py

# Ou usar timeout
sqlite3 whatsapp_data.db ".timeout 5000"
```

---

### Mensagens não enviando

**Possíveis Causas:**

1. **Rate limit atingido**
```python
# Verificar:
print(f"Enviadas na última hora: {len(sent_times)}")
# Deve ser < 40
```

2. **Horário inadequado**
```python
# Verificar:
hour = datetime.now().hour
if hour < 8 or hour > 22:
    print("Fora do horário permitido")
```

3. **Formato de telefone**
```python
# Correto:
"+5521999999999"  # Com + e código do país

# Errado:
"21999999999"     # Sem +
"5521999999999"   # Sem +
```

---

### Contatos duplicados

**Solução:**

```bash
python scripts/crm_sync.py --repair
```

Isso remove duplicatas e sincroniza members → contacts.

---

### Health score baixo

**Diagnóstico:**

```python
metrics = {
    'delivery_rate': 0.75,  # Baixo (<0.9)
    'blocks_received': 3,   # Alto (>0)
    'spam_reports': 1       # Crítico
}

score = calculate_health_score(metrics)  # Retorna < 50
```

**Ações:**

```
1. PARE campanhas imediatamente
2. Revise conteúdo das mensagens
3. Reduza volume em 50%
4. Aumente delays (15-30s)
5. Melhore personalização
6. Aguarde 48h antes de retomar
```

---

### Export Excel vazio

**Causa:** Sem dados no período

**Solução:**

```bash
# Aumentar período
python scripts/group_monitor.py --group "Nome" --days 30

# Verificar nome do grupo
python scripts/group_monitor.py --group "Nome Exato com Espaços"
```

---

## 📚 REFERÊNCIAS RÁPIDAS

### Segmentos Comuns

```python
# Atividade
"message_count > 100"           # Super ativos
"message_count BETWEEN 10 AND 50"  # Moderados
"message_count = 0"              # Silenciosos

# Tempo
"join_date > date('now', '-7 days')"     # Novos (7d)
"last_active < date('now', '-14 days')"  # Inativos (14d)
"join_date < date('now', '-90 days')"    # Veteranos

# Combinações
"message_count > 20 AND last_active < date('now', '-7 days')"  # Ativos em risco
"message_count > 0 AND message_count < 5"  # Engajamento inicial

# Ranking
"top_20_percent"     # Top 20%
"top_10"             # Top 10 absoluto
```

### Comandos Quick Reference

```bash
# MONITORAMENTO
python scripts/group_monitor.py --group "Nome" --days 7
python scripts/group_monitor.py --group "Nome" --export report.xlsx

# SEGMENTAÇÃO
python scripts/selective_outreach.py --segment new_members --dry-run
python scripts/selective_outreach.py --segment inactive_7_days --template engagement.json
python scripts/selective_outreach.py --segment top_20_percent --export vip.csv

# CRM
python scripts/crm_sync.py --export contacts.xlsx
python scripts/crm_sync.py --import contacts.xlsx --update
python scripts/crm_sync.py --tag "phone1,phone2" "vip"
python scripts/crm_sync.py --repair
python scripts/crm_sync.py --report crm_report.xlsx
```

### Rate Limits

```
Por hora:   40 mensagens (recomendado) / 50 (máximo)
Por dia:    200 mensagens (recomendado) / 256 (máximo)
Por semana: 800 mensagens (recomendado) / 1000 (máximo)
Delay:      5-15 segundos entre mensagens
Horário:    8h - 22h (nunca fora disso)
```

---

## 🎯 PRÓXIMOS PASSOS

**Setup Inicial (30 min):**
1. Instalar dependências
2. Inicializar banco de dados
3. Testar scripts com --dry-run
4. Ler compliance_guidelines.md

**Primeiro Dia:**
1. Monitorar 1 grupo por 24h
2. Analisar métricas geradas
3. Identificar segmentos interessantes
4. Criar 1-2 templates customizados

**Primeira Semana:**
1. Executar campanha small (10-20 pessoas)
2. Monitorar engagement
3. Ajustar templates baseado em feedback
4. Documentar learnings

**Escalando:**
1. Aumentar volume gradualmente (+20% por semana)
2. Implementar automações
3. A/B testing de templates
4. Otimizar timing baseado em dados

---

## ⚙️ CONFIGURAÇÃO ANTIGRAVITY

Para usar no Antigravity, copie todo este conteúdo Markdown e adicione como uma skill customizada. A skill está completa com:

✅ Documentação abrangente
✅ Workflows detalhados  
✅ Scripts prontos para implementar
✅ Templates de mensagens
✅ Compliance guidelines
✅ Casos de uso práticos
✅ Troubleshooting
✅ Quick reference

**Tamanho otimizado:** ~15k tokens (cabe confortavelmente no context window)

**Modular:** Seções independentes podem ser consultadas conforme necessário

**Prático:** Exemplos executáveis e código pronto para usar

---

**Versão:** 1.0  
**Última atualização:** 2024  
**Compatibilidade:** WhatsApp Business, WhatsApp Web, WhatsApp Business API