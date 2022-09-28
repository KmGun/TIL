# ES6

# this

→ this 는 쓰이는 맥락에 따라 달라지고, 4가지가 있다. (on DOM BOM)

1. 일반적 this (나를 포함하는 객체)
    - 전역 스코프에서 this
    - 객체 내의 function 안에있는 this
        
        ++ 함수 내의 this : 전역객체를 의미
        
        ```jsx
        function 함수(){
        	console.log(this) // 전역객체
        }
        ```
        
        ** 1번 경우가 예외적인것. (객체 안에 있어서)
        
        ** ‘use strict’ 옵션 사용하는 경우 undefined
        
        ** arrow function의 경우, this를 재정의 하지 않고, 바로위의 this를 물려받는다.
        
    
    ```jsx
    // 전역에서
    console.log(this) // window
    
    // 객체 내에서
    const obj = {
        func:function(){
          console.log(this)
        },
        func2:() => {
    			console.log(this)
        },
        var1:this,
    }
    obj.func() // obj
    obj.func2() // window
    obj.var1 // window
    ```
    
2. class 내에서의 this
    
    → 미래에 생겨날 객체
    
    ```jsx
    function 기계(){
    	this.이름 = 'Kim',
    	this.나이 = 24,
    }
    
    const 기계생성물 = new 기계();
    
    //this는 기계생성물을 가리킴
    ```
    

1. 이벤트 리스너 콜백 내의 this
    
    → event.currentTarget과 같다
    
    ```jsx
    document.getElementById('버튼').addEventListner('click',function(event){
    	console.log(this)
    	console.log(event.currentTarget)
    })
    ```
    
2. 콜백 내의 this
    
    → 전역객체를 가리킴
    
3. node.js 에서의 this
    - 전역 스코프에서 this
        
        ```jsx
        console.log(this, module.exports, exports); // {}, {}, {} 
        console.log(this === module.exports); // true 
        console.log(this === exports); // true 
        console.log(module.exports === exports); // true 
        console.log(this === global); // false
        ```
        
        ++ node.js 에서 파일 하나 = module 객체 하나.
        
        ++ node.js에서 파일에 코드를 작성하면, module.exports안에 함수 형태로 담긴다. 그렇기 때문에, 전역 스코프에서 this가 module.exports를 의미하는것.
        
    - global을 출력하는법?
        
        ```jsx
        function 함수(){
        	console.log(this) // global
        }
        ```
        

# 변수

- let const var
    
    → let , const, var 차이는 1. 재선언과 재할당 2. scope 두가지만 고려하면된다.
    
    - let은 재선언 안됨
    - scope
        - var은 function 스코프, let const는 { } 스코프
            
            ++ for (let i=0; i<~~){} 하는것도 { }안에 i 선언하는거랑 같은 말임
            
- hoisting
    
    → 함수의 선언부가 가장 위로 올라가고, 할당부와 분리되는 현상
    
    ```jsx
    const hello = "hi";
    
    // 위아래 코드가 같다.
    
    const hello;
    ...
    hello = "hi";
    ```
    
- 여러개 변수 선언하기
    
    ```jsx
    const var1=1,var2=2
    
    // 위아래 같은 코드
    
    const var1,var2;
    var1=1; var2=2;
    ```
    
- 전역 변수 선언하기
    1. 전역 scope에 선언하기
    2. **window.변수 로 선언하기**
        
        ** 명시적으로 하기위해, 2번방법이 좋음
        

# Tagged Literal

→ 문자 조작에 유용

- 사용법
    
    ```tsx
    function func(문자들,변수1,변수2){
    	console.log(문자들,변수1,변수2)
    }
    
    func`문자1 ${변수1} 문자2 ${변수2}`// ["문자1","문자2"] 변수1 변수2
    ```
    

# spread operator

→ … 을 spread operator 라고 함. 

- 용도
    1. Deep Copy
        
        → reference data type을 새롭게 만들때
        
    2. .apply() 대신
        
        → 함수에 par들을 빠르게 적용할때
        
        - 사용법
            
            ```tsx
            function 더하기(a,b,c){
            	return a+b+c;
            }
            
            const 어레이 = [1,2,3]
            
            //아래 두 코드는 똑같음
            더하기(...어레이)
            더하기.apply(undefined,어레이)
            더하기.call(undefined,...어레이)
            ```
            
            - func.apply(obj,어레이)
                
                → 다른 함수에 적용해서 이 함수를 실행해주세요
                
                ![Untitled](ES6%204ab918b4db054fe6a938e593e1d507dc/Untitled.png)
                
                ** 위 예시에서 undefined 인 이유 : 그냥 아무데도 적용하지 마시고 실행해주세요
                
    3. 문자에도 적용가능

** 주의

- ()[]{} 안에만 올수 있음.
- object 안에 쓸때 중복시 뒤에 오는것이 이김
    
    ```tsx
    const obj = {a:1,b:2}
    const newObj = {...obj,a:4} // a:4, b:2
    ```
    

# function +

- JS는 함수에 par 빠져도 에러 안남.
- arguments 키워드
    
    ```tsx
    function func(a,b,c) {
    	console.log(arguments) // [a,b,c] : par 들을 배열 형태로 배출해줌
    }
    ```
    
- Rest parameter (유연한 파라미터)
    
    ```tsx
    function func(a,b,...파라미터들){
    	console.log(파라미터들) 
    }
    
    func(1,2,3,4,5) // [3,4,5]
    ```
    

# data type

→ primitive data type ↔ reference data type 신규 할당에서의 차이 : 변수에 화살표를 저장하냐?

** 주의 1

![Untitled](ES6%204ab918b4db054fe6a938e593e1d507dc/Untitled%201.png)

** 주의 2

![Untitled](ES6%204ab918b4db054fe6a938e593e1d507dc/Untitled%202.png)

→ 함수의 par 자체가 var obj = {name : ‘김’} 이런의미이므로 , 화살표가 새롭게 obj에 저장된다. 위 변경() 은 obj에 또다른 화살표를 넣어주는 과정이므로, 이름1에는 변화가 없음

# constructor

→ object 만드는 기계

- 사용법
    
    ```tsx
    function Machine(par1,par2){
    	this.name = par1;
    	this.age = par2;
    }
    
    const 생성물1 = new Machine("김건",24)
    ```
    
    **** constructor 첫글자는 대문자인것이 관습이다.**
    

# prototype

→ prototype : 자신의 유전자

- 개념 정리
    - prototype은 함수에만 생성된다.
        
        ** 주의 : .prototype 은 진짜 유전자만 딱 출력. .proto나, 그냥 그 자체를 출력하면 contrcutor , [[Prototype]] 까지 모두 출력
        
    - prototype chain
        
        → prop 검색 : 자신의 인스턴스 - 부모의 prototype - 부모의 부모의 prototype 순으로 찾는다.
        
    - __proto__ : 바로 윗 부모의 prototype 보기
        
        ** obj도 뜸
        
        ![스크린샷 2022-09-24 오후 11.02.20.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-24_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_11.02.20.png)
        
        - 부모 강제 등록 가능
            
            → 아래와 같이 등록 해버리면, 기존의 proto는 사용할수가 없음.
            
            ** 주의 : 객체끼리 연결시엔 contructor 미포함
            
            ![Untitled](ES6%204ab918b4db054fe6a938e593e1d507dc/Untitled%203.png)
            
            - 고민했던 부분 (해결완)
                1. __proto__ : 바로 윗 부모만 보여주는것.
                2. [[Prototype]] === __proto__
                3. 강제등록 : 강제등록 한놈의 윗놈까지도 같이 등록되는것은 확실.
                4. 근데 왜 기계.유전자3 undefined??????
                    
                    → 깊이 알 필요는 없을듯. 일단 보류하고 넘어가자.
                    
                    ** 일반적으로 상속은, 객체간에 시킴.
                    
            
            ```
            function 기계(){
                this.name = "기계1"
            }
            function 기계2(){
                this.name = "기계2"
            }
            function 기계3(){
                this.name = "기계3"
            }
            
            기계2.prototype.유전자2 = "기계2유전자";
            기계3.prototype.유전자3 = "기계3유전자";
            
            기계.__proto__ = 기계2.prototype;
            기계2.__proto__ = 기계3.prototype;
            
            // console.log(기계.유전자3) // undefined
            // [[Prototype]]
            // 기계.__proto__ 가 보여주는것 : 바로 윗놈의 유전자만? 아니면 상속받는것까지 전부 보여주는?
            // contructor 안에 prototype 있는이유?
            ```
            
            - 추가 공부
                
                ![스크린샷 2022-09-26 오후 9.40.38.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_9.40.38.png)
                

- ES5, ES6 버전 prototype 구현
    - ES5
        
        ![Untitled](ES6%204ab918b4db054fe6a938e593e1d507dc/Untitled%204.png)
        
    - ES6 - class
        - 기본 사용법 , extends , super
            
            ```jsx
            class 할아버지기계 {
            	constructor(a){
            		this.name = a
            		this.sayHi = function(){}
            	}
            	// 기계.prototype에 해당하는 부분
            
            }
            
            class 아버지기계 extends 할아버지기계 { //extends : 할아버지기계.prototype 가지고옴
            	constructor(){
            		super(a); //할아버지기계의 constructor 부분 가지고옴
            
            	}
            	newFunc = super.sayHi // 여기서의 super은 할아버지기계.prototype자체를 의미
            }
            ```
            
        - getter setter
            
            → obj.name = “kim” ⇒ obj.setName(”kim”) 이렇게 하는게 트렌드. 이런걸 구현하기 위한 문법.
            
            ```jsx
            class 기계 {
            	constructor(a){
            		this.name = a
            		this.age = '30'
            		this.sayHi = function(){}
            	}
            	get getAge(){
            		return this.age; //getter엔 반드시 return 형태로
            	}
            	set setAge(나이){
            		this.age = parseInt(나이); //setter엔 실행할 코드. parameter 꼭하나여야됨
            	}
            
            }
            ```
            

# Destructing

→ [] , {} 에 대한 처리를 좀더 간편하게

- 사용법
    
    ```jsx
    const [a,b,c=3] = [1,2]
    
    const {name : 이름 , age} = {name:"Kim" , age:24}
    
    function func({a,b}){
    	console.log(a,b)
    }
    
    function func2([a,b]){
    	console.log(a,b)
    }
    
    ```
    

# export , import

- 사용법
    
    ```jsx
    // script type="module" 안에 이 코드 쓴다.
    	import newA, {b , c as newC} form "경로"; 
    		// newA : export default꺼. 작명 자유.
    		// export default랑 export 같이 쓰는 경우인데, 순서 잘 지켜서 적자.
    	import * from "경로"; // export 만 객체 형태로 갖고옴
    
    ```
    
    ```jsx
    export default a; // 한 파일에 한번만 쓸수 있음.
    export const b = 3;
    export 
    ```
    

** IE에서는 import ,export 호환성 떨어지므로 그냥 script src=”” 사용하자.

# heap,stack,background,queue

→ heap : 변수 저장소

→ stack : 실행 할것 들어감 , 동기적 (single-threaded)

→ background : 콜백 포함하는 함수가 stack에 들어가서 실행되면 바로 background로 전달

- 브라우저에서 WEB API라 불림

→ queue : background에서의 그 콜백들이 전달

- stack이 비었을때, stack으로 콜백들이 전달되어 실행

**교훈

1. stack은 동기적이니, cpu연산 오래걸리는 작업 하게 하지마라
2. queue에 너무 많은 콜백들어가게하지마라
    
    ? 정확히 이해못함
    

# Promise

→ Promise = 콜백을 예쁘게 해주고 + 성공실패 판정해주는기계

- 기본 개념
    - 상태 : pending / resolved / rejected
        
        → **프로미스 그 자체**가 성공() 실패() 실행됬는지 판정해주는 기능.
        
    - .then / catch / finally 하고나면 또다른 Promise 리턴.
- 사용법

```jsx
const 프로미스 = new Promise((성공,실패) => {
	성공()
})

프로미스
	.then()
	.catch()
	.finally()
```

- 효용
    
    → 비동기적으로 처리해야할 코드를 안에 넣어 사용
    

** Promise 자체는 동기를 비동기로 바꿔주는 마법이 아님!!!

```jsx
console.log(1)
const 프로미스 = new Promise((성공,실패)=>{
    console.log(2)
    console.log(3)
    성공();
})
console.log(4)

// 여기서는 프로미스가 있으나없으나 똑같다.
// 비동기를 구현하고 싶다면,  setTimeout() 같은 특수한 비동기 함수를 쓰거나, .then 같은걸 써야함
```

- async, await
    
    → 프로미스.then 을 좀더 직관적으로 구현
    
    **ES8 문법임을 주의
    
    ```jsx
    const 함수 = new Promise((성공,실패)=>{
    	if (a>b){
    		성공("성공!!")
    	} else {
    		실패("실패")
    	}
    })
    
    //위아래가 완벽히 같은 코드
    
    async function 함수(){
    	if (a>b) {
    		return "성공"
    	} else {
    		return Promise.reject("실패")
    	}
    }
    함수()
    ```
    
    - await
        
        → then 과 같은 기능.
        
        ** const 결과 = await 프로미스 // const 프로미스 = new Promise 랑 같다고 생각하면 된다. 차이점은 pending을 보여주냐의 유무.
        
        ** try 문안에 await 프로미스 넣은것 === 프로미스.catch 처리해준것
        
        ```jsx
        async function func(){
        	try {
        		const 결과 = await 프로미스 // 얘가 실패하면 에러 뿜음. try문안에서 에러 뿜을시 바로 catch문으로 넘어감
        	} catch {
        			
        	}
        }
        
        프로미스
        	.then(결과=>결과)
        	.catch(에러=>에러) //catch 문 처리를 해주면 에러를 안뿜음. 에러를 리턴만 해줌
        ```
        

# for 문

- for in 문
    
    → 객체의 enumerable한 값들의 **key** 배출.
    
    ++ enumerable : Object.getOwnPropertyDescriptor(오브젝트, ’name’) 하면 나옴.
    
    ⇒ for (let i in obj)
    
    ++ 부모의 prototype도 반복해줌. (class 로 생성된 객체일 경우) 
    
    ++ obj.hasOwnProperty(key) : 객체껀지, 부모껀지 확인해줌. 이걸로 부모꺼 반복안하게 짤수있음
    
- for of 문
    
    → iterable한 자료형의 **자료값** 배출
    
    ++ iterable 자료형 : array, 문자, arguments, Nodelist , Map, Set
    
    ⇒ for (자료 of iterable)
    

# Symbol

→ 객체의 비밀스러운 prop. import 해온 객체를 변형시킬때 사용.

→ enumerable == false 라 반복문 돌려도 안나옴

⇒ 

```jsx
let a = Symbol("설명")
let b = Symbol("설명")

console.log(a===b) //false

let c = Symbol.for('설명') // 전역 Symbol 만듬

```

++ array, object 안에도 내장된 Symbol 존재 ex. 어레이[Symbol.iterator]

# Map , Set 자료형

- Map :
    
    → 객체와 비슷하지만, 자료간의 의존성을 나타냄
    
    - 용도 : 수학연산, 데이터베이스 관리등을 할떄 사용 거의 안씀
    - 사용법
        
        ![스크린샷 2022-09-24 오전 12.05.19.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-24_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.05.19.png)
        
- Set
    
    → 중복 없는 array 생성기. array 중복 제거할때 많이씀
    
    ⇒ 
    
    → var 출석부 = [’a’,’b’,’c’,’a’]
    
    var 출석부2 = new Set(출석부) //[’a’,’b’,’c’]
    
    - 자료 추가 / 삭제 / has
        
        출석부2.add / delete / has
        
    - set ↔ array 전환
        
        → 출석부3 = […출석부2]
        
    - set에도 반복문 돌릴수있다.

# Optional Chaning

- user?.age?.value
    
    → . 이 여러개 있을때!! 안전하게 undefined 를 출력할수있다.
    
    ** 꼭 필요할때만
    
- undefined나 null ?? “대신 출력할것”
    
    ** 데이터 늦게 도착하는것 방지용
    

# Web - Components

![스크린샷 2022-09-24 오전 12.18.24.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-24_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.18.24.png)

- 재렌더링 구현법
    
    ![스크린샷 2022-09-24 오전 12.24.06.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-24_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.24.06.png)
    

# Shadow DOM

- shadow DOM 보일수 있게 설정하는법
- shadow DOM 을 만드는법?
    
    ![스크린샷 2022-09-24 오전 12.33.18.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-24_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.33.18.png)
    
- web - components + shadow dom 으로 모듈화하기
    
    → style 태그가 밖으로 튀어나가지 않게 하려면??? : shadow DOM 안에 넣어라!
    
    ![스크린샷 2022-09-24 오전 12.39.28.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-24_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.39.28.png)
    
    ![스크린샷 2022-09-24 오전 12.40.21.png](ES6%204ab918b4db054fe6a938e593e1d507dc/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-24_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.40.21.png)