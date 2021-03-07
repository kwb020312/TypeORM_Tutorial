### 참고

해당 저장소는 <a href="https://typeorm.io/#/">TypeORM</a>의 공식 홈페이지를 기반으로 작성되었음을 알립니다.

<img src="gitImages\TypeORM_Logo.png">

### TypeORM이 무엇인가?

TypeORM은 Query문을 대신 작성하여 데이터베이스 테이블을 생성하고 유지하기가 힘든 어려운 Query를 작성하지 않고도 빠르게 데이터를 수정, 삽입, 삭제 시켜주는 매우 간편한 라이브러리 이다.

### 설치방법

TypeORM을 전역으로 설치한다.

```javascript
npm install typeorm -g
```

예시로는 mysql을 사용하여 ProjectName이라는 프로젝트를 생성하는 예 이다.

```javascript
typeorm init --name ProjectName --database mysql
```

이제 해당 프로젝트로 이동한 후

```javascript
cd ProjectName
```

모든 라이브러리를 설치한다.

```javascript
npm install
```

설치가 진행되는 동안 json형식의 파일에 자신의 데이터베이스 정보를 넣어주는 것이 좋다.

```json
{
   "type": "mysql",
   "host": "localhost",
   "port": 3306,
   "username": "test",
   "password": "test",
   "database": "test",
   "synchronize": true,
   "logging": false,
   "entities": [
      "src/entity/**/*.ts"
   ],
   "migrations": [
      "src/migration/**/*.ts"
   ],
   "subscribers": [
      "src/subscriber/**/*.ts"
   ]
}
```

모두 진행되었다면 

```javascript
npm start
```

만약 express혹은 docker-compose구조의 프로젝트를 생성하고 싶다면

```javascript
typeorm init --name ProjectName --database mysql --express
```

```javascript
typeorm init --name ProjectName --database postgres --docker
```

로 만들어 줄 수 있다.