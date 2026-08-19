# 🏛️ Project Blueprint & Metodología de Desarrollo Frontend

Esta guía define el protocolo estándar para concebir, inicializar, estructurar y desarrollar aplicaciones web y PWAs de alto rendimiento utilizando un enfoque modular (**Feature-Driven Architecture**), **Atomic Design pragmático** y desarrollo asistido por IA.

---

## 🧭 Flujo Cronológico de Desarrollo (De Cero a Producción)

```text
[1. Definición & Stack]
          ▼
[2. Crear AI_CONTEXT.md]
          ▼
[3. Scaffold & UI Kit]
          ▼
[4. Base de Datos & Types]
          ▼
[5. Construcción Módulos]
```

## 📋 Fase 1: Definición de Negocio y Selección de Stack

Antes de inicializar código o generar reglas de IA, define:

1. **Tipo de Producto:** PWA de gestión/B2B, SPA administrativa, e-commerce B2C, etc.
2. **Stack Estándar Recomendado:**
* **Core:** React (Vite) + TypeScript (Modo Estricto).
* **Estilos:** Tailwind CSS V3.
* **Iconografía:** `lucide-react`.
* **BaaS / Backend:** Supabase (PostgreSQL + Auth + Storage + Realtime).
* **Despliegue:** Netlify / Vercel.


3. **Entidades Clave del Negocio:** Lista preliminar de 3 a 5 tablas maestras.

---

## 📄 Fase 2: Redacción del `AI_CONTEXT.md`

Adapta la plantilla de contexto al nuevo proyecto. Este archivo debe ubicarse en `docs/AI_CONTEXT.md` y contener:

1. **Stack & Entorno:** Variables, librerías y restricciones de sistema operativo (Linux/WSL2 case-sensitive).
2. **Estructura de Carpetas:** Reglas de `@/modules/`, `@/components/ui/`, `@/types/`, etc.
3. **Reglas de Oro:** TypeScript estricto (prohibido `any`, eliminación de imports no usados `TS6133`), uso obligatorio del UI Kit, no CSS plano.
4. **Diccionario de Componentes del UI Kit:** APIs exactas de botones, inputs, tablas, cabeceras y tarjetas.
5. **Roadmap & Servicios Futuros:** Lineamientos para Auth, Storage, Data Exchange y Multi-tenancy.

---

## 🛠️ Fase 3: Scaffold Inicial y Portabilidad del UI Kit

### 1. Inicialización de Proyecto

```bash
npm create vite@latest nombre-app -- --template react-ts
cd nombre-app
npm install
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
npm install lucide-react @supabase/supabase-js react-router-dom

```

### 2. Configuración de Alias (`@/` = `src/`)

* **`tsconfig.app.json`:**
```json
{
  "compilerOptions": {
    // ... otras configuraciones
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}

```


* **`vite.config.ts`:**
```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});

```

Instalar @types/node si te marca error en __dirname:
```
npm i -D @types/node
```

### 3. Portabilidad del UI Kit Base

Copia la carpeta `@/components/ui/` desde el repositorio base. Al ser agnóstica al negocio, heredas de inmediato:

* `Button.tsx`, `Input.tsx`, `Select.tsx`, `Badge.tsx` (Átomos).
* `SummaryCard.tsx`, `Tabs.tsx`, `DynamicRow.tsx` (Moléculas).
* `ModuleHeader.tsx`, `Table.tsx`, `DualAsignador.tsx` (Organismos).

---

## 🗄️ Fase 4: Infraestructura de Datos y Tipado Estricto

1. **Modelado en Supabase:**
* Diseñar las tablas iniciales en PostgreSQL.
* Guardar el respaldo DDL en `docs/database/schema.sql`.


2. **Automatización de Tipos en `package.json`:**
```json
"scripts": {
  "types:db": "supabase gen types typescript --project-id TU_PROJECT_ID > src/types/database.types.ts"
}

```


*Ejecutar:* `npm run types:db` tras cada cambio en base de datos.
3. **Instanciación del Cliente Tipado (`src/lib/supabase.ts`):**
```typescript
import { createClient } from '@supabase/supabase-js';
import type { Database } from '@/types/database.types';

export const supabase = createClient<Database>(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
);

```



---

## 🧩 Fase 5: Construcción de Módulos (Vertical Slice)

Organiza cada módulo dentro de `src/modules/[nombre]/` siguiendo la separación de responsabilidades:

```text
src/modules/[modulo]/
├── [Modulo]View.tsx      # Orquestador: Vistas activas (grilla/detalle/form), KPIs y header
├── components/           # Subvistas y modales exclusivos ([Modulo]Detail, [Modulo]Form)
├── hooks/                # Lógica de negocio y consultas Supabase (use[Modulo].ts)
└── types.ts              # Tipos efímeros locales (estados de formularios, filtros)

```

### Reglas de Implementación Modular:

* **UI Pura:** Los componentes visuales consumen props y emiten eventos; no contienen consultas directas a base de datos.
* **Hooks Dedicados:** Toda interacción con Supabase se encapsula en `hooks/use[Modulo].ts` manejando `cargando`, `error` y bloques `try/catch`.
* **Tipos Globales vs Locales:**
* Modelos compartidos por 2 o más módulos van en `src/types/[entidad].ts` o `database.types.ts`.
* Estados internos de componentes van en `src/modules/[modulo]/types.ts`.



---

## 🏛️ Checklist de Inicio Rápido (Nuevo Proyecto)

| Paso | Acción | Verificación |
| --- | --- | --- |
| **1** | Crear repositorio y ejecutar scaffold Vite + React + TS | `npm run dev` funciona |
| **2** | Configurar Tailwind y alias `@/` en tsconfig y vite | Imports `@/...` resuelven |
| **3** | Copiar `@/components/ui/` y `docs/AI_CONTEXT.md` | UI Kit disponible |
| **4** | Configurar Supabase, generar `schema.sql` y `database.types.ts` | `npm run types:db` genera tipos |
| **5** | Construir primer módulo orquestador en `src/modules/` | Vista operativa con UI Kit |

```

```
