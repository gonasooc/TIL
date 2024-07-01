## Next 프로젝트 생성 및 소개

### Next.js 폴더 구조 설명 - 설정 파일편

- dependencies, devDependencies로 웹 어플리케이션의 성격을 파악
    
    ```json
      "dependencies": {
        "react": "^18",
        "react-dom": "^18",
        "next": "14.2.4"
      },
      "devDependencies": {
        "eslint": "^8",
        "eslint-config-next": "14.2.4"
      }
    ```
    
- custom scripts
    
    ```json
      "scripts": {
        "dev": "next dev",
        "build": "next build",
        "start": "next start",
        "lint": "next lint"
      },
    ```
    

### Next.js 폴더 구조 설명 - 폴더편

- public - 정적 자원 관리

## Next.js 실무 프로젝트 환경 설정

### Next.js 실무 레벨 개발 환경 설정

- .eslintrc.json은 .js로 변경 가능한데, key-value로 한정해서 작성해야 하거나 주석 삽입이 안 되는 부분을 고려하면 .js로 변경해서 사용 가능, JSON 파일 내용을 JS 파일 내용에 맞게 수정

### ESLint 소개 및 설정 파일 내용 설명

```jsx
module.exports = {
	extends: ['next/core-web-vitals', 'plugin:prettier/recommended'],
	// plugins: ['prettier', 'unused-imports'],
	plugins: ['prettier'],
	rules: {
		// 선언되지 않은 변수 또는 임포트 구문 정리 규칙
		'no-undef': 'error',
		// 'unused-imports/no-unused-imports': 'error',

		// 프리티어 설정
		'prettier/prettier': [
			'error',
			// 아래 규칙들은 개인 선호에 따라 prettier 문법 적용
			// https://prettier.io/docs/en/options.html
			{
				singleQuote: true,
				semi: true,
				useTabs: true,
				tabWidth: 2,
				trailingComma: 'all',
				printWidth: 80,
				bracketSpacing: true,
				arrowParens: 'avoid',
			},
		],
	},
};

```

### 페이지 라우팅 기본 및 리액트 컴포넌트와 JSX 기초 설명

- 페이지 라우팅 - 도메인 끝 루트로 접근했을 때 pages/index.js로 접근
- pages 폴더 안에 파일명은 소문자로 작성하는 컨벤션

### 페이지 라우팅 구성 후 컴포넌트 분석

- 파일 기반 라우팅 자동 설정

### Link 컴포넌트를 이용한 페이지 이동

- _app.js → 커스텀 앱 컴포넌트
- 빌트인 컴포넌트 - Next.js에서 미리 구성된 컴포넌트

### Link 컴포넌트 설명

- <Link /> vs a tag - https://cracking-next.vercel.app/docs/page-router/navigating

### 커스텀 앱(_app 파일) 컴포넌트 소개

- _app 파일은 넥스트에서의 루트 컴포넌트를 의미
- 페이지 이동 시 유지할 공통 레이아웃 구성, 페이지에 추가 데이터 구성, 전역 CSS 추가

### layout 컴포넌트 생성 및 적용

```jsx
import Layout from '@/layouts/Layout';
import '@/styles/globals.css';
import Link from 'next/link';

export default function App({ Component, pageProps }) {
	return (
		<Layout>
			<Component {...pageProps} />
		</Layout>
	)
}
```

### 백엔드 서버 구성

- 별도의 backend 폴더 내에 구성을 갖추고 npm install 후 npm run dev 진행, 별도로 4000 포트로 서버를 띄워놓음

## 실전 프로젝트 - Image 컴포넌트와 Next.js 스타일링 방법

### 상품 목록 UI 구성 및 Image 컴포넌트 소개

- Next.js의 <Image /> 컴포넌트의 경우 next.config.js 설정 파일 등에서 호출하는 cdn이나 별도의 주소를 통해 확인 가능한 protocol, hostname, port, pathname 등을 정의해줘야 함
    
    ```jsx
    // next.config.js
    
    module.exports = {
      images: {
        remotePatterns: [
          {
            protocol: 'https',
            hostname: 'assets.example.com',
            port: '',
            pathname: '/account123/**',
          },
        ],
      },
    }
    ```
    

### Next.js의 CSS 스타일링 방법 - CSS Module

- 해당 컴포넌트와 동일한 경로 내에 ComponentName.module.css 파일을 위치하고, 컴포넌트 내에서 호출

```jsx
...
**import styles from './ProductList.module.css';**

const ProductList = () => {
	...

  return (
    <ul>
    {
      products && products.map((product) => {
        return (
          **<li key={product.id} className={styles.item}>
						...**
          </li>
        )
      })
    }
  </ul>
  );
};

export default ProductList;
```

## 실전 프로젝트 - 동적 라우팅과 데이터 호출

### getServerSideProps 소개

```jsx
import React from 'react';

export default function ProductDetailPage({ message }) {
  return (
    <div>상세페이지 - {message}</div>
  );
};

export async function getServerSideProps(context) {
  console.log(context.params.productId);
  return {
    props: {message: '서버에서 보내준 메세지'},
  }
}
```

### getServerSideProps로 데이터 호출 및 화면에 표시

- getServerSideProps는 페이지 컴포넌트를 그리기 전에 데이터를 받아오기 위한 데이터 호출 메서드

```jsx
import ProductHeader from '@/components/ProductHeader';
import axios from 'axios';
import React from 'react';

export default function ProductDetailPage({ message, productInfo }) {
  return (
    <>
      <ProductHeader title={'상품상세정보페이지'} />
      <div>상세페이지 - {message}</div>
      <p>{productInfo.name}</p>
    </>
  );
};

export async function getServerSideProps(context) {
  const id = context.params.productId;
  const response = await axios.get(`http://localhost:4000/products/${id}`);
  response.data

  return {
    props: {
      message: '서버에서 보내준 메세지',
      productInfo: response.data
    },
  }
}
```