# Next.js CheatSheet 

---

## Table des Matières

### Fondamentaux
- [Installation & Configuration](#installation--configuration)
  - [Créer un nouveau projet](#créer-un-nouveau-projet)
  - [Structure du projet](#structure-du-projet-app-router)
  - [Configuration de base](#configuration-de-base-nextconfigjs)

### Routing
- [Routing (App Router)](#routing-app-router)
  - [Pages et Routes](#pages-et-routes)
  - [Layouts](#layouts)
  - [Templates](#templates)
  - [Loading States](#loading-states)
  - [Error Handling](#error-handling)
  - [Route Groups](#route-groups)
  - [Parallel Routes](#parallel-routes)
  - [Intercepting Routes](#intercepting-routes)

### Navigation
- [Navigation](#navigation)
  - [Link Component](#link-component)
  - [useRouter Hook](#userouter-hook)
  - [usePathname & useSearchParams](#usepathname--usesearchparams)
  - [redirect](#redirect)

### Components
- [Server vs Client Components](#server-vs-client-components)
  - [Server Components](#server-components-par-défaut)
  - [Client Components](#client-components)
  - [Composition Pattern](#composition-pattern)

### Data Management
- [Data Fetching](#data-fetching)
  - [fetch API](#fetch-api-étendu)
  - [Suspense & Streaming](#suspense--streaming)
  - [Parallel Data Fetching](#parallel-data-fetching)
  - [Sequential Data Fetching](#sequential-data-fetching)
- [Server Actions](#server-actions)
  - [Définition](#définition)
  - [Utilisation dans un formulaire](#utilisation-dans-un-formulaire)
  - [useFormState & useFormStatus](#avec-useformstate)
- [Caching](#caching)
  - [Types de cache](#types-de-cache)
  - [Revalidation](#revalidation)
  - [Opt-out du cache](#opt-out-du-cache)

### Médias & Assets
- [Images](#images)
  - [Image Component](#image-component)
  - [Configuration](#configuration-des-images)
- [Fonts](#fonts-nextfont)
  - [Google Fonts](#google-fonts)
  - [Fonts locales](#fonts-locales)

### SEO & Metadata
- [Metadata API](#metadata-api)
  - [Metadata statique](#metadata-statique)
  - [Metadata dynamique](#metadata-dynamique)
  - [Metadata template](#metadata-template)
- [SEO Best Practices](#seo-best-practices)
  - [Sitemap](#sitemap)
  - [Robots.txt](#robotstxt)
  - [Structured Data](#structured-data-json-ld)

### API & Backend
- [API Routes](#api-routes)
  - [Route Handlers](#route-handlers)
  - [Route dynamique](#route-dynamique)
  - [Headers et Cookies](#headers-et-cookies)
  - [Streaming Response](#streaming-response)
- [Middleware](#middleware)

### Génération Statique
- [Static Site Generation (SSG)](#static-site-generation-ssg)
  - [generateStaticParams](#generatestaticparams)
  - [Configuration](#configuration-1)

### Configuration & Optimisation
- [Configuration Avancée](#configuration-avancée)
  - [Variables d'environnement](#variables-denvironnement)
  - [next.config.js complet](#nextconfigjs-complet)
- [Performance Tips](#performance-tips)

### Testing & Quality
- [Testing](#testing)
  - [Jest Configuration](#jest-configuration)
  - [Test Example](#test-example)

### Styling
- [Styling](#styling)
  - [Tailwind CSS](#tailwind-css)
  - [CSS Modules](#css-modules)
  - [CSS-in-JS](#css-in-js-styled-components)

### Features Avancées
- [Internationalisation (i18n)](#internationalisation-i18n)
- [Progressive Web App (PWA)](#progressive-web-app-pwa)
- [Patterns Avancés](#patterns-avancés)
- [Nouveautés Next.js 15](#nouveautés-nextjs-15)

### Déploiement
- [Déploiement](#déploiement)
  - [Build](#build)
  - [Optimisations](#optimisations)

### Outils & Ressources
- [Outils et Commandes Utiles](#outils-et-commandes-utiles)
- [Ressources et Communauté](#ressources-et-communauté)

---

## Installation & Configuration

### Créer un nouveau projet

```bash
npx create-next-app@latest mon-app
# Options: TypeScript, ESLint, Tailwind CSS, App Router, src/ directory
```

### Structure du projet (App Router)

```
mon-app/
├── app/
│   ├── layout.tsx          # Layout racine
│   ├── page.tsx            # Page d'accueil
│   ├── loading.tsx         # UI de chargement
│   ├── error.tsx           # Gestion d'erreurs
│   ├── not-found.tsx       # Page 404
│   └── api/
│       └── route.ts        # API Routes
├── public/                 # Fichiers statiques
├── components/             # Composants réutilisables
└── next.config.js          # Configuration

```

### Configuration de base (next.config.js)

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'example.com',
      },
    ],
  },
  experimental: {
    serverActions: {
      bodySizeLimit: '2mb',
    },
  },
}

module.exports = nextConfig
```

---

## Routing (App Router)

### Pages et Routes

```tsx
// app/page.tsx - Page d'accueil (/)
export default function Home() {
  return <h1>Accueil</h1>
}

// app/about/page.tsx - Route /about
export default function About() {
  return <h1>À propos</h1>
}

// app/blog/[slug]/page.tsx - Route dynamique
export default function BlogPost({ params }: { params: { slug: string } }) {
  return <h1>Article: {params.slug}</h1>
}

// app/shop/[...slug]/page.tsx - Catch-all routes
// Correspond à /shop/a, /shop/a/b, /shop/a/b/c
export default function Shop({ params }: { params: { slug: string[] } }) {
  return <div>{params.slug.join('/')}</div>
}

// app/docs/[[...slug]]/page.tsx - Optional catch-all
// Correspond aussi à /docs
```

### Layouts

```tsx
// app/layout.tsx - Layout racine (obligatoire)
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="fr">
      <body>{children}</body>
    </html>
  )
}

// app/dashboard/layout.tsx - Layout imbriqué
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <div>
      <nav>Navigation Dashboard</nav>
      {children}
    </div>
  )
}
```

### Templates

```tsx
// app/template.tsx - Remonte une nouvelle instance à chaque navigation
export default function Template({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}
```

### Loading States

```tsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <div>Chargement...</div>
}
```

### Error Handling

```tsx
// app/error.tsx
'use client'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div>
      <h2>Une erreur est survenue!</h2>
      <button onClick={() => reset()}>Réessayer</button>
    </div>
  )
}

// app/global-error.tsx - Erreurs du layout racine
'use client'

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <html>
      <body>
        <h2>Erreur globale!</h2>
        <button onClick={() => reset()}>Réessayer</button>
      </body>
    </html>
  )
}
```

### Route Groups

```tsx
// app/(marketing)/about/page.tsx
// app/(marketing)/contact/page.tsx
// app/(shop)/products/page.tsx
// Les parenthèses n'affectent pas l'URL
```

### Parallel Routes

```tsx
// app/layout.tsx
export default function Layout({
  children,
  team,
  analytics,
}: {
  children: React.ReactNode
  team: React.ReactNode
  analytics: React.ReactNode
}) {
  return (
    <>
      {children}
      {team}
      {analytics}
    </>
  )
}

// app/@team/page.tsx
// app/@analytics/page.tsx
```

### Intercepting Routes

```tsx
// app/photos/[id]/page.tsx - Page normale
// app/@modal/(.)photos/[id]/page.tsx - Intercepte la route
// (.) même niveau, (..) niveau parent, (...) racine
```

---

## Navigation

### Link Component

```tsx
import Link from 'next/link'

// Navigation de base
<Link href="/about">À propos</Link>

// Navigation dynamique
<Link href={`/blog/${post.slug}`}>Lire l'article</Link>

// Avec remplacement d'historique
<Link href="/dashboard" replace>Dashboard</Link>

// Préchargement désactivé
<Link href="/heavy-page" prefetch={false}>Page lourde</Link>

// Scroll désactivé
<Link href="/section" scroll={false}>Section</Link>
```

### useRouter Hook

```tsx
'use client'
import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button onClick={() => router.push('/dashboard')}>
      Aller au dashboard
    </button>
  )
}

// Méthodes disponibles
router.push('/dashboard')          // Naviguer
router.replace('/login')          // Remplacer
router.refresh()                  // Rafraîchir
router.prefetch('/about')         // Précharger
router.back()                     // Retour
router.forward()                  // Avant
```

### usePathname & useSearchParams

```tsx
'use client'
import { usePathname, useSearchParams } from 'next/navigation'

export default function Component() {
  const pathname = usePathname() // '/dashboard/settings'
  const searchParams = useSearchParams() // ?search=value
  const search = searchParams.get('search')

  return <div>Pathname: {pathname}</div>
}
```

### redirect

```tsx
import { redirect } from 'next/navigation'

export default async function Page() {
  const user = await getUser()
  
  if (!user) {
    redirect('/login')
  }
  
  return <div>Contenu protégé</div>
}
```

---

## Server vs Client Components

### Server Components (par défaut)

```tsx
// app/page.tsx - Server Component par défaut
async function getData() {
  const res = await fetch('https://api.example.com/data')
  return res.json()
}

export default async function Page() {
  const data = await getData()
  return <div>{data.title}</div>
}

// Avantages:
// - Accès direct à la base de données
// - Garde les secrets côté serveur
// - Réduit la taille du bundle JS
// - Streaming et Suspense
```

### Client Components

```tsx
'use client' // Directive obligatoire en haut du fichier

import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  )
}

// Utiliser 'use client' quand:
// - Hooks React (useState, useEffect, etc.)
// - Event listeners (onClick, onChange, etc.)
// - APIs du navigateur (localStorage, etc.)
// - Classes React
```

### Composition Pattern

```tsx
// Server Component
import ClientComponent from './client-component'

async function getData() {
  const data = await fetch('...')
  return data.json()
}

export default async function Page() {
  const data = await getData()
  
  return (
    <div>
      <h1>Server Content</h1>
      <ClientComponent data={data} />
    </div>
  )
}
```

---

## Data Fetching

### fetch API (étendu)

```tsx
// Cache par défaut (force-cache)
const data = await fetch('https://api.example.com/data')

// Pas de cache
const data = await fetch('https://api.example.com/data', {
  cache: 'no-store'
})

// Revalidation toutes les 60 secondes
const data = await fetch('https://api.example.com/data', {
  next: { revalidate: 60 }
})

// Tag pour revalidation on-demand
const data = await fetch('https://api.example.com/data', {
  next: { tags: ['posts'] }
})
```

### Suspense & Streaming

```tsx
import { Suspense } from 'react'

async function Posts() {
  const posts = await fetch('...')
  return <div>{/* afficher posts */}</div>
}

export default function Page() {
  return (
    <div>
      <h1>Mon Blog</h1>
      <Suspense fallback={<div>Chargement des posts...</div>}>
        <Posts />
      </Suspense>
    </div>
  )
}
```

### Parallel Data Fetching

```tsx
async function getData() {
  // Lancer les requêtes en parallèle
  const [user, posts] = await Promise.all([
    fetch('https://api.example.com/user'),
    fetch('https://api.example.com/posts')
  ])

  return {
    user: await user.json(),
    posts: await posts.json()
  }
}
```

### Sequential Data Fetching

```tsx
async function getData() {
  const user = await fetch('https://api.example.com/user')
  const userData = await user.json()
  
  // Dépend de user
  const posts = await fetch(`https://api.example.com/posts?userId=${userData.id}`)
  const postsData = await posts.json()
  
  return { user: userData, posts: postsData }
}
```

---

## Server Actions

### Définition

```tsx
// app/actions.ts
'use server'

export async function createPost(formData: FormData) {
  const title = formData.get('title')
  const content = formData.get('content')
  
  // Logique serveur
  await db.post.create({
    data: { title, content }
  })
  
  revalidatePath('/blog')
}
```

### Utilisation dans un formulaire

```tsx
// app/new-post/page.tsx
import { createPost } from '@/app/actions'

export default function NewPost() {
  return (
    <form action={createPost}>
      <input name="title" type="text" required />
      <textarea name="content" required />
      <button type="submit">Créer</button>
    </form>
  )
}
```

### Avec useFormState

```tsx
'use client'
import { useFormState } from 'react-dom'
import { createPost } from '@/app/actions'

export default function Form() {
  const [state, formAction] = useFormState(createPost, { message: '' })

  return (
    <form action={formAction}>
      <input name="title" />
      {state.message && <p>{state.message}</p>}
      <button type="submit">Soumettre</button>
    </form>
  )
}
```

### Avec useFormStatus

```tsx
'use client'
import { useFormStatus } from 'react-dom'

function SubmitButton() {
  const { pending } = useFormStatus()

  return (
    <button type="submit" disabled={pending}>
      {pending ? 'Envoi...' : 'Envoyer'}
    </button>
  )
}
```

### Server Actions inline

```tsx
export default function Page() {
  async function create(formData: FormData) {
    'use server'
    const name = formData.get('name')
    // Logique...
  }

  return (
    <form action={create}>
      <input name="name" />
      <button type="submit">Créer</button>
    </form>
  )
}
```

---

## Caching

### Types de cache

```tsx
// 1. Request Memoization (pendant un render)
const data = await fetch('https://api.example.com/data')

// 2. Data Cache (persistant entre requêtes)
fetch('url', { cache: 'force-cache' }) // Par défaut
fetch('url', { cache: 'no-store' })    // Pas de cache

// 3. Full Route Cache (HTML + RSC payload)
// Automatique pour les routes statiques

// 4. Router Cache (côté client)
// Cache les segments de route visités
```

### Revalidation

```tsx
// Revalidation basée sur le temps
export const revalidate = 60 // secondes

// Revalidation on-demand
import { revalidatePath, revalidateTag } from 'next/cache'

// Dans une Server Action
async function action() {
  'use server'
  revalidatePath('/blog')
  revalidateTag('posts')
}
```

### Opt-out du cache

```tsx
// Page entière
export const dynamic = 'force-dynamic'
export const revalidate = 0

// Fetch spécifique
fetch('url', { cache: 'no-store' })
fetch('url', { next: { revalidate: 0 } })
```

---

## Images

### Image Component

```tsx
import Image from 'next/image'

// Image avec dimensions
<Image
  src="/photo.jpg"
  width={500}
  height={300}
  alt="Description"
/>

// Image responsive (fill)
<div style={{ position: 'relative', width: '100%', height: '400px' }}>
  <Image
    src="/photo.jpg"
    fill
    style={{ objectFit: 'cover' }}
    alt="Description"
  />
</div>

// Image externe
<Image
  src="https://example.com/photo.jpg"
  width={500}
  height={300}
  alt="Description"
/>

// Priorité (LCP)
<Image
  src="/hero.jpg"
  width={1200}
  height={600}
  priority
  alt="Hero"
/>

// Lazy loading
<Image
  src="/image.jpg"
  width={500}
  height={300}
  loading="lazy"
  alt="Lazy"
/>

// Placeholder blur
<Image
  src="/photo.jpg"
  width={500}
  height={300}
  placeholder="blur"
  blurDataURL="data:image/..."
  alt="With blur"
/>
```

### Configuration des images

```javascript
// next.config.js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'example.com',
        port: '',
        pathname: '/images/**',
      },
    ],
    formats: ['image/avif', 'image/webp'],
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
  },
}
```

---

## Fonts (next/font)

### Google Fonts

```tsx
import { Inter, Roboto_Mono } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

const robotoMono = Roboto_Mono({
  subsets: ['latin'],
  weight: ['400', '700'],
  variable: '--font-roboto-mono',
})

export default function RootLayout({ children }) {
  return (
    <html lang="fr" className={inter.className}>
      <body className={robotoMono.variable}>{children}</body>
    </html>
  )
}
```

### Fonts locales

```tsx
import localFont from 'next/font/local'

const myFont = localFont({
  src: './my-font.woff2',
  display: 'swap',
  variable: '--font-my-font',
})

export default function RootLayout({ children }) {
  return (
    <html lang="fr" className={myFont.variable}>
      <body>{children}</body>
    </html>
  )
}
```

---

## Metadata API

### Metadata statique

```tsx
// app/page.tsx
import { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Mon Site',
  description: 'Description de mon site',
  keywords: ['Next.js', 'React', 'JavaScript'],
  authors: [{ name: 'John Doe' }],
  openGraph: {
    title: 'Mon Site',
    description: 'Description',
    url: 'https://example.com',
    siteName: 'Mon Site',
    images: [
      {
        url: 'https://example.com/og.jpg',
        width: 1200,
        height: 630,
      },
    ],
    locale: 'fr_FR',
    type: 'website',
  },
  twitter: {
    card: 'summary_large_image',
    title: 'Mon Site',
    description: 'Description',
    images: ['https://example.com/twitter.jpg'],
  },
  robots: {
    index: true,
    follow: true,
  },
}
```

### Metadata dynamique

```tsx
// app/blog/[slug]/page.tsx
export async function generateMetadata(
  { params }: { params: { slug: string } }
): Promise<Metadata> {
  const post = await getPost(params.slug)

  return {
    title: post.title,
    description: post.excerpt,
    openGraph: {
      title: post.title,
      description: post.excerpt,
      images: [post.image],
    },
  }
}
```

### Metadata template

```tsx
// app/layout.tsx
export const metadata = {
  title: {
    template: '%s | Mon Site',
    default: 'Mon Site',
  },
}

// app/blog/page.tsx
export const metadata = {
  title: 'Blog', // Devient "Blog | Mon Site"
}
```

---

## API Routes

### Route Handlers

```tsx
// app/api/hello/route.ts
import { NextRequest, NextResponse } from 'next/server'

// GET /api/hello
export async function GET(request: NextRequest) {
  return NextResponse.json({ message: 'Hello World' })
}

// POST /api/hello
export async function POST(request: NextRequest) {
  const body = await request.json()
  return NextResponse.json({ received: body })
}

// Autres méthodes: PUT, DELETE, PATCH, HEAD, OPTIONS
```

### Route dynamique

```tsx
// app/api/posts/[id]/route.ts
export async function GET(
  request: NextRequest,
  { params }: { params: { id: string } }
) {
  const post = await getPost(params.id)
  return NextResponse.json(post)
}
```

### Headers et Cookies

```tsx
import { cookies, headers } from 'next/headers'

export async function GET(request: NextRequest) {
  // Headers
  const headersList = headers()
  const userAgent = headersList.get('user-agent')

  // Cookies
  const cookieStore = cookies()
  const token = cookieStore.get('token')
  
  // Définir un cookie
  cookieStore.set('name', 'value', { httpOnly: true })

  return NextResponse.json({ userAgent })
}
```

### Redirections

```tsx
export async function GET() {
  return NextResponse.redirect(new URL('/new-url', request.url))
}
```

### Streaming Response

```tsx
export async function GET() {
  const encoder = new TextEncoder()
  
  const stream = new ReadableStream({
    async start(controller) {
      controller.enqueue(encoder.encode('Hello '))
      await new Promise(resolve => setTimeout(resolve, 1000))
      controller.enqueue(encoder.encode('World'))
      controller.close()
    },
  })

  return new Response(stream)
}
```

---

## Middleware

```tsx
// middleware.ts (à la racine du projet)
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Vérifier l'authentification
  const token = request.cookies.get('token')

  if (!token) {
    return NextResponse.redirect(new URL('/login', request.url))
  }

  // Modifier les headers
  const response = NextResponse.next()
  response.headers.set('x-custom-header', 'value')
  
  return response
}

// Configuration du matcher
export const config = {
  matcher: [
    '/dashboard/:path*',
    '/admin/:path*',
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
}
```

### Conditional Middleware

```tsx
export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith('/admin')) {
    // Logique pour /admin
    const isAdmin = checkAdmin(request)
    if (!isAdmin) {
      return NextResponse.redirect(new URL('/login', request.url))
    }
  }

  if (request.nextUrl.pathname.startsWith('/api')) {
    // Logique pour /api
    const response = NextResponse.next()
    response.headers.set('x-api-version', '1.0')
    return response
  }

  return NextResponse.next()
}
```

---

## Static Site Generation (SSG)

### generateStaticParams

```tsx
// app/blog/[slug]/page.tsx
export async function generateStaticParams() {
  const posts = await getPosts()

  return posts.map((post) => ({
    slug: post.slug,
  }))
}

export default async function Page({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug)
  return <article>{post.content}</article>
}
```

### Configuration

```tsx
// Route statique
export const dynamic = 'error' // force static

// Générer les pages à la demande
export const dynamicParams = true // par défaut
export const dynamicParams = false // 404 pour params non générés

// Revalidation incrémentale (ISR)
export const revalidate = 60 // secondes
```

---

## Configuration Avancée

### Variables d'environnement

```bash
# .env.local
DATABASE_URL=mysql://...
NEXT_PUBLIC_API_URL=https://api.example.com
```

```tsx
// Côté serveur uniquement
const dbUrl = process.env.DATABASE_URL

// Accessible côté client (préfixe NEXT_PUBLIC_)
const apiUrl = process.env.NEXT_PUBLIC_API_URL
```

### next.config.js complet

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Mode strict
  reactStrictMode: true,

  // Images
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '**.example.com',
      },
    ],
    formats: ['image/avif', 'image/webp'],
  },

  // Redirections
  async redirects() {
    return [
      {
        source: '/old-path',
        destination: '/new-path',
        permanent: true,
      },
    ]
  },

  // Rewrites
  async rewrites() {
    return [
      {
        source: '/api/:path*',
        destination: 'https://api.example.com/:path*',
      },
    ]
  },

  // Headers
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'X-DNS-Prefetch-Control',
            value: 'on',
          },
        ],
      },
    ]
  },

  // Experimental
  experimental: {
    serverActions: {
      bodySizeLimit: '2mb',
    },
  },

  // Compression
  compress: true,

  // Powered by header
  poweredByHeader: false,

  // Trailing slash
  trailingSlash: false,
}

module.exports = nextConfig
```

---

## Testing

### Jest Configuration

```javascript
// jest.config.js
const nextJest = require('next/jest')

const createJestConfig = nextJest({
  dir: './',
})

const customJestConfig = {
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  testEnvironment: 'jest-environment-jsdom',
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/$1',
  },
}

module.exports = createJestConfig(customJestConfig)
```

### Test Example

```tsx
// __tests__/page.test.tsx
import { render, screen } from '@testing-library/react'
import Home from '@/app/page'

describe('Home', () => {
  it('renders a heading', () => {
    render(<Home />)
    const heading = screen.getByRole('heading', { level: 1 })
    expect(heading).toBeInTheDocument()
  })
})
```

---

## Progressive Web App (PWA)

```javascript
// next.config.js avec next-pwa
const withPWA = require('next-pwa')({
  dest: 'public',
  disable: process.env.NODE_ENV === 'development',
})

module.exports = withPWA({
  // ... autres configs
})
```

---

## Déploiement

### Build

```bash
# Build de production
npm run build

# Démarrer en production
npm run start

# Analyser le bundle
npm run build -- --profile
```

### Variables d'environnement de build

```bash
# .env.production
NEXT_PUBLIC_API_URL=https://production-api.com
```

### Optimisations

```javascript
// next.config.js
module.exports = {
  // Minification
  swcMinify: true,

  // Analyse du bundle
  webpack: (config, { isServer }) => {
    if (!isServer) {
      config.resolve.fallback = {
        fs: false,
        net: false,
        tls: false,
      }
    }
    return config
  },

  // Output standalone
  output: 'standalone', // Pour Docker
}
```

---

## Styling

### Tailwind CSS

```javascript
// tailwind.config.js
module.exports = {
  content: [
    './app/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### CSS Modules

```tsx
// styles/Home.module.css
.container {
  padding: 2rem;
}

// app/page.tsx
import styles from './page.module.css'

export default function Home() {
  return <div className={styles.container}>Hello</div>
}
```

### CSS-in-JS (Styled Components)

```tsx
// app/registry.tsx
'use client'
import { useServerInsertedHTML } from 'next/navigation'
import { ServerStyleSheet, StyleSheetManager } from 'styled-components'

export default function StyledComponentsRegistry({
  children,
}: {
  children: React.ReactNode
}) {
  const [styledComponentsStyleSheet] = useState(() => new ServerStyleSheet())

  useServerInsertedHTML(() => {
    const styles = styledComponentsStyleSheet.getStyleElement()
    styledComponentsStyleSheet.instance.clearTag()
    return <>{styles}</>
  })

  return (
    <StyleSheetManager sheet={styledComponentsStyleSheet.instance}>
      {children}
    </StyleSheetManager>
  )
}
```

---

## Internationalisation (i18n)

### Middleware i18n

```tsx
// middleware.ts
import { match } from '@formatjs/intl-localematcher'
import Negotiator from 'negotiator'

const locales = ['en', 'fr', 'es']
const defaultLocale = 'en'

function getLocale(request: NextRequest): string {
  const headers = { 'accept-language': request.headers.get('accept-language') || '' }
  const languages = new Negotiator({ headers }).languages()
  return match(languages, locales, defaultLocale)
}

export function middleware(request: NextRequest) {
  const { pathname } = request.nextUrl
  const pathnameHasLocale = locales.some(
    (locale) => pathname.startsWith(`/${locale}/`) || pathname === `/${locale}`
  )

  if (pathnameHasLocale) return

  const locale = getLocale(request)
  request.nextUrl.pathname = `/${locale}${pathname}`
  return NextResponse.redirect(request.nextUrl)
}

export const config = {
  matcher: ['/((?!_next|api|favicon.ico).*)'],
}
```

### Dictionnaires

```tsx
// app/[lang]/dictionaries.ts
const dictionaries = {
  en: () => import('./dictionaries/en.json').then((module) => module.default),
  fr: () => import('./dictionaries/fr.json').then((module) => module.default),
}

export const getDictionary = async (locale: string) => dictionaries[locale]()

// app/[lang]/page.tsx
export default async function Page({ params: { lang } }) {
  const dict = await getDictionary(lang)
  return <h1>{dict.welcome}</h1>
}
```

---

## SEO Best Practices

### Sitemap

```tsx
// app/sitemap.ts
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://example.com',
      lastModified: new Date(),
      changeFrequency: 'yearly',
      priority: 1,
    },
    {
      url: 'https://example.com/about',
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.8,
    },
  ]
}
```

### Robots.txt

```tsx
// app/robots.ts
import { MetadataRoute } from 'next'

export default function robots(): MetadataRoute.Robots {
  return {
    rules: {
      userAgent: '*',
      allow: '/',
      disallow: '/private/',
    },
    sitemap: 'https://example.com/sitemap.xml',
  }
}
```

### Structured Data (JSON-LD)

```tsx
export default function BlogPost({ post }) {
  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'BlogPosting',
    headline: post.title,
    datePublished: post.publishedAt,
    author: {
      '@type': 'Person',
      name: post.author,
    },
  }

  return (
    <>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      <article>{/* contenu */}</article>
    </>
  )
}
```

---

## Patterns Avancés

### Parallel Routes avec Conditional Rendering

```tsx
// app/layout.tsx
export default function Layout({
  children,
  modal,
}: {
  children: React.ReactNode
  modal: React.ReactNode
}) {
  return (
    <>
      {children}
      {modal}
    </>
  )
}

// app/@modal/default.tsx
export default function Default() {
  return null
}
```

### Error Boundaries par segment

```tsx
// app/dashboard/error.tsx
'use client'

export default function DashboardError({ error, reset }) {
  return (
    <div>
      <h2>Erreur dans le dashboard</h2>
      <button onClick={reset}>Réessayer</button>
    </div>
  )
}
```

### Data Fetching avec React Query

```tsx
'use client'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

export default function Providers({ children }) {
  const [queryClient] = useState(() => new QueryClient())

  return (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  )
}
```

---

## Outils et Commandes Utiles

```bash
# Développement
npm run dev              # Démarrer en dev
npm run build            # Build production
npm run start            # Démarrer en prod
npm run lint             # Linter

# Analyse
npx @next/bundle-analyzer  # Analyser le bundle

# Nettoyage
rm -rf .next             # Supprimer le cache

# Mise à jour
npx next@latest upgrade  # Mettre à jour Next.js
```

---

## Nouveautés Next.js 15

### Turbopack (Stable)

```bash
# Utiliser Turbopack en dev
next dev --turbo
```

### React 19 Support

```tsx
// Support des nouvelles features React 19
import { use } from 'react'

async function getData() {
  const res = await fetch('...')
  return res.json()
}

function Component() {
  const data = use(getData())
  return <div>{data.title}</div>
}
```

### Améliorations du Caching

```tsx
// Contrôle plus fin du cache
export const fetchCache = 'default-cache'
export const dynamic = 'auto'

// Nouveau: unstable_cache
import { unstable_cache } from 'next/cache'

const getCachedData = unstable_cache(
  async () => {
    return await db.query('...')
  },
  ['cache-key'],
  {
    revalidate: 60,
    tags: ['data'],
  }
)
```

### Partial Prerendering (Expérimental)

```javascript
// next.config.js
module.exports = {
  experimental: {
    ppr: true, // Partial Prerendering
  },
}
```

---

## Performance Tips

1. **Utiliser les Server Components par défaut**
2. **Lazy loading des Client Components**
   ```tsx
   const ClientComp = dynamic(() => import('./ClientComp'), {
     ssr: false,
     loading: () => <p>Chargement...</p>
   })
   ```
3. **Optimiser les images avec next/image**
4. **Utiliser le streaming avec Suspense**
5. **Précharger les routes critiques**
   ```tsx
   router.prefetch('/important-page')
   ```
6. **Utiliser les Web Vitals**
   ```tsx
   // app/web-vitals.tsx
   'use client'
   import { useReportWebVitals } from 'next/web-vitals'
   
   export function WebVitals() {
     useReportWebVitals((metric) => {
       console.log(metric)
     })
   }
   ```

---

---

## Ressources et Communauté

### Documentation Officielle

- **Documentation Next.js**: https://nextjs.org/docs
  - Guide complet et à jour sur toutes les fonctionnalités
- **Learn Next.js**: https://nextjs.org/learn
  - Tutoriel interactif officiel pour débutants
- **API Reference**: https://nextjs.org/docs/app/api-reference
  - Référence complète de l'API Next.js
- **Blog Vercel**: https://vercel.com/blog
  - Annonces et articles techniques

### Cours et Tutoriels

- **Next.js Foundations** (gratuit): https://nextjs.org/learn/foundations/about-nextjs
- **App Router Incremental Adoption**: https://nextjs.org/docs/app/building-your-application/upgrading/app-router-migration
- **Next.js on YouTube**: https://www.youtube.com/@VercelHQ
- **Web Dev Simplified - Next.js**: https://www.youtube.com/@WebDevSimplified
- **Fireship - Next.js in 100 seconds**: https://www.youtube.com/watch?v=Sklc_fQBmcs
- **Frontend Masters - Next.js**: https://frontendmasters.com/courses/next-js-v3/
- **Udemy - Next.js & React**: https://www.udemy.com/topic/next-js/

### Outils et Libraries

**État Global et Data Fetching**
- **React Query / TanStack Query**: https://tanstack.com/query/latest
- **Zustand**: https://github.com/pmndrs/zustand
- **Jotai**: https://jotai.org/
- **Redux Toolkit**: https://redux-toolkit.js.org/

**Styling**
- **Tailwind CSS**: https://tailwindcss.com/
- **shadcn/ui**: https://ui.shadcn.com/
- **Styled Components**: https://styled-components.com/
- **Emotion**: https://emotion.sh/
- **CSS Modules**: (Built-in Next.js)

**Formulaires et Validation**
- **React Hook Form**: https://react-hook-form.com/
- **Zod**: https://zod.dev/
- **Formik**: https://formik.org/
- **Yup**: https://github.com/jquense/yup

**Base de Données et ORM**
- **Prisma**: https://www.prisma.io/
- **Drizzle ORM**: https://orm.drizzle.team/
- **Supabase**: https://supabase.com/
- **MongoDB**: https://www.mongodb.com/
- **PlanetScale**: https://planetscale.com/

**Authentification**
- **NextAuth.js / Auth.js**: https://authjs.dev/
- **Clerk**: https://clerk.com/
- **Supabase Auth**: https://supabase.com/docs/guides/auth
- **Auth0**: https://auth0.com/
- **Lucia**: https://lucia-auth.com/

**CMS Headless**
- **Sanity**: https://www.sanity.io/
- **Contentful**: https://www.contentful.com/
- **Strapi**: https://strapi.io/
- **Payload CMS**: https://payloadcms.com/
- **Hygraph**: https://hygraph.com/

**Testing**
- **Jest**: https://jestjs.io/
- **React Testing Library**: https://testing-library.com/react
- **Playwright**: https://playwright.dev/
- **Cypress**: https://www.cypress.io/
- **Vitest**: https://vitest.dev/

**Analytics et Monitoring**
- **Vercel Analytics**: https://vercel.com/analytics
- **Google Analytics**: https://analytics.google.com/
- **Plausible**: https://plausible.io/
- **PostHog**: https://posthog.com/
- **Sentry**: https://sentry.io/

**Déploiement**
- **Vercel**: https://vercel.com/ (recommandé)
- **Netlify**: https://www.netlify.com/
- **AWS Amplify**: https://aws.amazon.com/amplify/
- **Railway**: https://railway.app/
- **Render**: https://render.com/
- **Fly.io**: https://fly.io/
- **Docker**: https://www.docker.com/

### Templates et Starters

- **Next.js Examples**: https://github.com/vercel/next.js/tree/canary/examples
  - Plus de 500 exemples officiels
- **create-t3-app**: https://create.t3.gg/
  - Stack TypeScript avec Next.js, tRPC, Prisma, Tailwind
- **Next.js SaaS Starter**: https://github.com/vercel/nextjs-subscription-payments
- **Next.js Commerce**: https://vercel.com/templates/next.js/nextjs-commerce
- **Next.js Blog Starter**: https://github.com/vercel/next.js/tree/canary/examples/blog-starter
- **Taxonomy**: https://github.com/shadcn-ui/taxonomy
  - Application moderne avec App Router et shadcn/ui

### Communauté

**Discord et Forums**
- **Next.js Discord**: https://nextjs.org/discord
- **Vercel Community**: https://github.com/vercel/next.js/discussions
- **Reddit r/nextjs**: https://www.reddit.com/r/nextjs/
- **Stack Overflow**: https://stackoverflow.com/questions/tagged/next.js

**Newsletters**
- **Vercel Newsletter**: https://vercel.com/newsletter
- **Next.js Weekly**: https://nextjsweekly.com/
- **Bytes.dev**: https://bytes.dev/

**Twitter/X Accounts à Suivre**
- **@nextjs**: Compte officiel Next.js
- **@vercel**: Vercel
- **@leeerob**: Lee Robinson (VP of Developer Experience)
- **@delba_oliveira**: Delba de Oliveira (Developer Advocate)
- **@shadcn**: shadcn (Creator of shadcn/ui)
- **@t3dotgg**: Theo (create-t3-app)

### Articles et Blogs Techniques

- **Vercel Blog**: https://vercel.com/blog
- **Next.js Conf**: https://nextjs.org/conf
- **Lee Robinson's Blog**: https://leerob.io/
- **Josh Comeau**: https://www.joshwcomeau.com/
- **Kent C. Dodds**: https://kentcdodds.com/blog
- **Robin Wieruch**: https://www.robinwieruch.de/

### Chaînes YouTube

- **Vercel**: https://www.youtube.com/@VercelHQ
- **Theo - t3.gg**: https://www.youtube.com/@t3dotgg
- **Lee Robinson**: https://www.youtube.com/@leerob
- **Web Dev Simplified**: https://www.youtube.com/@WebDevSimplified
- **Fireship**: https://www.youtube.com/@Fireship
- **Jack Herrington**: https://www.youtube.com/@jherr
- **Josh tried coding**: https://www.youtube.com/@joshtriedcoding
- **Code with Antonio**: https://www.youtube.com/@codewithantonio

### Awesome Lists

- **Awesome Next.js**: https://github.com/unicodeveloper/awesome-nextjs
  - Collection de ressources, bibliothèques et outils
- **Awesome React**: https://github.com/enaqx/awesome-react
  - Ressources React compatibles avec Next.js

### Outils de Développement

**Extensions VS Code**
- **ES7+ React/Redux/React-Native snippets**
- **Tailwind CSS IntelliSense**
- **Prisma** (si vous utilisez Prisma)
- **ESLint**
- **Prettier**
- **Error Lens**
- **Auto Rename Tag**
- **GitLens**

**DevTools**
- **React Developer Tools**: Extension navigateur
- **Next.js DevTools**: Intégré dans Next.js 14+
- **Lighthouse**: Audit de performance
- **Web Vitals Extension**: Mesure des Core Web Vitals

### Benchmarks et Comparaisons

- **Next.js vs React**: https://nextjs.org/learn/foundations/from-react-to-nextjs
- **App Router vs Pages Router**: https://nextjs.org/docs/app/building-your-application/upgrading/app-router-migration
- **Vercel Speed Insights**: https://vercel.com/docs/concepts/speed-insights

### Projets Open Source Inspirants

- **Cal.com**: https://github.com/calcom/cal.com
  - Plateforme de planification
- **Dub.co**: https://github.com/dubinc/dub
  - Raccourcisseur de liens
- **Plane**: https://github.com/makeplane/plane
  - Alternative à Jira
- **Papermark**: https://github.com/mfts/papermark
  - Partage de documents sécurisé
- **Documenso**: https://github.com/documenso/documenso
  - Alternative open source à DocuSign

### Ressources Mobile et PWA

- **PWA with Next.js**: https://github.com/shadowwalker/next-pwa
- **Capacitor**: https://capacitorjs.com/
  - Pour créer des apps natives à partir de Next.js

### Internationalisation

- **next-intl**: https://next-intl-docs.vercel.app/
- **next-i18next**: https://github.com/i18next/next-i18next
- **Format.JS**: https://formatjs.io/

### Sécurité

- **OWASP Top 10**: https://owasp.org/www-project-top-ten/
- **Security Headers**: https://securityheaders.com/
- **Next.js Security**: https://nextjs.org/docs/app/building-your-application/configuring/content-security-policy

### SEO

- **Google Search Console**: https://search.google.com/search-console
- **Next.js SEO**: https://github.com/garmeeh/next-seo
- **Schema.org**: https://schema.org/
- **Rich Results Test**: https://search.google.com/test/rich-results

### Conseils et Best Practices

1. **Suivez les conventions Next.js** pour une meilleure DX
2. **Utilisez TypeScript** pour plus de sécurité
3. **Optimisez les images** avec next/image
4. **Préférez les Server Components** quand c'est possible
5. **Utilisez le caching intelligemment** pour de meilleures performances
6. **Mesurez les Web Vitals** régulièrement
7. **Implémentez un bon SEO** dès le début
8. **Testez votre code** pour éviter les régressions
9. **Documentez votre code** pour faciliter la maintenance
10. **Restez à jour** avec les dernières versions de Next.js

### Aide et Support

- **GitHub Issues**: https://github.com/vercel/next.js/issues
- **GitHub Discussions**: https://github.com/vercel/next.js/discussions
- **Vercel Support**: https://vercel.com/support
- **Stack Overflow**: Tag `next.js`
- **Discord Community**: Pour des questions rapides

### Événements

- **Next.js Conf**: Conférence annuelle (octobre)
- **Vercel Ship**: Événements produits réguliers
- **Local Meetups**: Recherchez des meetups Next.js dans votre ville


