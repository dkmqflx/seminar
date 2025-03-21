### 서버에서 받아야하는 상태들의 특성

- 주문을 생성해서 결제를 한다고 하자
- 주문 데이터는 클라이언트에서 관리해야할 데이터가 아니고 서버 DB에 저장되어 있는 값
- 서버에서 관리하는 값을 가져오기 위해서는 비동기 api가 필요하다
- 사장님이 접수를 대기하고 있다가 내가 모르는 동안에 접수를 완료해서 변경될 수 있다
- 나는 접수 대기라고 처리하고 있는데 접수가 완료되면 out of date가 될 가능성이 있다.

### Query Key

- Array 형태
- paginataion이나 각종 옵션을 줄 때, 또는 query key의 convention을 맞추기 위해 사용할 수 있다
- react-query v4가 새로 나왔는데(아직 베타) 이제는 string형태의 key는 못쓰게 바뀐다

### useQuery Opition

- enabled :기본적으로 useQuery 는 컴포넌트가 mount 될 때 실행되는데, enabled 속성을 통해서 이를 제어할 수 있다.

### queries 파일 분리도 추천

- useQuery를 컴포넌트에 직접 쓰면 관리를 하기 힘들수도 있기 때문에 도메인 별로 queries라는 파일을 따로 빼서 도메인 별로 묶어서 관리하고 있다

### 그럼 질문 !

- 질문 1.

  - Case by case 라고 생각한다
  - 1번을 쓰고 1번으로 불가능 하면 2번을 쓸 것이다
  - 1번은 간단하다는 장점이 있다
  - 2번을 쓰게 되면 코드의 복잡도가 올라갈 수 밖에 없기 때문
  - 간단한 것이라면 1번, 복잡한, 좀 더 관리할 것들이 많다면 2번 사용할 것

- 질문 2.
  - onSuccess에서 액션을 dispatch 해주면 가장 좋을 것 같다.

### useMutation Option

- Optimistic update ?
  - 페이스북에서 좋아요를 누르면 api 통신을 한다. 서버에 저장 되어야 하니까
  - 그런데 서버에서 응답을 받기 전에 클릭하는 것 만으로도 파랗게 좋아요 버튼의 색깔이 변한다
  - 그 이유는 api 통신이 성공할 것이라고, optimisitc 즉, 낙관적으로 보고 일단은 ui를 먼저 업데이트 하기 때문이다
  - 그리고 api 성공하면 그대로 두고, 실패하면 roll back 한다.

### Query Invalidation

- Stale
  - React query는 기본적으로 cache된 데이터를 stale 한 상태로 취급한다
  - stale이란 최신화가 필요한 데이터라는 의미로, stale한 상태가 아래와 같은 상황에서 refetch 된다
  - 새로운 query 인스턴스가 마운트될 때 (페이지를 이동했다가 다시 왔을 때를 의미)
  - 브라우저 화면을 이탈했다가 다시 포커스할 때
  - 네트워크가 다시 연결될 때
  - 특별히 설정한 refetch interval에 의해서 (refetchInterval)

### RFC 5861

- Stale-while-revalidate

  - Stale -> 신선하지 않은, 낡은
  - 예를 들어 주문하고 나면 내 앞에 보이는 것은 주문 접수 전 데이터인데, 사장님이 접수를 하게 되면 이 데이터는 stale 한 데이터가 된다
  - 클라이언트에서 stale 데이터가 있으면 다시 api 요청을 해야 한다
  - Api 요청하는 동안에는, 아래 cache-control에서 보면
  - max-age=600 이므로 캐시가 600초 통안 유효하다고 볼 것이고
  - Stale-while-revalidate = 30초 조건에서,
  - 600초 내에서는 캐시가 유효하다고 보니까 계속 캐시가 오는데 600초가 지나고 610초가 되면 이 데이터는 낡은 데이터가 된다
  - Api 요청을 하는 동안에는 loader가 나타나 던가 할 텐데 Stale-while-revalidate 조건에 의해 600초가 지난 30초 동안은 stale한 데이터를 반환한다
  - 610초에 조회하면 loader가 아닌 stale 한 데이터를 보여준다 대신에 백그라운드에서 다시 refetch 해서 데이터를 update 한다.

### 이 컨셉을 메모리 캐시에도 적용해보자!

- staleTime
  - 주문 같은 경우는 데이터의 최신성이 중요하기 때문에 잘 사용하지 않는다.
  - 하지만 광고 같은 경우에는 기존에 데이터를 어느 시간 정도에는 보여주도록 한다면 statleTime을 조정할 수 있을 것
  - 기본적으로 default 값이 0이기 때문에 탭을 다른 탭으로 갔다가 다시 윈도우로 돌아오면, 바로 윈도우 포커스 시점에 다시 refech가 일어난다

### Query 상태흐름

- inactive가 되어도 내부 메모리에는 있다
- Cache time이 만료되면 즉, Default time이 5분이니까 5분이 지나면 가비지 컬렉터에 의해 삭제가 된다

### Zero-config에서도 이런 역할을

- Stale time이 30초인 경우, 컴포넌트 언마운트 되었다가 다시 마운트 되면
- 데이터가 fresh한 상태이기 때문에 다시 refetch가 일어나지 않는다
- 또 다른 예시로 staleTime이 3분이고 cacheTime이 0이면, 데이터가 3분 동안 fresh 상태이더라도
- cache 되지 않기 때문에, 페이지 이동 후, 다시 돌아오면 isLoading과 isFetching은 계속 true인 상태가 되어 새로운 데이터를 fetching 한다.
- retry → 3
  - api 실패했을 때 3번 까지는 요청하고, 요청하는 간격이 계속해서 늘어난다

### 전역상태처럼 관리되는 데이터들

- 컴포넌트 A의 staleTime이 30초인데 10초뒤에 컴포넌트 B가 마운트 되면 api 호출이 발생하지 않는다.
- 코드보면 useContext 사용된 것 확인할 수 있다
- redux-persist를 사용해서 스토리지에 접근할 수 있는 것 처럼, experimental 기능으로 persist client를 제공하고 있다
- 나중에 Storage에 값들을 저장할 수 있지 않을까

### 어떻게 바뀌었나요

- client store는 ui 정도 또는 사용자 정보 정도만 남아 있다

### Q&A

- 질문1) 레거시 기준은 어떻게 정해졌나요 ?
  - 내부적으로는 생산성 측면과 유지보수가 가능한가를 기준으로
  - 우아한 JS를 더 이상 하용하지 않았기 때문에 그러한 것들을 합치면서 기술스택을 통일하는 과정에서 생산성과 기술측면에서 정했다
  
- 질문2) swr을 사용하고 있는데 react-query랑 특별히 다른점이 있나 궁금하네요 swr과 react-query의 차이점이 궁금해요 !
  - 비슷한 역할을 수행하는 라이브러리인데 개인적으로는 리액트 쿼리가 더 많은 기능, 인터페이스를 제공하는 것 같다
  - 리액트 쿼리 docs 가면 comparison 항목 있는데 (\***\*[Comparison | React Query vs SWR vs Apollo vs RTK Query vs React Router](https://react-query.tanstack.com/comparison))**
  - 실질적으로 리액트 쿼리에서 제공하는 기능이 더 많고 좋아보여서 선정하게 되었다
  
- 질문3) useQueries로 처리하면 각 쿼리에 option들이 적용되질 않아서 각 useQuery 별로 options값을 다르게 줘야할 때 parallel한 방식으로 쓰게 되는데, parallel로 선언한 useQuery 부분들의 데이터 패칭이 다 끝날 때 까지 기다릴 때는 어떻게 처리하시나요 ? isLoading을 여러개 선언하시 나요 ?
  - isLoading을 전체적으로 관리하는 변수나 state를 만들어주는 편
  - 긱자 로딩을 하거나 먼저보여줘도 되는 데이터는 먼저보여준다
  - 상황에 따라 다르게 처리하고 있다.
  
- 질문 4) Client Store는 어떤 거를 쓰시는지 궁금합니다
  - 현재는 MobX 사용, 리덕스는 코드가 너무 길어지는 단점 있다
  - 스토어의 역할이 많이 줄어들기도 해서 간략하고 파워풀한 MobX사용
  
- 질문 5) query 키들이 중복되면 안된다고 하셨는데 어떻게 관리하시나요 ?
  - 쿼리를 도메인별로 묶다보니까 중복적으로 되지 않고
  - 어차피 여러군데에서 사용하지 않고 ppt에서 설명한 것 처럼 쿼리를 하나의 파일 안에서 선언부로 놓고 다른 파일에서 가져가서 사용하기 때문에 함수명을 쿼리키로 쓰기도 한다.
  - 변수명 자체가 중복되지 않기 때문에 useQuery로 선언된 변수명 자체를 많이 사용하기도 한다

- 질문 6) 배민에선 리덕스와 리액트 쿼리를 같이 사용하나요 ? 리액트 쿼리로만으로도 프론트엔드에서 대부분의 데이터를 관리할 수 있을까요 ?
  - 배민도 FE조직 많아서, 주문 조직에서는 MobX와 리액트 쿼리 사용
  - client state는 MobX로, server state는 리액트 쿼리로 관리하고 있다
  
- 질문 7) 디폴트 옵션 어떻게 쓰시는지 궁금합니다.
  - 크게 신경쓰지 않는다
  - api 별로 커스텀해야 되는 옵션들이 많아서, api 별로 따로 관리하고 있다
  - 주문상태 조회, 배달 상태 조회 등 각각의 특성이 다르기 때문
  
- 질문 8) 오늘 리액트 쿼리의 장점들을 많이 다루어주셔서 좋았습니다. 혹시 리액트 쿼리를 사용하시면서 아쉬웠던 부분들이 있으면 공유해주시면 좋겠습니다.
  - ppt에 마무리에 언급한바와 같지 컴포넌트가 비대해질 수 있다
  - onSuccess에서 처음 데이터를 받아왔을 때만 실행할 수 있는 옵션들이 제공되면 좋을 것 같다.
  -
- 질문 9) 지금 리덕스 및 사가를 배우는 중입니다. 얘네는 러닝커브가 긴데 배우던거 마저 쓰는게 좋을까요 리덕스를 경험해보고 쿼리를 경험해 보는 게 좋을까요 ?
  - 공부하는 입장에서는 둘다 배우는게 좋다
  - 마무리하고 리덕스의 문제점이나 불편함을 느낀 다음 쿼리로 넘어가는게 좋은 로드맵 일 것 같다

- 질문 10) 리덕스만 경험해보았는데요. 리액트 쿼리는 무슨 기술의 대체가 가능한 건지 알 수 있을까요? 리덕스 전체인지, api와 관련된 전역상택 관리인지, 리덕스 사가인지 등등 .. 초보적인 질문이라 죄송합니다
  - 대체 가능히디는 이야기 보다는 api 와 관련해서 전역상태 관리하는 것을 리액트 쿼리를 통해서 해결하고 있다가 맞는 것 같다
  - 리덕스를 대체하는 것은 아니다, 리덕스는 애초애 클라이언트 state를 관리하는 도구이기 때문
  - 그래서 MobX를 따로 사용하는 것
  - 즉 api와 관련된 전역 상태 관리를 대체하는 것
  
- 질문 11) 리액트 쿼리에서 받아온 데이터로 파생된 데이터는 어떤 방법으로 관리하시는지 궁금합니다
  - 성격에 따라 다를 것 같은데
  - 어떤 것은 로컬 state로, 또 다른 어떤 것은 쿼리 cache에 넣어서 사용할 수 있을 것 같다
  - 한마디로 코드에서 상태관리가 필요하다는 이야기인데 성격에 따라 어떤건 클라이언트 스테이트, 또 다른 경우에는 로컬 스테이트로 관리할 수 있을 것 같다
  - 제일 중요한건 파생된 데이터를 관리하지 않기 위해서 데이터 상태를 모으는 것이 중요하지 않을까 싶다
  - 개인적으로는 이러한 데이터를 관리하는 함수를 만들어서 사용할 것 같다
  - select를 쓰거나 select를 사용하지 말아야할 데이터는 filter 같이 함수를 만들어서 사용했을 것 같다

- 질문 12) 에러바운더리는 어떻게 가져가시나요? 싱글턴으로 관리되는 에러바운더리 옵션은 에러페이지 이동 정도인데 각 쿼리마다의 에러 처리 전략은 어떻게 가져가시는지 궁금합니다.
  - 기획이랑 연관되어 있는 부분이 있기 때문에 에러바운더리는 사용하지 않고 있다.
  - 정확히는 에러 바운더리는 사용하는데, Suspense를 모드를 켜서 에러 바운더리로 토스하지는 않고 있다
  - 어떤 api는 스택바를 보여줘야 하고 또 다른 api의 경우에는 에러가 발생했을 때 아무것도 보여주지 않기도 하기 때문
  - Suspense 지금 사용하지 않는 이유는 현재 기준으로 experiment 단계이기 때문
  - 개인적으로는 experiment 단계에 있는 것을 프로덕션에 사용하는 것을 지양하는 편

- 질문 13) Recoil과 리액트 쿼리를 섞어 쓰는 케이스도 있는지 궁금합니다
  - 섞어 쓰면 편하다
  - Recoil도 experiment 단계라서 안쓰는 것
  - 클라이언트 스테이트와 서버 스테이트를 각각 나누어서 관리하면 편할 것

- 질문 14) 도메인별로 쿼리를 관리한다고 하셨는데, 화면에서도 공통적으로 사용하는 쿼리들이 있을 것 같아요. 이러한 쿼리들은 도메인을 어떻게 분리해서 관리하시나요 ?
  - 도메인을 묶는 단위는 배너, 오더, 카트 등으로 묶는다
  - 공통적으로 사용하는 쿼리들은 도메인 코어에 넣는다던가 할 것 같다
  - 개인적으로 api랑 상관있는 것은 백엔드 서버의 도메인에 따라가도 괜찮을 것 같다
  - 개인적으로는 공통적인 것들도 배너, 카트 내에서도 도메인을 각각 따로 나누어서 사용하고 있다
  - mutation도 똑같은 queries 파일 내에서 관리하고 있다

- 질문 15) 리덕스와 리액트 쿼리를 같이 사용할 때 구조 설계를 어떻게 하는지 궁금해요
  - 아예 별개로 생각하면 된다
  - 리덕스는 클라이언트 스테이트 사용
  - 파일은 따로 관리하되 컴포넌트 안에서 필요한 값들이 합쳐진다

- 질문 16) 리액트 쿼리 도입하신 뒤 스토어에 남은 순수 클라이언트 스테이트는 어떤게 남았나요 ?
  - ui 관련해서 팝업이 열리거나 닫히거나 포스트가 보이거나
  - 앱에서 인증정보, 회원정보 같은 것들
  - 일반적으로 생각할 수 있는 진짜 순수 공통 스테이트들이 남아 있다

- 질문 17) 쿼리 무효화시 refetch와 invalidate 중 어느 것을 사용하는게 좋을까요
  - 질문에 답이 있다
  - 무효화는 invalidate가 맞다
  - refetch는 무효화라고 단정짓기 보다는 그냥 새롭게 데이터를 불러오는 것
  
- 질문 18) 상태 관리 도구를 한번에 바꾸는게 부담이 될 수 있을 것 같은데, 최종적인 배포는 한꺼번에 진행하신건가요? 순차적으로 적용해 나간다던지 하는 방법은 없었나 궁금합니다
  - 부담이긴 했다
  - 아키텍쳐 통합이랑 엮어서 진행
  - 프로덕트 단위로 끊어서 배포
  - 상황에 따라서 다르다
  - 프로덕트가 작으면 세부 프로덕트로 나눠서 하는게 좋을 것 같다

- 질문 19) graphQL에서는 같은 쿼리라도 페이지의 요구에 따라 요청 값이 달라지는데, 이걸 useQuery에서 같은 키로 관리한다면 페이지당 다른 처리는 어떻게 적용할 지 궁금합니다.
  - key를 배열로 관리할수 있다
  - 주문이 infinite scrolling이기 때문에 페이지 단위로 받고 있는데 이 key를 page 숫자와 관리 하고 있다.

- 질문 20) 조회 조건에 따라 검색 버튼을 눌렀을 때 API를 호출해야 되는 상황인데 조회 조건이 바뀌지 않은 상태에서 검색버튼을 누르면 api 호출이 되지 않았어요.
  이 문제를 해결하기 위해서 refetch를 사용해서 재호출을 했는데 조회조건은 바뀌지 않은 상태로 refetch를 하더라구요
  그래서 invalidateQueries를 이용하였고 조회 조건 parameter 값이 바뀌면서 refetch가 되더라구요, 그런데 이렇게 해결한게 찝찝해서요.
  refetch를 사용하면서 useQuery에 파라미터 값이 바뀌면서 사용할 수는 없을까요 ? 
  - 컴포넌트의 렌더링 트리거가 일어나지 않았을 것 같다 
  - 내부적인 검색 조건 같은 것은 스테이트로 관리하면 좋다 
  - ref나 변수로 사용하는 것 있다면 그런 조건들을 useState 로 관리하면 잘 변경될 것 같다

- 질문 21) useQuery를 사용할 때 parameter 값이 변하여도 refetch가 일어나지 않게 할 수 있나요 ?
  - enabled를 false 값을 주면 된다

- 질문 22) queryClient Provider를 적절하게 나누어서 사용하는지 궁금합니다
  - 한번에 감싸서 사용한다
  - 굳이 나눠서 사용할 필요성 못느낀다
  - 그런 practice 본 적 없다

- 질문 23) api 부분은 리액트 쿼리로 관리하고 ,클라이언트와 관련된 부분 (레이어 뷰 온오프라던지)은 클아이언트 스토어를 쓰는 것이라고 이해했는데요, 혹시 리액트 쿼리로 전부 커버하지 않는 이유가 있나요 ? 기존의 클라이언트 상태관리 라이브러리가 가능하다면 굳이 라이브러리 두 개 쓰는 이유가 궁금합니다.
  - 클라이언트 상태가 관리가능하긴 한대 내부적인 상태 조작하기에는 MobX가 더 파워풀 하기 때문 즉, 생산성과 개발의 편함 때문에 사용한다

- 질문 24) ui test등을 위해서 뷰 컴포넌트와 로직을 분리해야되는 경우가 있을 텐데 어떤 방법을 사용하시나요 ? 컨테이터 컴포넌트를 만드시는 편인지 .. 궁금합니다
  - 컨테이너 컴포넌트라고 명시적인 것을 만들고 있지는 않다
  - 뷰 컴포넌트는 분리하는 편, 완전히 뷰만 담당하는 역할 하도록
  - ui 테스트에 편리하기도 하고, 로직만 수정해야 될 때 최대한 작업의 영향을 줄일 수 있기 때문
  - 보통 컨테이너 컴포넌트를 많이 사용한다

- 질문 25) Client 상태 관리로 MobX를 선택한 이유가 궁금합니다
  - 리덕스 보다 보일러플레이트도 작고 간결하게 관리하고 싶어서
  
- 질문 26) query hook에 대한 유닛 테스트도 진행되고 있나요 ?
  - 고려중인데 완벽하게 커버되고 있지 않다
  - 노력은 하고 있다

## Reference

- [LIVE React Query와 상태관리 :: 2월 우아한테크세미나](https://www.youtube.com/watch?v=MArE6Hy371c)
- [My구독의 React Query 전환기](https://tech.kakao.com/2022/06/13/react-query/)
- [React-Query 도입을 위한 고민 (feat. Recoil)](https://tech.osci.kr/2022/07/13/react-query/)
- [React Query 기본](https://thinkforthink.tistory.com/340?category=864727)
- [stale-while-revalidate](https://jooonho.com/web/2021-05-16-stale-while-revalidate/)
- [S.W.R = Stale-While-Revalidate](https://iborymagic.tistory.com/135)
- [stale-while-revalidate Cache-Control extension으로 UX 향상해보기](https://okky.kr/article/978951)
- [React Query의 useQueries에 대해 알아보기](https://jforj.tistory.com/245)
