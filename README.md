# B2C 차량대여 서비스 모바일 웹 제작 

## 배포 링크 : [바로가기 클릭](https://3rd-assignment.vercel.app/)


## 실행결과

<img src="https://user-images.githubusercontent.com/99943583/206390851-44860da7-6cd4-4935-b0aa-18534b5ce190.gif">

#### STACK
<img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=React&logoColor=white"> <img src="https://img.shields.io/badge/TypeScript-3178C6.svg?&style=for-the-badge&logo==TypeScript&logoColor=white" ><img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=JavaScript&logoColor=white">
<img src="https://img.shields.io/badge/styled components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white">
<img src="https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=React Router&logoColor=white">

## 팀 소개

- [이빛나 (팀장)](https://github.com/bitnaleeeee)
- [모상빈](https://github.com/Topbin2)
- [김진석](https://github.com/genuine-seok)
- [박우빈](https://github.com/Debonchocola)
- [이의연](https://github.com/strongpond)
- [조성호](https://github.com/CSH111)
- [전대원](https://github.com/eodnjs467)


<br>

## 프로젝트 소개 

- 목표 : B2C 차량대여 서비스 사이트 구축
- 기간 : 2022.11.01 ~ 2022.11.03
- 주관 : 원티드 
<br>

## 구현 사항
- [x] Figma 상의 디자인 및 기능 구현
- [x] 차량 리스트 : 차량이 없을 때 처리, 차량 불러오는 중 처리
- [x] 차량 상세 페이지 


<br />

## 폴더 구조 

```
📦 src
├── 📂  apis
├── 📂  assets
├── 📂  components
├── 📂  context
├── 📂  hooks
├── 📂  pages
├── 📂  types
├── 📂  utils
```

- apis : issue 관련 api service 요청하는 폴더입니다
- assets : 전역 스타일링 관리 폴더입니다
- components : 공용 컴포넌트 입니다
- context : issue 상태관리 context 폴더 입니다
- hooks : scroll 관련 커스텀 훅 폴더입니다
- pages : 페이지 관리 컴포넌트 입니다
- types : 공용타입 관리 폴더 입니다
- utils : dateformatting, axios 관련 유틸 함수 폴더입니다

<br/>

## 주요 기능

###  Design token 과 theme

```javascript
import { DefaultTheme } from "styled-components";

export const theme: DefaultTheme = {
  colors: {
    white: "#ffffff",
    black: "#000000",
    gray: "#D9D9D9",
    blue: "#0094FF",
  },
  fontSize: {
    small: "12px",
    medium: "14px",
    large: "17px",
    xLarge: "21px",
  },
};

export const Container = styled.div`
  display: flex;
  align-items: center;
  height: 48px;
  padding: 0 20px;
  ${({ theme }) => css`
    color: ${theme.colors.white};
    background-color: ${theme.colors.blue};
    font-size: ${theme.fontSize.large};
    font-weight: 700;
  `}
`;
```

### 유틸 함수를 이용한 상수값 관리

```javascript
export const converSegmentToCategory = (segment: TSegment) => {
  switch (segment) {
    case "C":
      return "소형";
    case "D":
      return "중형";
    case "E":
      return "대형";
    case "SUV":
      return "SUV";
    default:
      throw new Error("segment 타입을 확인해주세요.");
  }
};
```

### 비동기 통신 로직 최적화 


```javascript
import { useQuery } from "@tanstack/react-query";
import { useSearchParams } from "react-router-dom";

import { getCarDetail } from "../apis";

export const useGetCarDetail = () => {
  const [searchParams] = useSearchParams();
  const brand = searchParams.get("brand") || "";
  const name = searchParams.get("name") || "";
  const segment = searchParams.get("segment") || "";
  const fuelType = searchParams.get("fuelType") || "";

  const { data, isLoading } = useQuery({
    queryKey: ["car-detail", brand, name, segment, fuelType],
    queryFn: () => getCarDetail({ brand, name, segment, fuelType }),
    onError: (error) => alert(error),
    retry: false,
    refetchOnMount: false,
    refetchOnWindowFocus: false,
    staleTime: 60 * 1000 * 5, // 5 minutes
  });

  return { data, isLoading };
};
```

### 

```javascript
import { ROUTER } from "./routes";
import CarList from "../pages/CarList";
import CarDetail from "../pages/CarDetail";
import { BrowserRouter, Route, Router, Routes } from "react-router-dom";
function RootRouter() {
  const ListRoutes = [
    {
      path: ROUTER.CAR_INFO,
      component: <CarList />,
    },
    {
      path: ROUTER.CAR_DETAIL,
      component: <CarDetail />,
    },
  ];

  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<CarList />} />
        {ListRoutes.map((page, index) => {
          return (
            <Route
              key={"route" + index}
              path={page.path}
              element={page.component}
            />
          );
        })}
      </Routes>
    </BrowserRouter>
  );
}

export default RootRouter;
```
### 카카오톡, 페이스북 공유 시 미리보기

`index.html` `meta` 태그를 활용

```html
  <meta name="title" property="og:title" content="현대 아이오닉5" />
  <meta name="description" property="og:description" content="월 600,000원" />
  <meta name="image" property="og:image" content="https://interview.platdev.net/ioniq5.png" />
```

<br>

## 프로젝트 설치 및 실행

<br/>

1. Git Clone
```plaintext
$ git clone https://github.com/pre-onboading-2team/Week1_2_Issue_List.git
```

2. 프로젝트 패키지 설치
```plaintext
$ npm install
```
3. 프로젝트 실행

```plaintext
$ npm start
```


