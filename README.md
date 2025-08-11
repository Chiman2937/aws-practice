# 📜 AWS S3 배포 연습

cloudfront URL: https://d15iyn5bpzfcok.cloudfront.net/

<br></br>

## ❗ 주의사항

AWS S3는 정적 파일만 배포할 수 있으므로, Next.js를 S3에 배포하려면 `next.config.js`에 `output: "export"`를 설정하여 정적 HTML/CSS/JS로 변환된 `out` 디렉토리를 생성해야 한다.

이 방식은 SSG 전용이므로 빌드 시점에 HTML을 생성할 수 없는 동적 라우트(SSR/ISR/무한경로)는 S3로 배포할 수 없다.
동적 라우트를 사용하려면 빌드 시점에 모든 경로를 `generateStaticParams`(App Router) 또는 `getStaticPaths`(Pages Router)로 미리 정의해야한다.

<br></br>

### ❓ 동적 라우트를 S3 배포시도 할 경우
`next.config.js` 에 `output: "export"` 설정이 되어있을 때 아래와 같이 동적라우팅 페이지를 만들면 Build Error가 발생한다.

동적 라우팅 페이지
```tsx
interface Props {
	params: Promise<{number: string}>
}

const RoutePage = async ({params}: Props) => {
	const {number} = await params;
	return (
		<div>{number}</div>
	)
}

export default RoutePage;
```

에러 메시지
```bash
> Build error occurred
[Error: Page "/[number]" is missing "generateStaticParams()" so it cannot be used with "output: export" config.]
```
