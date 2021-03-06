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

### 모델 생성

데이터베이스의 기본 작업 중 하나는 테이블의 생성이라고 볼 수 있다.
만약 TypeORM에게 이 작업을 지시하려면 어떻게 해야할까??

방법은 NOSQL과 상당히 비슷한데 먼저 Class 모델을 정의해준다.

```javascript
export class Photo {
    id: number
    name: string
    description: string
    filename: string
    views: number
    isPublished: boolean
}
```

기존 TypeScript와도 호환이 되기 때문에 간편하게 모델을 생성할 수 있다. 즉 모델이 테이블이라 생각하면 편하다.

하지만 아직까지는 그저 Class일 뿐 엔티티가 아닌데, 이를 호환시켜주려면 간단한 데코레이터를 불러와야한다.

### 엔티티 생성

```javascript
import { Entity } from 'typeorm'

// @뒤에 오는 함수를 데코레이터 라고 말한다.
@Entity()
export class Photo {
    id: number
    name: string
    description: string
    filename: string
    views: number
    isPublished: boolean
}
```

### 열 추가하기

이제 테이블을 만들었기 때문에 해당 테이블에 열을 삽입하고 싶을 것이다.
이 때는 Column데코레이터 를 사용하면 된다.

```javascript
import { Entity, Column } from 'typeorm'

@Entity()
export class Photo {
    @Column()
    id: number

    @Column()
    name: string

    @Column()
    description: string

    @Column()
    filename: string

    @Column()
    views: number

    @Column()
    isPublished: boolean
}
```

하지만 현재 부족한 것은 여러 열을 생성했지만 데이터베이스 테이블에는 기본 키가 포함되어있는 열이 있어야한다.

### Primary열 만들기

기본 열을 생성할 경우 PrimaryColumn이라는 데코레이터를 사용해야한다.

```javascript
import { Entity, Column, PrimaryColumn } from 'typeorm'

@Entity()
export class Photo {
    
    @PrimaryColumn()
    id: number

    // ...
}
```

### 자동 증가 열 생성

MYSQL은 PrimaryKey에 한해서 Auto Increment기능이 기본 탑재되어 있다. 즉 열을 생성할 때 마다 중복되지않게 자동으로 증가하는 설정값을 말하는데, TypeORM에서는 따로 함수를 사용해주어야 자동증가 되는 기본키를 만들 수 있다.

이 때 사용되는 함수가 PrimaryGeneratedColumn함수이며 이를 사용하면 자동 증가되는 값을 만들 수 있다.

```javascript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm'

@Entity()
export class Photo {

    @PrimaryGeneratedColumn()
    id: number

    // ...
}
```