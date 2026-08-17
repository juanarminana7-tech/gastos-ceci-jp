# Gastos JP & Ceci

App mobile-first / PWA para controlar los gastos compartidos de la pareja, con division automatica 50/50 y calculo de quien le debe a quien.

## Stack

- Next.js 14 (App Router) + TypeScript
- Tailwind CSS
- Supabase (Postgres + Realtime) via `@supabase/supabase-js`
- lucide-react

## 1. Configurar Supabase

1. Crea un proyecto en [supabase.com](https://supabase.com).
2. Anda a **SQL Editor** y ejecuta el contenido de `supabase/schema.sql` (crea la tabla `expenses`, indices y las policies de RLS).
3. Anda a **Project Settings > API** y copia:
   - `Project URL` -> `NEXT_PUBLIC_SUPABASE_URL`
   - `anon public` key -> `NEXT_PUBLIC_SUPABASE_ANON_KEY`

## 2. Variables de entorno

Copia `.env.local.example` a `.env.local` y completa los valores:

```bash
cp .env.local.example .env.local
```

```
NEXT_PUBLIC_SUPABASE_URL=https://TU-PROYECTO.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=TU_ANON_KEY
```

## 3. Instalar y correr en local

```bash
npm install
npm run dev
```

Abri http://localhost:3000 (probala en el celular con "Agregar a pantalla de inicio" para instalarla como PWA).

## 4. Deploy en Vercel

1. Sube el proyecto a un repo de GitHub/GitLab.
2. Importa el repo en [vercel.com/new](https://vercel.com/new).
3. En **Environment Variables** cargá `NEXT_PUBLIC_SUPABASE_URL` y `NEXT_PUBLIC_SUPABASE_ANON_KEY`.
4. Deploy. Vercel detecta Next.js automaticamente (build command `next build`).

## Logica de negocio

- Cada gasto se guarda con monto, categoria (Super, Verduleria, Carniceria, Forrajeria, Otros), quien lo pago y fecha.
- El total se divide 50/50: se suma lo que puso cada uno y se compara contra la mitad del total.
- Si Juampi puso mas que la mitad, Ceci le debe la diferencia (y viceversa).
- El dashboard actualiza el balance en tiempo real (Supabase Realtime) apenas se carga o borra un gasto, incluso desde otro dispositivo.
