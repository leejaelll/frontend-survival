---
description: CRA에서 Next.js로 마이그레이션하기
---

# Migrating from Create React App

## Next.js dependency 설치하기

```bash
npm install next@latest
```



## Next.js configuration 파일 작성하기

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export', // Outputs a Single-Page Application (SPA).
  distDir: './dist', // Changes the build output directory to `./dist/`.
}
 
export default nextConfig
```



## Typescript와 Next.js 호환시키기 (optional)

```bash
yarn add typescript --dev
```

{% code title="tsconfig.json" %}
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": false,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "baseUrl": ".",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "strictNullChecks": true
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx",
    ".next/types/**/*.ts",
    "./dist/types/**/*.ts"
  ],
  "exclude": ["node_modules"]
}
```
{% endcode %}



## TailwindCSS 설치하기

```bash
npm install -D tailwindcss
```

{% code title="tailwind.config.ts" %}
```typescript
import type { Config } from "tailwindcss";

const config: Config = {
  content: [
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
    "./app/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      backgroundImage: {
        "gradient-radial": "radial-gradient(var(--tw-gradient-stops))",
        "gradient-conic":
          "conic-gradient(from 180deg at 50% 50%, var(--tw-gradient-stops))",
      },
    },
  },
  plugins: [],
};
export default config;
```
{% endcode %}

{% code title="postcss.config.mjs" %}
```javascript
/** @type {import('postcss-load-config').Config} */
const config = {
  plugins: {
    tailwindcss: {},
  },
};

export default config;

```
{% endcode %}



## layout.tsx, page.tsx 파일

app 폴더 내에 layout.tsx, page.tsx  파일을 생성한다.&#x20;

```typescript
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'React App',
  description: 'Web site created with Next.js.',
};

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang='en'>
      <body>{children}</body>
    </html>
  );
}

```

```typescript
export default function Home() {
  return <div>Hello, world!</div>;
}
```
