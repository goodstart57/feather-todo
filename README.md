# feather-todo

작업 목록 관리 (★경량)

## 1. node js 환경세팅

### 1.1. node js 설치

설치 이후 node js 실행

```jsx
(base)  jfrost  ~/Documents/workspace/feather-todo   main  node
Welcome to Node.js v18.13.0.
Type ".help" for more information.
> console.log("Hello Node.js")
Hello Node.js
```

### 1.2. Node.js project 생성

```jsx
npm init
```

npm init 실행 시 프로젝트명, 설명, 저자명, git repository 주소 등 프로젝트 관련 정보를 입력할 수 있다.

### 1.3. express package 설치 및 세팅

```jsx
npm install express

added 57 packages, and audited 58 packages in 3s

7 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

Node.js를 위한 웹 프레임워크

```jsx
vim /app.js
```

```jsx
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```

```jsx
node app.js
Example app listening at http://localhost:3000
```

브라우저에서 localhost:3000에 접속하면 Hello World를 볼 수 있다.

### 1.4. sqlite3

[https://velog.io/@sekkaro96/노드JS-SQLite를-사용한-CRUD](https://velog.io/@sekkaro96/%EB%85%B8%EB%93%9CJS-SQLite%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-CRUD)

```jsx
(base)  ✘ jfrost  ~/Documents/workspace/feather-todo   main ±  npm install sqlite3
npm WARN deprecated @npmcli/move-file@1.1.2: This functionality has been moved to @npmcli/fs

added 109 packages, and audited 167 packages in 12s

11 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
(base)  jfrost  ~/Documents/workspace/feather-todo   main ±  npm install sqlite3 --no-fund

up to date, audited 167 packages in 470ms

found 0 vulnerabilities
```

sqlite3를 설치하니 펀딩이 필요하니 167개 파일이 설치되지 않은것 처럼 나온다.

--no-fund 옵션을 추가해서 다시 실행하니 167개 패키지가 추가로 설치되었다.

## 2. Manage Task

### 2.1. DB 스키마 구성

sqlite3를 이용하여 DB를 구성

TASK : PLAN : RESULT = 1 : N : N 관계를 가지고 있다.

- TASK : 작업
    - String taskDt [PK]
    - int taskId [PK]
    - String taskStatus
    - String[] tag
    - Date created_at
    - Date updated_at
- PLAN : 해야할 일 (TODO)
    - String planDt [PK]
    - int planId [PK]
    - String planStatus
    - int taskId [PK]
    - String content
    - Date created_at
    - Date updated_at
- RESULT : 한 일 (DONE)
    - String resultDt [PK]
    - int resultId [PK]
    - int taskId [PK]
    - int planId [PK]
    - String content
    - Date created_at
    - Date updated_at
- TAG : Task의 유형, 관련 hashtag 기록

```jsx
CREATE TABLE IF NOT EXISTS TASK (
	PRIMARY KEY taskDt VARCHAR(8)
	PRIMARY KEY taskId INTEAGER
	taskStatus TEXT
	created_at TIMESTAMP
	updated_at TIMESTAMP
)

CREATE TABLE IF NOT EXISTS PLAN (
	PRIMARY KEY planDt VARCHAR(8)
	PRIMARY KEY planId INTEAGER
	planStatus VARCHAR(2)
	taskId INTEGER
	content TEXT
	created_at TIMESTAMP
	updated_at TIMESTAMP
)

CREATE TABLE IF NOT EXISTS RESULT (
	PRIMARY KEY resultDt VARCHAR(8)
	PRIMARY KEY resultId INTEAGER
	taskId INTEGER
	planId INTEGER
	content TEXT
	created_at TIMESTAMP
	updated_at TIMESTAMP
)

CREATE TABLE IF NOT EXISTS TAG (
	PRIMARY KEY tagId INTEAGER
	tagName TEXT
	type TEXT
	taskId INTEGER
	created_at TIMESTAMP
	updated_at TIMESTAMP
)
```

[https://www.sqlite.org/datatypes.html](https://www.sqlite.org/datatypes.html)

| Expression Affinity | Column Declared Type |
| --- | --- |
| TEXT | "TEXT" |
| NUMERIC | "NUM" |
| INTEGER | "INT" |
| REAL | "REAL" |
| BLOB (a.k.a "NONE") | "" (empty string) |

### 2.2. DB CRUD API

### 2.3. UI & UX

[https://aedi.tistory.com/117](https://aedi.tistory.com/117)

[http://kyleyiweb.blogspot.com/2013/04/table-div-table.html](http://kyleyiweb.blogspot.com/2013/04/table-div-table.html)

## 3. Export

[https://nrogap.medium.com/generate-excel-with-node-js-b779898e5d0](https://nrogap.medium.com/generate-excel-with-node-js-b779898e5d0)

## 4. Import