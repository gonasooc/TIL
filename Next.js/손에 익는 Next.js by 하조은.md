[왜 Next.js를 배우는가?](https://book.hajoeun.dev/friendly-next-js/)

# 섹션 1. 왜 Next.js를 배우는가?

## Next.js를 배우는 이유

[왜 Next.js를 배우는가?](https://book.hajoeun.dev/friendly-next-js/)

# 섹션 2. Next.js 기본 배우기

## Next.js는 무엇인가?

[Next.js는 무엇인가?](https://book.hajoeun.dev/friendly-next-js/next.js-1/next.js)

- React 개발에 필요한 선택을 최소화, 최적화

## Next.js 13

[Next.js 13](https://book.hajoeun.dev/friendly-next-js/next.js-1/next.js-13)

- App Router (before: Pages Router)

## 서버 컴포넌트(Server Component)

[서버 컴포넌트](https://book.hajoeun.dev/friendly-next-js/next.js-1/undefined)

- Next.js의 기본 컴포넌트 서버 컴포넌트
- ‘use client’로 명시되어야 클라이언트 컴포넌트
- 클라이언트 컴포넌트에 서버 컴포넌트를 import 할 수 없다
- 클라이언트 컴포넌트는 CSR 컴포넌트가 아니다 → 클라이언트 컴포넌트도 SSR, SSG 방식으로 랜더링 가능
- 서버 컴포넌트에서는 hook, event를 사용할 수 없다
- 그래서 Next.js에서는 말단에 클아이언트 컴포넌트를 두는 방식을 권장함 → ‘use client’ 최대한 뒤로 미루기

## 라우팅

[라우팅](https://book.hajoeun.dev/friendly-next-js/next.js-1/undefined-1)

- 폴더 이름을 따르는 URL Path → ex) `app\**dashboard**\page.tsx`
- 경로의 이름으로 사용되는 폴더를 segment라고 부름
- app은 가장 기초가 되는 segment라고 해서 root segment
- 동적 라우팅(Dynamic Routes)은 ex) `app\blog\[id]\page.tsx`

## 페이지 간 이동

[페이지 간 이동](https://book.hajoeun.dev/friendly-next-js/next.js-1/undefined-2)

- `<Link>` 컴포넌트 사용하기
  ```jsx
  import Link from "next/link";

  export default function Page() {
    return <Link href="/dashboard">Dashboard</Link>;
  }
  ```
  - 페이지 이동 전에 필요한 미리 데이터를 페칭하는 프리페칭 기능을 지원
- `useRouter()` Hook 사용하기
  ```jsx
  "use client";

  import { useRouter } from "next/navigation";

  export default function Page() {
    const router = useRouter();

    return (
      <button type="button" onClick={() => router.push("/dashboard")}>
        Dashboard
      </button>
    );
  }
  ```
  - useRouter가 클라이언트 컴포넌트에서만 사용할 수 있다는 점

## 스타일링

[스타일링](https://book.hajoeun.dev/friendly-next-js/next.js-1/undefined-3)

- CSS Modules

  - `styles.module.css`
    ```jsx
    .dashboard {
      padding: 24px;
    }
    ```
  - `layout.tsx`
    ```jsx
    import styles from "./styles.module.css";

    export default function DashboardLayout({ children }) {
      return <section className={styles.dashboard}>{children}</section>;
    }
    ```

- 전역 스타일링
  - `app/global.css`
    ```jsx
    body {
      padding: 20px 20px 60px;
      max-width: 680px;
      margin: 0 auto;
    }
    ```
  - `app/layout.tsx`
    ```jsx
    // 이렇게 적용된 스타일은 어플리케이션의 모든 경로에 적용됩니다.
    import "./global.css";

    export default function RootLayout({ children }) {
      return (
        <html>
          <body>{children}</body>
        </html>
      );
    }
    ```
  - 클래스 오염이 생길 수 있기 때문에 캐스캐이딩을 잘 이용해야 하고, 어떤 컴포넌트건 불러와서 쓸 수 있지만 가급적 app segment 가장 최상단 영역 쪽에서 불러와서 적용하는 편이 좋음

## 데이터 페칭

[데이터 페칭](https://book.hajoeun.dev/friendly-next-js/next.js-1/undefined-4)

- 서버에서 fetch
  - Link 컴포넌트처럼 기존의 fetch API를 Next.js 내에서 조금 더 확장해서 제공
  ```jsx
  async function getData() {
    const res = await fetch("https://api.example.com/...");
    // 직렬화되지 않기 때문에 데이터 타입을 바로 사용할 수 있습니다.

    if (!res.ok) {
      // 에러를 던지면 가장 가까이 있는 error.tsx 파일에 걸립니다.
      throw new Error("Failed to fetch data");
    }

    return res.json();
  }

  export default async function Page() {
    const data = await getData();

    return <main></main>;
  }
  ```
- 데이터 캐싱(Caching)
- 데이터 재검증(Revalidating) → 기존에 썼던 댓글만 출력해주는 것이 아니라 새로 등록한 댓글이 있다면 다시 데이터를 확인해서 출력해야 하는데, 이런 과정을 데이터 재검증
  - 시간 기반 재검증 → 특정 시간 동안만 캐시를 살려 두고 특정 시간이 지나면 캐시를 무효화
    ```jsx
    // 3600초(60분) 동안 유효한 fetch
    fetch("https://...", { next: { revalidate: 3600 } });
    ```
  - 온디맨드 재검증 → 수요(demand)가 있을 때 재검증
    ```jsx
    // 데이터에 태그를 달아둡니다.
    fetch("https://...", { next: { tags: ["collection"] } });

    // 태그된 데이터를 재검증합니다.
    revalidateTag("collection");
    ```

## 메타데이터

[메타데이터](https://book.hajoeun.dev/friendly-next-js/next.js-1/undefined-5)

- 정적 메타데이터 → 정적 방식은 Metadata 객체를 정의하는 방식
  ```jsx
  import type { Metadata } from "next";

  export const metadata: Metadata = {
    title: "...",
    description: "...",
  };

  export default function Page() {}
  ```
- 동적 메타데이터 → generateMetadata라는 함수를 이용해 동적으로 Metadata 객체를 생성하는 방식
  ```jsx
  import type { Metadata, ResolvingMetadata } from 'next'

  type Props = {
    params: { id: string }
    searchParams: { [key: string]: string | string[] | undefined }
  }

  export async function generateMetadata(
    { params, searchParams }: Props,
    parent?: ResolvingMetadata
  ): Promise<Metadata> {
    const id = params.id

    const post = await fetch(`https://.../${id}`).then((res) => res.json())

    const previousImages = (await parent).openGraph?.images || []

    return {
      title: post.title,
      openGraph: {
        images: ['/some-specific-page-image.jpg', ...previousImages],
      },
    }
  }

  export default function Page({ params, searchParams }: Props) {}
  ```

# 섹션 3. Next.js 손에 익히기

## 프로젝트 소개

[프로젝트 소개](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined)

- `node -v`
- `nvm install 18.17.0`
- `nvm use 18.17.0 --default`
- `npx create-next-app weather-app`
- package.json에 어떤 명령어로 실행 가능한지 정의
  ```json
  "scripts": {
      "dev": "next dev",
      "build": "next build",
      "start": "next start",
      "lint": "next lint"
    },
  ```

## 페이지 구성하기

[페이지 구성하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-2)

## 페이지 간 이동하기

[페이지 간 이동하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-3)

- 클라이언트 컴포넌트는 최대한 말단으로 보내야 하기 때문에 `‘use client’` 선언이 필요한 영역을 최소한의 컴포넌트로 빼서 작업

## 페이지 스타일 입히기

[페이지 스타일 입히기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-4)

## 날씨 API 사용하기

[날씨 API 사용하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/api)

- 받아온 JSON 기반으로 Type 지정하기
  - https://transform.tools/json-to-typescript

## 날씨 데이터 조회하기

[날씨 데이터 조회하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-5)

## 에러, 로딩 페이지 만들기

[날씨 데이터 조회하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-5#undefined-2)

## 날씨 데이터 조회하기 2

[날씨 데이터 조회하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-5)

## 날씨 데이터 재검증하기

[날씨 데이터 조회하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-5#undefined-3)

- `app\api\revalidate\route.ts`
  ```tsx
  import { revalidateTag } from "next/cache";
  import { NextRequest, NextResponse } from "next/server";

  export async function POST(req: NextRequest) {
    const tag = req.nextUrl.searchParams.get("tag");
    if (!tag) throw new Error("태그는 필수입니다.");

    revalidateTag(tag);
    return NextResponse.json({ message: "재검증에 성공했습니다", tag });
  }
  ```
- `components\RevalidateButton.tsx`
  ```tsx
  "use client";

  type Props = {
    tag: string;
  };

  const RevalidateButton = ({ tag }: Props) => {
    const handleClick = async () => {
      const res = await fetch("/api/revalidate?tag=" + tag, {
        method: "POST",
      });
      console.log(res);
    };

    return (
      <div>
        <button onClick={handleClick}>캐시 비우기</button>
      </div>
    );
  };

  export default RevalidateButton;
  ```

## 메타데이터 다루기

[메타데이터 다루기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-6)

- 정적 메타데이터
  ```tsx
  // app\layout.tsx

  import "./global.css";

  import type { Metadata } from "next";
  import { Inter } from "next/font/google";

  const inter = Inter({ subsets: ["latin"] });

  export const metadata: Metadata = {
    title: "날씨 앱",
    description: "날씨를 알려드립니다",
  };

  export default function RootLayout({
    children,
  }: {
    children: React.ReactNode;
  }) {
    return (
      <html lang="en">
        <body className={inter.className}>{children}</body>
      </html>
    );
  }
  ```
- 동적 메타데이터
  ```tsx
  export function generateMetadata({ params, searchParams }: Props) {
    return {
      title: `날씨 앱 - ${searchParams.name}`,
      description: `${searchParams.name} 날씨를 알려드립니다`,
    };
  }
  ```

## 서비스 배포하기

[서비스 배포하기](https://book.hajoeun.dev/friendly-next-js/next.js-2/undefined-7)
