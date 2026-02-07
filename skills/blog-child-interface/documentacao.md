# Skill: blog-child-interface

## Descrição
Cria interfaces de blog filho completas e seguras que consomem conteúdo de uma API centralizada. Oferece múltiplos layouts personalizáveis, páginas de conformidade legal e integração robusta com sistema de gerenciamento de conteúdo centralizado.

## Quando Usar
- Criar novos blogs filhos conectados ao sistema centralizado
- Implementar diferentes identidades visuais para cada blog
- Garantir conformidade legal com políticas necessárias para monetização
- Estabelecer comunicação segura com API de conteúdo centralizada

## Arquitetura e Tecnologias

### Stack Recomendado
```yaml
Frontend:
  - Next.js 14+ (App Router)
  - TypeScript
  - Tailwind CSS
  - Shadcn/ui (componentes)
  - React Query (cache e sincronização)
  - Zod (validação)

Performance:
  - Next.js Image Optimization
  - Dynamic Imports
  - ISR (Incremental Static Regeneration)
  - Edge Caching

SEO:
  - Next.js Metadata API
  - Structured Data (JSON-LD)
  - Sitemap automático
  - RSS Feed

Segurança:
  - API Key Authentication
  - Rate Limiting
  - CORS configurado
  - Content Security Policy
  - Input Sanitization
```

## Estrutura de Dados da API

### Endpoint Principal
```typescript
// GET /api/v1/blog/{blogId}/posts
interface BlogPost {
  id: string;
  slug: string;
  title: string;
  excerpt: string;
  content: string; // HTML sanitizado
  author: {
    name: string;
    avatar?: string;
    bio?: string;
  };
  categories: string[];
  tags: string[];
  featuredImage: {
    url: string;
    alt: string;
    width: number;
    height: number;
  };
  images: Array<{
    url: string;
    alt: string;
    caption?: string;
  }>;
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
  readTime: number; // minutos
}

// Configuração do Blog
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

## Layouts Disponíveis

### 1. Magazine Layout
**Características:**
- Hero com carrossel de destaques
- Grid de posts em 3 colunas
- Sidebar com categorias e populares
- Cards com imagens grandes

**Ideal para:** Blogs de notícias, lifestyle, entretenimento

### 2. Minimal Layout
**Características:**
- Hero simples com último post
- Lista vertical de artigos
- Tipografia destacada
- Espaçamento generoso

**Ideal para:** Blogs pessoais, literatura, filosofia

### 3. Corporate Layout
**Características:**
- Hero com CTA empresarial
- Grid 2 colunas
- Sidebar com recursos
- Cores sóbrias e profissionais

**Ideal para:** Blogs corporativos, B2B, consultoria

### 4. Creative Layout
**Características:**
- Hero com animações
- Masonry grid assimétrico
- Hover effects elaborados
- Tipografia criativa

**Ideal para:** Design, arte, fotografia, portfólio

### 5. Tech Layout
**Características:**
- Hero com código/terminal style
- Cards com syntax highlighting preview
- Tags destacadas
- Dark mode por padrão

**Ideal para:** Programação, tecnologia, tutoriais

## Componentes Base

### Navbar Component
```typescript
interface NavbarConfig {
  type: 'sticky' | 'fixed' | 'static' | 'transparent' | 'mega';
  logo: {
    url: string;
    height: number;
  };
  menu: Array<{
    label: string;
    href: string;
    submenu?: Array<{
      label: string;
      href: string;
    }>;
  }>;
  search: boolean;
  cta?: {
    text: string;
    href: string;
    style: 'primary' | 'outline';
  };
  darkMode: boolean;
}
```

**Variações:**
1. **Sticky Navbar**: Permanece no topo ao rolar
2. **Fixed Navbar**: Sempre visível
3. **Static Navbar**: Scroll normal
4. **Transparent Navbar**: Transparente sobre hero
5. **Mega Menu Navbar**: Menu dropdown grande com categorias

### Hero Component
```typescript
interface HeroConfig {
  type: 'carousel' | 'featured' | 'split' | 'video' | 'minimal';
  height: 'small' | 'medium' | 'large' | 'fullscreen';
  overlay: boolean;
  overlayOpacity: number;
  animation: boolean;
}
```

**Variações:**
1. **Carousel Hero**: Múltiplos posts em rotação
2. **Featured Hero**: Um post destacado grande
3. **Split Hero**: Imagem + texto lado a lado
4. **Video Hero**: Background de vídeo
5. **Minimal Hero**: Apenas título e subtítulo

### Categories Section
```typescript
interface CategoriesConfig {
  display: 'grid' | 'carousel' | 'tabs' | 'sidebar' | 'dropdown';
  showCount: boolean;
  icons: boolean;
  style: 'cards' | 'pills' | 'badges' | 'buttons';
}
```

**Variações:**
1. **Grid Categories**: Cards em grid
2. **Carousel Categories**: Scroll horizontal
3. **Tabs Categories**: Filtro por abas
4. **Sidebar Categories**: Lista lateral
5. **Dropdown Categories**: Menu dropdown

### Highlights Section
```typescript
interface HighlightsConfig {
  type: 'featured' | 'trending' | 'recent' | 'popular' | 'mixed';
  layout: 'grid' | 'list' | 'masonry' | 'slider' | 'timeline';
  postsPerPage: number;
  showExcerpt: boolean;
  showAuthor: boolean;
  showDate: boolean;
  showReadTime: boolean;
}
```

### CTA Component
```typescript
interface CTAConfig {
  type: 'newsletter' | 'download' | 'contact' | 'product' | 'social';
  position: 'top' | 'middle' | 'bottom' | 'sidebar' | 'popup';
  style: 'banner' | 'card' | 'inline' | 'modal' | 'sticky';
  form?: {
    fields: Array<{
      name: string;
      type: string;
      required: boolean;
    }>;
    submitText: string;
    endpoint: string;
  };
}
```

**Variações:**
1. **Newsletter CTA**: Captura de email
2. **Download CTA**: Lead magnet
3. **Contact CTA**: Formulário de contato
4. **Product CTA**: Venda de produto
5. **Social CTA**: Redes sociais

### Footer Component
```typescript
interface FooterConfig {
  type: 'minimal' | 'standard' | 'mega' | 'newsletter' | 'social';
  columns: 2 | 3 | 4;
  sections: Array<{
    title: string;
    links: Array<{
      text: string;
      href: string;
    }>;
  }>;
  social: SocialLinks;
  copyright: string;
  legalPages: boolean;
}
```

**Variações:**
1. **Minimal Footer**: Copyright e links legais
2. **Standard Footer**: 3-4 colunas organizadas
3. **Mega Footer**: Muitas seções e links
4. **Newsletter Footer**: Foco em inscrição
5. **Social Footer**: Destaque para redes sociais

## Templates de Páginas Legais

### Estrutura de Arquivos
```
/app
  /legal
    /cookies-policy
      page.tsx
      template.ts
    /privacy-policy
      page.tsx
      template.ts
    /terms-of-service
      page.tsx
      template.ts
    /security-policy
      page.tsx
      template.ts
    /return-policy (opcional)
      page.tsx
      template.ts
```

### Template: Cookies Policy
```typescript
export const cookiesPolicyTemplate = {
  title: "Política de Cookies",
  lastUpdated: "auto", // data automática
  sections: [
    {
      title: "O que são cookies",
      content: `Cookies são pequenos arquivos de texto armazenados no seu dispositivo 
      quando você visita nosso site. Eles nos ajudam a melhorar sua experiência.`
    },
    {
      title: "Tipos de cookies que utilizamos",
      subsections: [
        {
          type: "Cookies Essenciais",
          description: "Necessários para funcionamento do site",
          duration: "Sessão",
          canDisable: false
        },
        {
          type: "Cookies de Performance",
          description: "Medem uso e performance do site",
          duration: "1 ano",
          canDisable: true
        },
        {
          type: "Cookies de Marketing",
          description: "Personalizam anúncios",
          duration: "2 anos",
          canDisable: true
        }
      ]
    },
    {
      title: "Gerenciar preferências",
      content: `Você pode gerenciar suas preferências de cookies através do banner 
      ou configurações do navegador.`
    },
    {
      title: "Cookies de terceiros",
      providers: ["Google Analytics", "Google Ads", "Facebook Pixel", "Hotjar"]
    }
  ]
};
```

### Template: Privacy Policy
```typescript
export const privacyPolicyTemplate = {
  title: "Política de Privacidade",
  compliance: ["LGPD", "GDPR"], // conforme localização
  sections: [
    {
      title: "Informações que coletamos",
      types: [
        "Dados de cadastro (nome, email)",
        "Dados de navegação (IP, cookies)",
        "Dados de interação (comentários, formulários)"
      ]
    },
    {
      title: "Como usamos suas informações",
      purposes: [
        "Fornecer e melhorar nossos serviços",
        "Enviar comunicações relevantes",
        "Análise e pesquisa",
        "Segurança e prevenção de fraudes"
      ]
    },
    {
      title: "Compartilhamento de dados",
      recipients: [
        "Provedores de serviços (hosting, analytics)",
        "Parceiros de marketing (com consentimento)",
        "Autoridades legais (quando requerido)"
      ]
    },
    {
      title: "Seus direitos",
      rights: [
        "Acessar seus dados",
        "Corrigir dados incorretos",
        "Solicitar exclusão",
        "Revogar consentimento",
        "Portabilidade de dados"
      ]
    },
    {
      title: "Segurança",
      measures: [
        "Criptografia SSL/TLS",
        "Acesso restrito a dados",
        "Backups regulares",
        "Monitoramento de segurança"
      ]
    },
    {
      title: "Contato",
      dpo: {
        email: "{{DPO_EMAIL}}",
        address: "{{COMPANY_ADDRESS}}"
      }
    }
  ]
};
```

### Template: Terms of Service
```typescript
export const termsOfServiceTemplate = {
  title: "Termos de Serviço",
  sections: [
    {
      title: "Aceitação dos Termos",
      content: `Ao acessar e usar este site, você aceita e concorda em cumprir 
      estes termos e condições.`
    },
    {
      title: "Uso do Serviço",
      allowed: [
        "Acessar e ler conteúdo",
        "Compartilhar artigos",
        "Comentar (se aplicável)",
        "Inscrever-se em newsletter"
      ],
      prohibited: [
        "Copiar conteúdo sem autorização",
        "Usar para spam ou phishing",
        "Tentar acessar áreas restritas",
        "Interferir no funcionamento do site"
      ]
    },
    {
      title: "Propriedade Intelectual",
      content: `Todo o conteúdo, incluindo textos, imagens, logos e código, 
      é protegido por direitos autorais.`
    },
    {
      title: "Limitação de Responsabilidade",
      disclaimers: [
        "Conteúdo fornecido 'como está'",
        "Sem garantias de precisão",
        "Não responsáveis por links externos",
        "Não responsáveis por interrupções"
      ]
    },
    {
      title: "Modificações",
      content: `Reservamos o direito de modificar estes termos a qualquer momento. 
      Alterações serão notificadas nesta página.`
    }
  ]
};
```

### Template: Security Policy
```typescript
export const securityPolicyTemplate = {
  title: "Política de Segurança",
  sections: [
    {
      title: "Nosso Compromisso",
      content: `Levamos a segurança dos seus dados a sério e implementamos medidas 
      robustas para protegê-los.`
    },
    {
      title: "Medidas de Segurança Técnicas",
      measures: [
        {
          name: "Criptografia",
          description: "SSL/TLS em todas as conexões"
        },
        {
          name: "Firewall",
          description: "Proteção contra ataques"
        },
        {
          name: "Backups",
          description: "Backups automáticos diários"
        },
        {
          name: "Monitoramento",
          description: "Vigilância 24/7 de ameaças"
        }
      ]
    },
    {
      title: "Medidas Organizacionais",
      measures: [
        "Acesso baseado em funções",
        "Treinamento de equipe",
        "Políticas de senha forte",
        "Revisões de segurança regulares"
      ]
    },
    {
      title: "Relatar Vulnerabilidades",
      content: `Se você descobrir uma vulnerabilidade de segurança, por favor 
      entre em contato imediatamente:`,
      contact: {
        email: "{{SECURITY_EMAIL}}",
        pgp: "{{PGP_KEY_OPTIONAL}}"
      }
    },
    {
      title: "Resposta a Incidentes",
      steps: [
        "Detecção e análise",
        "Contenção e erradicação",
        "Recuperação",
        "Notificação (se necessário)",
        "Análise pós-incidente"
      ]
    }
  ]
};
```

### Template: Return Policy (E-commerce)
```typescript
export const returnPolicyTemplate = {
  title: "Política de Devolução",
  applicable: "{{HAS_ECOMMERCE}}", // só se houver vendas
  sections: [
    {
      title: "Prazo para Devolução",
      period: "7 dias", // conforme CDC brasileiro
      startDate: "Data de recebimento do produto"
    },
    {
      title: "Condições para Devolução",
      requirements: [
        "Produto em embalagem original",
        "Sem sinais de uso",
        "Acompanhado da nota fiscal",
        "Dentro do prazo estabelecido"
      ]
    },
    {
      title: "Produtos Não Devolvíveis",
      exceptions: [
        "Produtos digitais após download",
        "Produtos personalizados",
        "Produtos perecíveis"
      ]
    },
    {
      title: "Processo de Devolução",
      steps: [
        "Entrar em contato via {{CONTACT_METHOD}}",
        "Receber autorização e instruções",
        "Embalar adequadamente o produto",
        "Enviar para endereço fornecido",
        "Aguardar processamento"
      ]
    },
    {
      title: "Reembolso",
      timeline: "Até 10 dias úteis após recebimento",
      method: "Mesmo método de pagamento original",
      shippingCost: "Reembolsável em caso de defeito"
    }
  ]
};
```

## Implementação de Segurança

### Autenticação com API
```typescript
// lib/api-client.ts
import axios from 'axios';

const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Interceptor para adicionar API Key
apiClient.interceptors.request.use(
  (config) => {
    const apiKey = process.env.API_KEY;
    if (apiKey) {
      config.headers['X-API-Key'] = apiKey;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Interceptor para tratamento de erros
apiClient.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Log erro de autenticação
      console.error('API authentication failed');
    }
    return Promise.reject(error);
  }
);

export default apiClient;
```

### Rate Limiting no Cliente
```typescript
// lib/rate-limiter.ts
import { LRUCache } from 'lru-cache';

const rateLimitCache = new LRUCache({
  max: 500,
  ttl: 60000, // 1 minuto
});

export function rateLimit(identifier: string, limit: number = 10) {
  const count = (rateLimitCache.get(identifier) as number) || 0;
  
  if (count >= limit) {
    return false;
  }
  
  rateLimitCache.set(identifier, count + 1);
  return true;
}
```

### Sanitização de Conteúdo
```typescript
// lib/sanitize.ts
import DOMPurify from 'isomorphic-dompurify';

export function sanitizeHTML(html: string): string {
  return DOMPurify.sanitize(html, {
    ALLOWED_TAGS: [
      'p', 'br', 'strong', 'em', 'u', 'h1', 'h2', 'h3', 'h4', 'h5', 'h6',
      'ul', 'ol', 'li', 'a', 'img', 'blockquote', 'code', 'pre',
      'table', 'thead', 'tbody', 'tr', 'th', 'td'
    ],
    ALLOWED_ATTR: ['href', 'src', 'alt', 'title', 'class', 'id'],
    ALLOW_DATA_ATTR: false,
  });
}
```

### Content Security Policy
```typescript
// next.config.js
const securityHeaders = [
  {
    key: 'Content-Security-Policy',
    value: `
      default-src 'self';
      script-src 'self' 'unsafe-eval' 'unsafe-inline' *.googletagmanager.com;
      style-src 'self' 'unsafe-inline';
      img-src 'self' data: https:;
      font-src 'self';
      connect-src 'self' ${process.env.NEXT_PUBLIC_API_URL};
      frame-src 'self' *.youtube.com;
    `.replace(/\s{2,}/g, ' ').trim()
  },
  {
    key: 'X-Frame-Options',
    value: 'SAMEORIGIN'
  },
  {
    key: 'X-Content-Type-Options',
    value: 'nosniff'
  },
  {
    key: 'Referrer-Policy',
    value: 'strict-origin-when-cross-origin'
  },
  {
    key: 'Permissions-Policy',
    value: 'camera=(), microphone=(), geolocation=()'
  }
];

module.exports = {
  async headers() {
    return [
      {
        source: '/:path*',
        headers: securityHeaders,
      },
    ];
  },
};
```

## Performance e Otimização

### Estratégia de Cache
```typescript
// app/blog/[slug]/page.tsx
export const revalidate = 3600; // ISR: revalidar a cada hora

// Gerar páginas estáticas para posts principais
export async function generateStaticParams() {
  const posts = await fetchPosts({ limit: 100 });
  
  return posts.map((post) => ({
    slug: post.slug,
  }));
}
```

### Lazy Loading de Imagens
```typescript
// components/OptimizedImage.tsx
import Image from 'next/image';

interface OptimizedImageProps {
  src: string;
  alt: string;
  width: number;
  height: number;
  priority?: boolean;
}

export function OptimizedImage({ 
  src, 
  alt, 
  width, 
  height, 
  priority = false 
}: OptimizedImageProps) {
  return (
    <Image
      src={src}
      alt={alt}
      width={width}
      height={height}
      priority={priority}
      loading={priority ? 'eager' : 'lazy'}
      placeholder="blur"
      blurDataURL="data:image/jpeg;base64,/9j/4AAQSkZJRg..."
      quality={85}
      sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
    />
  );
}
```

### Infinite Scroll com React Query
```typescript
// hooks/useInfinitePosts.ts
import { useInfiniteQuery } from '@tanstack/react-query';

export function useInfinitePosts(category?: string) {
  return useInfiniteQuery({
    queryKey: ['posts', category],
    queryFn: ({ pageParam = 1 }) => 
      fetchPosts({ page: pageParam, category }),
    getNextPageParam: (lastPage, pages) => {
      return lastPage.hasMore ? pages.length + 1 : undefined;
    },
    staleTime: 5 * 60 * 1000, // 5 minutos
  });
}
```

## SEO e Structured Data

### Metadata Dinâmico
```typescript
// app/blog/[slug]/page.tsx
import type { Metadata } from 'next';

export async function generateMetadata({ 
  params 
}: { 
  params: { slug: string } 
}): Promise<Metadata> {
  const post = await fetchPost(params.slug);
  
  return {
    title: post.seo.metaTitle,
    description: post.seo.metaDescription,
    keywords: post.seo.keywords,
    openGraph: {
      title: post.seo.metaTitle,
      description: post.seo.metaDescription,
      type: 'article',
      publishedTime: post.publishedAt,
      authors: [post.author.name],
      images: [
        {
          url: post.seo.ogImage || post.featuredImage.url,
          width: 1200,
          height: 630,
          alt: post.title,
        },
      ],
    },
    twitter: {
      card: 'summary_large_image',
      title: post.seo.metaTitle,
      description: post.seo.metaDescription,
      images: [post.seo.ogImage || post.featuredImage.url],
    },
    alternates: {
      canonical: post.seo.canonicalUrl,
    },
  };
}
```

### JSON-LD Schema
```typescript
// components/ArticleSchema.tsx
export function ArticleSchema({ post }: { post: BlogPost }) {
  const schema = {
    '@context': 'https://schema.org',
    '@type': 'Article',
    headline: post.title,
    description: post.excerpt,
    image: post.featuredImage.url,
    datePublished: post.publishedAt,
    dateModified: post.updatedAt,
    author: {
      '@type': 'Person',
      name: post.author.name,
    },
    publisher: {
      '@type': 'Organization',
      name: '{{BLOG_NAME}}',
      logo: {
        '@type': 'ImageObject',
        url: '{{BLOG_LOGO}}',
      },
    },
    mainEntityOfPage: {
      '@type': 'WebPage',
      '@id': `{{BASE_URL}}/blog/${post.slug}`,
    },
  };

  return (
    <script
      type="application/ld+json"
      dangerouslySetInnerHTML={{ __html: JSON.stringify(schema) }}
    />
  );
}
```

### Sitemap Automático
```typescript
// app/sitemap.ts
import { MetadataRoute } from 'next';

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const posts = await fetchAllPosts();
  const baseUrl = process.env.NEXT_PUBLIC_BASE_URL;

  const postEntries = posts.map((post) => ({
    url: `${baseUrl}/blog/${post.slug}`,
    lastModified: new Date(post.updatedAt),
    changeFrequency: 'weekly' as const,
    priority: 0.8,
  }));

  const staticPages = [
    {
      url: baseUrl,
      lastModified: new Date(),
      changeFrequency: 'daily' as const,
      priority: 1,
    },
    {
      url: `${baseUrl}/legal/privacy-policy`,
      lastModified: new Date(),
      changeFrequency: 'monthly' as const,
      priority: 0.3,
    },
    // ... outras páginas legais
  ];

  return [...staticPages, ...postEntries];
}
```

## Estrutura de Projeto Completa

```
blog-child/
├── app/
│   ├── layout.tsx              # Layout raiz com providers
│   ├── page.tsx                # Homepage
│   ├── blog/
│   │   ├── page.tsx            # Lista de posts
│   │   ├── [slug]/
│   │   │   └── page.tsx        # Post individual
│   │   └── category/
│   │       └── [slug]/
│   │           └── page.tsx    # Posts por categoria
│   ├── legal/
│   │   ├── cookies-policy/
│   │   ├── privacy-policy/
│   │   ├── terms-of-service/
│   │   ├── security-policy/
│   │   └── return-policy/
│   ├── sitemap.ts
│   ├── robots.ts
│   └── rss.xml/
│       └── route.ts
├── components/
│   ├── layout/
│   │   ├── Navbar/
│   │   │   ├── StickyNavbar.tsx
│   │   │   ├── FixedNavbar.tsx
│   │   │   ├── TransparentNavbar.tsx
│   │   │   ├── MegaMenuNavbar.tsx
│   │   │   └── StaticNavbar.tsx
│   │   ├── Hero/
│   │   │   ├── CarouselHero.tsx
│   │   │   ├── FeaturedHero.tsx
│   │   │   ├── SplitHero.tsx
│   │   │   ├── VideoHero.tsx
│   │   │   └── MinimalHero.tsx
│   │   └── Footer/
│   │       ├── MinimalFooter.tsx
│   │       ├── StandardFooter.tsx
│   │       ├── MegaFooter.tsx
│   │       ├── NewsletterFooter.tsx
│   │       └── SocialFooter.tsx
│   ├── sections/
│   │   ├── Categories/
│   │   ├── Highlights/
│   │   └── CTA/
│   ├── blog/
│   │   ├── PostCard.tsx
│   │   ├── PostGrid.tsx
│   │   ├── PostList.tsx
│   │   ├── RelatedPosts.tsx
│   │   └── TableOfContents.tsx
│   ├── common/
│   │   ├── OptimizedImage.tsx
│   │   ├── LoadingSpinner.tsx
│   │   └── ErrorBoundary.tsx
│   └── seo/
│       ├── ArticleSchema.tsx
│       └── BreadcrumbSchema.tsx
├── lib/
│   ├── api-client.ts
│   ├── sanitize.ts
│   ├── rate-limiter.ts
│   └── utils.ts
├── hooks/
│   ├── useInfinitePosts.ts
│   ├── usePost.ts
│   └── useBlogConfig.ts
├── config/
│   ├── blog-config.ts          # Configuração específica do blog
│   ├── layouts.ts              # Definições de layouts
│   └── legal-templates.ts      # Templates de páginas legais
├── styles/
│   └── globals.css
├── public/
│   ├── images/
│   └── fonts/
├── .env.local
├── .env.example
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

## Configuração do Blog

### Arquivo de Configuração
```typescript
// config/blog-config.ts
export const blogConfig = {
  blogId: process.env.NEXT_PUBLIC_BLOG_ID!,
  name: process.env.NEXT_PUBLIC_BLOG_NAME!,
  description: process.env.NEXT_PUBLIC_BLOG_DESCRIPTION!,
  url: process.env.NEXT_PUBLIC_BASE_URL!,
  
  // Layout escolhido
  layout: 'magazine', // ou 'minimal', 'corporate', 'creative', 'tech'
  
  // Componentes
  navbar: {
    type: 'sticky',
    // ... resto da configuração
  },
  
  hero: {
    type: 'carousel',
    // ... resto da configuração
  },
  
  footer: {
    type: 'standard',
    // ... resto da configuração
  },
  
  // Features
  features: {
    search: true,
    darkMode: true,
    comments: false,
    newsletter: true,
    relatedPosts: true,
    tableOfContents: true,
    readingProgress: true,
  },
  
  // SEO
  seo: {
    defaultTitle: process.env.NEXT_PUBLIC_BLOG_NAME!,
    titleTemplate: '%s | ' + process.env.NEXT_PUBLIC_BLOG_NAME!,
    defaultDescription: process.env.NEXT_PUBLIC_BLOG_DESCRIPTION!,
    siteUrl: process.env.NEXT_PUBLIC_BASE_URL!,
    twitterHandle: process.env.NEXT_PUBLIC_TWITTER_HANDLE,
  },
  
  // Analytics
  analytics: {
    googleAnalytics: process.env.NEXT_PUBLIC_GA_ID,
    googleTagManager: process.env.NEXT_PUBLIC_GTM_ID,
  },
  
  // Ads
  ads: {
    googleAdsense: process.env.NEXT_PUBLIC_ADSENSE_ID,
    headerAd: true,
    sidebarAd: true,
    inContentAd: true,
    footerAd: false,
  },
  
  // Legal
  legal: {
    companyName: '{{COMPANY_NAME}}',
    companyAddress: '{{COMPANY_ADDRESS}}',
    dpoEmail: '{{DPO_EMAIL}}',
    securityEmail: '{{SECURITY_EMAIL}}',
  },
};
```

### Variáveis de Ambiente
```env
# .env.example

# Blog Identity
NEXT_PUBLIC_BLOG_ID=blog-001
NEXT_PUBLIC_BLOG_NAME=Meu Blog Incrível
NEXT_PUBLIC_BLOG_DESCRIPTION=Conteúdo de qualidade sobre diversos assuntos
NEXT_PUBLIC_BASE_URL=https://meublog.com.br

# API
NEXT_PUBLIC_API_URL=https://api.central.com/v1
API_KEY=sk_live_xxxxxxxxxxxxx

# Analytics
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
NEXT_PUBLIC_GTM_ID=GTM-XXXXXXX

# Ads
NEXT_PUBLIC_ADSENSE_ID=ca-pub-xxxxxxxxxxxxxxxx

# Social
NEXT_PUBLIC_TWITTER_HANDLE=@meublog
NEXT_PUBLIC_FACEBOOK_PAGE=meublog
NEXT_PUBLIC_INSTAGRAM_HANDLE=meublog

# Legal
COMPANY_NAME=Minha Empresa LTDA
COMPANY_ADDRESS=Rua Exemplo, 123 - São Paulo, SP
DPO_EMAIL=dpo@meublog.com.br
SECURITY_EMAIL=security@meublog.com.br
```

## Deployment e CI/CD

### Dockerfile
```dockerfile
FROM node:20-alpine AS base

# Dependencies
FROM base AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app

COPY package.json package-lock.json* ./
RUN npm ci

# Builder
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

ENV NEXT_TELEMETRY_DISABLED 1

RUN npm run build

# Runner
FROM base AS runner
WORKDIR /app

ENV NODE_ENV production
ENV NEXT_TELEMETRY_DISABLED 1

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000

ENV PORT 3000
ENV HOSTNAME "0.0.0.0"

CMD ["node", "server.js"]
```

### GitHub Actions
```yaml
# .github/workflows/deploy.yml
name: Deploy Blog Child

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Build
        env:
          NEXT_PUBLIC_API_URL: ${{ secrets.API_URL }}
          API_KEY: ${{ secrets.API_KEY }}
        run: npm run build
      
      - name: Deploy to VPS
        uses: appleboy/ssh-action@v1.0.0
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            cd /var/www/blog-child
            git pull origin main
            docker-compose down
            docker-compose up -d --build
```

## Checklist de Setup

### Pré-Deploy
- [ ] Configurar variáveis de ambiente
- [ ] Testar conexão com API central
- [ ] Escolher e configurar layout
- [ ] Personalizar cores e logo
- [ ] Configurar páginas legais com dados da empresa
- [ ] Testar todos os componentes
- [ ] Configurar analytics
- [ ] Configurar ads (se aplicável)
- [ ] Gerar favicon e app icons
- [ ] Testar performance (Lighthouse)
- [ ] Testar SEO (meta tags, structured data)
- [ ] Testar segurança (CSP, headers)

### Pós-Deploy
- [ ] Verificar sitemap
- [ ] Submeter sitemap ao Google Search Console
- [ ] Configurar domínio e SSL
- [ ] Testar formulários de contato/newsletter
- [ ] Monitorar erros (Sentry/similar)
- [ ] Configurar backup automático
- [ ] Documentar processo de deploy
- [ ] Treinar equipe no painel admin

## Monitoramento e Manutenção

### Health Check
```typescript
// app/api/health/route.ts
export async function GET() {
  try {
    // Verificar conexão com API
    const apiHealth = await fetch(
      `${process.env.NEXT_PUBLIC_API_URL}/health`,
      {
        headers: {
          'X-API-Key': process.env.API_KEY!,
        },
      }
    );

    if (!apiHealth.ok) {
      throw new Error('API não disponível');
    }

    return Response.json({
      status: 'healthy',
      timestamp: new Date().toISOString(),
      version: process.env.npm_package_version,
    });
  } catch (error) {
    return Response.json(
      {
        status: 'unhealthy',
        error: error.message,
        timestamp: new Date().toISOString(),
      },
      { status: 503 }
    );
  }
}
```

### Error Tracking
```typescript
// lib/error-tracking.ts
export function logError(error: Error, context?: any) {
  // Enviar para serviço de tracking (Sentry, etc)
  console.error('Error:', error);
  console.error('Context:', context);
  
  // Poderia enviar para API central também
  if (process.env.NODE_ENV === 'production') {
    // sendToErrorTracking(error, context);
  }
}
```

## Considerações Finais

### Boas Práticas
1. **Segurança**: Sempre validar e sanitizar dados da API
2. **Performance**: Usar ISR e cache agressivamente
3. **SEO**: Implementar todas as meta tags e structured data
4. **Acessibilidade**: Seguir WCAG 2.1 AA
5. **Mobile-first**: Design responsivo obrigatório
6. **Analytics**: Monitorar performance e comportamento

