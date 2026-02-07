# WhatsApp Management Skill - Guia de Uso

## 📦 Conteúdo da Skill

Esta skill fornece um sistema completo de gestão de WhatsApp Business com:

### ✅ Funcionalidades Principais

1. **Monitoramento de Grupos**
   - Análise de atividade em tempo real
   - Métricas de engajamento
   - Identificação de membros silenciosos
   - Exportação para Excel/CSV

2. **Abordagem Seletiva**
   - Segmentação inteligente de membros
   - Campanhas personalizadas
   - Rate limiting automático
   - Templates prontos

3. **CRM Integrado**
   - Banco de dados de contatos
   - Histórico de interações
   - Sincronização com CRMs externos
   - Tags e campos customizados

4. **Conformidade & Compliance**
   - Anti-ban automático
   - Respeito às políticas WhatsApp
   - Opt-out management
   - Rate limiting inteligente

## 🚀 Instalação Rápida

### Passo 1: Instalar a Skill

Faça upload do arquivo `whatsapp-management.skill` no Claude.

### Passo 2: Instalar Dependências

```bash
pip install pandas openpyxl selenium whatsapp-web.py --break-system-packages
```

### Passo 3: Testar Instalação

```bash
# Verificar scripts
python scripts/group_monitor.py --help
python scripts/selective_outreach.py --help
python scripts/crm_sync.py --help
```

## 📖 Exemplos de Uso

### Exemplo 1: Monitorar Grupo

```bash
# Analisar últimos 7 dias
python scripts/group_monitor.py --group "Meu Grupo" --days 7

# Exportar para Excel
python scripts/group_monitor.py --group "Meu Grupo" --days 7 --export relatorio.xlsx
```

### Exemplo 2: Enviar Boas-Vindas

```bash
# Preview (dry-run)
python scripts/selective_outreach.py \
  --segment new_members \
  --template assets/templates/welcome.json \
  --dry-run

# Enviar para real
python scripts/selective_outreach.py \
  --segment new_members \
  --template assets/templates/welcome.json
```

### Exemplo 3: Reengajar Membros Inativos

```bash
python scripts/selective_outreach.py \
  --segment inactive_7_days \
  --template assets/templates/engagement.json
```

### Exemplo 4: Exportar Contatos para CRM

```bash
# Exportar todos os contatos
python scripts/crm_sync.py --export contatos.xlsx

# Importar contatos atualizados
python scripts/crm_sync.py --import contatos.xlsx --update
```

### Exemplo 5: Identificar Membros VIP

```bash
# Exportar top 20% mais ativos
python scripts/selective_outreach.py \
  --segment top_20_percent \
  --export vip_members.csv
```

## 🎯 Casos de Uso

### Caso 1: Comunidade de Flamengo
**Objetivo:** Engajar torcedores e monetizar

```bash
# 1. Monitorar atividade do grupo
python scripts/group_monitor.py --group "Nação Rubro-Negra" --days 30

# 2. Identificar super-fãs (>100 mensagens)
python scripts/selective_outreach.py --segment "message_count > 100"

# 3. Enviar oferta especial para VIPs
python scripts/selective_outreach.py \
  --segment "message_count > 100" \
  --template oferta_vip.json
```

### Caso 2: SDR / Vendas B2B
**Objetivo:** Nutrir leads e qualificar prospects

```bash
# 1. Sync contatos para CRM
python scripts/crm_sync.py --export leads.xlsx

# 2. Tag hot leads no CRM
# (editar leads.xlsx manualmente)

# 3. Importar tags de volta
python scripts/crm_sync.py --import leads.xlsx --update

# 4. Campanha para hot leads
python scripts/selective_outreach.py \
  --segment "tag = hot_lead" \
  --template follow_up_vendas.json
```

### Caso 3: Community Management
**Objetivo:** Manter engajamento alto

```bash
# 1. Identificar membros em risco (7+ dias sem mensagem)
python scripts/selective_outreach.py --segment inactive_7_days --export em_risco.csv

# 2. Campanha de reengajamento
python scripts/selective_outreach.py \
  --segment inactive_7_days \
  --template assets/templates/engagement.json

# 3. Monitorar resultados
python scripts/group_monitor.py --group "Community" --days 7
```

## ⚙️ Personalização

### Criar Novo Template

Copie `assets/templates/welcome.json` e edite:

```json
{
  "name": "Minha Campanha",
  "message": "Olá {name}! Sua mensagem aqui...",
  "options": {
    "rate_limit": 40
  }
}
```

### Criar Segmento Customizado

Use SQL-like queries:

```bash
# Membros que entraram em janeiro
--segment "join_date BETWEEN '2024-01-01' AND '2024-01-31'"

# Super ativos (>200 mensagens)
--segment "message_count > 200"

# Inativos há 2 semanas
--segment "last_active < date('now', '-14 days')"
```

## 📊 Estrutura do Banco de Dados

```sql
-- Membros
members (id, phone, name, join_date, last_active, message_count, status)

-- Mensagens
messages (id, member_id, group_name, content, timestamp, media_type)

-- Contatos CRM
contacts (id, phone, name, email, company, tags, lifecycle_stage)

-- Interações
interactions (id, contact_id, type, content, timestamp)

-- Campanhas
outreach (id, member_id, campaign_name, message, sent_at)
```

## 🛡️ Compliance & Segurança

**IMPORTANTE:** Sempre respeite os limites:

- ✅ Máximo 40 mensagens/hora
- ✅ Delay de 5-15 segundos entre mensagens
- ✅ Horário comercial (8h-22h)
- ✅ Incluir opt-out em broadcasts
- ✅ Personalizar cada mensagem

**Veja:** `references/compliance_guidelines.md` para regras completas

## 📚 Documentação Completa

### Scripts Principais

- `scripts/group_monitor.py` - Monitoramento e analytics
- `scripts/selective_outreach.py` - Campanhas direcionadas
- `scripts/crm_sync.py` - Sincronização CRM

### Referências

- `references/monitoring_config.md` - Configurações avançadas
- `references/campaign_templates.md` - Templates prontos
- `references/compliance_guidelines.md` - Anti-ban e políticas

### Assets

- `assets/templates/` - Templates de mensagens
- `assets/configs/` - Configurações pré-definidas

## 🆘 Solução de Problemas

**Erro: "Phone not connected"**
→ Verifique se WhatsApp Web está ativo

**Erro: "Database locked"**
→ Feche outras conexões ao banco

**Mensagens não enviando**
→ Verifique rate limits e horários

**Contatos duplicados**
→ Execute: `python scripts/crm_sync.py --repair`

## 💡 Próximos Passos

1. Leia `references/compliance_guidelines.md` (CRÍTICO!)
2. Teste com dry-run antes de enviar mensagens
3. Comece com volumes baixos (10-20 msgs/dia)
4. Monitore métricas de engajamento
5. Ajuste templates baseado em feedback

## 📧 Suporte

Para dúvidas ou melhorias, consulte:
- `SKILL.md` para workflows completos
- `references/` para documentação detalhada
- Templates em `assets/` como exemplos

---

**Desenvolvido para gestão profissional de WhatsApp Business**