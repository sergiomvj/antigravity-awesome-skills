# Paid Traffic Management Skill (Instagram & Facebook)

**Skill para gestão profissional de tráfego pago em Meta Ads (Instagram & Facebook)**

---

## 📋 METADADOS DA SKILL

```yaml
name: meta-ads-traffic-management
version: 1.0
description: Sistema completo de gestão de tráfego pago para Instagram e Facebook Ads, incluindo estruturação de campanhas, otimização de conversão, análise de métricas, automação de lances, creative testing e scaling estratégico
tags: meta-ads, facebook-ads, instagram-ads, paid-traffic, performance-marketing, roi, conversion-optimization
author: Custom Skill
```

---

## 🎯 QUANDO USAR ESTA SKILL

Use esta skill quando o usuário precisar:
- Estruturar campanhas de tráfego pago no Meta Ads (Facebook/Instagram)
- Otimizar ROI e ROAS de campanhas existentes
- Analisar métricas de performance e identificar gargalos
- Implementar estratégias de creative testing e scaling
- Automatizar gestão de lances e orçamentos
- Criar funis de conversão completos
- Desenvolver estratégias de retargeting avançadas
- Estruturar testes A/B de criativos, copies e públicos
- Calcular e otimizar CAC, LTV e margens de lucro

---

## 🚀 QUICK START

### Pré-requisitos

```bash
# Instalar dependências
pip install facebook-business pandas numpy matplotlib seaborn requests openpyxl --break-system-packages

# Para automação avançada
pip install python-dotenv schedule APScheduler --break-system-packages
```

### Setup Inicial - Meta Business Suite

```python
# 1. Obter credenciais da API
# - Acesse: business.facebook.com
# - Menu: Configurações de Negócios > Usuários do Sistema
# - Criar token de acesso com permissões: ads_management, ads_read, business_management

# 2. Configurar variáveis de ambiente
# Criar arquivo .env:
"""
META_ACCESS_TOKEN=seu_token_aqui
META_AD_ACCOUNT_ID=act_123456789
META_APP_ID=seu_app_id
META_APP_SECRET=seu_app_secret
"""

# 3. Testar conexão
python scripts/test_connection.py
```

---

## 📊 CORE WORKFLOWS

### Workflow 1: Estruturação de Campanha (BOF - Bottom of Funnel)

**Objetivo:** Criar campanha otimizada para conversão direta (vendas)

**Estrutura Recomendada:**

```
CAMPANHA: Vendas - Produto X - Jan/2024
├── CONJUNTO 1: Público Quente (Retargeting 30d)
│   ├── Anúncio 1A: Vídeo Testimonial
│   ├── Anúncio 1B: Carrossel Benefícios
│   └── Anúncio 1C: Estático Oferta
├── CONJUNTO 2: Lookalike 1% (Compradores)
│   ├── Anúncio 2A: Vídeo Produto em Uso
│   ├── Anúncio 2B: Carrossel Cases
│   └── Anúncio 2C: Estático Before/After
└── CONJUNTO 3: Interesses Quentes (Concorrentes)
    ├── Anúncio 3A: Vídeo Comparativo
    ├── Anúncio 3B: Carrossel Features
    └── Anúncio 3C: Estático Prova Social
```

**Implementação:**

```python
# Criar campanha BOF
python scripts/create_campaign.py \
  --objective CONVERSIONS \
  --name "Vendas - Produto X - Jan/2024" \
  --budget-daily 500 \
  --config configs/bof_campaign.json

# Template configs/bof_campaign.json
{
  "campaign": {
    "objective": "CONVERSIONS",
    "optimization_goal": "OFFSITE_CONVERSIONS",
    "bid_strategy": "LOWEST_COST_WITH_BID_CAP"
  },
  "ad_sets": [
    {
      "name": "Retargeting 30d",
      "audience": {
        "type": "custom",
        "days": 30,
        "event": "ViewContent"
      },
      "budget": 200,
      "bid_cap": 15.00
    },
    {
      "name": "Lookalike 1% Compradores",
      "audience": {
        "type": "lookalike",
        "source": "purchasers",
        "ratio": 0.01
      },
      "budget": 200,
      "bid_cap": 20.00
    },
    {
      "name": "Interesses Concorrentes",
      "audience": {
        "type": "interests",
        "interests": ["Produto Concorrente A", "Produto Concorrente B"]
      },
      "budget": 100,
      "bid_cap": 25.00
    }
  ]
}
```

---

### Workflow 2: Estruturação de Campanha (MOF - Middle of Funnel)

**Objetivo:** Nutrir leads e gerar engajamento

**Estrutura:**

```
CAMPANHA: Engajamento - Conteúdo - Jan/2024
├── CONJUNTO 1: Lookalike 2-3% (Engajados)
│   ├── Anúncio 1A: Vídeo Educativo
│   ├── Anúncio 1B: Carrossel Tutorial
│   └── Anúncio 1C: Estático Infográfico
├── CONJUNTO 2: Interesses Amplos
│   ├── Anúncio 2A: Vídeo Case Study
│   ├── Anúncio 2B: Carrossel Dicas
│   └── Anúncio 2C: Estático Quiz
└── CONJUNTO 3: Retargeting Engajados (7-30d)
    ├── Anúncio 3A: Vídeo Behind Scenes
    ├── Anúncio 3B: Carrossel FAQ
    └── Anúncio 3C: Estático Estatísticas
```

**Objetivos MOF:**

```python
objectives_mof = {
    "TRAFFIC": "Enviar para blog/conteúdo",
    "ENGAGEMENT": "Likes, comments, shares",
    "VIDEO_VIEWS": "ThruPlay views",
    "LEAD_GENERATION": "Formulários nativos",
    "MESSAGES": "Conversas no WhatsApp/Messenger"
}
```

---

### Workflow 3: Estruturação de Campanha (TOF - Top of Funnel)

**Objetivo:** Gerar awareness e capturar novos públicos

**Estrutura:**

```
CAMPANHA: Awareness - Marca - Jan/2024
├── CONJUNTO 1: Interesses Amplos (Broad)
│   ├── Anúncio 1A: Vídeo Viral/Entretenimento
│   ├── Anúncio 1B: Carrossel Curiosidades
│   └── Anúncio 1C: Estático Meme/Trending
├── CONJUNTO 2: Lookalike 5-10% (Fanpage)
│   ├── Anúncio 2A: Vídeo Brand Story
│   ├── Anúncio 2B: Carrossel Valores
│   └── Anúncio 2C: Estático Lifestyle
└── CONJUNTO 3: Teste Advantage+ (Automático)
    └── Anúncio 3A-F: 6 variações criativas
```

**Métricas TOF:**

```python
kpis_tof = {
    "CPM": "<R$20",
    "CPC": "<R$0.50",
    "CPV": "<R$0.05",
    "Reach": ">100k/mês",
    "Frequency": "2-3x",
    "ThruPlay": ">10k vídeos completos"
}
```

---

### Workflow 4: Otimização de Campanha Existente

**Passo a Passo:**

```python
# 1. Analisar performance atual
python scripts/analyze_campaign.py --campaign-id 123456789 --days 7

# Output esperado:
"""
📊 ANÁLISE DE PERFORMANCE - Últimos 7 dias

MÉTRICAS GERAIS:
- Gasto: R$ 3,500.00
- Receita: R$ 8,750.00
- ROAS: 2.5x
- Conversões: 35
- CPA: R$ 100.00

⚠️ ALERTAS:
- Conjunto "Interesses Amplos" com CPA alto (R$ 180)
- Anúncio "Vídeo A" com CTR baixo (0.3%)
- Frequência alta no Conjunto "Retargeting" (5.2x)

💡 RECOMENDAÇÕES:
1. Pausar Conjunto "Interesses Amplos"
2. Duplicar Anúncio "Carrossel B" (melhor performance)
3. Reduzir orçamento Conjunto "Retargeting" (-30%)
4. Criar novo público Lookalike 1% de Compradores
"""

# 2. Implementar otimizações automaticamente
python scripts/optimize_campaign.py \
  --campaign-id 123456789 \
  --auto-pause-cpa 150 \
  --auto-scale-roas 3.0 \
  --frequency-cap 4.0

# 3. Testar novos criativos
python scripts/creative_test.py \
  --campaign-id 123456789 \
  --test-type split \
  --variations 3 \
  --budget-per-variation 50
```

**Regras de Otimização:**

```python
optimization_rules = {
    "pause_rules": {
        "cpa_above": 150,      # Pausar se CPA > R$150
        "roas_below": 1.5,     # Pausar se ROAS < 1.5x
        "ctr_below": 0.5,      # Pausar se CTR < 0.5%
        "frequency_above": 5.0 # Pausar se frequência > 5x
    },
    "scale_rules": {
        "roas_above": 3.0,     # Aumentar budget +20% se ROAS > 3x
        "cpa_below": 80,       # Aumentar budget +30% se CPA < R$80
        "spend_threshold": 0.7 # Aumentar se gastou <70% do orçamento
    },
    "duplicate_rules": {
        "roas_above": 4.0,     # Duplicar ad set se ROAS > 4x
        "min_conversions": 10  # Mínimo 10 conversões para duplicar
    }
}
```

---

### Workflow 5: Creative Testing (Testes A/B)

**Metodologia de Teste:**

```
TESTE 1: Formato de Criativo
├── Variação A: Vídeo curto (15s)
├── Variação B: Vídeo longo (60s)
├── Variação C: Carrossel (5 cards)
├── Variação D: Imagem estática
└── Variação E: Coleção (produto)

TESTE 2: Copy/Hook
├── Variação A: Dor (problema)
├── Variação B: Desejo (aspiracional)
├── Variação C: Prova social (depoimentos)
├── Variação D: Urgência (escassez)
└── Variação E: Benefício direto

TESTE 3: Call-to-Action
├── Variação A: "Compre Agora"
├── Variação B: "Saiba Mais"
├── Variação C: "Garantir Desconto"
├── Variação D: "Aproveitar Oferta"
└── Variação E: "Quero Este Resultado"
```

**Implementação:**

```python
# Criar teste A/B estruturado
python scripts/ab_test.py \
  --campaign-id 123456789 \
  --test-variable creative_format \
  --variations 5 \
  --budget-per-variation 100 \
  --duration-days 5 \
  --significance-level 0.95

# Configuração de teste
{
  "test_config": {
    "hypothesis": "Vídeos curtos têm melhor CTR que estáticos",
    "metric": "CTR",
    "sample_size": 1000,  # impressões por variação
    "min_duration": 3,     # dias mínimos
    "max_duration": 7,     # dias máximos
    "early_stop": true,    # parar se significância atingida
    "winner_auto_scale": true  # escalar vencedor automaticamente
  }
}
```

**Análise Estatística:**

```python
# Analisar resultados do teste
python scripts/analyze_ab_test.py --test-id 987654321

# Output:
"""
📈 RESULTADOS DO TESTE A/B

VENCEDOR: Variação A (Vídeo 15s)
Confiança: 95% ✅

MÉTRICAS:
                 CTR      CPC     Conversões   CPA
Vídeo 15s       2.3%    R$0.45      23       R$95
Vídeo 60s       1.8%    R$0.52      18      R$115
Carrossel       1.5%    R$0.58      12      R$145
Estático        1.1%    R$0.65       8      R$175
Coleção         1.3%    R$0.60      10      R$160

RECOMENDAÇÃO:
✅ Pausar variações D e E (performance fraca)
✅ Escalar variação A (+50% budget)
✅ Manter B e C para diversificação
✅ Criar novas variações baseadas no padrão vencedor
"""
```

---

### Workflow 6: Scaling (Escalonamento Estratégico)

**Estratégias de Scaling:**

**1. Vertical Scaling (Aumentar orçamento)**

```python
# Scaling gradual (recomendado)
scaling_strategy = {
    "method": "gradual",
    "increase_rate": 0.20,  # +20% a cada 3 dias
    "frequency": 3,          # dias
    "max_increase": 3.0,     # até 3x do orçamento inicial
    "conditions": {
        "min_roas": 2.5,
        "min_conversions": 15,
        "max_cpa": 120
    }
}

# Implementar scaling
python scripts/scale_campaign.py \
  --campaign-id 123456789 \
  --strategy gradual \
  --increase 20 \
  --check-metrics

# Exemplo de scaling timeline:
"""
Dia 1-3:  R$100/dia → ROAS 3.2x → ✅ Aumentar
Dia 4-6:  R$120/dia → ROAS 2.8x → ✅ Aumentar
Dia 7-9:  R$144/dia → ROAS 2.6x → ✅ Aumentar
Dia 10-12: R$173/dia → ROAS 2.3x → ⚠️ Monitorar
Dia 13-15: R$173/dia → ROAS 2.1x → ❌ Manter
"""
```

**2. Horizontal Scaling (Duplicar conjuntos)**

```python
# Duplicar ad sets vencedores
python scripts/duplicate_adset.py \
  --adset-id 456789123 \
  --duplicates 3 \
  --variations audience  # ou: creative, placement, schedule

# Estratégia de duplicação:
"""
Ad Set Original: Lookalike 1% Brasil
├── Duplicata 1: Lookalike 1% SP/RJ (geo)
├── Duplicata 2: Lookalike 1% 25-45 anos (idade)
└── Duplicata 3: Lookalike 1% Mobile only (device)
"""
```

**3. CBO Scaling (Campaign Budget Optimization)**

```python
# Migrar para CBO
python scripts/convert_to_cbo.py \
  --campaign-id 123456789 \
  --total-budget 1000 \
  --min-spend-per-adset 50

# Vantagens CBO:
"""
✅ Meta distribui budget automaticamente
✅ Foca em ad sets com melhor performance
✅ Reduz micromanagement
✅ Melhora learning phase
⚠️ Menos controle granular
⚠️ Pode concentrar muito em 1-2 ad sets
"""
```

---

### Workflow 7: Retargeting Avançado

**Estrutura de Funil Completo:**

```
FUNIL DE RETARGETING:

TOF (0-7 dias)
├── Visitaram site mas não engajaram
└── Objetivo: Engagement / Video Views

MOF (7-30 dias)
├── Visitaram produto mas não adicionaram carrinho
├── Adicionaram carrinho mas não iniciaram checkout
└── Objetivo: Add to Cart / Initiate Checkout

BOF (30-90 dias)
├── Iniciaram checkout mas não compraram
├── Compradores (upsell/cross-sell)
└── Objetivo: Purchase / Repeat Purchase

WINBACK (90-180 dias)
├── Clientes inativos há 90+ dias
└── Objetivo: Re-engagement / Win-back
```

**Implementação de Públicos:**

```python
# Criar públicos de retargeting
audiences = {
    "hot_audience_3d": {
        "name": "Visitantes Site - 3 dias",
        "retention": 3,
        "events": ["ViewContent", "AddToCart"],
        "exclusions": ["Purchase"]
    },
    "warm_audience_7d": {
        "name": "Engajados Conteúdo - 7 dias",
        "retention": 7,
        "events": ["VideoView", "PageLike", "PostEngagement"],
        "exclusions": ["ViewContent"]
    },
    "cart_abandoners_14d": {
        "name": "Abandonaram Carrinho - 14 dias",
        "retention": 14,
        "events": ["AddToCart", "InitiateCheckout"],
        "exclusions": ["Purchase"]
    },
    "checkout_abandoners_30d": {
        "name": "Abandonaram Checkout - 30 dias",
        "retention": 30,
        "events": ["InitiateCheckout"],
        "exclusions": ["Purchase"]
    },
    "purchasers_90d": {
        "name": "Compradores - 90 dias",
        "retention": 90,
        "events": ["Purchase"],
        "min_value": 50.00  # AOV mínimo
    }
}

# Criar públicos automaticamente
python scripts/create_audiences.py --config retargeting_audiences.json
```

**Sequência de Anúncios (Dynamic Remarketing):**

```python
# Configurar catálogo de produtos
catalog_config = {
    "product_feed_url": "https://seusite.com/feed.xml",
    "update_frequency": "daily",
    "dynamic_ads": {
        "template_1": "Você viu: {{product.name}} - Ainda interessado?",
        "template_2": "{{product.name}} está com desconto! Aproveite agora!",
        "template_3": "Complete sua compra: {{product.name}} + frete grátis"
    },
    "audiences": {
        "viewed_not_added": {
            "events": ["ViewContent"],
            "exclusions": ["AddToCart"],
            "template": "template_1"
        },
        "added_not_purchased": {
            "events": ["AddToCart"],
            "exclusions": ["Purchase"],
            "template": "template_2"
        },
        "checkout_abandoners": {
            "events": ["InitiateCheckout"],
            "exclusions": ["Purchase"],
            "template": "template_3"
        }
    }
}
```

---

## 📐 ESTRUTURAS DE CAMPANHA PRÉ-CONFIGURADAS

### Estrutura 1: E-commerce (Loja Online)

```
CAMPANHA MESTRE: E-commerce - Loja X

├── CAMPANHA 1: TOF - Awareness
│   ├── Ad Set: Interesses Amplos (Broad)
│   ├── Ad Set: Lookalike 5-10% (All Users)
│   └── Budget: R$300/dia - Objetivo: Reach
│
├── CAMPANHA 2: MOF - Consideração
│   ├── Ad Set: Retargeting 1-7d (Site Visitors)
│   ├── Ad Set: Lookalike 2-3% (Engagers)
│   ├── Ad Set: Engajados Instagram (90d)
│   └── Budget: R$400/dia - Objetivo: Traffic
│
├── CAMPANHA 3: BOF - Conversão
│   ├── Ad Set: Cart Abandoners (14d)
│   ├── Ad Set: Checkout Abandoners (30d)
│   ├── Ad Set: Lookalike 1% (Purchasers)
│   └── Budget: R$800/dia - Objetivo: Conversions
│
└── CAMPANHA 4: LOYALTY - Pós-Venda
    ├── Ad Set: Buyers 30-90d (Upsell)
    ├── Ad Set: Buyers 90-180d (Winback)
    └── Budget: R$200/dia - Objetivo: Conversions
```

**Budget Total:** R$1,700/dia (R$51k/mês)

**Distribuição Recomendada:**
- TOF: 18% (R$300)
- MOF: 23% (R$400)
- BOF: 47% (R$800)
- LOYALTY: 12% (R$200)

---

### Estrutura 2: Infoprodutos (Curso/Mentoria)

```
CAMPANHA MESTRE: Infoproduto - Curso Y

├── CAMPANHA 1: Lead Magnet (Isca Digital)
│   ├── Ad Set: Interesses Específicos
│   ├── Ad Set: Lookalike 3% (Leads)
│   └── Budget: R$200/dia - Objetivo: Lead Generation
│
├── CAMPANHA 2: VSL (Video Sales Letter)
│   ├── Ad Set: Leads 0-3d (Quentes)
│   ├── Ad Set: Engajados Vídeo (7d)
│   └── Budget: R$300/dia - Objetivo: Video Views
│
├── CAMPANHA 3: Webinar/Masterclass
│   ├── Ad Set: Leads 3-7d (Nutridos)
│   ├── Ad Set: Watched VSL >75%
│   └── Budget: R$400/dia - Objetivo: Conversions (Registration)
│
└── CAMPANHA 4: Carrinho Aberto
    ├── Ad Set: Webinar Attendees (não compraram)
    ├── Ad Set: Página de Vendas (não compraram)
    └── Budget: R$600/dia - Objetivo: Conversions (Purchase)
```

**Budget Total:** R$1,500/dia (R$45k/mês)

**Funil típico:**
```
1000 Leads → 300 VSL Views → 100 Webinar → 15 Vendas
CPL: R$5 → CPV: R$3 → CPR: R$20 → CPA: R$400
```

---

### Estrutura 3: Serviços Locais (Restaurante, Salão, etc)

```
CAMPANHA MESTRE: Serviço Local - Restaurante Z

├── CAMPANHA 1: Awareness Local
│   ├── Ad Set: Raio 5km (Moradores)
│   ├── Ad Set: Raio 10km (Trabalhadores)
│   └── Budget: R$100/dia - Objetivo: Reach
│
├── CAMPANHA 2: Tráfego para Instagram
│   ├── Ad Set: Engajados Concorrentes
│   ├── Ad Set: Interesses Gastronomia
│   └── Budget: R$150/dia - Objetivo: Profile Visits
│
└── CAMPANHA 3: Conversão (WhatsApp/Reserva)
    ├── Ad Set: Retargeting Perfil (7d)
    ├── Ad Set: Salvaram Post (14d)
    └── Budget: R$250/dia - Objetivo: Messages
```

**Budget Total:** R$500/dia (R$15k/mês)

---

### Estrutura 4: SaaS / Software

```
CAMPANHA MESTRE: SaaS - Ferramenta W

├── CAMPANHA 1: Content Marketing
│   ├── Ad Set: Interesses Profissionais
│   ├── Ad Set: Lookalike Website Visitors
│   └── Budget: R$300/dia - Objetivo: Traffic (Blog)
│
├── CAMPANHA 2: Free Trial
│   ├── Ad Set: Retargeting Blog Readers
│   ├── Ad Set: Lookalike Trial Users
│   ├── Ad Set: Concorrentes
│   └── Budget: R$600/dia - Objetivo: Conversions (Sign Up)
│
└── CAMPANHA 3: Paid Conversion
    ├── Ad Set: Trial Users (não converteram)
    ├── Ad Set: Demo Requesters
    └── Budget: R$400/dia - Objetivo: Conversions (Purchase)
```

**Budget Total:** R$1,300/dia (R$39k/mês)

**Métricas SaaS:**
```
Trial CPL: R$15-30
Paid Conversion Rate: 15-25%
CPA (Paid User): R$100-200
LTV: R$1,200 (12 meses)
LTV/CAC: 6-12x
```

---

## 🎨 CREATIVE GUIDELINES

### Formatos de Alta Performance

**1. Vídeo Curto (15-30s)**
```
Estrutura:
[0-3s]   Hook visual impactante
[3-10s]  Problema ou dor
[10-20s] Solução (produto)
[20-30s] CTA claro

Specs:
- Resolução: 1080x1080 (quadrado) ou 1080x1920 (vertical)
- Formato: MP4
- Tamanho: <30MB
- Legendas: Obrigatórias (85% assistem sem som)
- Logo: Primeiros 3 segundos
```

**2. Carrossel (3-10 cards)**
```
Card 1: Hook (problema/atenção)
Card 2-4: Benefícios/Features
Card 5-7: Prova social (antes/depois)
Card 8-9: Oferta/Preço
Card 10: CTA + link

Specs:
- Resolução: 1080x1080
- Formato: JPG ou PNG
- Texto: <20% da imagem
- Cards ideais: 5-7
```

**3. Estático (Imagem única)**
```
Elementos:
- Visual forte (rosto, produto, resultado)
- Texto mínimo (máx 5 palavras)
- Cores contrastantes
- CTA visível

Specs:
- Resolução: 1200x1200
- Formato: JPG (menor) ou PNG (qualidade)
- Tamanho: <1MB
- Texto: <20% da imagem (regra antiga, mas ainda válida)
```

**4. Coleção (Product Catalog)**
```
Uso: E-commerce com catálogo
- Imagem principal (cover)
- 4 produtos em grid
- Tag de preço automática
- Link direto para produto

Ideal para:
- Lojas com múltiplos produtos
- Dynamic remarketing
- Catálogo já configurado
```

---

### Copywriting de Alta Conversão

**Framework AIDA:**

```
A - ATENÇÃO (Hook)
"🔥 PARE DE PERDER DINHEIRO com tráfego pago!"

I - INTERESSE (Problema)
"A maioria dos empreendedores queima R$10k+ em ads sem resultado porque não sabem estruturar campanhas corretamente."

D - DESEJO (Solução)
"Aprenda o método exato que usei para gerar R$2.3M em vendas com apenas R$180k investidos em Meta Ads."

A - AÇÃO (CTA)
"👉 Clique e garanta sua vaga na Masterclass GRATUITA"
```

**Framework PAS:**

```
P - PROBLEMA
"Gastando muito e vendendo pouco no Instagram?"

A - AGITAÇÃO
"Enquanto você lê isso, seus concorrentes estão dominando seu público e fazendo vendas que deveriam ser suas. A cada dia que passa, você perde mais market share."

S - SOLUÇÃO
"Sistema completo de tráfego pago que já gerou +R$50M em vendas para nossos clientes."
```

**Framework BAB (Before-After-Bridge):**

```
B - ANTES
"Você está cansado de investir em ads e não ver resultado?"

A - DEPOIS
"Imagine acordar com notificações de vendas todos os dias, sabendo que cada R$1 investido retorna R$5."

B - PONTE
"Com nossa metodologia de tráfego pago, isso é possível. Clique e descubra como."
```

**Hooks de Alta Performance:**

```
❌ Dor/Problema:
"PARE de desperdiçar dinheiro em tráfego pago"
"Sua campanha NÃO converte? Descubra o motivo"
"Ads caros e vendas baixas? Você está fazendo ERRADO"

✅ Benefício/Resultado:
"Como gerei R$100k em 30 dias com apenas R$3k em ads"
"De R$0 a R$50k/mês com tráfego pago (passo a passo)"
"ROAS 8x: O segredo que ninguém te conta"

🎯 Curiosidade:
"O erro nº1 que mata 90% das campanhas"
"Por que suas ads não vendem (não é o que você pensa)"
"Meta Ads mudou TUDO em 2024. Saiba o que fazer"

⏰ Urgência:
"ÚLTIMAS HORAS: Método de tráfego pago com 60% OFF"
"Apenas 5 vagas: Consultoria de Meta Ads GRATUITA"
"ATENÇÃO: Esse método para de funcionar em 72h"

📊 Prova Social:
"Como ela gerou R$280k com um produto de R$97"
"+1.200 alunos já lucraram com esse método"
"Case: De falido a R$40k/mês em 60 dias"
```

---

### Creative Testing - O que testar

**Variáveis de Teste:**

```python
test_variables = {
    "formato": ["video_15s", "video_60s", "carrossel", "estatico", "colecao"],
    "hook": ["dor", "beneficio", "curiosidade", "urgencia", "prova_social"],
    "angulo": ["economizar_tempo", "ganhar_dinheiro", "status", "transformacao"],
    "visual": ["rosto", "produto", "lifestyle", "before_after", "infografico"],
    "cta": ["compre_agora", "saiba_mais", "garantir_desconto", "quero_resultado"],
    "tom": ["urgente", "educativo", "casual", "profissional", "humoristico"],
    "target": ["homem", "mulher", "jovem", "adulto", "empreendedor"]
}
```

**Matriz de Teste (exemplo):**

```
TESTE: Qual formato + hook converte melhor?

         │ Dor      │ Benefício │ Curiosidade │
─────────┼──────────┼───────────┼─────────────┤
Vídeo 15s│ Ad 1A    │ Ad 1B     │ Ad 1C       │
Carrossel│ Ad 2A    │ Ad 2B     │ Ad 2C       │
Estático │ Ad 3A    │ Ad 3B     │ Ad 3C       │

Budget: R$50 por célula x 9 = R$450 total
Duração: 5 dias mínimo
Métrica: CTR + CPA
```

---

## 📈 MÉTRICAS E KPIs

### Métricas por Objetivo de Campanha

**TOF (Awareness):**
```python
kpis_tof = {
    "CPM": "R$15-30",           # Custo por mil impressões
    "Reach": ">50k/semana",     # Alcance único
    "Frequency": "2-3x",        # Frequência ideal
    "CPV": "R$0.03-0.08",       # Custo por visualização (vídeo)
    "ThruPlay": ">5k",          # Vídeos assistidos completos
    "CPC": "R$0.30-0.80",       # Custo por clique
    "CTR": ">1%"                # Taxa de clique
}
```

**MOF (Consideração):**
```python
kpis_mof = {
    "CPC": "R$0.50-1.50",       # Custo por clique
    "CTR": ">1.5%",             # Taxa de clique
    "CPL": "R$5-25",            # Custo por lead
    "Landing_CR": ">20%",       # Taxa conversão landing page
    "CPE": "R$0.10-0.30",       # Custo por engajamento
    "Video_Views_75%": ">2k"    # Views de 75%+ do vídeo
}
```

**BOF (Conversão):**
```python
kpis_bof = {
    "CPA": "R$50-200",          # Custo por aquisição (depende do AOV)
    "ROAS": ">2.5x",            # Return on Ad Spend
    "CTR": ">2%",               # Taxa de clique
    "CR": ">3%",                # Taxa de conversão
    "AOV": ">R$150",            # Valor médio do pedido
    "Frequency": "<3x",         # Frequência (retargeting)
    "CPC": "R$1-3"              # Custo por clique
}
```

### Benchmarks por Nicho

```python
benchmarks = {
    "ecommerce": {
        "CPC": "R$0.80-1.50",
        "CPA": "R$80-150",
        "ROAS": "2.5-4x",
        "CTR": "1.5-3%",
        "AOV": "R$120-250"
    },
    "infoproduto": {
        "CPL": "R$3-15",
        "CPA": "R$200-500",
        "ROAS": "3-8x",
        "CTR": "2-5%",
        "Conversion_Rate": "2-8%"
    },
    "servicos_locais": {
        "CPC": "R$0.50-1.20",
        "CPL": "R$10-40",
        "CTR": "2-4%",
        "Cost_per_Message": "R$5-20"
    },
    "saas": {
        "CPL": "R$15-60",
        "Trial_CR": "20-40%",
        "Trial_to_Paid": "10-25%",
        "CPA": "R$100-300",
        "LTV": "R$800-2000"
    },
    "servicos_alto_ticket": {
        "CPL": "R$20-80",
        "SQL_CR": "10-30%",
        "CPA": "R$500-2000",
        "AOV": "R$3000-15000",
        "LTV": "R$5000-30000"
    }
}
```

### Cálculos Essenciais

**1. ROAS (Return on Ad Spend)**
```python
def calculate_roas(revenue, ad_spend):
    """
    ROAS = Receita / Gasto com Ads
    
    Exemplo:
    - Gasto: R$1,000
    - Receita: R$3,500
    - ROAS: 3.5x (para cada R$1 gasto, retornam R$3.50)
    """
    return revenue / ad_spend

# Benchmarks:
# - ROAS < 2x: Prejuízo (considerar margens)
# - ROAS 2-3x: Break-even / Lucro baixo
# - ROAS 3-5x: Saudável
# - ROAS > 5x: Excelente (escalar!)
```

**2. CPA (Cost Per Acquisition)**
```python
def calculate_cpa(ad_spend, conversions):
    """
    CPA = Gasto com Ads / Número de Conversões
    
    Exemplo:
    - Gasto: R$1,000
    - Conversões: 25
    - CPA: R$40
    """
    return ad_spend / conversions if conversions > 0 else float('inf')

# Comparar com:
# - Margem de lucro por produto
# - LTV do cliente
# - CPA dos concorrentes
```

**3. LTV (Lifetime Value)**
```python
def calculate_ltv(avg_purchase_value, purchase_frequency, customer_lifespan):
    """
    LTV = Ticket Médio × Frequência de Compra × Tempo de Vida
    
    Exemplo e-commerce:
    - Ticket médio: R$150
    - Compras/ano: 3
    - Tempo de vida: 2 anos
    - LTV: R$150 × 3 × 2 = R$900
    """
    return avg_purchase_value * purchase_frequency * customer_lifespan

# Regra de ouro:
# LTV / CPA > 3x (mínimo aceitável)
# LTV / CPA > 5x (saudável)
# LTV / CPA > 10x (excelente)
```

**4. Break-even ROAS**
```python
def calculate_breakeven_roas(margin_percentage):
    """
    ROAS Break-even = 1 / Margem
    
    Exemplo:
    - Margem: 40% (0.40)
    - Break-even ROAS: 1 / 0.40 = 2.5x
    
    Interpretação:
    Precisa de ROAS > 2.5x para lucrar
    """
    return 1 / margin_percentage

# Exemplos por margem:
# 20% → 5.0x ROAS necessário
# 30% → 3.3x ROAS necessário
# 40% → 2.5x ROAS necessário
# 50% → 2.0x ROAS necessário
```

**5. CTR (Click-Through Rate)**
```python
def calculate_ctr(clicks, impressions):
    """
    CTR = (Cliques / Impressões) × 100
    
    Exemplo:
    - Impressões: 100,000
    - Cliques: 2,000
    - CTR: 2%
    """
    return (clicks / impressions) * 100 if impressions > 0 else 0

# Benchmarks:
# CTR < 1%: Criativo fraco, público errado
# CTR 1-2%: Médio, precisa melhorar
# CTR 2-3%: Bom
# CTR > 3%: Excelente
```

**6. Conversion Rate**
```python
def calculate_conversion_rate(conversions, clicks):
    """
    CR = (Conversões / Cliques) × 100
    
    Exemplo:
    - Cliques: 1,000
    - Conversões: 35
    - CR: 3.5%
    """
    return (conversions / clicks) * 100 if clicks > 0 else 0

# Benchmarks (Landing Page):
# CR < 2%: Landing page ruim
# CR 2-5%: Médio
# CR 5-10%: Bom
# CR > 10%: Excelente
```

---

## 🔧 SCRIPTS E AUTOMAÇÕES

### Script 1: Análise de Performance

```python
#!/usr/bin/env python3
"""
Meta Ads Performance Analyzer
Analisa performance de campanhas e identifica oportunidades
"""

import os
from datetime import datetime, timedelta
from facebook_business.api import FacebookAdsApi
from facebook_business.adobjects.adaccount import AdAccount
from facebook_business.adobjects.campaign import Campaign
import pandas as pd

class MetaAdsAnalyzer:
    def __init__(self, access_token, ad_account_id):
        FacebookAdsApi.init(access_token=access_token)
        self.account = AdAccount(f'act_{ad_account_id}')
    
    def get_campaign_insights(self, campaign_id, days=7):
        """Buscar insights de campanha"""
        date_from = (datetime.now() - timedelta(days=days)).strftime('%Y-%m-%d')
        date_to = datetime.now().strftime('%Y-%m-%d')
        
        params = {
            'time_range': {'since': date_from, 'until': date_to},
            'level': 'campaign',
            'fields': [
                'campaign_name',
                'spend',
                'impressions',
                'clicks',
                'cpc',
                'cpm',
                'ctr',
                'conversions',
                'cost_per_conversion',
                'frequency'
            ]
        }
        
        insights = self.account.get_insights(params=params)
        return insights
    
    def analyze_performance(self, insights):
        """Analisar performance e gerar recomendações"""
        analysis = {
            'metrics': {},
            'alerts': [],
            'recommendations': []
        }
        
        for insight in insights:
            data = dict(insight)
            
            # Métricas básicas
            spend = float(data.get('spend', 0))
            conversions = int(data.get('conversions', 0))
            cpa = float(data.get('cost_per_conversion', 0))
            ctr = float(data.get('ctr', 0))
            frequency = float(data.get('frequency', 0))
            
            analysis['metrics'] = {
                'spend': spend,
                'conversions': conversions,
                'cpa': cpa,
                'ctr': ctr,
                'frequency': frequency
            }
            
            # Alertas
            if cpa > 150:
                analysis['alerts'].append(f"⚠️ CPA alto: R${cpa:.2f}")
                analysis['recommendations'].append("Revisar segmentação e criativos")
            
            if ctr < 1.0:
                analysis['alerts'].append(f"⚠️ CTR baixo: {ctr:.2f}%")
                analysis['recommendations'].append("Testar novos criativos e copies")
            
            if frequency > 4.0:
                analysis['alerts'].append(f"⚠️ Frequência alta: {frequency:.1f}x")
                analysis['recommendations'].append("Expandir público ou pausar campanha")
            
            if conversions < 10:
                analysis['alerts'].append(f"⚠️ Poucas conversões: {conversions}")
                analysis['recommendations'].append("Aguardar mais dados ou aumentar budget")
        
        return analysis
    
    def export_report(self, analysis, output_file):
        """Exportar relatório para Excel"""
        df_metrics = pd.DataFrame([analysis['metrics']])
        df_alerts = pd.DataFrame({'Alertas': analysis['alerts']})
        df_recs = pd.DataFrame({'Recomendações': analysis['recommendations']})
        
        with pd.ExcelWriter(output_file, engine='openpyxl') as writer:
            df_metrics.to_excel(writer, sheet_name='Métricas', index=False)
            df_alerts.to_excel(writer, sheet_name='Alertas', index=False)
            df_recs.to_excel(writer, sheet_name='Recomendações', index=False)
        
        print(f"✅ Relatório exportado: {output_file}")

# Uso:
# analyzer = MetaAdsAnalyzer(access_token, ad_account_id)
# insights = analyzer.get_campaign_insights(campaign_id, days=7)
# analysis = analyzer.analyze_performance(insights)
# analyzer.export_report(analysis, 'campaign_report.xlsx')
```

---

### Script 2: Otimização Automática

```python
#!/usr/bin/env python3
"""
Meta Ads Auto-Optimizer
Otimiza campanhas automaticamente baseado em regras
"""

from facebook_business.api import FacebookAdsApi
from facebook_business.adobjects.adset import AdSet
from facebook_business.adobjects.ad import Ad

class AutoOptimizer:
    def __init__(self, access_token):
        FacebookAdsApi.init(access_token=access_token)
        self.rules = {
            'pause_high_cpa': {'threshold': 150, 'action': 'pause'},
            'pause_low_ctr': {'threshold': 0.5, 'action': 'pause'},
            'pause_high_freq': {'threshold': 5.0, 'action': 'pause'},
            'scale_high_roas': {'threshold': 3.5, 'action': 'scale', 'increase': 0.20},
            'duplicate_winner': {'min_roas': 4.0, 'min_conversions': 15}
        }
    
    def check_and_apply_rules(self, adset_id, metrics):
        """Verificar métricas e aplicar regras"""
        actions_taken = []
        
        adset = AdSet(adset_id)
        
        # Regra: Pausar CPA alto
        if metrics.get('cpa', 0) > self.rules['pause_high_cpa']['threshold']:
            adset.api_update(params={'status': 'PAUSED'})
            actions_taken.append(f"⏸️ Pausado: CPA alto (R${metrics['cpa']:.2f})")
        
        # Regra: Pausar CTR baixo
        if metrics.get('ctr', 0) < self.rules['pause_low_ctr']['threshold']:
            adset.api_update(params={'status': 'PAUSED'})
            actions_taken.append(f"⏸️ Pausado: CTR baixo ({metrics['ctr']:.2f}%)")
        
        # Regra: Pausar frequência alta
        if metrics.get('frequency', 0) > self.rules['pause_high_freq']['threshold']:
            adset.api_update(params={'status': 'PAUSED'})
            actions_taken.append(f"⏸️ Pausado: Frequência alta ({metrics['frequency']:.1f}x)")
        
        # Regra: Escalar ROAS alto
        if metrics.get('roas', 0) > self.rules['scale_high_roas']['threshold']:
            current_budget = float(adset[AdSet.Field.daily_budget])
            new_budget = current_budget * (1 + self.rules['scale_high_roas']['increase'])
            adset.api_update(params={'daily_budget': new_budget})
            actions_taken.append(f"📈 Escalado: +20% budget (ROAS {metrics['roas']:.1f}x)")
        
        # Regra: Duplicar vencedor
        if (metrics.get('roas', 0) > self.rules['duplicate_winner']['min_roas'] and
            metrics.get('conversions', 0) >= self.rules['duplicate_winner']['min_conversions']):
            # Duplicar ad set (código simplificado)
            actions_taken.append(f"📋 Ad Set duplicado (ROAS {metrics['roas']:.1f}x)")
        
        return actions_taken
    
    def run_optimization(self, campaign_id):
        """Executar otimização em toda campanha"""
        print(f"\n🔧 Iniciando otimização automática...")
        
        # Buscar ad sets da campanha
        # Analisar métricas de cada ad set
        # Aplicar regras
        # Retornar resumo
        
        summary = {
            'paused': 0,
            'scaled': 0,
            'duplicated': 0
        }
        
        return summary

# Uso:
# optimizer = AutoOptimizer(access_token)
# summary = optimizer.run_optimization(campaign_id)
```

---

### Script 3: Creative Testing Automático

```python
#!/usr/bin/env python3
"""
A/B Test Manager
Gerencia testes A/B de criativos automaticamente
"""

import pandas as pd
from scipy import stats

class ABTestManager:
    def __init__(self, significance_level=0.95):
        self.significance_level = significance_level
        self.min_sample_size = 1000  # impressões mínimas
    
    def create_test(self, campaign_id, variations, budget_per_variation):
        """Criar teste A/B com múltiplas variações"""
        test_config = {
            'test_id': f"test_{campaign_id}_{int(time.time())}",
            'campaign_id': campaign_id,
            'variations': variations,
            'budget_per_variation': budget_per_variation,
            'status': 'running',
            'created_at': datetime.now().isoformat()
        }
        
        # Criar ad sets para cada variação
        # Distribuir budget igualmente
        # Configurar mesmos parâmetros exceto criativo
        
        return test_config
    
    def analyze_test(self, test_id):
        """Analisar resultados estatísticos do teste"""
        # Buscar dados de cada variação
        variations_data = self.get_variations_data(test_id)
        
        results = {
            'winner': None,
            'confidence': 0,
            'significant': False,
            'metrics_comparison': []
        }
        
        # Análise estatística (teste t)
        if len(variations_data) >= 2:
            var_a = variations_data[0]
            var_b = variations_data[1]
            
            # Teste t para CTR
            t_stat, p_value = stats.ttest_ind(
                var_a['ctr_samples'],
                var_b['ctr_samples']
            )
            
            confidence = 1 - p_value
            results['confidence'] = confidence
            results['significant'] = confidence >= self.significance_level
            
            # Determinar vencedor
            if results['significant']:
                if var_a['ctr_mean'] > var_b['ctr_mean']:
                    results['winner'] = var_a['name']
                else:
                    results['winner'] = var_b['name']
        
        return results
    
    def auto_scale_winner(self, test_id, scale_factor=1.5):
        """Escalar automaticamente variação vencedora"""
        results = self.analyze_test(test_id)
        
        if results['significant'] and results['winner']:
            winner_adset = self.get_winner_adset(test_id, results['winner'])
            
            # Aumentar budget do vencedor
            current_budget = float(winner_adset['daily_budget'])
            new_budget = current_budget * scale_factor
            
            # Pausar perdedores
            losers = self.get_loser_adsets(test_id, results['winner'])
            for loser in losers:
                loser.api_update(params={'status': 'PAUSED'})
            
            print(f"✅ Teste concluído!")
            print(f"🏆 Vencedor: {results['winner']}")
            print(f"📊 Confiança: {results['confidence']*100:.1f}%")
            print(f"📈 Budget escalado: R${current_budget:.2f} → R${new_budget:.2f}")
        else:
            print(f"⏳ Teste ainda sem significância estatística")
            print(f"📊 Confiança atual: {results['confidence']*100:.1f}%")

# Uso:
# test_manager = ABTestManager(significance_level=0.95)
# test_config = test_manager.create_test(campaign_id, variations, budget=100)
# results = test_manager.analyze_test(test_id)
# test_manager.auto_scale_winner(test_id, scale_factor=2.0)
```

---

## 💰 ESTRATÉGIAS DE BUDGET

### Distribuição de Budget por Funil

**Budget Total: R$10,000/mês**

```
TOF (20%): R$2,000
├── Awareness
├── Reach
└── Video Views

MOF (30%): R$3,000
├── Traffic
├── Engagement
└── Lead Generation

BOF (45%): R$4,500
├── Conversions
├── Catalog Sales
└── Store Visits

RETENTION (5%): R$500
├── Upsell/Cross-sell
└── Win-back
```

### Scaling Progressivo

**Semana 1: Teste (Budget: R$500)**
```
Objetivo: Validar hipóteses
- 3-5 ad sets
- 3 ads por ad set
- Budget: R$50-100/ad set
- Duração: 5-7 dias
```

**Semana 2-3: Otimização (Budget: R$1,000)**
```
Objetivo: Refinar vencedores
- Pausar perdedores
- Duplicar vencedores
- Testar variações
- Budget: +100% nos winners
```

**Semana 4+: Scaling (Budget: R$2,000+)**
```
Objetivo: Maximizar lucro
- Scaling vertical (+20-30% a cada 3 dias)
- Scaling horizontal (duplicar ad sets)
- Novos públicos lookalike
- Budget: Crescimento gradual
```

### Regras de Budget Diário

```python
budget_rules = {
    "minimum_per_adset": 50,     # R$50/dia mínimo
    "maximum_increase": 0.30,    # Máximo +30% por vez
    "testing_phase": {
        "duration_days": 7,
        "min_conversions": 10,   # Mínimo para validar
        "budget_cap": 100        # Máximo no teste
    },
    "scaling_phase": {
        "min_roas": 2.5,         # ROAS mínimo para escalar
        "increase_frequency": 3,  # Dias entre aumentos
        "max_daily_budget": 1000 # Limite diário por ad set
    }
}
```

---

## 🎯 PÚBLICOS E SEGMENTAÇÃO

### Tipos de Público

**1. Públicos Salvos (Interests)**

```python
interest_audiences = {
    "ecommerce_moda": {
        "interests": [
            "Fashion",
            "Online shopping",
            "Fast fashion",
            "Luxury goods"
        ],
        "demographics": {
            "age_min": 18,
            "age_max": 45,
            "genders": [2],  # Mulheres
            "locations": ["BR"]
        }
    },
    "infoproduto_marketing": {
        "interests": [
            "Digital marketing",
            "Entrepreneurship",
            "Online advertising",
            "Social media marketing"
        ],
        "demographics": {
            "age_min": 25,
            "age_max": 55,
            "genders": [0],  # Todos
            "locations": ["BR"]
        },
        "behaviors": [
            "Small business owners",
            "Engaged shoppers"
        ]
    }
}
```

**2. Públicos Personalizados (Custom Audiences)**

```python
custom_audiences = {
    "website_visitors": {
        "type": "WEBSITE",
        "retention_days": 180,
        "events": ["PageView", "ViewContent"],
        "url_rules": [
            {"type": "url_contains", "value": "/produto"}
        ]
    },
    "video_viewers": {
        "type": "VIDEO",
        "retention_days": 90,
        "engagement": "ThruPlay",  # Assistiu >95%
        "videos": ["video_id_1", "video_id_2"]
    },
    "engagement_instagram": {
        "type": "ENGAGEMENT",
        "retention_days": 365,
        "engagement_type": "PROFILE",  # Visitou perfil
        "account_id": "instagram_account_id"
    },
    "customer_list": {
        "type": "CUSTOMER_LIST",
        "data_source": "EMAIL",
        "file": "customers.csv",  # email, phone, name
        "remove_if_no_purchase_days": 180
    }
}
```

**3. Públicos Semelhantes (Lookalike)**

```python
lookalike_audiences = {
    "lal_1pct_purchasers": {
        "source": "custom_audience_purchasers",
        "ratio": 0.01,  # 1%
        "country": "BR",
        "optimization": "SIMILARITY"  # ou REACH
    },
    "lal_3pct_leads": {
        "source": "custom_audience_leads",
        "ratio": 0.03,  # 3%
        "country": "BR"
    },
    "lal_5pct_engagers": {
        "source": "instagram_engagers_90d",
        "ratio": 0.05,  # 5%
        "country": "BR"
    },
    "lal_value_based": {
        "source": "purchasers_high_ltv",
        "ratio": 0.02,
        "country": "BR",
        "optimization": "VALUE"  # Baseado em valor
    }
}
```

### Estratégia de Públicos por Estágio

**Fase 1: Validação (Semana 1-2)**
```
- Lookalike 1% Purchasers (se tiver dados)
- Interesses Específicos (3-5 interesses)
- Retargeting Website 30d
```

**Fase 2: Expansão (Semana 3-4)**
```
- Lookalike 2-3% Purchasers
- Interesses Amplos Relacionados
- Retargeting Engagers 60d
- Lookalike 1% High LTV Customers
```

**Fase 3: Scaling (Mês 2+)**
```
- Lookalike 5-10%
- Broad Targeting (sem interesses)
- Advantage+ Audiences (automático)
- Stacked Audiences (combinações)
```

### Exclusões Estratégicas

```python
exclusions = {
    "cold_traffic": {
        "exclude": [
            "purchasers_180d",
            "cart_abandoners_30d",
            "website_visitors_7d"
        ]
    },
    "retargeting": {
        "exclude": [
            "purchasers_7d",  # Já compraram recentemente
            "opted_out"       # Pediram para sair
        ]
    },
    "lookalike": {
        "exclude": [
            "source_audience",  # Não mostrar para quem já está na base
            "purchasers_90d"
        ]
    }
}
```

---

## 🔥 TROUBLESHOOTING COMUM

### Problema 1: Campanha não gasta budget

**Sintomas:**
- Budget diário: R$200
- Gasto real: R$30-50
- Status: Ativo

**Causas Possíveis:**

1. **Público muito pequeno**
```
Solução: Ampliar público
- Adicionar mais interesses
- Aumentar idade range
- Expandir localização
- Criar Lookalike maior (3-5%)
```

2. **Lance muito baixo**
```
Solução: Aumentar lance
- Mudar de Bid Cap para Lowest Cost
- Aumentar bid cap em 30-50%
- Testar Cost Cap strategy
```

3. **Ad set em Learning Phase**
```
Solução: Aguardar ou consolidar
- Esperar ~50 conversões
- Consolidar budget em menos ad sets
- Usar CBO (Campaign Budget Optimization)
```

4. **Sobreposição de públicos**
```
Solução: Verificar audience overlap
- Tools > Audiences > Selecionar 2+ > Ver sobreposição
- Se >30%: Consolidar públicos
- Usar exclusões
```

---

### Problema 2: CPM muito alto

**Sintomas:**
- CPM: R$80-150+
- Esperado: R$15-40

**Causas e Soluções:**

1. **Concorrência alta (Black Friday, Natal)**
```
Solução:
- Evitar datas de pico
- Aumentar budget para competir
- Focar em nichos menos competitivos
- Usar horários off-peak
```

2. **Público saturado**
```
Solução:
- Expandir público
- Criar novos lookalikes
- Testar Advantage+ Audience
- Refrescar criativos
```

3. **Qualidade do criativo baixa**
```
Solução:
- Melhorar relevance score
- Testar novos formatos
- Adicionar legendas em vídeos
- Usar UGC (User Generated Content)
```

---

### Problema 3: CTR baixo (<1%)

**Causas:**

1. **Criativo fraco**
```
Checklist:
☐ Hook nos primeiros 3s?
☐ Problema/Benefício claro?
☐ CTA visível?
☐ Legendas no vídeo?
☐ Thumbnail atraente?
```

2. **Copy genérica**
```
Melhorias:
- Usar framework AIDA/PAS
- Hook mais impactante
- Incluir números/dados
- Criar urgência/escassez
```

3. **Público errado**
```
Soluções:
- Refinar segmentação
- Testar novos interesses
- Usar lookalikes
- Analisar Audience Insights
```

---

### Problema 4: Alto CTR mas baixa conversão

**Sintomas:**
- CTR: 3-5% (ótimo)
- CR (Landing Page): <2% (ruim)

**Causas:**

1. **Landing page ruim**
```
Checklist LP:
☐ Carrega rápido (<3s)?
☐ Mobile-friendly?
☐ Mensagem consistente com ad?
☐ CTA claro e visível?
☐ Prova social?
☐ Remover distrações?
```

2. **Expectativa vs realidade**
```
Problemas:
- Ad promete X, LP entrega Y
- Preço não mencionado no ad
- Benefícios exagerados
- Oferta diferente
```

3. **Processo de checkout complexo**
```
Otimizações:
- Simplificar formulário
- Checkout em 1 página
- Múltiplas formas de pagamento
- Guest checkout (sem cadastro)
- Progress bar
```

---

### Problema 5: Pixel não rastreando

**Diagnóstico:**

```javascript
// Testar pixel no console do navegador
fbq('track', 'PageView');

// Verificar se pixel está instalado
// Chrome DevTools > Network > Filter: "facebook"
// Deve aparecer requests para facebook.com/tr
```

**Soluções:**

1. **Pixel não instalado corretamente**
```html
<!-- Verificar se está no <head> -->
<script>
!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};
if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];
s.parentNode.insertBefore(t,s)}(window, document,'script',
'https://connect.facebook.net/en_US/fbevents.js');
fbq('init', 'SEU_PIXEL_ID');
fbq('track', 'PageView');
</script>
```

2. **Eventos não configurados**
```javascript
// AddToCart
fbq('track', 'AddToCart', {
  value: 99.90,
  currency: 'BRL',
  content_ids: ['produto_123'],
  content_type: 'product'
});

// Purchase
fbq('track', 'Purchase', {
  value: 199.90,
  currency: 'BRL',
  content_ids: ['produto_123', 'produto_456'],
  content_type: 'product',
  num_items: 2
});
```

3. **AdBlockers bloqueando**
```
Solução:
- Usar Conversions API (server-side)
- Implementar CAPI + Pixel (redundância)
- Testar com navegador em modo incognito
```

---

## 📚 CHECKLISTS

### ✅ Checklist: Antes de Lançar Campanha

```
PRÉ-REQUISITOS TÉCNICOS:
☐ Pixel instalado e testado
☐ Domínio verificado
☐ Catálogo configurado (e-commerce)
☐ Conversões customizadas criadas
☐ Método de pagamento ativo
☐ Budget definido e aprovado

ESTRUTURA:
☐ Nome de campanha claro
☐ Objetivo correto selecionado
☐ Budget adequado (mín R$50/ad set)
☐ 3+ ad sets por campanha
☐ 3+ ads por ad set
☐ Públicos sem sobreposição >30%

CRIATIVOS:
☐ Imagens/vídeos em alta resolução
☐ Specs corretos (1080x1080, 1080x1920)
☐ Legendas em vídeos
☐ CTA claro
☐ Copy com framework (AIDA/PAS)
☐ Hook impactante nos primeiros 3s

LANDING PAGE:
☐ Carrega em <3s
☐ Mobile-friendly
☐ Mensagem consistente com ad
☐ CTA acima da fold
☐ Prova social
☐ Checkout simplificado

COMPLIANCE:
☐ Políticas Meta Ads respeitadas
☐ Sem claims médicos/financeiros não comprovados
☐ Imagens sem texto >20%
☐ Termos de uso e privacidade
☐ LGPD compliance
```

---

### ✅ Checklist: Otimização Semanal

```
SEGUNDA-FEIRA - ANÁLISE:
☐ Revisar performance da semana
☐ Exportar relatórios
☐ Identificar winners e losers
☐ Calcular ROAS e CPA
☐ Verificar budget spent %

TERÇA-FEIRA - PAUSAS:
☐ Pausar ad sets com CPA >150% da meta
☐ Pausar ads com CTR <0.5%
☐ Pausar frequência >5x
☐ Pausar ROAS <1.5x

QUARTA-FEIRA - SCALING:
☐ Aumentar budget winners (+20%)
☐ Duplicar ad sets ROAS >4x
☐ Criar novos lookalikes
☐ Testar novas variações criativas

QUINTA-FEIRA - TESTES:
☐ Lançar novos creative tests
☐ Testar novos públicos
☐ Testar novos copies
☐ A/B test landing pages

SEXTA-FEIRA - PLANEJAMENTO:
☐ Planejar semana seguinte
☐ Criar novos criativos
☐ Preparar budget allocation
☐ Definir metas da semana
```

---

### ✅ Checklist: Scaling Seguro

```
ANTES DE ESCALAR:
☐ Mínimo 50 conversões no ad set
☐ ROAS >2.5x consistente (7+ dias)
☐ CPA dentro da meta
☐ Gasto >70% do budget
☐ Frequência <3x

COMO ESCALAR:
☐ Aumentar 20-30% a cada 3 dias
☐ Nunca dobrar budget de uma vez
☐ Monitorar métricas daily
☐ Preparar para queda temporária performance
☐ Manter criativos atualizados

SE PERFORMANCE CAIR:
☐ Reduzir budget em 20%
☐ Pausar por 12-24h
☐ Testar novos criativos
☐ Expandir público
☐ Revisar landing page
```

---

## 🎓 BEST PRACTICES

### 1. Naming Convention (Nomenclatura)

**Padrão Recomendado:**

```
CAMPANHA:
[Objetivo]_[Produto]_[Funil]_[Mês]
Ex: CONV_ProdutoX_BOF_Jan24

AD SET:
[Campanha]_[Público]_[Idade]_[Geo]_[Budget]
Ex: CONV_ProdutoX_BOF_Jan24_LAL1%_25-45_BR_R200

AD:
[AdSet]_[Formato]_[Variação]
Ex: CONV_ProdutoX_BOF_Jan24_LAL1%_25-45_BR_R200_Video_A1
```

**Benefícios:**
- Fácil identificação
- Análise rápida
- Export para relatórios
- Organização escalável

---

### 2. Testing Cadence

**Cronograma de Testes:**

```
SEMANAL:
- 3-5 novos criativos
- 2-3 novas copies
- 1-2 novos formatos

QUINZENAL:
- 2-3 novos públicos
- 1 novo objetivo/posicionamento
- Teste de landing page

MENSAL:
- Revisão completa de estratégia
- Análise de tendências
- Benchmarking vs concorrentes
- Planning próximo mês
```

---

### 3. Budget Allocation

**Regra 70/20/10:**

```
70% - Campanhas comprovadas (winners)
20% - Scaling de campanhas promissoras
10% - Testes e experimentos novos
```

**Por Funil:**

```
E-commerce B2C:
- TOF: 15-25%
- MOF: 25-35%
- BOF: 40-50%
- Retention: 5-10%

Infoproduto:
- Lead Magnet: 20-30%
- Nurture: 30-40%
- Conversion: 40-50%

SaaS:
- Awareness: 20%
- Trial: 50%
- Paid Conversion: 30%
```

---

### 4. Creative Refresh

**Quando Refrescar Criativos:**

```
SINAIS DE FADIGA:
- CTR caiu >30%
- CPM aumentou >50%
- Frequência >4x
- Engagement baixo
- Comentários negativos

FREQUÊNCIA DE REFRESH:
- E-commerce: A cada 2-3 semanas
- Infoproduto: A cada 3-4 semanas
- SaaS: A cada 4-6 semanas
- Serviços: A cada 6-8 semanas

ESTRATÉGIA:
- Manter 40% criativos antigos (winners)
- Adicionar 40% variações
- Testar 20% completamente novos
```

---

### 5. Audience Expansion

**Sequência de Expansão:**

```
FASE 1 (Budget <R$500/dia):
- Lookalike 1%
- Interesses Específicos (3-5)
- Retargeting 30d

FASE 2 (Budget R$500-1500/dia):
- Lookalike 2-3%
- Interesses Amplos (10+)
- Retargeting 60-90d
- Stacked audiences

FASE 3 (Budget >R$1500/dia):
- Lookalike 5-10%
- Broad Targeting (sem interesses)
- Advantage+ Audiences
- Multiple countries
```

---

## 🚀 CASOS DE USO AVANÇADOS

### Caso 1: E-commerce com Catálogo de 500+ Produtos

**Desafio:** Promover múltiplos produtos eficientemente

**Solução: Dynamic Product Ads (DPA)**

```python
dpa_strategy = {
    "campaign_structure": {
        "campaign_1": {
            "name": "DPA - Broad Reach",
            "objective": "CATALOG_SALES",
            "audience": "Broad (all Brazil)",
            "budget": 1000,
            "products": "All catalog"
        },
        "campaign_2": {
            "name": "DPA - Retargeting",
            "objective": "CATALOG_SALES",
            "audiences": [
                "Viewed but not added (7d)",
                "Added but not purchased (14d)",
                "Purchased (upsell 30-90d)"
            ],
            "budget": 1500,
            "products": "Personalized per user"
        }
    },
    "creative_templates": {
        "carousel": "Show 4-10 products",
        "collection": "Cover + 4 products grid",
        "dynamic_video": "Auto-generate from images"
    },
    "optimization": {
        "bid_strategy": "Lowest Cost",
        "optimization_event": "Purchase",
        "attribution": "7-day click, 1-day view"
    }
}
```

**Resultado Esperado:**
- ROAS: 3-6x
- Reduction in manual work: 80%
- More products promoted: 100%

---

### Caso 2: Lançamento de Infoproduto (R$1,997)

**Desafio:** Gerar vendas de produto high-ticket

**Estratégia: Funil Completo**

```
SEMANA 1-2: LEAD MAGNET (Budget: R$3k)
├── E-book Gratuito "10 Erros que Te Impedem de [Resultado]"
├── CPL Target: R$5-10
└── Meta: 500-1000 leads

SEMANA 3-4: NURTURE (Budget: R$5k)
├── VSL de 20min mostrando método
├── Webinar gratuito com Q&A
├── Sequência de emails (5 emails)
└── Retargeting agressivo

SEMANA 5: LANÇAMENTO (Budget: R$12k)
├── Carrinho aberto 5 dias
├── Bônus por tempo limitado
├── Depoimentos e cases
├── Countdown timer
└── Última chance (último dia)

POST-LAUNCH: EVERGREEN (Budget: R$5k/mês)
├── VSL sempre disponível
├── Webinar automatizado
├── Nurture de 7 dias
└── Ofertas periódicas
```

**Métricas Esperadas:**
```
1000 leads x R$10 CPL = R$10k
1000 leads x 8% conversão = 80 vendas
80 vendas x R$1,997 = R$159,760
ROI: R$159k / R$25k = 6.4x
```

---

### Caso 3: App Mobile (Installs + In-App Purchases)

**Objetivo:** Maximizar installs qualificados e purchases no app

**Estrutura:**

```python
app_campaigns = {
    "campaign_1_installs": {
        "objective": "APP_INSTALLS",
        "optimization": "APP_INSTALLS",
        "audiences": [
            "Lookalike 1-3% (purchasers)",
            "Interesses relacionados ao app"
        ],
        "bid_strategy": "Lowest Cost",
        "budget": 2000,
        "kpi": "CPI <R$5"
    },
    "campaign_2_engagement": {
        "objective": "APP_INSTALLS",
        "optimization": "APP_EVENTS",
        "event": "Level Achieved",  # Ou outro evento relevante
        "audiences": [
            "App users (installed but not engaged)"
        ],
        "budget": 1000,
        "kpi": "Cost per Event <R$2"
    },
    "campaign_3_purchases": {
        "objective": "APP_INSTALLS",
        "optimization": "APP_EVENTS",
        "event": "Purchase",
        "audiences": [
            "Engaged users (não compraram)",
            "Lookalike 1% (purchasers in-app)"
        ],
        "budget": 3000,
        "kpi": "ROAS >3x"
    }
}
```

**Deep Links:**
```
Usar deep links para direcionar para telas específicas:
- Onboarding direto
- Produto específico
- Oferta especial
- Carrinho pré-preenchido
```

---

## 📖 GLOSSÁRIO

```
ROAS - Return on Ad Spend (Retorno sobre gasto com anúncios)
CPA - Cost Per Acquisition (Custo por aquisição)
CPM - Cost Per Mille (Custo por mil impressões)
CPC - Cost Per Click (Custo por clique)
CTR - Click-Through Rate (Taxa de cliques)
CR - Conversion Rate (Taxa de conversão)
CPL - Cost Per Lead (Custo por lead)
AOV - Average Order Value (Valor médio do pedido)
LTV - Lifetime Value (Valor vitalício do cliente)

BOF - Bottom of Funnel (Fundo do funil - conversão)
MOF - Middle of Funnel (Meio do funil - consideração)
TOF - Top of Funnel (Topo do funil - awareness)

CBO - Campaign Budget Optimization (Otimização automática de budget)
ABO - Ad Set Budget Optimization (Budget manual por ad set)
DPA - Dynamic Product Ads (Anúncios dinâmicos de produto)
CTA - Call to Action (Chamada para ação)
VSL - Video Sales Letter (Carta de vendas em vídeo)

LAL - Lookalike Audience (Público semelhante)
CA - Custom Audience (Público personalizado)
WCA - Website Custom Audience (Público do site)

ThruPlay - Vídeo assistido por 15s ou 97% (o que vier primeiro)
Frequency - Frequência (quantas vezes mesma pessoa viu o ad)
Reach - Alcance (número de pessoas únicas)
Impressions - Impressões (total de vezes que ad foi mostrado)

Pixel - Código de rastreamento Meta
CAPI - Conversions API (API de conversões server-side)
SDK - Software Development Kit (para apps)
```

---

## 🎯 PRÓXIMOS PASSOS

**Setup Inicial (Semana 1):**
1. Configurar Meta Business Manager
2. Instalar e testar Pixel
3. Criar estrutura de campanha
4. Preparar criativos (mínimo 9)
5. Definir budget e metas

**Lançamento (Semana 2):**
1. Ativar campanhas com budget teste
2. Monitorar daily primeiros 3 dias
3. Fazer ajustes rápidos se necessário
4. Aguardar learning phase (50 conversões)

**Otimização (Semana 3-4):**
1. Pausar losers (CPA alto, CTR baixo)
2. Escalar winners (+20-30%)
3. Duplicar ad sets vencedores
4. Testar novas variações

**Scaling (Mês 2+):**
1. Aumentar budget gradualmente
2. Expandir para novos públicos
3. Testar novos formatos
4. Otimizar landing pages
5. Implementar automações

---

## ⚙️ CONFIGURAÇÃO PARA ANTIGRAVITY

Esta skill está otimizada para uso no Antigravity com:

✅ **Formato Markdown único** - Todo conteúdo em 1 arquivo  
✅ **Seções modulares** - Navegação fácil por tópico  
✅ **Scripts implementáveis** - Código pronto para usar  
✅ **Estratégias acionáveis** - Workflows passo a passo  
✅ **Exemplos práticos** - Casos reais de uso  
✅ **Checklists completos** - Guias de execução  
✅ **Português BR** - Linguagem localizada  

**Tamanho:** ~25k tokens (otimizado para context window)

---

**Versão:** 1.0  
**Última atualização:** 2024  
**Compatibilidade:** Meta Ads Manager, Facebook Business Suite, Instagram Ads