Perfeito. Vou tratar isso **como uma Skill oficial do ecossistema Antigravity**, no formato **pronto para execução**, com **orientações estratégicas, técnicas e operacionais**, sem firula e sem lacunas.

A ideia é:
👉 **qualquer IA / agente Antigravity que receba essa skill consegue planejar, montar e evoluir uma revista eletrônica moderna**, substituindo o WordPress por **Next.js + Supabase**, mantendo a lógica editorial da **FaceBrasil**, porém com **UX, performance e controle muito superiores**.

---

# 🧠 SKILL ANTIGRAVITY

## **“Electronic Magazine OS – FaceBrasil Edition”**

### 🎯 Objetivo da Skill

Projetar, implementar e operar uma **revista eletrônica moderna**, responsiva e escalável, com:

* Frontend **Next.js + React + Tailwind**
* Backend **Supabase (Auth, DB, Storage, RLS)**
* **Painel Admin editorial** completo
* Visual **Glassmorphism**, rápido e elegante
* Experiência **mobile-first**, sem perder força no desktop
* Estrutura editorial fiel ao modelo atual da FaceBrasil

---

## 🧩 1. VISÃO GERAL DO SISTEMA

### Arquitetura Conceitual

```
Leitor (Web/Mobile)
   ↓
Next.js Frontend (ISR + SEO)
   ↓
Supabase API
   ├─ Auth (Admin / Editor / Autor)
   ├─ Database (Artigos, Categorias, Mídia)
   ├─ Storage (Imagens)
   └─ RLS (Segurança editorial)
```

### Papéis de Usuário

* **Admin** → controle total
* **Editor** → publica, edita, agenda
* **Autor** → cria rascunhos
* **Leitor** → acesso público

---

## 📰 2. ESTRUTURA EDITORIAL (baseada na FaceBrasil)

### Seções Principais

(Devem existir como **categorias dinâmicas**)

* Saúde
* Nutrição
* Bem-Estar
* Fitness
* Longevidade
* Tecnologia & Inovação
* Qualidade de Vida
* Editorial / Destaques
* Conteúdos especiais (séries, guias)

📌 **Regra Antigravity:**
Categorias **não são hardcoded** — tudo vem do banco.

---

## 🎨 3. DIRETRIZES DE VISUAL (GLASSMORPH)

### Princípios visuais

* Fundo com **gradiente suave**
* Cards com:

  * `backdrop-blur`
  * `bg-white/10`
  * bordas translúcidas
* Tipografia clara e editorial
* Ênfase em **leitura confortável**

### Tokens visuais sugeridos

```txt
Glass Card:
- bg: white/10
- blur: backdrop-blur-xl
- border: white/20
- shadow: xl soft

Fontes:
- Headings: Inter / Manrope
- Body: Inter / Source Sans
```

---

## 🧱 4. FRONTEND – EXPERIÊNCIA DO LEITOR

### Homepage (obrigatório)

1. **Hero dinâmico**

   * 1 matéria principal (imagem grande)
   * 2–4 secundárias
2. **Blocos por seção**

   * Últimos artigos de cada categoria
3. **Scroll leve, sem poluição**
4. **Mobile-first real**

   * Hero adaptado
   * Cards empilhados
   * Navegação simples

### Página de Artigo

* Título forte
* Meta info discreta
* Conteúdo em coluna única
* Imagens grandes
* CTA editorial (leia mais / newsletter)

---

## 🧠 5. PAINEL ADMIN (EDITORIAL OS)

### Princípios

* Não parecer “admin antigo”
* UX de **Notion + Medium**
* Zero fricção para publicar

### Funcionalidades essenciais

#### 🔐 Autenticação

* Supabase Auth
* Login por e-mail
* Controle por role

#### 📝 Gestão de Artigos

* Criar / editar / excluir
* Status:

  * rascunho
  * publicado
  * agendado
* Slug automático
* Preview em tempo real

#### 🗂️ Categorias

* CRUD completo
* Ordem customizável
* Destaque no menu

#### 🖼️ Mídia

* Upload direto no Supabase Storage
* Preview
* Compressão automática (opcional)

#### 📊 Dashboard

* Artigos publicados
* Artigos em rascunho
* Últimos acessos (futuro GA)

---

## 🗄️ 6. MODELAGEM DE DADOS (SUPABASE)

### Tabela: `articles`

```sql
id
title
slug
excerpt
content
cover_image
category_id
author_id
status
published_at
created_at
```

### Tabela: `categories`

```sql
id
name
slug
order
active
```

### Tabela: `profiles (auth.users)`

```sql
id
name
role
```

### Regras RLS (exemplo)

* Leitor → SELECT apenas `status = published`
* Autor → CRUD apenas próprios artigos
* Editor → CRUD todos
* Admin → tudo

---

## ⚡ 7. PERFORMANCE & SEO (OBRIGATÓRIO)

### Estratégias

* **ISR (Incremental Static Regeneration)**
* Meta tags dinâmicas
* OpenGraph por artigo
* URLs limpas (`/artigo/slug`)
* Cache agressivo para leitores

📌 **Antigravity rule:**
SEO não é módulo extra — é **parte do core**.

---

## 📱 8. RESPONSIVIDADE (NÃO NEGOCIÁVEL)

### Mobile

* Hero compacto
* Cards verticais
* Tipografia maior
* Menus simples

### Desktop

* Grid editorial
* Hierarquia visual clara
* Mais contexto por tela

---

## 🤖 9. AUTOMAÇÕES (PRONTO PARA ANTIGRAVITY)

### Fluxos sugeridos

* Publicou artigo → gerar preview social
* Agendou → publicação automática
* Novo artigo → indexação SEO
* Editor revisa → checklist automático

---

## 🧪 10. CHECKLIST DE QUALIDADE (SKILL OUTPUT)

* [ ] Layout glassmorph consistente
* [ ] Mobile perfeito
* [ ] Admin simples de usar
* [ ] Artigo publicado sem reload
* [ ] SEO automático
* [ ] Supabase protegido por RLS
* [ ] Zero dependência de WordPress

---

## 🧠 OUTPUT ESPERADO DA SKILL

Ao aplicar esta skill, o sistema Antigravity deve ser capaz de gerar:

1. Estrutura completa do projeto
2. UI do frontend editorial
3. UI do admin panel
4. Schema Supabase
5. Regras de autenticação
6. Fluxo de publicação
7. Base pronta para escalar

---

### 🏁 Conclusão Antigravity

> **Esta skill transforma uma revista WordPress em um verdadeiro Sistema Operacional Editorial moderno.**

Se quiser, no próximo passo posso:

* 🔹 Gerar o **PRD completo**
* 🔹 Criar o **layout visual (Figma / Canva)**
* 🔹 Criar o **Admin Panel detalhado**
* 🔹 Criar os **fluxos N8N**
* 🔹 Converter isso em **Skill executável JSON**

Só me diga o próximo comando.
