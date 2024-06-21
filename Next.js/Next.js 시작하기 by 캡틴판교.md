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