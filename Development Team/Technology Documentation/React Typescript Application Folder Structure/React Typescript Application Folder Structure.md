
==`Structure`==

```
src/
├── app/                     # Next.js App Router (routing only)
│   ├── (public)/            # Public routes (no auth)
│   │   ├── page.tsx
│   │   └── layout.tsx
│   │
│   ├── (auth)/              # Auth related routes
│   │   ├── login/
│   │   │   └── page.tsx
│   │   ├── register/
│   │   │   └── page.tsx
│   │   └── layout.tsx
│   │
│   ├── (dashboard)/         # Protected routes
│   │   ├── dashboard/
│   │   │   ├── page.tsx
│   │   │   ├── loading.tsx
│   │   │   └── error.tsx
│   │   │
│   │   ├── users/
│   │   │   ├── page.tsx
│   │   │   └── [id]/page.tsx
│   │   │
│   │   └── layout.tsx
│   │
│   ├── api/            # Route handlers (backend-for-frontend)
│   │   ├── auth/route.ts
│   │   └── users/route.ts
│   │
│   ├── layout.tsx           # Root layout
│   ├── globals.css
│   ├── error.tsx
│   └── not-found.tsx
|   └── loading.tsx
│
├── features/                # Feature-based modules (CORE)
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services.ts
│   │   ├── schema.ts
│   │   └── types.ts
│   │
│   ├── users/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services.ts
│   │   ├── schema.ts
│   │   └── types.ts
│   │
│   └── dashboard/
│       ├── components/
│       └── hooks/
│
├── components/              # Shared / reusable UI components
│   ├── ui/                  # Design system (shadcn)
│   ├── layout/
│   ├── common/
│   └── feedback/            # toast, modal, loader
│
├── hooks/                   # Global reusable hooks
│   ├── useDebounce.ts
│   └── useMounted.ts
│
├── lib/                     # Core libraries & config
│   ├── axios.ts             # API client
│   ├── auth.ts              # auth helpers
│   ├── env.ts               # env validation
│   └── constants.ts
│
├── server/                  # Server-only logic
│   ├── db/
│   │   ├── index.ts
│   │   └── schema.ts
│   ├── services/
│   └── actions/             # Server Actions
│
├── store/                   # Global state (Zustand / Redux)
│   └── auth.store.ts
│
├── styles/                  # Non-global styles
│   └── themes/
│
├── types/                   # Global TypeScript types
│   └── index.d.ts
│
├── utils/                   # Pure helper functions
│   ├── date.ts
│   ├── format.ts
│   └── logger.ts
│
├── middleware.ts            # Next.js middleware
└── config/                  # App-level configuration
    └── routes.ts
```