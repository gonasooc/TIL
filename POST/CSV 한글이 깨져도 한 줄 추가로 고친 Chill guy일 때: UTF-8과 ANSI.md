### 요구사항

기능 자체는 크게 복잡하지 않았다.

1. 별도의 코드 타입과 변경 전 코드, 변경 후 코드를 CSV나 XLSX, XLS 형식으로 업로드(별도의 API로 POST 요청)
2. 기존에 등록된 변경 전 코드와 중복된 값을 해당 API의 응답으로 CSV 데이터로 반환
3. 프론트에서는 사용자가 CSV 파일로 다운로드할 수 있도록 구현
4. 해당 CSV 파일을 검토한 후 해당 파일로 강제 업데이트 가능하도록 구현

`input`과 `button` 등을 구현했고 해당 `input`의 `onChange` 이벤트로 확장명에 대한 유효성 검사를 했다. POST 요청 시 `FormData` 객체를 생성해 request body에 담아서 보냈다. `headers`에 `accessToken`을 담는 등의 처리는 별도의 `createPostRequest` 커스텀 훅에서 처리했다.

```tsx
  const handleReplaceExcelList = async () => {
    if (!excelFile) {
      alert('파일이 선택되지 않았습니다');
      return;
    }

    try {
      setIsLoading(true);

      const formData = new FormData();
      formData.append("forceUpdate", forceUpdate.toString());
      formData.append("excel", excelFile);

      const result = await createPostRequest("mapping/excel/upload", formData, true, false);
      setIsLoading(false);
      setExcelFile(null);
      setValues({ platform: "", beforeCode: "", afterCode: "" });

			// 생략
			
      }
    } catch (e) {
      setIsLoading(false);
      if (e instanceof Error) {
        if (e.message === "LOGOUT") {
          return setInvalidToken(e.message);
        }
      }
      
      // 생략
    }
  };
```

`result`에 담겨 내려오는 CSV 데이터는 별도의 Blob 객체와 `a` tag 생성으로 다운로드 기능을 구현했다.

```tsx
  const downloadCSV = (data: string) => {
    if (!data) {
      alert('CSV 다운로드에 사용할 데이터가 없습니다.');
      return;
    }

    const blob = new Blob([data], { type: 'text/csv;charset=utf-8;' });
    const link = document.createElement('a');
    const url = URL.createObjectURL(blob);

    link.href = url;
    link.setAttribute('download', 'duplicate_list.csv');
    document.body.appendChild(link);
    link.click();

    document.body.removeChild(link);
    URL.revokeObjectURL(url);
  };
```

### Windows에서 CSV 한글이 깨져요

MacOS 환경에서 테스트한 결과 문제없이 동작했지만, Windows 환경에서 Excel로 파일을 열었을 때 한글이 깨지는 이슈가 발생했다.

![image.png](attachment:203613a3-5b10-45a8-b241-510fb9f4209b:image.png)

체크해보니 이슈는 명확했고 수정은 간단하나 유니코드와 인코딩, 그리고 UTF-8에 대한 대략적인 이해가 필요했다. 이번 기회에 간단하게라도 정리하는 편이 좋겠다 싶었다.

### 유니코드(Unicode)

유니코드(Unicode)는 전 세계의 모든 문자를 컴퓨터에서 일관되게 표현하고 다룰 수 있도록 설계된 산업 표준이다. 기존의 문자 집합은 영어 중심으로 설계되어 다국어 환경에서 호환성이 떨어지는 문제가 있었다.(Windows의 CP949). 유니코드는 이런 문제를 해결하기 위해 서로 다른 언어와 쓰기 방식, 기호 등을 모아 번호를 할당하고 문자를 정의하는 표준 문자 집합이다. 유니코드에 할당된 문자의 값을 표현하기 위해서는 코드 포인트(code point)를 사용하고 `U+`를 붙여서 표시한다. JavaScript에서는 `\uAC00`과 같이 표현하기도 한다.

- `A` → `U+0041`
- `가` → `U+AC00`

### 인코딩(Encoding)

컴퓨터는 0과 1로 표현되기 때문에 글자, 이미지, 파일 등은 결국 0과 1로 저장이 되어야 한다. 0과 1로 변환하는 것을 인코딩(Encoding, 부호화)이라 하고, 이 중 글자를 0과 1로 변환하는 것을 문자 인코딩(Character Encoding)이라고 한다. 대표적인 문자 인코딩 방식은 다음과 같다.

| **인코딩** | **특징** |
| --- | --- |
| UTF-8 | 가변 길이(1~4바이트), ASCII와 호환, 웹 표준 |
| UTF-16 | 고정 길이(2 또는 4바이트), BOM 필요 가능성 있음 |
| UTF-32 | 4바이트 고정 길이, 메모리 사용량 많음 |
| CP949 (EUC-KR) | 한글 Windows 기본 인코딩, UTF-8과 호환되지 않음 |
| Shift_JIS | 일본어 인코딩, EUC-KR과 혼용 시 깨짐 발생 |

### Windows에서 CSV 한글이 깨지는 이유

Windows에서 CSV 한글이 깨지는 이유는 기본적으로 Excel이 UTF-8을 ANSI(CP949)로 잘못 해석하기 때문이다. Excel이 UTF-8을 잘못 해석하는 과정은 다음과 같다.

1. CSV를 더블 클릭해서 Excel에서 열면 Excel은 파일을 기본 인코딩(ANSI, CP949)으로 가정함
2. UTF-8로 저장된 한글이 CP949로 해석되면서 깨짐

Windows가 유니코드를 채택했을 때는 UTF-8이 없었기 때문에 기본 인코딩 방식이 ANSI 또는 UTF-16 LE인 경우이다. 따라서 Windows에서 제대로 한글을 표현하고자 한다면 인코딩을 ANSI로 지정해야 하지만 JavaScript에서는 직접 지원하지 않고 기본적으로 UTF-8을 사용해서 텍스트를 다룬다. 해결 방법은 BOM을 추가해서 Excel이 UTF-8로 인식하도록 하는 것이다.

### BOM(Byte Order Mark)

BOM은 간단히 말해 문서의 맨 앞에 눈이 보이지 않는 특정 바이트를 넣고 이걸 해석함으로써 어떤 인코딩 방식이 사용되었는지 알아내는 방법이다. 인코딩 방식이 little-endian이나 big-endian, 혹은 UTF-8인지 알 수 있도록 유니코드 파일이 시작되는 첫 부분에 보이지 않게 2~3바이트의 문자열을 추가한다. 상단에 문제가 생겼던 코드를 수정하면 아래와 같다.

```tsx
  const downloadCSV = (data: string) => {
    if (!data) {
      alert('CSV 다운로드에 사용할 데이터가 없습니다.');
      return;
    }

    const BOM = '\uFEFF'; // 한글 인코딩 처리를 위해 추가
    const blob = new Blob([BOM + data], { type: 'text/csv;charset=utf-8;' });
    const link = document.createElement('a');
    const url = URL.createObjectURL(blob);

    link.href = url;
    link.setAttribute('download', 'duplicate_list.csv');
    document.body.appendChild(link);
    link.click();

    document.body.removeChild(link);
    URL.revokeObjectURL(url);
  };
```

### BOM 사용 시 주의 사항

다만 유닉스나 리눅스 편집 프로그램에서는 UTF-8을 기본으로 사용하고 있어 공백 관련 오류가 날 수 있고, 데이터를 비교하는 경우 육안으로는 동일한 데이터지만 문자열 앞에 BOM이 붙어 있어 다른 데이터로 나올 수 있음을 인지할 필요가 있다. 또한 JSON 파일에서는 BOM을 사용하지 않는 것이 일반적인데, BOM이 포함되면 JSON을 파싱하는 과정에서 오류가 발생할 수 있다는 점을 주의할 필요가 있겠다.

### 참고자료

- https://bumday.tistory.com/185
- [https://velog.io/@csw98213/javascript-BOMByte-Order-Mark을-이용한-CSV-파일-내보내기-한글-깨짐-현상-해결](https://velog.io/@csw98213/javascript-BOMByte-Order-Mark%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-CSV-%ED%8C%8C%EC%9D%BC-%EB%82%B4%EB%B3%B4%EB%82%B4%EA%B8%B0-%ED%95%9C%EA%B8%80-%EA%B9%A8%EC%A7%90-%ED%98%84%EC%83%81-%ED%95%B4%EA%B2%B0)
- https://stackoverflow.com/questions/66072117/why-does-windows-use-utf-16le
- [https://jeongdowon.medium.com/unicode와-utf-8-간단히-이해하기-b6aa3f7edf96](https://jeongdowon.medium.com/unicode%EC%99%80-utf-8-%EA%B0%84%EB%8B%A8%ED%9E%88-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-b6aa3f7edf96)
- [https://nnoco.github.io/study/gardenist/2020/05/01/문자-인코딩,-문자셋,-유니코드.html](https://nnoco.github.io/study/gardenist/2020/05/01/%EB%AC%B8%EC%9E%90-%EC%9D%B8%EC%BD%94%EB%94%A9,-%EB%AC%B8%EC%9E%90%EC%85%8B,-%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C.html)
- https://developer.mozilla.org/ko/docs/Glossary/Unicode
- https://travelpark.tistory.com/84
- https://brownbears.tistory.com/124
- https://inbird81.github.io/posts/utf8-bom-sourcetree/