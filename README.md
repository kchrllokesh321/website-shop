# Maruthi Enterprises Website

A professional showcase website for **Maruthi Enterprises**, a business specializing in **cement products, sand, metal, and related building materials**.

## 🚀 Quick Start

### Prerequisites

-   Node.js (v20.x recommended)
-   npm (or yarn/pnpm)

### Installation

1.  Clone the repository:
    ```bash
    git clone <repository-url>
    cd maruthi-enterprises-website
    ```

2.  Install dependencies:
    ```bash
    npm install
    ```

3.  Start the development server:
    ```bash
    npm run dev
    ```

    The site will be available at `http://localhost:3000`.

4.  Build for production:
    ```bash
    npm run build
    ```

5.  Start production server:
    ```bash
    npm run start
    ```

---

## 🛠️ Tech Stack

-   **Framework**: **Next.js 15** (App Router)
-   **Language**: **TypeScript**
-   **Styling**: **Tailwind CSS v4** + **Radix UI**
-   **Icons**: **Lucide React**
-   **Deployment**: **Vercel** (optimized for)

---

## 📂 Project Structure

```
maruthi-enterprises-website/
├── app/                      # Next.js App Router
│   ├── (marketing)/          # Public marketing pages
│   │   ├── page.tsx          # Homepage
│   │   └── ...
│   ├── admin/                # Admin dashboard
│   │   ├── page.tsx
│   │   └── ...
│   ├── api/                  # API routes
│   ├── layout.tsx            # Root layout
│   └── globals.css           # Global styles
├── components/               # Reusable components
│   ├── ui/                   # Radix UI + custom components
│   ├── marketing/            # Marketing-specific components
│   └── admin/                # Admin-specific components
├── lib/                      # Business logic
│   ├── database.ts           # Database operations
│   ├── validation.ts         # Validation schemas
│   └── utils.ts              # Utility functions
├── public/                   # Static assets
└── prisma/                   # Prisma ORM
    ├── schema.prisma         # Database schema
    └── migrations/           # Database migrations
```

---

## 🏗️ Architecture

### 1. **Routing (App Router)**

```
app/page.tsx             → / (Homepage)
app/about/page.tsx       → /about
app/services/page.tsx    → /services
app/(marketing)/contact/page.tsx → /contact
app/admin/page.tsx       → /admin (Dashboard)
```

### 2. **Database Schema**

The database uses **Prisma** with the following models:

-   `User`: Admin users
-   `ProductCategory`: Product categories
-   `Product`: Products (cement, sand, metal, etc.)
-   `Order`: Customer orders
-   `OrderProduct`: Order items

**Schema location**: `prisma/schema.prisma`

---

## 🗄️ Database Setup

### Requirements

-   **PostgreSQL** (recommended for production)
-   **SQLite** (default for development)

### Configuration

Update `prisma/schema.prisma` with your database connection:

```prisma
generator client { provider = "prisma-client-js" }

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// ... model definitions
```

### Migration

Run migrations to create/update tables:

```bash
npx prisma migrate dev --name init
```

### Database URL Environment Variable

Add to your `.env` file:

```env
DATABASE_URL="postgresql://user:password@host:port/dbname"
```

---

## 👤 Admin Authentication

### Initial Setup

Create the first admin user:

```bash
# Add this script to package.json
"scripts": {
  "create-admin": "ts-node scripts/create-admin.ts"
}

# Run it
npm run create-admin
```

The script will prompt for:
-   Admin email
-   Admin password
-   Admin name

### Admin URLs

-   **Dashboard**: `/admin`
-   **Login**: `/admin/login`

---

## 📋 Admin Operations

### Product Management

#### Add Product

1.  Go to **Products** > **Add Product**
2.  Fill in:
    -   Name (e.g., "Birla Cement - 50kg")
    -   Category (Cement, Sand, Metal, etc.)
    -   Unit (Bag, Ton, Cu.Meter)
    -   Price per unit
    -   Description
    -   Features (bullet points)
3.  Upload product image (optional)

#### Categories

Manage categories from **Categories** tab:
-   Add new categories
-   Edit category names
-   Delete categories

### Order Management

#### View Orders

1.  Go to **Orders**
2.  Browse all customer orders
3.  Filter by status (Pending, Processing, Completed, Cancelled)
4.  View order details:
    -   Customer information
    -   Order date and time
    -   Total amount
    -   Products ordered
    -   Status history

#### Update Order Status

1.  Click on an order to view details
2.  Select new status
3.  Add internal notes (optional)
4.  Update order

---

## 📱 Responsiveness

The website is fully responsive and optimized for all devices:

-   **Mobile**: Portrait and landscape
-   **Tablet**: iPad, Android tablets
-   **Desktop**: All screen sizes

### Breakpoints

```typescript
// tailwind.config.ts
const theme = {
  screens: {
    sm: '480px',    // Mobile (Portrait)
    md: '768px',    // Tablet (Portrait)
    lg: '1024px',   // Desktop (Small)
    xl: '1280px',   // Desktop (Medium)
    '2xl': '1536px' // Desktop (Large)
  }
}
```

### Mobile Navigation

-   **Hamburger menu** on mobile devices
-   **Full-screen overlay** for mobile navigation
-   **Back button** in mobile headers

---

## 🔐 Security

### Authentication

-   **NextAuth.js** with email/password
-   **Password hashing**: bcrypt
-   **Session management**

### Input Validation

-   **Zod** schemas for all forms
-   **Type safety** with TypeScript
-   **Server-side validation**

### Environment Variables

Create `.env` file in the root directory:

```env
# Database
DATABASE_URL="postgresql://user:password@host:port/dbname"

# NextAuth
NEXTAUTH_SECRET="your-nextauth-secret"
NEXTAUTH_URL="http://localhost:3000"
```

**Generate NextAuth secret**:
```bash
node -e 'console.log(crypto.randomBytes(32).toString("hex"))'
```

---

## 📝 Content Management

### Adding Pages

Create a new page in `app/` directory:

```typescript
// app/new-page/page.tsx

export default function NewPage() {
  return (
    <main>
      <h1>New Page Title</h1>
      {/* Page content */}
    </main>
  )
}
```

### SEO Optimization
