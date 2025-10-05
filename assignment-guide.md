# Week 3 과제: Todo 앱 React로 업그레이드

## 목차 (Table of Contents)

1. [과제 개요](#과제-개요)
2. [개발 환경 설정](#개발-환경-설정)
3. [필수 구현 사항](#필수-구현-사항)
4. [반응형 디자인 구현 가이드](#반응형-디자인-구현-가이드-필수)
5. [파일 구조](#파일-구조)
6. [개발 순서 가이드](#개발-순서-가이드)
7. [Week 2 → Week 3 변환 가이드](#week-2--week-3-변환-가이드)
8. [심화 과제](#심화-과제)
9. [제출 방법](#제출-방법)
10. [도움 받기](#도움-받기)
11. [학습 완료 후 성취](#학습-완료-후-성취)

---

## 과제 개요

Week 2에서 Vanilla JavaScript로 만든 Todo 앱을 React로 완전히 다시 구현하여, React의 장점을 직접 체험하고 두 방식의 차이점을 이해합니다.

### 제출 기한
- **마감**: Week 3 금요일 저녁 6시

### 과제의 목적
1. **React 핵심 개념 적용**: useState, 컴포넌트, Props, 이벤트 처리
2. **코드 비교 체험**: Vanilla JS vs React의 실질적 차이점 이해
3. **포트폴리오 발전**: 같은 기능의 진화 과정 보여주기

---

## 개발 환경 설정

### ⚠️ 중요: CDN vs Build Tools 차이점
```javascript
// CDN 사용시 (우리 수업에서 사용)
const [count, setCount] = React.useState(0);
React.useEffect(() => {}, []);

// Create React App이나 Vite 사용시 (import 가능)
const [count, setCount] = useState(0);
useEffect(() => {}, []);

// 에러 해결: "useState is not defined" → React.useState 사용
```

### 시작하기
1. **빈 HTML 파일 생성**
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React Todo 앱</title>
    <!-- React CDN -->
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
        // 여기에 React 코드 작성
    </script>
</body>
</html>
```

2. **기본 컴포넌트 구조**
```javascript
function TodoApp() {
    // 1. State 정의 - CDN 사용시 React.useState 필수!
    const [todos, setTodos] = React.useState([]);
    const [inputText, setInputText] = React.useState('');

    // 2. 이벤트 핸들러 함수들
    const addTodo = () => { /* 구현 */ };
    const toggleTodo = (id) => { /* 구현 */ };
    const deleteTodo = (id) => { /* 구현 */ };

    // 3. JSX 반환
    return (
        <div>
            {/* UI 구성 */}
        </div>
    );
}

// 4. 렌더링
ReactDOM.createRoot(document.getElementById('root')).render(<TodoApp />);
```

---

## 필수 구현 사항

**목표**: React 핵심 개념을 활용한 완전한 Todo 앱 구현

### ✅ 1. React 기본 구현
```javascript
// React 함수 컴포넌트 사용
function TodoApp() {
    // CDN 사용시 React.useState 필수!
    const [todos, setTodos] = React.useState([]);
    const [inputText, setInputText] = React.useState('');

    return (
        // JSX로 UI 구성
    );
}
```

### ✅ 2. 핵심 CRUD 기능
- **Create**: 새로운 할일 추가 (Enter 키 지원)
- **Read**: 할일 목록 표시
- **Update**: 완료/미완료 상태 토글
- **Delete**: 할일 삭제

### ✅ 3. 상태 관리 및 데이터 지속성
```javascript
// useState로 상태 관리
const [todos, setTodos] = React.useState([]);

// LocalStorage 연동
React.useEffect(() => {
    localStorage.setItem('react-todos', JSON.stringify(todos));
}, [todos]);
```

### ✅ 4. 기본 사용자 경험
- 빈 상태일 때 안내 메시지
- 입력 유효성 검사 (빈 문자열 방지)
- **할일 통계 표시** (전체/완료/진행중 개수)

### ✅ 5. React 이벤트 처리
```javascript
// React 방식의 이벤트 처리
<button onClick={handleAddTodo}>추가</button>
<input onChange={(e) => setInputText(e.target.value)} />
<input type="checkbox" onChange={() => toggleTodo(id)} />
```

### ✅ 6. 컴포넌트 분리 및 Props
- 최소 3개 이상 컴포넌트 분리 (TodoApp, TodoItem, TodoList)
- Props를 통한 데이터 전달
- 콜백 함수로 이벤트 처리
- 컴포넌트 재사용성 체험

### ✅ 7. 필터링 및 상태 관리
- 전체/진행중/완료 필터링 구현
- 할일 검색 기능 추가
- 진행률 바 표시
- 일괄 작업 (모두 완료/삭제)

### ✅ 8. 반응형 디자인
- 태블릿과 데스크톱에서 최적화된 레이아웃
- Tailwind CSS 활용한 반응형 구현
- 자세한 구현 방법은 아래 섹션 참조

### 반응형 디자인 구현 가이드 (필수)

#### 반응형 디자인이란?
반응형 디자인은 다양한 기기의 화면 크기(모바일, 태블릿, 데스크톱)에 따라 레이아웃이 자동으로 조정되어 최적의 사용자 경험을 제공하는 웹 디자인 기법입니다.

**우리 과제에서의 구현 범위:**
- **태블릿 (768px 이상)**: 좁은 화면에 맞춘 세로 배치
- **데스크톱 (1024px 이상)**: 넓은 화면을 활용한 가로 배치

#### 구현 방법

**Tailwind CSS로 반응형 Todo 앱 만들기**

#### Tailwind CSS 반응형 클래스 이해하기

Tailwind에서 `md:`는 "768px 이상에서만 적용"을 의미합니다:
```html
<!-- 기본(태블릿): flex-col, 768px 이상(데스크톱): flex-row -->
<div className="flex flex-col md:flex-row">
```

#### 실제 Todo 앱 예제

**1. Todo 입력 영역 - 태블릿에서는 세로, 데스크톱에서는 가로 배치**
```html
<!-- Todo 입력 컨테이너 -->
<div className="flex flex-col md:flex-row gap-4 mb-6">
    <!-- 입력 필드: 태블릿 전체너비, 데스크톱 늘어남 -->
    <input
        className="flex-1 px-4 py-2 border rounded-lg"
        placeholder="할일을 입력하세요"
    />
    <!-- 버튼: 태블릿 전체너비, 데스크톱 자동크기 -->
    <button className="w-full md:w-auto px-6 py-2 bg-blue-500 text-white rounded-lg">
        추가
    </button>
</div>
```

**2. Todo 목록 - 태블릿 1열, 데스크톱 2열**
```html
<!-- Todo 목록 그리드 -->
<div className="grid grid-cols-1 md:grid-cols-2 gap-4">
    <div className="p-4 border rounded-lg">
        <!-- Todo 아이템 1 -->
    </div>
    <div className="p-4 border rounded-lg">
        <!-- Todo 아이템 2 -->
    </div>
</div>
```

**3. 전체 앱 레이아웃**
```html
<div className="container mx-auto">
    <!-- 제목: 태블릿 작게, 데스크톱 크게 -->
    <h1 className="text-2xl md:text-4xl font-bold text-center mb-8">
        Todo 앱
    </h1>

    <!-- 메인 컨테이너: 태블릿 작은 패딩, 데스크톱 큰 패딩 -->
    <div className="p-4 md:p-8 max-w-4xl mx-auto">
        <!-- Todo 앱 내용 -->
    </div>
</div>
```

#### 핵심 Tailwind CSS 반응형 클래스들

| 클래스 | 의미 | 태블릿에서 | 데스크톱에서 |
|--------|------|------------|-------------|
| `flex flex-col md:flex-row` | 세로→가로 배치 | 세로 배치 | 가로 배치 |
| `w-full md:w-auto` | 전체→자동 너비 | 전체 너비 | 내용에 맞는 너비 |
| `grid-cols-1 md:grid-cols-2` | 1열→2열 그리드 | 1열 배치 | 2열 배치 |
| `text-2xl md:text-4xl` | 작은→큰 글씨 | 24px | 36px |
| `p-4 md:p-8` | 작은→큰 패딩 | 16px | 32px |

#### 화면 크기별 변화 시뮬레이션

**태블릿 (768px) 보기:**
```
┌─────────────────┐
│     Todo 앱     │
├─────────────────┤
│ [입력필드]      │
│ [추가버튼]      │
├─────────────────┤
│ [Todo 1]        │
│ [Todo 2]        │
│ [Todo 3]        │
└─────────────────┘
```

**데스크톱 (1024px 이상) 보기:**
```
┌─────────────────────────────┐
│         Todo 앱             │
├─────────────────────────────┤
│ [입력필드]      [추가버튼]  │
├─────────────────────────────┤
│ [Todo 1]    │   [Todo 2]    │
│ [Todo 3]    │   [Todo 4]    │
└─────────────────────────────┘
```

#### 반응형 테스트 방법

**Chrome DevTools 사용하기**
1. **DevTools 열기**: F12 또는 우클릭 → 검사
2. **반응형 모드 활성화**: Ctrl+Shift+M (Windows) / Cmd+Shift+M (Mac)
3. **기기 선택** (우리는 2가지만 테스트):
   - iPad (768×1024) - 태블릿 크기
   - Desktop (1200×800) - 데스크톱 크기
4. **테스트 체크리스트**:
   ```
   ✅ 태블릿에서 세로 배치가 잘 되는가?
   ✅ 데스크톱에서 가로 배치가 자연스러운가?
   ✅ 입력 필드와 버튼 배치가 적절한가?
   ✅ Todo 목록이 화면 크기에 맞게 배치되는가?
   ✅ 텍스트 크기가 각 화면에서 읽기 좋은가?
   ```

#### 반응형 디자인 필수 체크리스트 (태블릿-데스크톱)
- [ ] **태블릿 기준 설계**: 768px에서 자연스러운 세로 배치
- [ ] **데스크톱 확장**: 1024px 이상에서 가로 배치 활용
- [ ] **입력 요소**: 태블릿에서 세로 배치, 데스크톱에서 가로 배치
- [ ] **Todo 목록**: 태블릿 1열, 데스크톱 2열 그리드
- [ ] **텍스트 크기**: 태블릿 기본, 데스크톱 확대
- [ ] **패딩/마진**: 화면 크기에 따른 적절한 여백
- [ ] **최대 너비**: 데스크톱에서 읽기 편한 최대 너비 설정

---

## 파일 구조

```
your-project/
├── index.html              # 메인 HTML 파일
├── style.css               # CSS 스타일 (필수)
├── components/             # 컴포넌트 분리 (권장)
│   ├── TodoApp.js         # 메인 앱 컴포넌트 (선택사항)
│   ├── TodoItem.js        # 개별 할일 컴포넌트 (선택사항)
│   └── TodoList.js        # 할일 목록 컴포넌트 (선택사항)
└── README.md              # 프로젝트 설명 및 사용법 (필수)
```

**참고**: 컴포넌트 분리는 선택사항이지만, 코드 구조화를 위해 권장합니다.

---

## 개발 순서 가이드

### 권장 개발 순서
1. **기본 구조 만들기** (컴포넌트, state 정의)
2. **할일 추가 기능** (가장 기본적인 CRUD)
3. **할일 표시 기능** (목록 렌더링)
4. **완료 토글 기능** (상태 업데이트)
5. **삭제 기능** (배열에서 제거)
6. **통계 기능** (할일 개수 계산 및 표시)
7. **LocalStorage 연동** (데이터 지속성)
8. **반응형 디자인** (마지막 단계)

---

## Week 2 → Week 3 변환 가이드

### 1. 전역 변수 → React State
```javascript
// Week 2 (Vanilla JS)
let todos = [];
let currentId = 1;

// Week 3 (React) - CDN 사용시 주의!
const [todos, setTodos] = React.useState([]);
// ID는 Date.now() 사용 권장
// CDN으로 React 사용시 React.useState 필수!
```

### 2. DOM 조작 → 선언적 렌더링
```javascript
// Week 2 (명령형)
function renderTodos() {
    const todoList = document.getElementById('todoList');
    todoList.innerHTML = '';
    todos.forEach(todo => {
        const li = document.createElement('li');
        li.innerHTML = `...복잡한 HTML...`;
        todoList.appendChild(li);
    });
}

// Week 3 (선언형)
return (
    <ul>
        {todos.map(todo => (
            <li key={todo.id}>
                <input type="checkbox" checked={todo.completed} />
                <span>{todo.text}</span>
                <button onClick={() => deleteTodo(todo.id)}>삭제</button>
            </li>
        ))}
    </ul>
);
```

### 3. 이벤트 리스너 → JSX 이벤트 핸들러
```javascript
// Week 2
document.getElementById('addBtn').addEventListener('click', addTodo);
document.getElementById('todoInput').addEventListener('keypress', handleKeyPress);

// Week 3
<button onClick={addTodo}>추가</button>
<input onKeyPress={handleKeyPress} />
```

### 4. 수동 업데이트 → 자동 리렌더링
```javascript
// Week 2 - 상태 변경 후 수동으로 화면 업데이트
todos.push(newTodo);
renderTodos();
saveTodos();

// Week 3 - state 변경시 자동 리렌더링
setTodos([...todos, newTodo]);
// useEffect로 자동 저장
```

---

## 심화 과제


### 고급 UX 기능
- 드래그 앤 드롭으로 할일 순서 변경
- 다크/라이트 모드 전환
- 애니메이션 및 트랜지션 효과
- 키보드 단축키 지원 (Enter, ESC 등)
- 로딩 상태 및 에러 처리

---

## 제출 방법

### 1. GitHub 저장소 생성
1. GitHub에서 새 저장소 생성
2. 저장소명: `week3-react-todo` (권장)
3. Public으로 설정

### 2. 파일 업로드
1. 모든 프로젝트 파일 업로드
2. README.md 포함
3. 커밋 메시지: "Week 3: React Todo 앱 완성"

### 3. GitHub Pages 배포
1. Settings → Pages 탭
2. Source: Deploy from a branch
3. Branch: main
4. 배포 URL 확인

### 4. 제출 양식
```
이름: 홍길동
Week 3 React Todo 과제 제출

GitHub 저장소: https://github.com/username/week3-react-todo
배포 URL: https://username.github.io/week3-react-todo

✅ 필수 구현 기능 체크:
- [ ] React 컴포넌트로 Todo 앱 구현
- [ ] useState/useEffect로 상태 관리
- [ ] 기본 CRUD 기능 (추가/표시/토글/삭제)
- [ ] 할일 통계 표시 (전체/완료/진행중 개수)
- [ ] 컴포넌트 분리 및 Props 활용
- [ ] 필터링 및 상태 관리 기능
- [ ] 반응형 디자인 (태블릿/데스크톱 대응)

```

---

## 도움 받기

### 질문 방법
1. **먼저 시도**: 에러 메시지 확인, console.log() 활용
2. **수업 자료 참고**: 수업시간에 공유한 html 예제들
3. **동료 도움**: 같은 과제를 하는 친구들과 토론
4. **강사 질문**: 구체적인 상황과 시도한 방법 설명

### 자주 발생하는 에러와 해결법
```javascript
// ❌ 에러: useState is not defined
const [todos, setTodos] = useState([]);

// ✅ 해결: React.useState 사용 (CDN 방식)
const [todos, setTodos] = React.useState([]);

// ❌ 에러: useEffect is not defined
useEffect(() => {}, []);

// ✅ 해결: React.useEffect 사용 (CDN 방식)
React.useEffect(() => {}, []);
```


### 🚫 주의사항
- **직접 코딩**: AI 도구로 전체 코드 생성 금지
- **이해 우선**: 작동하더라도 이해하지 못한 코드는 지양
- **점진적 완성**: 한 번에 모든 기능을 구현하려 하지 말 것
- **과정 중시**: 완성도보다 학습 과정과 이해도가 더 중요

---

## 학습 완료 후 성취

Week 3 과제를 완료하면:
- ✅ **React 핵심 개념 체득**: 컴포넌트, useState, useEffect 활용법 습득
- ✅ **개발 패러다임 전환**: 명령형(Vanilla JS) → 선언형(React) 사고방식 체험
- ✅ **현대적 웹 개발**: 컴포넌트 기반 개발과 상태 관리 이해
- ✅ **실무 역량 향상**: 반응형 디자인과 사용자 경험 고려
- ✅ **포트폴리오 강화**: 취업 시장에서 요구하는 React 기초 스킬 확보

---

**Global PBL Fall 2025 - Kookmin University**
*Week 3 Assignment: Upgrade Your Todo App with React*