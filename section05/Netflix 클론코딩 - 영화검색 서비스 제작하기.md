# Netflix 클론코딩 - 영화검색 서비스 제작하기

사용 기술
- Next.js 14
    - Server Action
- [Material Tailwind CSS](https://www.material-tailwind.com/)
- Supabase
- React Query
- [Recoil](https://recoiljs.org/) (전역 상태관리 라이브러리)
- Typescript
- **+ react-intersection-observer**

<br>

주요 기능
- 무한 스크롤, 영화 검색 기능, 영화 상세 페이지 구현
- seo 작업 (타이틀, 설명, ogimage)

<br>

## Part 3. 영화 검색 기능 & 영화 개별 상세페이지 구현

### Recoil 사용법

Recoil 설치: Recoil을 사용하려면 먼저 패키지를 설치해야 합니다. npm을 사용하는 경우, 터미널에 npm install recoil을 입력하세요. yarn을 사용하는 경우, yarn add recoil을 입력하세요.

Atom: Recoil에서 atom은 상태의 단위입니다. atom은 우리가 읽고 쓸 수 있는 상태의 조각으로 생각할 수 있습니다. atom을 생성하려면, atom 함수를 사용하고 필요한 key와 default 값을 제공해야 합니다.
import { atom } from 'recoil';

```tsx
const textState = atom({
  key: 'textState', // unique ID (with respect to other atoms/selectors)
  default: '', // default value (aka initial value)
});
```

​
useRecoilState: 이 hook은 Recoil 상태를 읽고 쓰는 데 사용됩니다. useState hook과 비슷하게 동작합니다.

```tsx
import { useRecoilState } from 'recoil';

function TextInput() {
  const [text, setText] = useRecoilState(textState);

  const onChange = (event) => {
    setText(event.target.value);
  };

  return (
    <div>
      <input type="text" value={text} onChange={onChange} />
      <br />
      Echo: {text}
    </div>
  );
}
```
​
useRecoilValue: 이 hook은 Recoil 상태를 읽는 데 사용됩니다.
import { useRecoilValue } from 'recoil';

```tsx
function TextDisplay() {
  const text = useRecoilValue(textState);

  return <div>Echo: {text}</div>;
}
```
<br>
<br>

## Part 4. 무한 스크롤 기능 구현하기 & 더 나은 검색을 위한 SEO 작업하기

### react-intersection-observer 라이브러리와 useInView 함수 사용법

`react-intersection-observer`는 Intersection Observer API를 React에서 사용하기 쉽도록 만든 라이브러리입니다. 이 라이브러리에서 제공하는 `useInView` 훅을 사용하면, 특정 요소가 화면에 노출되었는지 감지하는 기능을 쉽게 구현할 수 있습니다. 이는 무한 스크롤 기능을 만들 때 유용하게 사용될 수 있습니다.

`useInView` 훅을 사용하는 방법은 다음과 같습니다

```jsx
import { useInView } from 'react-intersection-observer';

...

const [ref, inView, entry] = useInView({
  threshold: 0,
});

```

여기서 `ref`는 감지할 요소에 연결해야 하는 참조이며, `inView`는 해당 요소가 화면에 노출되었는지 여부를 나타내는 불리언 값입니다. `entry`는 Intersection Observer Entry 객체로, 여러 정보를 포함하고 있습니다.

이렇게 반환된 `ref`를 감지할 요소에 연결하고, `inView`를 이용하여 화면에 요소가 노출되었을 때 원하는 동작을 수행하면 됩니다.

```jsx
<div ref={ref}>
  {inView && 'Element is in view!'}
</div>

```

위 코드에서는 `ref`를 div 요소에 연결하고, 해당 요소가 화면에 노출되면 'Element is in view!'라는 문자열이 출력되도록 하였습니다.

<br>

### useInfiniteQuery 기본 사용법 (무한스크롤 제외)

`useInfiniteQuery`는 `react-query` 라이브러리의 핵심 기능 중 하나입니다. 이를 사용하면 무한 스크롤과 같은 기능을 쉽게 구현할 수 있습니다.

먼저, `useInfiniteQuery` 훅을 사용하는 방법은 다음과 같습니다:

```jsx
import { useInfiniteQuery } from 'react-query';

...

const {
  data,
  fetchNextPage,
  hasNextPage,
  isFetchingNextPage,
  status,
} = useInfiniteQuery('todos', fetchTodos, {
  getNextPageParam: (lastPage, pages) => lastPage.nextPage,
});

```

여기서 `fetchTodos`는 API 호출을 수행하는 함수이며, `getNextPageParam`는 다음 페이지를 가져오는 데 사용되는 매개변수를 반환하는 함수입니다.

페이지를 불러오는 방법은 다음과 같이 `fetchNextPage` 함수를 사용하면 됩니다.

```jsx
<button
  onClick={() => fetchNextPage()}
  disabled={!hasNextPage || isFetchingNextPage}
>
  {isFetchingNextPage
    ? 'Loading more...'
    : hasNextPage
    ? 'Load More'
    : 'Nothing more to load'}
</button>

```

이 코드는 'Load More' 버튼을 만들며, 더 이상 로드할 페이지가 없거나 다음 페이지를 불러오는 중일 때는 버튼이 비활성화됩니다.

<br>

### Next.js 14의 generateMetadata 함수 사용법

Next.js 14에서는 `generateMetadata` 함수를 통해 페이지별 메타데이터를 동적으로 생성할 수 있습니다. 이 함수는 페이지의 파라미터와 URL의 검색 파라미터를 인수로 받아서 메타데이터 객체를 반환합니다.

다음은 `generateMetadata` 함수의 사용 예시입니다.

```jsx
import { getMovie } from "actions/movieActions";

export async function generateMetadata({ params, searchParams }) {
  const movie = await getMovie(params.id);

  return {
    title: movie.title,
    description: movie.overview,
    openGraph: {
      images: [movie.image_url],
    },
  };
}

```

위의 코드에서 `generateMetadata` 함수는 `getMovie` 함수를 사용하여 영화 정보를 불러온 후, 이 정보를 바탕으로 메타데이터 객체를 생성하고 반환합니다. 생성된 메타데이터 객체에는 `title`, `description`, `openGraph` 속성이 포함되어 있습니다.

- `title`: 페이지의 제목을 설정합니다.
- `description`: 페이지의 설명을 설정합니다.
- `openGraph`: Open Graph 프로토콜에 따른 메타데이터를 설정합니다. `images` 속성을 통해 페이지의 대표 이미지를 설정할 수 있습니다.

따라서, 이 함수를 사용하면 각 페이지의 메타데이터를 동적으로 생성하고 관리할 수 있습니다. 이는 SEO 최적화에 도움이 됩니다.