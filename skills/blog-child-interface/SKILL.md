# blog-child-interface

## Overview
Esta skill cria interfaces completas de blog filho que consomem conteúdo de uma API centralizada. Oferece 5 layouts pré-configurados (Magazine, Minimal, Corporate, Creative, Tech), componentes modulares (Navbar, Hero, Categories, Highlights, CTA, Footer com 5 variações cada), templates de páginas legais para conformidade com anunciantes, e implementação robusta de segurança, performance e SEO.

## When to Use This Skill
Use esta skill quando:
- Criar novo blog filho conectado ao sistema centralizado
- Implementar diferentes identidades visuais para cada blog
- Garantir conformidade legal (Cookies Policy, Privacy Policy, Terms of Service, Security Policy, Return Policy)
- Estabelecer comunicação segura com API de conteúdo centralizada
- Precisar de múltiplos layouts profissionais prontos
- Implementar SEO, analytics e monetização com ads

NÃO use esta skill para:
- Criar o sistema de gerenciamento centralizado (backend)
- Implementar o editor de conteúdo ou "mente criadora"
- Criar APIs ou sistemas de autenticação do lado servidor
- Projetos que não sejam blogs ou não precisem de múltiplas instâncias

## Key Concepts

### Arquitetura do Sistema
O sistema consiste em três partes:
1. **API Central**: Backend centralizado que armazena todo conteúdo
2. **Blog Child Interface**: Frontend (esta skill) que consome a API
3. **Admin Panel**: Interface de administração (não coberta por esta skill)

Cada blog filho é uma instância Next.js independente que se conecta à API central via API Key, permitindo que dezenas de blogs compartilhem o mesmo banco de dados de conteúdo mas mantenham identidades visuais únicas.

### Stack Tecnológico Padrão
```yaml
Frontend:
  - Next.js 14+ (App Router)
  - TypeScript
  - Tailwind CSS + Shadcn/ui
  - React Query (cache/sincronização)
  - Zod (validação)

Performance:
  - ISR (Incremental Static Regeneration)
  - Next.js Image Optimization
  - Edge Caching

SEO:
  - Next.js Metadata API
  - JSON-LD Structured Data
  - Sitemap/RSS automáticos

Segurança:
  - API Key Authentication
  - Rate Limiting
  - Content Security Policy
  - Input Sanitization (DOMPurify)
```

### Estrutura de Dados da API

**Endpoint Principal:**
```typescript
GET /api/v1/blog/{blogId}/posts
GET /api/v1/blog/{blogId}/posts/{slug}
GET /api/v1/blog/{blogId}/config
```

**Interface BlogPost:**
```typescript
interface BlogPost {
  id: string;
  slug: string;
  title: string;
  excerpt: string;
  content: string; // HTML sanitizado
  author: { name: string; avatar?: string; bio?: string };
  categories: string[];
  tags: string[];
  featuredImage: { url: string; alt: string; width: number; height: number };
  images: Array<{ url: string; alt: string; caption?: string }>;
  seo: {
    metaTitle: string;
    metaDescription: string;
    keywords: string[];
    ogImage?: string;
    canonicalUrl?: string;
  };
  publishedAt: string;
  updatedAt: string;
  status: 'draft' | 'published' | 'scheduled';
  readTime: number;
}
```

**Interface BlogConfig:**
```typescript
interface BlogConfig {
  blogId: string;
  name: string;
  description: string;
  logo: string;
  favicon: string;
  primaryColor: string;
  secondaryColor: string;
  layout: 'magazine' | 'minimal' | 'corporate' | 'creative' | 'tech';
  navbar: NavbarConfig;
  hero: HeroConfig;
  footer: FooterConfig;
  social: SocialLinks;
  analytics: AnalyticsConfig;
  ads: AdsConfig;
}
```

## Implementation Guide

### Fase 1: Setup Inicial do Projeto

**Criar estrutura base Next.js:**
```bash
npx create-next-app@latest blog-child --typescript --tailwind --app --src-dir
cd blog-child
npm install @tanstack/react-query axios zod lucide-react
npm install isomorphic-dompurify lru-cache
npm install -D @types/node
```

**Configurar Shadcn/ui:**
```bash
npx shadcn-ui@latest init
npx shadcn-ui@latest add button card input
```

**Estrutura de diretórios:**
```
blog-child/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── blog/
│   │   ├── page.tsx
│   │   ├── [slug]/page.tsx
│   │   └── category/[slug]/page.tsx
│   ├── legal/
│   │   ├── cookies-policy/page.tsx
│   │   ├── privacy-policy/page.tsx
│   │   ├── terms-of-service/page.tsx
│   │   ├── security-policy/page.tsx
│   │   └── return-policy/page.tsx
│   ├── sitemap.ts
│   └── robots.ts
├── components/
│   ├── layout/
│   │   ├── Navbar/
│   │   ├── Hero/
│   │   └── Footer/
│   ├── sections/
│   ├── blog/
│   └── seo/
├── lib/
│   ├── api-client.ts
│   ├── sanitize.ts
│   └── rate-limiter.ts
├── config/
│   ├── blog-config.ts
│   ├── layouts.ts
│   └── legal-templates.ts
└── hooks/
```

### Fase 2: Configuração de Segurança

**Criar cliente API seguro (`lib/api-client.ts`):**
```typescript
import axios from 'axios';

const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' },
});

apiClient.interceptors.request.use((config) => {
  const apiKey = process.env.API_KEY;
  if (apiKey) config.headers['X-API-Key'] = apiKey;
  return config;
});

apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      console.error('API authentication failed');
    }
    return Promise.reject(error);
  }
);

export default apiClient;
```

**Implementar sanitização (`lib/sanitize.ts`):**
```typescript
import DOMPurify from 'isomorphic-dompurify';

export function sanitizeHTML(html: string): string {
  return DOMPurify.sanitize(html, {
    ALLOWED_TAGS: ['p', 'br', 'strong', 'em', 'u', 'h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'ul', 'ol', 'li', 'a', 'img', 'blockquote', 'code', 'pre', 'table', 'thead', 'tbody', 'tr', 'th', 'td'],
    ALLOWED_ATTR: ['href', 'src', 'alt', 'title', 'class', 'id'],
    ALLOW_DATA_ATTR: false,
  });
}
```

**Configurar Content Security Policy (`next.config.js`):**
```typescript
const securityHeaders = [
  {
    key: 'Content-Security-Policy',
    value: `
      default-src 'self';
      script-src 'self' 'unsafe-eval' 'unsafe-inline' *.googletagmanager.com;
      style-src 'self' 'unsafe-inline';
      img-src 'self' data: https:;
      connect-src 'self' ${process.env.NEXT_PUBLIC_API_URL};
    `.replace(/\s{2,}/g, ' ').trim()
  },
  { key: 'X-Frame-Options', value: 'SAMEORIGIN' },
  { key: 'X-Content-Type-Options', value: 'nosniff' },
  { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
];

module.exports = {
  async headers() {
    return [{ source: '/:path*', headers: securityHeaders }];
  },
};
```

### Fase 3: Implementar Layouts Base

**Os 5 layouts disponíveis:**

1. **Magazine Layout**: Hero com carrossel, grid 3 colunas, sidebar. Ideal para notícias, lifestyle.
2. **Minimal Layout**: Hero simples, lista vertical, tipografia destacada. Ideal para blogs pessoais.
3. **Corporate Layout**: Hero com CTA, grid 2 colunas, cores sóbrias. Ideal para B2B, consultoria.
4. **Creative Layout**: Hero animado, masonry grid, hover effects. Ideal para design, arte.
5. **Tech Layout**: Hero estilo terminal, syntax highlighting, dark mode. Ideal para programação.

**Criar sistema de seleção de layout (`config/blog-config.ts`):**
```typescript
export const blogConfig = {
  blogId: process.env.NEXT_PUBLIC_BLOG_ID!,
  name: process.env.NEXT_PUBLIC_BLOG_NAME!,
  layout: 'magazine', // magazine | minimal | corporate | creative | tech
  
  navbar: { type: 'sticky' }, // sticky | fixed | static | transparent | mega
  hero: { type: 'carousel' }, // carousel | featured | split | video | minimal
  footer: { type: 'standard' }, // minimal | standard | mega | newsletter | social
  
  features: {
    search: true,
    darkMode: true,
    newsletter: true,
    relatedPosts: true,
  },
};
```

### Fase 4: Componentes de Layout

**Cada componente tem 5 variações:**

**Navbar (5 tipos):**
- `StickyNavbar.tsx`: Permanece no topo ao rolar
- `FixedNavbar.tsx`: Sempre visível
- `StaticNavbar.tsx`: Scroll normal
- `TransparentNavbar.tsx`: Transparente sobre hero
- `MegaMenuNavbar.tsx`: Menu dropdown grande

**Hero (5 tipos):**
- `CarouselHero.tsx`: Múltiplos posts em rotação
- `FeaturedHero.tsx`: Um post destacado
- `SplitHero.tsx`: Imagem + texto lado a lado
- `VideoHero.tsx`: Background de vídeo
- `MinimalHero.tsx`: Apenas título e subtítulo

**Footer (5 tipos):**
- `MinimalFooter.tsx`: Copyright e links legais
- `StandardFooter.tsx`: 3-4 colunas organizadas
- `MegaFooter.tsx`: Muitas seções
- `NewsletterFooter.tsx`: Foco em inscrição
- `SocialFooter.tsx`: Destaque redes sociais

### Fase 5: Templates de Páginas Legais

**Criar templates reutilizáveis (`config/legal-templates.ts`):**
```typescript
export const legalTemplates = {
  cookiesPolicy: {
    title: "Política de Cookies",
    sections: [
      {
        title: "O que são cookies",
        content: "Cookies são pequenos arquivos de texto..."
      },
      {
        title: "Tipos de cookies que utilizamos",
        subsections: [
          { type: "Essenciais", duration: "Sessão", canDisable: false },
          { type: "Performance", duration: "1 ano", canDisable: true },
          { type: "Marketing", duration: "2 anos", canDisable: true }
        ]
      },
      {
        title: "Cookies de terceiros",
        providers: ["Google Analytics", "Google Ads", "Facebook Pixel"]
      }
    ]
  },
  
  privacyPolicy: {
    title: "Política de Privacidade",
    compliance: ["LGPD", "GDPR"],
    sections: [
      {
        title: "Informações que coletamos",
        types: ["Dados de cadastro", "Dados de navegação", "Dados de interação"]
      },
      {
        title: "Seus direitos",
        rights: ["Acessar seus dados", "Corrigir dados", "Solicitar exclusão", "Portabilidade"]
      }
    ]
  },
  
  termsOfService: {
    title: "Termos de Serviço",
    sections: [
      { title: "Aceitação dos Termos" },
      { title: "Uso do Serviço", allowed: [...], prohibited: [...] },
      { title: "Propriedade Intelectual" },
      { title: "Limitação de Responsabilidade" }
    ]
  },
  
  securityPolicy: {
    title: "Política de Segurança",
    measures: [
      { name: "Criptografia", description: "SSL/TLS" },
      { name: "Firewall", description: "Proteção contra ataques" },
      { name: "Backups", description: "Diários automáticos" }
    ]
  },
  
  returnPolicy: { // Apenas se houver e-commerce
    title: "Política de Devolução",
    period: "7 dias",
    conditions: ["Embalagem original", "Sem uso", "Com nota fiscal"]
  }
};
```

**Implementar páginas legais (`app/legal/privacy-policy/page.tsx`):**
```typescript
import { legalTemplates } from '@/config/legal-templates';

export default function PrivacyPolicyPage() {
  const template = legalTemplates.privacyPolicy;
  
  return (
    <div className="container mx-auto px-4 py-16 max-w-4xl">
      <h1 className="text-4xl font-bold mb-8">{template.title}</h1>
      <p className="text-sm text-muted-foreground mb-8">
        Última atualização: {new Date().toLocaleDateString('pt-BR')}
      </p>
      
      {template.sections.map((section, idx) => (
        <section key={idx} className="mb-8">
          <h2 className="text-2xl font-semibold mb-4">{section.title}</h2>
          {/* Renderizar conteúdo baseado no template */}
        </section>
      ))}
      
      <div className="mt-12 p-6 bg-muted rounded-lg">
        <h3 className="font-semibold mb-2">Contato</h3>
        <p>DPO: {process.env.DPO_EMAIL}</p>
        <p>Endereço: {process.env.COMPANY_ADDRESS}</p>
      </div>
    </div>
  );
}
```

### Fase 6: SEO e Performance

**Implementar metadata dinâmico:**
```typescript
// app/blog/[slug]/page.tsx
export async function generateMetadata({ params }): Promise<Metadata> {
  const post = await fetchPost(params.slug);
  
  return {
    title: post.seo.metaTitle,
    description: post.seo.metaDescription,
    openGraph: {
      title: post.seo.metaTitle,
      type: 'article',
      publishedTime: post.publishedAt,
      images: [{ url: post.featuredImage.url, width: 1200, height: 630 }],
    },
  };
}
```

**Adicionar JSON-LD Schema:**
```typescript
// components/seo/ArticleSchema.tsx
export function ArticleSchema({ post }) {
  const schema = {
    '@context': 'https://schema.org',
    '@type': 'Article',
    headline: post.title,
    image: post.featuredImage.url,
    datePublished: post.publishedAt,
    author: { '@type': 'Person', name: post.author.name },
  };

  return (
    <script
      type="application/ld+json"
      dangerouslySetInnerHTML={{ __html: JSON.stringify(schema) }}
    />
  );
}
```

**Configurar ISR:**
```typescript
// app/blog/[slug]/page.tsx
export const revalidate = 3600; // Revalidar a cada hora

export async function generateStaticParams() {
  const posts = await fetchPosts({ limit: 100 });
  return posts.map((post) => ({ slug: post.slug }));
}
```

### Fase 7: Deployment

**Configurar variáveis de ambiente (`.env.example`):**
```env
# Blog Identity
NEXT_PUBLIC_BLOG_ID=blog-001
NEXT_PUBLIC_BLOG_NAME=Meu Blog
NEXT_PUBLIC_BASE_URL=https://meublog.com.br

# API
NEXT_PUBLIC_API_URL=https://api.central.com/v1
API_KEY=sk_live_xxxxxxxxxxxxx

# Analytics
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX

# Ads
NEXT_PUBLIC_ADSENSE_ID=ca-pub-xxxxxxxxxxxxxxxx

# Legal
COMPANY_NAME=Minha Empresa LTDA
COMPANY_ADDRESS=Rua Exemplo, 123 - São Paulo, SP
DPO_EMAIL=dpo@empresa.com.br
SECURITY_EMAIL=security@empresa.com.br
```

**Criar Dockerfile:**
```dockerfile
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci

FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV production
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3000
CMD ["node", "server.js"]
```

## Best Practices

### Segurança
- SEMPRE sanitizar HTML retornado pela API antes de renderizar
- NUNCA expor API_KEY no código frontend (usar server-side apenas)
- Implementar rate limiting para prevenir abuso
- Validar todos os dados com Zod antes de usar
- Configurar CSP headers rigorosos

### Performance
- Usar ISR para páginas de blog (revalidate: 3600)
- Implementar lazy loading de imagens com Next.js Image
- Cachear respostas da API com React Query (staleTime: 5min)
- Gerar páginas estáticas para os 100 posts mais populares
- Usar Edge Caching quando possível

### SEO
- Implementar TODOS os meta tags (title, description, OG, Twitter)
- Adicionar JSON-LD structured data em TODAS as páginas
- Gerar sitemap.xml e robots.txt automaticamente
- Usar Next.js Metadata API (não react-helmet)
- Implementar canonical URLs para evitar conteúdo duplicado

### Acessibilidade
- Garantir contraste mínimo WCAG AA (4.5:1)
- Usar semantic HTML (header, nav, main, article, footer)
- Adicionar alt text em TODAS as imagens
- Garantir navegação completa por teclado
- Testar com screen readers

### Manutenção
- Documentar processo de setup em README.md
- Criar health check endpoint (/api/health)
- Implementar error tracking (Sentry ou similar)
- Configurar CI/CD para deploys automáticos
- Manter logs de erros de API

## Common Issues and Solutions

### Problema: API retorna 401 Unauthorized
**Solução**: Verificar se API_KEY está configurada corretamente e se está sendo enviada no header. Checar se variável de ambiente está disponível no servidor.

### Problema: Imagens não carregam
**Solução**: Adicionar domínio da CDN em `next.config.js`:
```javascript
images: {
  domains: ['cdn.seudominio.com', 'api.central.com'],
}
```

### Problema: Build falha com "Too many files"
**Solução**: Limitar geração de páginas estáticas em `generateStaticParams`:
```typescript
const posts = await fetchPosts({ limit: 100 }); // Não gerar TODAS
```

### Problema: CSP bloqueia scripts necessários
**Solução**: Adicionar domínios permitidos no CSP header, especialmente para analytics/ads.

### Problema: Pages legais não aparecem no Google
**Solução**: Verificar se estão linkadas no footer e incluídas no sitemap.xml.

## Examples

### Exemplo 1: Blog Magazine Style
```typescript
// config/blog-config.ts
export const blogConfig = {
  layout: 'magazine',
  navbar: { type: 'sticky', logo: '/logo.png', search: true },
  hero: { type: 'carousel', height: 'large', animation: true },
  highlights: { type: 'featured', layout: 'grid', postsPerPage: 9 },
  footer: { type: 'standard', columns: 4 },
};
```

### Exemplo 2: Blog Tech Minimal
```typescript
export const blogConfig = {
  layout: 'tech',
  navbar: { type: 'fixed', darkMode: true },
  hero: { type: 'minimal', height: 'small' },
  highlights: { type: 'recent', layout: 'list' },
  footer: { type: 'minimal' },
};
```

### Exemplo 3: Blog Corporate com Newsletter
```typescript
export const blogConfig = {
  layout: 'corporate',
  navbar: { type: 'transparent', cta: { text: 'Contato', href: '/contact' } },
  hero: { type: 'split' },
  cta: { type: 'newsletter', position: 'sidebar', style: 'card' },
  footer: { type: 'mega', sections: [...] },
};
```

## Testing Checklist

Antes de fazer deploy, verificar:

**Funcionalidade:**
- [ ] Homepage carrega corretamente
- [ ] Lista de posts funciona
- [ ] Post individual renderiza com formatação
- [ ] Categorias filtram corretamente
- [ ] Busca retorna resultados (se habilitada)
- [ ] Todas as páginas legais acessíveis

**Segurança:**
- [ ] API Key não exposta no frontend
- [ ] Content Security Policy configurado
- [ ] Inputs sanitizados
- [ ] Rate limiting funcionando
- [ ] HTTPS configurado

**SEO:**
- [ ] Meta tags em todas as páginas
- [ ] JSON-LD structured data presente
- [ ] Sitemap.xml acessível
- [ ] Robots.txt configurado
- [ ] Lighthouse Score > 90

**Performance:**
- [ ] First Contentful Paint < 1.5s
- [ ] Largest Contentful Paint < 2.5s
- [ ] Imagens otimizadas e lazy loaded
- [ ] ISR configurado para posts

**Monetização:**
- [ ] Google Analytics rastreando
- [ ] Google Adsense (se aplicável) carregando
- [ ] Páginas legais aprovadas para anunciantes

## Additional Resources

- Next.js Documentation: https://nextjs.org/docs
- React Query: https://tanstack.com/query/latest
- Tailwind CSS: https://tailwindcss.com/docs
- Shadcn/ui: https://ui.shadcn.com
- Schema.org: https://schema.org
- LGPD Guide: https://www.gov.br/esporte/pt-br/acesso-a-informacao/lgpd
- GDPR Guide: https://gdpr.eu/ e https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/