# 이준용

[https://github.com/ju-nong/Vue-Note](https://github.com/ju-nong/Vue-Note)
오 대박

## ****⚙️**** **Development environment setting**

### 📢 Setup List

- Chrome
- Visual Studio Code
- Node.js
- Git

### 📢 Visual Studio Code

- **Extension Program**
    - Prettier
    - Vetur
    - Vue VSCode Snippets
- **Default Setting**
    - Mouse Wheel Zoom ✔ (마우스 휠로 줌 아웃)
    - Default Formatter -  Prettier (기본 포맷터 설정)
    - Format On Save ✔ (자동 저장 설정)
    
- **JSON Setting**
    
    ```json
    {
        "workbench.colorTheme": "Default Dark+",
        "editor.mouseWheelZoom": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode",
        "editor.formatOnSave": true,
        //Custom
        "editor.formatOnType": true,
        "prettier.tabWidth": 4,
        "prettier.trailingComma": "all",
        "vetur.validation.template": false,
        "vetur.validation.script": false,
        "vetur.validation.style": false
    }
    ```
    

### 📢 Node.js

- **PowerShell (실행 정책 변경)**
    
    관리자 권한 실행 > Set-ExecutionPolicy RemoteSigned > [A] 모두 예(A)
    
- **Node command prompt**
    - npm install -g @vue/cli
    - npm install -g router
    - npm install -g pinia
    - npm install -g nodemon  (server)
    - npm install -g sass-loader node-sass
    - ~~npm install -g yarn~~  (이제 사용 안 함)
    

## ****🖌️**** **Style Guide (version 3)**

*코드 리팩토링할 때, 읽어보면서 해보면 좋을 듯*

### 📢 필수

<aside>
💡 내장 컴포넌트와의 충돌을 방지하기 위해, 컴포넌트명은 합성어를 사용

- **DETAILS**
    
    ```jsx
    ❌
    app.component('todo', {
      // ...
    })
    
    export default {
      name: 'Todo',
      // ...
    }
    ```
    
    ```jsx
    ✅
    app.component('todo-item', {
      // ...
    })
    export default {
      name: 'TodoItem',
      // ...
    }
    ```
    
</aside>

<aside>
💡 Prop은 상세하게 정의하고 Type을 명시

- **DETAILS**
    
    ```jsx
    ❌
    props: ['status']
    ```
    
    ```jsx
    ✅
    props: {
      status: String,
    	multiple : [Array, String],
    }
    
    // 더 나은 방법!
    props: {
        status: {
            type: String,                // 타입 명시
            required: true,              // 필수 여부
            validator: (value) => {      // 값 체크
                return value.length > 10;// 일치하지 않을 시, console 경고
            },
        },
    },
    ```
    
</aside>

<aside>
💡 v-for에는 항상 key 사용

- **DETAILS**
    
    ```jsx
    ❌
    <ul>
      <li v-for="todo in todos">
        {{ todo.text }}
      </li>
    </ul>
    ```
    
    ```jsx
    ✅
    <ul>
      <li
        v-for="todo in todos"
        :key="todo.id"
      >
        {{ todo.text }}
      </li>
    </ul>
    ```
    
</aside>

<aside>
💡 v-if와 v-for 동시 사용 금지

- **DETAILS**
    
    ```jsx
    ❌ active 유저만 보여주고 싶을 때
    <ul>
      <li
        v-for="user in users"
        v-if="user.isActive"
        :key="user.id"
      >
        {{ user.name }}
      </li>
    </ul>
    ```
    
    ```jsx
    ✅
    // Step 1: active인 유저 따로 만들어서 for 돌리기
    <ul>
      <li
        v-for="user in activeUsers"
        :key="user.id"
      >
        {{ user.name }}
      </li>
    </ul>
    
    // Step 2: active 속성을 컨테이너로 감싸기
    <ul>
      <template v-for="user in users" :key="user.id">
        <li v-if="user.isActive">
          {{ user.name }}
        </li>
      </template>
    </ul>
    ```
    
</aside>

<aside>
💡 최상위 App과 레이아웃 컴포넌트의 스타일은 전역 가능

**그외 모든 컴포넌트는 scoped 속성 사용**

**기타 CSS 라이브러리를 이용할 때는 class 사용**

- **DETAILS**
    
    ```jsx
    ❌
    <template>
      <button class="btn btn-close">×</button>
    </template>
    
    <style>
    .btn-close {
      background-color: red;
    }
    </style>
    ```
    
    ```jsx
    ✅
    <template>
      <button class="button button-close">×</button>
    </template>
    
    <!-- `scoped` 속성 사용 -->
    <style scoped>
    .button {
      border: none;
      border-radius: 2px;
    }
    
    .button-close {
      background-color: red;
    }
    </style>
    ```
    
</aside>

<aside>
💡 모듈 범위 지정 및 속성명에  $, _ 식별자 사용 (관례)

**다른 코드와 충돌하지 않도록 지정된 범위까지 포함**

`$ : 객체를 지칭하거나 return 해줄 때 사용`

`_ : private 명시`

- **DETAILS**
    
    ```jsx
    ❌
    const myGreatMixin = {
      // ...
      methods: {
        update() {
          // ...
        }
      }
    }
    ```
    
    ```jsx
    ✅
    const myGreatMixin = {
      // ...
      methods: {
        $_myGreatMixin_update() {
          // ...
        }
      }
    }
    ```
    
</aside>

### 📢 적극 권장 (가독성 향상)

- **컴포넌트명**
    
    <aside>
    💡 컴포넌트 파일명은 파스칼과 케밥식을 사용
    
    | 파스칼 | MyComponent |
    | --- | --- |
    | 케밥 | my-component |
    | 카멜 | myComponent |
    | 스네이크 | my_component |
    </aside>
    
    <aside>
    💡 베이스 컴포넌트는 앞에 Base, App, V와 같은 접두사를 붙임
    
    - **DETAILS**
        
        ```jsx
        ❌
        components/
        |- MyButton.vue
        |- VueTable.vue
        |- Icon.vue
        ```
        
        ```jsx
        ✅
        components/
        |- BaseButton.vue
        |- BaseTable.vue
        |- BaseIcon.vue
        
        components/
        |- AppButton.vue
        |- AppTable.vue
        |- AppIcon.vue
        
        components/
        |- VButton.vue
        |- VTable.vue
        |- VIcon.vue
        ```
        
    </aside>
    
    <aside>
    💡 하나의 활성 인스턴스를 갖는 컴포넌트는 앞에 The 접두사로 시작
    
    **ex) Header, Footer, SideBar, Navigation**
    
    `페이지당 한 번만 사용된다는 의미`
    
    - **DETAILS**
        
        ```jsx
        ❌
        components/
        |- Heading.vue
        |- MySidebar.vue
        ```
        
        ```jsx
        ✅
        components/
        |- TheHeading.vue
        |- TheSidebar.vue
        ```
        
    </aside>
    
    <aside>
    💡 부모 컴포넌트와 밀접하게 연관된 자식 컴포넌트는 접두사로 부모명 사용
    
    - **DETAILS**
        
        ```jsx
        ❌
        components/
        |- TodoList.vue
        |- TodoItem.vue
        |- TodoButton.vue
        ```
        
        ```jsx
        ✅
        components/
        |- TodoList.vue
        |- TodoListItem.vue
        |- TodoListItemButton.vue
        ```
        
    </aside>
    
    <aside>
    💡 컴포넌트명은 최상위 수준의 단어(대부분, 자주, 일반적)로 시작하고
    설명을 나타내는 단어로 끝냄
    
    - **DETAILS**
        
        ```jsx
        ❌
        components/
        |- ClearSearchButton.vue
        |- ExcludeFromSearchInput.vue
        |- LaunchOnStartupCheckbox.vue
        |- RunSearchButton.vue
        |- SearchInput.vue
        |- TermsCheckbox.vue
        ```
        
        ```jsx
        ✅
        components/
        |- SearchButtonClear.vue
        |- SearchButtonRun.vue
        |- SearchInputQuery.vue
        |- SearchInputExcludeGlob.vue
        |- SettingsCheckboxTerms.vue
        |- SettingsCheckboxLaunchOnStartup.vue
        ```
        
    </aside>
    
    <aside>
    💡 내용이 없는 컴포넌트는 self-closing 처리
    `DOM 템플릿은 해당 안 됨`
    
    - **DETAILS**
        
        ```jsx
        ❌
        <!-- 싱글 파일 컴포넌트, 문자열 템플릿, JSX에서 -->
        <MyComponent></MyComponent>
        
        <!-- DOM 템플릿에서 -->
        <my-component/>
        ```
        
        ```jsx
        ✅
        <!-- 싱글 파일 컴포넌트, 문자열 템플릿, JSX에서 -->
        <MyComponent/>
        
        <!-- DOM 템플릿에서 -->
        <my-component></my-component>
        ```
        
    </aside>
    
    <aside>
    💡 컴포넌트명은 약어보다 전체 단어로
    
    - **DETAILS**
        
        ```jsx
        ❌
        components/
        |- SdSettings.vue
        |- UProfOpts.vue
        ```
        
        ```jsx
        ✅
        components/
        |- StudentDashboardSettings.vue
        |- UserProfileOptions.vue
        ```
        
    </aside>
    
    <aside>
    💡 prop명을 선언할 때는 카멜, 템플릿 및 JSX에서는 케밥
    
    - **DETAILS**
        
        ```jsx
        ❌
        props: {
          'greeting-text': String
        }
        
        <WelcomeMessage greetingText="hi"/>
        ```
        
        ```jsx
        ✅
        props: {
          greetingText: String
        }
        
        <WelcomeMessage greeting-text="hi"/>
        ```
        
    </aside>
    
- **기타**
    
    <aside>
    💡 템플릿에는 computed나 methods 및 간단한 표현식만 포함
    
    `computed는 여러 개의 간단한 속성으로 분할해야함`
    
    - **DETAILS**
        
        ```jsx
        ❌
        {{
          fullName.split(' ').map((word) => {
            return word[0].toUpperCase() + word.slice(1)
          }).join(' ')
        }}
        ```
        
        ```jsx
        ✅
        <!-- 템플릿에서 -->
        {{ normalizedFullName }}
        
        // 복잡한 표현식이 computed 속성으로 이동되었습니다.
        computed: {
          normalizedFullName() {
            return this.fullName.split(' ')
              .map(word => word[0].toUpperCase() + word.slice(1))
              .join(' ')
          }
        }
        ```
        
    </aside>
    
    <aside>
    💡 디렉티브 약어는 모두 통일하거나 전부 사용하지 않기
    
    `혼용하여 사용하지 않고, 하나로 통일`
    
    | : | v-bind: |
    | --- | --- |
    | @ | v-on: |
    | # | v-slot |
    </aside>
    

### 📢 권장 (임의 선택과 인지 오버헤드 최소화)

<aside>
💡 요소 속성 순서  (사용해본 것만 나열해봄)⭐

1. is
2. v-for
3. v-if… (조건)
4. id
5. ref, key
6. v-model
7. v-on (@)
</aside>

<aside>
💡 컴포넌트와 인스턴스 옵션 순서  (역시 사용해본 것만)

1. name
2. components
3. extends
4. props, emits
5. setup
6. computed
7. watch, 라이프 사이클 이벤트들
</aside>

### 📢 주의 필요

<aside>
💡 scoped에서 요소 선택보다는 class로 선택하는 것이 빠름

- **DETAILS**
    
    ```jsx
    ❌
    <template>
      <button>×</button>
    </template>
    
    <style scoped>
    button {
      background-color: red;
    }
    </style>
    ```
    
    ```jsx
    ✅
    <template>
      <button class="btn btn-close">×</button>
    </template>
    
    <style scoped>
    .btn-close {
      background-color: red;
    }
    </style>
    ```
    
</aside>

## ****📄**** **Basic Manual**

*숙지 필요*

### 📢 setup

<aside>
💡 setup 훅 내부에는 data와 function 요소들로 이루어져 있음

</aside>

```jsx
export default {
	name: "Admin",
	setup() {
		// 정적 data
		const name = "이준용";
		const money = 0;
		
		const myWallet = () => {
			console.log(`내 지갑에는 ${money}원이`);
		}

		return { name, money, myWallet };
	}
}
```

### 📢 ref, reactive

<aside>
💡 반응형 data를 만들 때 사용
ref는 모든 값에 대해 반응형을 
reactive는 원시값이 아닌 객체와 배열만 사용 가능

`함수내에서 ref는 .value를 붙이고, 템플릿에서는 ref명만 붙임`

</aside>

```jsx
<template>
    <p>구분 : {{ type }}</p>
    <p>이름 : {{ state.name }}</p>
    <p>나이 : {{ state.age }}</p>
    <button @click="change">CHANGE!</button>
</template>

<script>
import { ref, reactive } from "vue";

export default {
    name: "HelloWorld",
    setup() {
        const type = ref("Guest");
        const state = reactive({ name: "사용자1", age: 100 });

        const change = () => {
            type.value = "Admin";
            state.name = "이준용";
            state.age = 22;
        };

        return { type, state, change };
    },
};
</script>
```

### 📢 computed

<aside>
💡 템플릿내의 값이 변환이 되어야 하는 경우
많은 연산을 해야 할 경우
종속대상을 캐싱하여 값이 변경 될 때만 호출됨
read만하고 무언가를 반환만 함

</aside>

```jsx
<template>
    <input type="text" v-model="age" placeholder="당신의 나이는?" />
    <p>100년 뒤에는 {{ hundredAge }}살</p>
</template>

<script>
import { ref, computed } from "vue";

export default {
    name: "HelloWorld",
    setup() {
        const age = ref(0);
        const hundredAge = computed(() => {
            const _age = parseInt(age.value);
            return isNaN(_age) ? 100 : _age + 100;
        });

        return { age, hundredAge };
    },
};
</script>
```

### 📢 watch

<aside>
💡 값이 변경된 시점에서 원하는 액션을 취할 때
new와 old 값을 확인할 수 있다.
ex) router 변화 감지

</aside>

```jsx
<template>
    <input type="text" v-model="age" placeholder="당신의 나이는?" />
</template>

<script>
import { ref, watch } from "vue";

export default {
    name: "HelloWorld",
    setup() {
        const age = ref(0);

        watch(age, (newVal, oldVal) => {
            console.log(`before : ${oldVal}, after : ${newVal}`);
        });

        return { age };
    },
};
</script>
```

### 📢 동적 컴포넌트

<aside>
💡 탭을 예로 듦
현재 탭에서 새로운 탭을 선택하면, Vue는 새로운 컴포넌트 인스턴스를 생성함
동적 컴포넌트를 사용하면 컴포넌트 인스턴스를 생성했을 때, 캐싱시켜 리랜더링을 막고 상태 유지가 가능함.

</aside>

```jsx
<template>
    <div id="dynamic-component-demo" class="demo">
        <button
            v-for="(tab, index) in tabs"
            :key="index"
            :class="['tab-button', { active: currentTab === tab }]"
            @click="currentTab = tab"
        >
            {{ tab }}
        </button>
        <keep-alive>
            <component :is="currentTabComponent" class="tab"></component>
        </keep-alive>
    </div>
</template>

<script>
import { TabHome, TabPosts, TabArchive } from "@/components/tabs";
import { ref, computed } from "vue";

export default {
    name: "HelloWorld",
    components: { TabHome, TabPosts, TabArchive },
    setup() {
        const currentTab = ref("Home");
        const tabs = ["Home", "Posts", "Archive"];

        const currentTabComponent = computed(() => {
            return `tab-${currentTab.value.toLowerCase()}`;
        });

        return { currentTab, tabs, currentTabComponent };
    },
};
</script>
```

## 📃**Deep Manual** ~~작업중~~

### **📢 <script setup>**

<aside>
💡 더 간결한 코드
더 빠른 성능
setup 기능만 제공
내보내기가 필요하면 일반 setup 블록을 이용할 것

</aside>

```jsx
<template>
	<MyComponent />
  <button @click="log">{{ msg }}</button>
</template>

<script setup>
	// Composition API
	import {ref} from "vue";

	// import Component
	import MyComponent from "./MyComponent.vue";

	// props
	const props = defineProps({
	  foo: String
	});

	// emit	
	const emit = defineEmits(['change', 'delete']);

	// variable
	const msg = 'Hello!';
	const name = ref("이준용");
	
	// functions
	const log = () => {
		console.log(
	}
</script>
```
