# [Testing] - Supabase auth (Next.js 15 + Supabase SSR)

[![Next.js](https://img.shields.io/badge/Next.js-15-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react)](https://react.dev/)
[![Supabase](https://img.shields.io/badge/Supabase-SSR-3ECF8E?style=for-the-badge&logo=supabase)](https://supabase.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4-38B2AC?style=for-the-badge&logo=tailwind-css)](https://tailwindcss.com/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)

ระบบ Authentication และ User Registration เว็บแอปพลิเคชัน พัฒนาด้วย **Next.js 15 (App Router)** ร่วมกับ **Supabase SSR (`@supabase/ssr`)** โดยใช้สถาปัตยกรรม Cookie-based Session ปลอดภัยสำหรับการ Render ฝั่ง Server Component และ Middleware พร้อมระบบ Theme และ UI Components จาก shadcn/ui

---

## 📑 สารบัญ (Table of Contents)

- [ฟีเจอร์หลัก (Key Features)](#-ฟีเจอร์หลัก-key-features)
- [Tech Stack](#-tech-stack)
- [โครงสร้างโปรเจกต์ (Project Structure)](#-โครงสร้างโปรเจกต์-project-structure)
- [สถาปัตยกรรมการพิสูจน์ตัวตน (Authentication Architecture)](#-สถาปัตยกรรมการพิสูจน์ตัวตน-authentication-architecture)
- [เริ่มต้นใช้งาน (Getting Started)](#-เริ่มต้นใช้งาน-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Development & Build](#development--build)
- [Scripts คำสั่งในโปรเจกต์](#-scripts-คำสั่งในโปรเจกต์)
- [ผู้พัฒนา (Author)](#-ผู้พัฒนา-author)

---

## ✨ ฟีเจอร์หลัก (Key Features)

- ✅ **Cookie-Based Authentication:** จัดการ Session ผ่าน HttpOnly Cookies ด้วย `@supabase/ssr` รองรับ Server Components, Server Actions และ Edge Middleware
- ✅ **Complete Auth Lifecycle:**
  - ลงทะเบียนผู้ใช้ใหม่ (Sign Up พร้อมระบบยืนยันตัวตนผ่าน Email)
  - เข้าสู่ระบบ (Login ด้วย Email และ Password)
  - รีเซ็ตรหัสผ่าน (Forgot Password & Password Reset Email)
  - เปลี่ยนรหัสผ่านใหม่ (Update Password)
  - ออกจากระบบ (Logout และ Revoke Session ทันที)
- ✅ **Route Protection:** ระบบ Route Guard ป้องกันหน้าลับ (`/protected`) ระดับ Middleware และ Server Component Fallback ปราศจากปัญหา FOUC (Flash of Unauthenticated Content)
- ✅ **Modern Responsive UI:** สร้างด้วย [Tailwind CSS](https://tailwindcss.com) และ [shadcn/ui](https://ui.shadcn.com) ปรับแต่งได้ง่าย
- ✅ **Theme Switcher:** รองรับทั้ง Dark Mode และ Light Mode อย่างสมบูรณ์ผ่าน `next-themes`
- ✅ **Type Safety:** พัฒนาด้วย TypeScript ทั้งหมด พร้อม ESLint 9

---

## 🛠 Tech Stack

| Category | Technology |
| :--- | :--- |
| **Framework** | [Next.js 15](https://nextjs.org/) (App Router) |
| **Library** | [React 19](https://react.dev/) |
| **Language** | [TypeScript 5](https://www.typescriptlang.org/) |
| **Authentication & Database** | [Supabase](https://supabase.com/) (`@supabase/ssr`, `@supabase/supabase-js`) |
| **Styling** | [Tailwind CSS 3.4](https://tailwindcss.com/), [tailwindcss-animate](https://github.com/jamiebuilds/tailwindcss-animate) |
| **UI Components** | [shadcn/ui](https://ui.shadcn.com/) (Radix UI primitives) |
| **Icons** | [Lucide React](https://lucide.dev/) |
| **Theming** | [next-themes](https://github.com/pacocoursey/next-themes) |

---

## 📁 โครงสร้างโปรเจกต์ (Project Structure)

```text
rmz-register/
├── app/
│   ├── auth/
│   │   ├── confirm/
│   │   │   └── route.ts             # Route Handler สำหรับยืนยัน Email OTP / Token
│   │   ├── error/
│   │   │   └── page.tsx             # หน้าแสดงผลเมื่อเกิดข้อผิดพลาดในการยืนยันตัวตน
│   │   ├── forgot-password/
│   │   │   └── page.tsx             # หน้าขอรีเซ็ตรหัสผ่าน
│   │   ├── login/
│   │   │   └── page.tsx             # หน้าเข้าสู่ระบบ
│   │   ├── sign-up/
│   │   │   └── page.tsx             # หน้าสมัครสมาชิก
│   │   ├── sign-up-success/
│   │   │   └── page.tsx             # หน้าแจ้งเตือนการส่ง Email ยืนยันตัวตน
│   │   └── update-password/
│   │       └── page.tsx             # หน้าตั้งรหัสผ่านใหม่
│   ├── protected/
│   │   ├── layout.tsx
│   │   └── page.tsx                 # หน้าสำหรับ Authenticated Users เท่านั้น
│   ├── globals.css                  # Global Tailwind CSS & Design System Variables
│   ├── layout.tsx                   # Root Layout พร้อม ThemeProvider
│   └── page.tsx                     # Landing Page
├── components/
│   ├── ui/                          # UI Primitives (Button, Card, Input, Label, etc.)
│   ├── tutorial/                    # Component แนะนำขั้นตอนการเชื่อมต่อ Supabase
│   ├── auth-button.tsx              # Component แสดงสถานะ Auth / ปุ่ม Sign In / Profile
│   ├── deploy-button.tsx
│   ├── forgot-password-form.tsx
│   ├── hero.tsx
│   ├── login-form.tsx               # Client Component สำหรับ Form Login
│   ├── logout-button.tsx            # Client Component สำหรับ Sign Out
│   ├── sign-up-form.tsx             # Client Component สำหรับ Form Sign Up
│   ├── theme-switcher.tsx           # สลับ Light / Dark Mode
│   └── update-password-form.tsx
├── lib/
│   ├── supabase/
│   │   ├── client.ts                # Browser Client (createBrowserClient)
│   │   ├── server.ts                # Server Client สำหรับ Server Components (createServerClient)
│   │   └── proxy.ts                 # Session Updater & Cookie Synchronization
│   └── utils.ts                     # Helper utilities (cn, hasEnvVars)
├── proxy.ts                         # Edge Proxy / Middleware Request Interceptor
├── components.json                  # shadcn/ui configuration
├── next.config.ts                   # Next.js Configuration
├── package.json
├── tailwind.config.ts
└── tsconfig.json
```

---

## 🔒 สถาปัตยกรรมการพิสูจน์ตัวตน (Authentication Architecture)

สถาปัตยกรรมของโปรเจกต์นี้ทำงานร่วมกัน 3 เลเยอร์:

1. **Client Layer (`components/login-form.tsx`):**
   - เรียกใช้ `createBrowserClient` เพื่อพิสูจน์ตัวตนผ่าน Supabase GoTrue API
   - Token จะถูกเขียนลงใน Browser Cookies โดยตรง
2. **Edge Interceptor Layer (`proxy.ts` / `lib/supabase/proxy.ts`):**
   - ดักจับ Request เพื่อตรวจสอบสถานะ Session ผ่าน `getClaims()`
   - ซิงค์และ Refresh Token ใน Response Cookie อัตโนมัติ ป้องกัน Session หลุด
3. **Server Component Guard (`app/protected/page.tsx`):**
   - ดึงข้อมูล User Claims ผ่าน `createServerClient` อ่านจาก Cookies
   - Redirect ผู้ใช้ที่ไม่มีสิทธิ์ไปยัง `/auth/login` ทันทีก่อนที่จะเริ่ม Render หน้าเว็บ

---

## 🚀 เริ่มต้นใช้งาน (Getting Started)

### Prerequisites

- [Node.js](https://nodejs.org/) (Version 18.17 หรือใหม่กว่า)
- [Supabase](https://supabase.com/) Account & Project

### Installation

1. Clone repository นี้ลงเครื่อง:

```bash
git clone https://github.com/6ruce-wayne/-Testing---Supabase-auth.git
cd rmz-register
```

2. ติดตั้ง Dependencies:

```bash
npm install
```

### Environment Variables

คัดลอกไฟล์ `.env.example` เป็น `.env.local`:

```bash
cp .env.example .env.local
```

กำหนดค่า Supabase Project Credentials ใน `.env.local`:

```env
NEXT_PUBLIC_SUPABASE_URL=your-supabase-project-url
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=your-supabase-publishable-key-or-anon-key
```

> [!NOTE]
> สามารถดู URL และ Publishable/Anon Key ได้จาก **Supabase Dashboard** > **Project Settings** > **API**

### Development & Build

รัน Local Development Server:

```bash
npm run dev
```

เปิดเบราว์เซอร์ไปที่ [http://localhost:3000](http://localhost:3000)

สร้าง Production Build:

```bash
npm run build
npm run start
```

---

## 📜 Scripts คำสั่งในโปรเจกต์

| Script | คำอธิบาย |
| :--- | :--- |
| `npm run dev` | เริ่มต้น Development Server แบบ Hot-Reload |
| `npm run build` | Compile และ Build โปรเจกต์สำหรับ Production |
| `npm run start` | รันเซิร์ฟเวอร์ Production Build |
| `npm run lint` | ตรวจสอบ Code Quality ด้วย ESLint |

---

## 👨‍💻 ผู้พัฒนา (Author)

**ปริญญา กันชัย (Parinya Kanchai)**  
*IT Support · SAP Administrator · Developer · System Administrator*

- 🌐 Website: [rmzstudio.xyz](https://rmzstudio.xyz)
- 🐙 GitHub: [@6ruce-wayne](https://github.com/6ruce-wayne)
- 📧 Email: rmninthailand@gmail.com
