# masterJS

# Styled-Component

→ 자동으로 className 생성해줌

### 기본 사용법

1. 설치 : npm i + import
2. 정의 : const Father = styled.div`css 코드`
3. 사용 : <Father/>

### 심화 사용법

- css 옵션 (prop 주기)
    
    → color : {props ⇒ props.prop이름}
    
- css 추가
    
    → styled(Component)`추가할 css 코드`
    
- 태그 종류 변경
    
    → <Component as=”변경할 태그”/>
    
- attribute 주기
    
    → styled.div.attrs({color:”red”,border-radius:”5px”})
    
- styled component 내 태그 / styled component 선택
    
    →`` 안에    태그이름{} or ${Component} {}
    
- :hover 주는법 - pseudo selector
    
    → &:hover {}
    
- 애니메이션 주기
    1. import {keyframes} from “styled-components”;
    2. const anime1 = keyframes`애니메이션 코드`
        - 애니메이션 코드 : from{} to{} , 0%{} 50%{} 100%{}
- Theme
    
    → index.js에 
    
    1. import {ThemeProvider} from “styled-components”;
    2. 다음과 같이 코드 작성
        
        ```jsx
        // index.tsx
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(
          <React.StrictMode>
            <ThemeProvider theme={객체이름}>
              <App />
            </ThemeProvider>
            
          </React.StrictMode>
        );
        ```
        
        ```tsx
        // theme.ts
        import { DefaultTheme } from "styled-components";
        
        export const darkTheme : DefaultTheme = {
            bgColor: "black",
            textColor: "white",
            btnColor: "tomato",
        }
        
        export const lightTheme : DefaultTheme = {
            bgColor: "white",
            textColor: "black",
            btnColor: "tomato",
        }
        ```
        
    3. styled Component 등에 활용가능
        
        → 다른 디렉토리 파일의 ``안에 {props => props.theme.textColor} 
        
        → 전역 props 같은 느낌.
        
        *다크모드 구현의 50%구현한것. theme을 바꾸는건 추후에
        

# TypeScript + React

→ 쓰는이유

1. 양쪽에서 불평
2. 자동완성 기능 

## 기본설정

### 설치

1. 처음부터 타입스크립트로 프로젝트 만드는경우
    1. npx create-react-app my-app --template typescript
    2. npm install --save typescript @types/node @types/react @types/react-dom @types/jest @types/styled-components styled-components
        
        *styled-components TS 버전 아닌것도 설치 필요
        
2. 중간단계부터 타입스크립트 도입하는 경우
    1. npm install --save typescript @types/node @types/react @types/react-dom @types/jest @types/styled-components
    2. 확장자 tsx로 변경
    3. npm 들 TS 지원 안하는것들 @types = DefinitelyTyped 에서 따로 다시 설치
        
        → @types 붙여서 그냥 설치해 보자!!
        

### 초기세팅+

- npm 추가 install
    
    → npm i react-router-dom react-query
    
    → react-query : fetch같은것과 관련
    
    *기본적으로 ts를 내장한 라이브러리 인가봄.
    
    → —save-dev 옵션 붙여서 react-query / react-router-dom / styled-components 도 깔자!
    
- theme
    1. styled.d.ts : 기본틀
        
        ```tsx
        import 'styled-components';
        
        declare module 'styled-components' {
          export interface DefaultTheme {
            bgColor: string;
            textColor: string;
            btnColor: string;
          }
        }
        ```
        
    2.  theme.ts : 테마 저장소
        
        ```tsx
        import { DefaultTheme } from "styled-components";
        
        export const theme : DefaultTheme = {
            bgColor: "white",
            textColor: "black",
            btnColor: "tomato",
        }
        
        export const theme2 : DefaultTheme = {
            bgColor: "white",
            textColor: "black",
            btnColor: "tomato",
        }
        ```
        
- Router
    
    → routes/Router.tsx , page1.tsx , page2.tsx … 파일 생성
    
    - useParams 사용시
        
        ```tsx
        interface RouteParams {
        	id:number,
        	coinId:string,
        }
        const {id,coinId} = useParams<RouteParams>
        ```
        
- css
    - 전역 css 적용법
        1. import { createGlobalStyle } from "styled-components";
        2. const GlobalStyle = createGlobalStyle`전역 css 코드`
            
            *reset.css 사용하자
            
        3. App 컴포넌트 최상단에 <GlobalStyle>
        
        ++ flatuicolors : 컬러 쉽게 볼수 있음
        
        ++ 글꼴설정 : css에 import 할수 있던것 기억나제?
        
        1. @import url
        2. 글꼴 적용 ㄱ
- api.ts
    
    → fetcher 함수도 따로 정리해서, react-query에 활용한다
    
    - 사용법
        
        → BASE_URL적용해주는것 잊지 않는다.
        
    
    ```tsx
    const BASE_URL = `https://api.coinpaprika.com/v1`;
    
    export function fetchCoinInfo(coinId : string){
    	return fetch(`${BASE_URL}/coins/${coinId}`).then(res=>res.json())
    }
    ```
    

## TS typing for React

### 기본 typing 방법

```tsx
interface I {
	category : "A" | "B" | "C";
	function : () => void; // 아무것도 리턴하지 않는 함수를 의미함
}
```

### Component prop typing (Component에 TS 적용하기)

- 사용법
    1. interface 설명객체 {bgColor:string, borderColor?:string, }
        
        *? : optional props
        
    2. Component / Styled Component에 적용
        - Component : shortcut 뒤에 : 설명객체
        - Styled : styled.div<설명객체>
    
    **** children prop**
    
    **→ Coin.tsx를 import 해서 <Coin>으로 사용 할때, prop을 주려면 그 Coin의 children을 typing 해줘야한다.** 
    
    *설명 객체 자체가 불평을 부리진 않는다 
    
    **→ 불평은 1. Component shortcut 2. <Component> 둘중에 하나에서만 한다.**
    
- 위계가 중첩될때
    
    ```tsx
    //Circle.tsx
    
    import styled from "styled-components";
    
    interface ContainerProps {
        bgColor:string,
        borderColor:string,
    }
    
    const Container = styled.div<ContainerProps>`
        width:100px; height:100px;
        background-color: ${props => props.bgColor};
        border-color: ${props => props.borderColor};
    
    `
    
    interface CircleProps {
        bgColor:string,
        borderColor?:string,
    }
    
    function Circle({bgColor,borderColor = "기본컬러"}:CircleProps){
        return (
            <div>
                <Container bgColor={bgColor} borderColor = {borderColor ?? "기본컬러"}></Container>
            </div>
        );
    }
    
    export default Circle;
    ```
    
    → 상위 컴포넌트에선 필수적이지 않고 , 하위 컴포넌트에선 필수적인경우 : 기본값을 설정 해줄수 있다
    
    1. ?? 문법을 활용
    2. JS 문법 활용
    
    ### State typing , Form Event Typing (State, Form Event에 TS 적용하기)
    
    - State Typing
        
        → useState(기본값)의 기본값에 따라 자동으로 typing 된다. 
        
        *추가 지정 : useState<string | number>(기본값)
        
    - Form Event Typing
        
        → State Typing ↔ event.currentTarget.value Typing
        
        → event : React.FormEvent<HTMLInputElement>
        
        *구글링으로 찾는것이다.
        
        → React ANSTJDML SyntheticEvent를 찾아보면 된다!
        
        *React.js 말고 다른것을 쓸때는 또 다를것이다
        
        ```tsx
        function App() {
          const [value,setValue] = useState<number | string>(1);
          const onChange = (event : React.FormEvent<HTMLInputElement>) => {
            const { currentTarget : {value} } = event;
            setValue(parseFloat(value));
            // setValue(value)
          }
          const onSubmit = (event : React.FormEvent<HTMLFormElement>) => {
            event.preventDefault();
            console.log(value)
          }
          return (
            <div className="App">
              <form onSubmit={onSubmit}>
                  <input type="number" value={value} onChange = {onChange}/>
                  <button>submit</button>
              </form>
            </div>
          );
        }
        ```
        
        *주의 : HTMLInputElement는 무조건 string 취급이더라.
        
        → parseFloat(value) 로 해결
        
        - 다지울시 NaN 되는 경우는 해결 못함
        
        ### API Typing
        
        - 사용법
            
            ```tsx
            interface Idata {
            id : number,
            name : string,
            
            } 
            const [data,setData] = useState<Idata[]>([]);
            ```
            
            * API data  interface 생성 사이트
            
            [Instantly parse JSON in any language | quicktype](https://app.quicktype.io/?l=ts)
            
            * 수동으로 하는방법
            
            ```
            1. 전역 변수로 만들기 (우클릭)
            2. Object.keys(temp1).join() vscode로 가지고 와서 조작해서 key 쭉 만들고
            3. Object.values(temp1) map으로 typeof 배출해서 복붙
            
            	* 이렇게하면 문제점 : array는 object 로 표시되는데, 이것을 또 따로 설명 	해줘야함
            		-> interface 내 interface 설명
            ```
            
    
    ### useEffect() 의 dependency []
    
    → useEffect안에 state가 들어있을때, dependency에 안넣어주면 불평한다. (coinId 등등)
    

# React-Router-Dom

## Link로 데이터 넘겨주기

1. useLocation (State로 넘겨주기)
    
    → 리액트에서 페이지 넘어갈때마다, State가 초기화 되는데, 이는 비 효율적일때가 있다. 이때 넘겨주는 방법!
    
    - 사용법
        
        ```tsx
        // 보내는 쪽
        <Link to="/path" state={dataObj}/>
        
        // 받는 쪽
        const [loading,setLoading] = useState(true);
        const {state} = useLocation(); // dataObj 
        ...
        <Title>{state?.name ? state.name : loading ? <p> loading </p> : 로딩된 데이터.name }</Title>
        ```
        
        *dataObj는 …로 분해했다가 새로 넣어줘야 하더라.
        
        *state?.name : state가 undefined 아닐 경우에만 읽어들인다.
        
        *3항 연산자 이렇게 사용 가능!!
        
2. useParams

## Nested Routing

→ Router 내 Router. 탭 기능 구현할때 유용

- 사용법
    1. Router.tsx 에 Route>Route
        
        ```tsx
        //Router.tsx
        import Chart from "../components/Detail_Chart";
        import Price from "../components/Detail_Price";
        ...
        <Route path="/detail/:coinId" element={<Detail></Detail>}>
          <Route path={`chart`} element={<Chart></Chart>}></Route>
          <Route path={`price`} element={<Price></Price>}></Route> //이부분이 Outlet에 해당하는 부분
        </Route> 
        ...
        ```
        
    2. Detail.tsx 에
        
        ```tsx
        import {Routes,Route,**useMatch,Outlet,Link**} from "react-router-dom";
        ...
        const Tabs = styled.div``;
        const Tab = styled.span<{isActive : boolean}>`
        	color: ${(props) =>
        	    props.isActive ? props.theme.accentColor : props.theme.textColor};
        `;
        ...
        function Detail(){
        	...
        		const priceMatch = useMatch(`/:coinId/price`)
            const chartMatch = useMatch(`/:coinId/chart`)
        	...
        
        	return(
        			...
        				<Tabs>
                  <Tab isActive={chartMatch !== null}>
        						<Link to={`/detail/${coinId}/chart`}>Chart</Link>
        					</Tab>
                  <Tab isActive={priceMatch !== null}>
        						<Link to={`/detail/${coinId}/price`}>Price</Link>
        					</Tab>
                </Tabs>
        				<Outlet/>
        			...
        	)
        }
        ```
        
        ** useMatch는 객체 | null 을 리턴
        
        ** useMatch 의 경로에는, 상대경로로 써주면 됨.
        
        ** isActive는 tab(styled component)의 색상 지정에 사용해 준다.
        
        ** Link , Outlet도 잊지 않는다.
        

# React-query

→ useEffect + 데이터/로딩 State 관련 코드 확 줄일 수 있게 해줌

- 사용법
    1. npm i react-query + import { QueryClient, QueryClientProvider }
    2. index.tsx에
        
        ```tsx
        import {QueryClient, QueryClientProvider} from "react-query";
        ...
        
        return(
        	<QueryClientProvider client={queryClient}>
        	...
        	</QueryClientProvider>
        )
        ```
        

1. api.ts
    
    ```tsx
    const BASE_URL = `https://api.coinpaprika.com/v1`;
    
    export function fetchCoins(){
    	return fetch(`${BASE_URL}/coins`).then(res=>res.json())
    }
    
    export function fetchCoinInfo(coinId : string | undefined){
    	return fetch(`${BASE_URL}/coins/${coinId}`).then(res=>res.json())
    }
    ```
    
    ** coinId를 저렇게 해줘야 오류가 안뜸!
    
    ** BASE_URL 주의
    
2. Detail.tsx
    
    ```tsx
    import { fetchCoins,fetchCoinInfo } from "../api";
    import { useQuery } from "react-query";
    ...
    const {isLoading : coinLoading , data : coinData} = useQuery<IcoinData>([coinId,"1"],()=>fetchCoinInfo(coinId),{refetchInterval : 10000});
    const {isLoading : coinLoading2 , data : coinData2} = useQuery<IcoinData2>([coinId,"2"],()=>fetchCoinInfo(coinId));
    ...
    let loading = coinLoading || coinLoading2;
    <h1>{state?.name ? state.name : loading ? <p>Loading....</p> : coinData?.name }</h1>
    
    ```
    
    ** [” ”] : 고유 식별자. 여러줄 써야할떄는 어레이 형태로
    
    ** fetcher 함수에 인자 줘야할때는 콜백 형식으로 준다
    
    ** h1태그 안의 내용 : 그 루트로 바로 접속했을때, 어떻게 대처할 것인지를 보여줌
    
    ** let loading = coinLoading || coinLoading2; 
    
    → JS의 단축평가 : 둘중 true인것 반환.
    
    [[자바스크립트] 논리연산자(&&, ||) 단축평가](https://curryyou.tistory.com/193)
    
    ** 세번째 인자로 옵션 줄수 있다.
    
    ** 실시간으로 변하는 데이터 보여줄때 용이함.
    
    - 최대 장점 : 캐시에 데이터가 저장되기 때문에, useEffect와 달리, 매번 로딩이 되지 않는다.
    
    - devTools
        
        → 캐시에 있는 데이터를 볼수있게 해주는 툴.
        
        - 사용법
            
            ```tsx
            import {ReactQueryDevtools} from "react-query/devtools";
            ...
            <ReactQueryDevtools initialIsOpen={true}></ReactQueryDevtools>
            ```
            
    

# Apex-Charts

→ 데이터 시각화를 위한 라이브러리

- 사용법
    1. npm i , import
        
        [ApexCharts.js - Open Source JavaScript Charts for your website](https://apexcharts.com/)
        
    2. ApexChart 컴포넌트를 , 넣고 prop들로 세팅을 해주는 방식
        
        ```jsx
        <ApexChart
              type="line"
              series={[{
                  name : "close price",
                  data : data ? data.map(each => each.close) : []
                }]}
              options={{
                  theme: {
                    mode: "dark",
                  },
                  chart: {
                    height: 300,
                    width: 500,
                    toolbar: {
                      show: false,
                    },
                    background: "transparent",
                  },
                  grid: { show: false },
                  stroke: {
                    curve: "smooth",
                    width: 4,
                  },
                  yaxis: {
                    show: false,
                  },
                  xaxis: {
                      categories : data ? data.map(each => data.indexOf(each)) : []
                  },
              }}
                
          ></ApexChart>
        ```
        
    

# React-Helmet

→ favicon, 페이지 head 를 설정할수 있음

- 사용법
    1. npm i , import
    2. <Helmet><title>타이틀 관련 내용</title></Helmet>

# Recoil

→ global state 관리를 위함 , atom === global state 라 생각.

- 사용법
    1. npm , import
    2. index.tsx 에 <RecoilRoot>
    3. atoms.ts 
        
        ```tsx
        import {atom} from "recoil"
        
        export const isDarkAtom = atom({
            key: "isDark", //고유한 값
            default : false, // useState같은 역할
        })
        ```
        
        - atom에 typing을 해줘야하는 경우
            
            → atom이 API calling등을 해서, default가 [{}] 형태일때 , 그냥 [] 이라고만 써주면 never[] 가 되어서, 빈 객체만 넣을수 있게 되므로, 따로 interface 정의를 해준다.
            
            ```tsx
            //예시
            interface IisDark {
            
            	text:string;
            	
            	id:number;
            	
            	category : “A” | “B” | “C” ;
            	
            }
            ```
            
    4. global state 쓰고 싶은 곳에 
        
        ```tsx
        import {isDarkAtom} from "./atoms.ts"
        
        // 세가지 중에 하나 선택해서 사용하자
        const isDark = useRecoilValue(idsDarkAtom);
        const setIsDark = useSetRecoilState(isDarkAtom)
        const [isDark, setIsDark] = useRecoilState(isDarkAtom);
        
        ```
        
- recoil selector
    
    ⇒
    
    1. import {selector} from “recoil”;
    2. atom.ts에 코드 셋팅
        
        ```tsx
        export const toDoSelectorAtom = selector({
            key : "toDoSelector",
            get : ({get}) => { //get : 다른 atom을 가지고 와서 사용가능
                const categoryState = get(categoryStateAtom);
                const toDoList = get(toDoListAtom);
                return toDoList.filter(toDo => toDo.category === categoryState);
            },
        		set : ({set},newValue) => { // newValue : setValue를 통해 들어온 값.
                set(valueAtom,newValue);
            } //set : selector 하나로 여러개의 atom을 한꺼번에 수정할수 있다.
        })
        ```
        

# React-Hook-Form

→ React 에서 Form 부분을 쉽게 핸들링 할수 있게 해줌 (기본 셋팅, 유효성 검사 , 에러났을때 등등)

- 사용법
    1. npm i react-hook-form , import {useForm}
    2. useForm() 해서 도구들 꺼내옴
        
        ```tsx
        interface IForm {
        	email : string;
        	...
        	extraError? : string;
        }
        
        function RegisterPage(){
        	const {register,watch, handleSubmit, formState : {errors}, setValue} = useForm<IForm>({defaultValues:{email:"@naver.com"},});
        	console.log(watch()) // state들을 보여준다.
        	console.log(errors()) // 에러를 객체 형태로 보여준다
        	const onValid = function(data:IForm){
        		console.log(data) //watch() 와 비슷하게, 매번 form 입력될때마다 데이터를 보여준다.
        		if (data.password !== data.password1){
        			setError("password1",{message : "Password are not the same!!"},{shouldFocus:true}) 
        		} //조건문으로 커스텀 에러 주기 , 세번째 인자는 마우스 이동여부
        
        		if (서버가 꺼져있으면){
        			setError('Internal server error')
        		}
        		setValue("text",""); //form의 내용을 바꿔줌
        		
        	}
        	return (
        		<form onSubmit = {handleSubmit(onValid,onInValid)}> // onValid만 필수인자
                <input {...register("email",{required:true , minLength:10})} placeholder="Email" />
                <input {...register("firstName")} placeholder="First Name" />
                <input {...register("lastName")} placeholder="Last Name" />
                <input {...register("username")} placeholder="Username" />
                <input {...register("password")} placeholder="Password" />
                <input {...register("password1")} placeholder="Password1" />
                <button>Add</button>
        				<span>{errors?.pw1?.message}</span> //에러메세지 표시
            </form>
        	)
        }
        ```
        
        **** useForm이랑 , onValid에 typing 둘다 안해주면 에러나더라.**
        
        - validation 처리
            
            → html태그 대신, javscript로 invalid 처리 해주는것 (html 태그로 하면, 사용자가 임의 조작이 가능해짐.)
            
            → 처음 제출 된 이후부터, validation체크를 매 입력시마다 해줌.
            
            → validation이 안되면 에러 객체 (formState.errors) 가 배출됨. 
            
            - required: true 또는 “error message” : 둘다 필수라는 의미
            - minLength : minLength 또는 {value:5 , message:”~~”}
            - pattern : RegExp
            - validate : value ⇒ return true / false / string (error message)
                - {func1, func2 } 여러개의 검사를 진행할수 있다,
        

# React-Beautiful-dnd

⇒

1. npm i react-beautiful-dnd
2. *import* { DragDropContext, Droppable, Draggable } *from* "react-beautiful-dnd";
- 기본 틀
    
    ![스크린샷 2022-09-30 오전 11.59.55.png](masterJS%20159001ece79f476d94e72a0365641948/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-30_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.59.55.png)
    
    ```tsx
    <DragDropContext onDragEnd={onDragEnd}> //
      <div>
        <Droppable droppableId="one">
          {(provided)=> // 함수 형태로 children을 주는것이 원칙
            <ul ref = {provided.innerRef} {...provided.droppableProps}> //ref : HTML 선택 가능하게 해줌 droppableProps : 기본 들어가야할 prop (암기)
              <Draggable draggableId="first" index={0}>
                {(provided) =>
                 <li ref={provided.innerRef} {...provided.draggableProps}>
                    <span {...provided.dragHandleProps}>🔥</span> // dragHandleProps : 그 항목이 손잡이가 됨
                    One
                </li>
                }
              </Draggable>
              <Draggable draggableId="second" index={1}>
                {(provided) => 
                  <li ref={provided.innerRef} {...provided.draggableProps}>
                    <span {...provided.dragHandleProps}>🔥</span>
                    Two
                  </li>
                }
              </Draggable>
            </ul>
          }
        </Droppable>
      </div>
    </DragDropContext>
    ```
    
    ** 잘 안되면 React.StrictMode 제거 해보기
    
    - styled-component 적용한 형태
        
        ```tsx
        <DragDropContext onDragEnd={onDragEnd}>
            <Wrapper>
        	    <Boards>
        	        <Droppable droppableId="one">
        		        {(provided) => (
        		            <Board ref={provided.innerRef} {...provided.droppableProps}>
        		            {toDos.map((toDo, index) => (
        		                <Draggable key = {toDo.id} toDoId = {toDo.id} text={toDo.text} index = {index}> //key와 draggableId는 항상 같게
        		                {(provided) => (
        		                    <Card
        		                    ref={provided.innerRef}
        		                    {...provided.dragHandleProps}
        		                    {...provided.draggableProps}
        		                    >
        		                    {toDo}
        		                    </Card>
        		                )}
        		                </Draggable>
        		            ))}
        		            {provided.placeholder} // 크기 안줄어들게 해줌
        		            </Board>
        		        )}
        	        </Droppable>
        	    </Boards>
            </Wrapper>
        </DragDropContext>
        ```
        
    - onDragEnd 짜는법
        
        ```tsx
        const onDragEnd = ({ draggableId, destination, source }: DropResult) => { //매번의 드롭마다 나오는 객체
            if (!destination) return; //어느 Board에도 도착지가 속하지 않으면 null이라고 나오기 때문에 안전장치.
            setToDos(prev => {
              const oldToDos = [...prev];
              const oldToDo = oldToDos.splice(source.index,1)[0];
              oldToDos.splice(destination.index,0,oldToDo);
              const newToDos = oldToDos;
              return newToDos
            })
          };
        ```
        
- 리렌더링 문제 해결
    
    → 드래그한 상태로 이리저리 끌고 다니는게 부모의 State를 변경시키는 행위 === 계속 리렌더링 됨
    
    ⇒ DraggableCard의 Prop 변경되지 않는 한,  DraggableCard 리렌더링 되지 말라고 해주는 React.memo(DraggableCard)
    
- snapshot : droppable , draggable의 활동 현황을 보여줌.
    - droppable
        
        ```
        isDraggingOver: boolean
        현재 선택한 Draggable이 특정 Droppable위에 드래깅 되고 있는지 여부 확인
        
        draggingOverWith: ?DraggableId
        Droppable 위로 드래그하는 Draggable ID
        
        draggingFromThisWith: ?DraggableId
        현재 Droppable에서 벗어난 드래깅되고 있는 Draggable ID
        ```
        
        1. provided 옆에 snapshot prop 주고
        2. Board isDraggingOver = {snapshot.isDraggingOver}
        3. draggoingFromThis = {Boolean(snapshot.draggoingFromThisWith)}
        4. Board 에 prop주기, ${}로 조절
    - draggable도 같은 방식으로 하면된다.
- ref
    - ref
        
        → document.getElementBy~~ 랑 같은역할 === HTML element 요소를 가져올수 있게 해줌.
        
        ```tsx
        const inputRef = useRef<HTMLInputElement>(null);
        ...
        const onClick = ()=>{
        	inputRef.current?.focus() // current까지가 html요소를 의미하는듯. 이부분까진 react. focus는 js
        }
        ```
        

# Immer

→ immutable 지키는거 안귀찮게 해줌.

```tsx
const LandingPage = () => {
  const [todo, setTodo] = useRecoilState(atomTodo);
  // setTodo를 produce함수를 이용해서 불변성을 관리한다.
  const handleChangeTodo = (idx: number) => {
    setTodo((prev) =>
      produce(prev, (draft) => {
        draft[idx] = { name: 'hacked', finished: false };
        return draft;
      }),
    );
  };
```

# 기타

- onClick에 event 이외의 par주는법
    
    ```
    onClick = {**event=>onClickBtn(다른parameter,event)**}
    ```
    
- 모드에 따라 다른 화면을 보여주기 = Nested Routes / State 사용