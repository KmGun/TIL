# TypeScript

# TS 셋팅

- 설치
    1. node js 설치, npm i -g typescript
    2. index.ts , tsconfig.json
        - tsconfig. json
            - 기본 설정법
                
                ```tsx
                {
                    "compilerOptions": {
                        "target": "es5", //es3 , es5, es6 esnext등 지정
                        "module": "commonjs",
                    }
                }
                ```
                
                - [추가 참고](https://www.typescriptlang.org/tsconfig)
                    
                    ```tsx
                    {
                     "compilerOptions": {
                    
                      "target": "es5", // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
                      "module": "commonjs", //무슨 import 문법 쓸건지 'commonjs', 'amd', 'es2015', 'esnext'
                      "allowJs": true, // js 파일들 ts에서 import해서 쓸 수 있는지 
                      "checkJs": true, // 일반 js 파일에서도 에러체크 여부 
                      "jsx": "preserve", // tsx 파일을 jsx로 어떻게 컴파일할 것인지 'preserve', 'react-native', 'react'
                      "declaration": true, //컴파일시 .d.ts 파일도 자동으로 함께생성 (현재쓰는 모든 타입이 정의된 파일)
                      "outFile": "./", //모든 ts파일을 js파일 하나로 컴파일해줌 (module이 none, amd, system일 때만 가능)
                      "outDir": "./", //js파일 아웃풋 경로바꾸기
                      "rootDir": "./", //루트경로 바꾸기 (js 파일 아웃풋 경로에 영향줌)
                      "removeComments": true, //컴파일시 주석제거 
                    
                      "strict": true, //strict 관련, noimplicit 어쩌구 관련 모드 전부 켜기
                      **"noImplicitAny"**: true, //any타입 금지 여부
                      "strictNullChecks": true, //null, undefined 타입에 이상한 짓 할시 에러내기 
                      "strictFunctionTypes": true, //함수파라미터 타입체크 강하게 
                      "strictPropertyInitialization": true, //class constructor 작성시 타입체크 강하게
                      "noImplicitThis": true, //this 키워드가 any 타입일 경우 에러내기
                      "alwaysStrict": true, //자바스크립트 "use strict" 모드 켜기
                    
                      "noUnusedLocals": true, //쓰지않는 지역변수 있으면 에러내기
                      "noUnusedParameters": true, //쓰지않는 파라미터 있으면 에러내기
                      "noImplicitReturns": true, //함수에서 return 빼먹으면 에러내기 
                      "noFallthroughCasesInSwitch": true, //switch문 이상하면 에러내기 
                     }
                    }
                    ```
                    
    3. tsc -w : 자동변환 해서 index.js 로 저장

# TS 사용법

```tsx
const 변수 : 타입지정부분 = 값 ;

// type
type Type = string | number; //보통 영어대문자
let 변수 : Type = ~~

// 기본 지정법
let 변수 : number[] = [1,2,3]
let 변수 : (string | number)[] = [1,"문자",3]
let 변수 : {name? : string} = {name : "Kim"} // 변수자리에는 name만 들어올수 있다. (? : 선택적으로)
let 변수 : string | number = 5;

// array 에 자리 지정 = tuple 타입
type Member = [number,boolean];
const 변수 : Member = [1,true];

// 함수 typing
	// return typing시 void 적어주면, return 없다는것을 명시하는것.
	// par1 : string | undefined === par1? : string

function func(x : number) : number {
	return x * 2
}

	//위아래가 같은코드

type 함수타입 = (x:number) => number
const func : 함수타입 = function (x){return x*2}
	
// 객체 typing
type Member = {
	name : string,
	[key : string] : string,  // key로 string이 들어오면 그것은 모두 string
}

// class typing
class User {
	name : string; // 이런식으로 constructor 바깥에서도 한번더 지정해줘야함.
	constructor (name : string){
		this.name = name;
	}
}
```

# TS types

→ typescript는 타입 기반으로 돌아감. type종류 : Primitive / Union / any / unknown

- 3 types
    - Primitive type
        
        → 기본 타입.
        
        ** typing을 매번 안해줘도, 기본적으로 자동적으로 해준다.
        
    - union type
        
        → 여러개가 겹쳐있는 type
        
    - any vs unknown
        
        → any는 ts에서 무적(=쉴드 해제) , unknown은 union과 비슷하게 애매한 느낌.
        
        ** any : 안정성 떨어지므로 되도록 안쓰는게 좋다.
        
        ** let a ; : 그냥 any타입이 배정된다.
        
        ⇒ 차이 설명
        
        ```tsx
        let 이름 : unknown;
        이름 = {};
        
        let 변수 : string = 이름; // 오류남. 근데 제일 윗줄을 any로 하면 그냥 됨.
        ```
        
- TypeScript는 애매한것을 싫어한다.
    
    ```tsx
    // 이거 오류 안남 : JS 식 연산 허용
    let 변수 : string;
    let 변수2 : number;
    
    변수 + 변수2
    
    // 애매한 예 : 에러남
    let 변수 : number | string;
    변수 + 1 
    
    // 애매한 예 2 : 에러남
    let 변수 : unknown = 1;
    변수 + 1
    ```
    

# Narrowing , Assertion

## Narrowing

→ 애매한걸 싫어하는 TS에게 추가설명 해주는 방법.

⇒ 

1. if 문
2. in , instanceof

## Assertion

→ Typescript를 안쓰겠다. as로 때우겠다.

** 이때만 쓰자.

1. 디버깅하다가 임시로 as 붙여놔야할때
2. 어떤 데이터 들어올지 100% 확실할때

# Type Alias , readonly

## Type Alias

→ type 으로 타입 변수 생성하는것을 의미. 

** 재선언 불가

- type 합치기 가능.
    
    ```tsx
    type T1 = string;
    type T2 = number;
    type Person = T1 | T2;
    
    type Obj1 = {x:number};
    type Obj2 = {y:string};
    type NewObj = Obj1 & Obj2;
    ```
    

## readonly

→ object.freeze() 와 비슷

⇒ 

```tsx
type Girlfriend = {
  readonly name : string,
}

let 여친 :Girlfriend = {
  name : '엠버'
}

여친.name = '유라' //readonly라서 에러남
```

# Literal Types

→ 3 types + literal type 

→ 값을 직접 타입으로 지정

⇒ 

```tsx
let 변수 : 123;
let 변수2 : "대머리" | "솔로";
// function return 에도 할당 가능
```

- 문제점
    
    ```tsx
    let 자료 = {
    	name : "kim"
    }
    function 함수 (a : "kim"){
    
    }
    함수(자료.name) // 이러면 오류남 : 자료.name을 string으로 인식하기 때문. = "kim"이라는 type !== string type 이라서.
    
    //해결법
    1. 자료 typing
    2. 자료.name as "kim" 이렇게
    
    3. as const 로 자료를 잠근다 (prop 변경 안됨)
    	-> literal typing을 자료에 바로 적용해줌
    4. object 속성들에 모두 readonly 붙여줌 (잠그기)
    ```
    

# interface vs type

→  obj은 보통 interface로 typing 한다.

- 차이점 1 : extends 사용가능
    
    ```tsx
    interface Teacher extends Student {
    
    }
    
    interface Teacher = {} & Student
    
    ```
    
- 차이점2 : 중복선언 가능
    
    → 중복선언시 extends한것과 똑같이 작용됨.
    
    - 용도
        
        → 유연한게 장점.
        
        - 외부 라이브러리에 추가 할수 있다. = 다른 사람이 내 코드많이 쓸것 같을때 쓰면 된다.
        - 보통 interface로 객체typing, 이외는 type으로 하는경우도 있다고함.

** prop 이 겹겨칠겨우itnfefc cee 더 낫다

→ 미리 오류 띄워줌, type & 는 미리 안띄워줌

# html에 적용하기

→ index.ts - index.js - index.html

- 방법
    1. tsconfig에 
        
        ```tsx
        {
            "compilerOptions": {
                "target": "ES5",
                "module": "commonjs",
                "strictNullChecks": true
            }
        }
        ```
        
    2. ts 파일에
        
        ```tsx
        let 제목 = document.querySelector('#title');
        if (제목 != null) {
          제목.innerHTML = '반갑소'
        }
        
        let 제목 = document.querySelector('#title');
        if (제목 instanceof HTMLElement) {
          제목.innerHTML = '반갑소'
        } // 가장 좋은방법
        
        	let 링크 = document.querySelector('#link');
        		if (링크 instanceof HTMLAnchorElement) {
        	  링크.href = 'https://kakao.com'  //  이거의 경우 더 정확히 지정 해줘야함
        	}
        
        	let 버튼 = document.getElementById('button');
        		버튼?.addEventListener('click', function(){
        	  console.log('안녕')
        	})
        
        let 제목 = document.querySelector('#title') as HTMLElement;
        제목.innerHTML = '반갑소'
        
        let 제목 = document.querySelector('#title');
        if (제목?.innerHTML != undefined) {
          제목.innerHTML = '반갑소'
        } //이방법이 약간 니꼬가 알려줬던 그방법이랑 비슷하네
        
        ```