#SQL
 Structured Query Language 의 약자로 위키 백과 참조 ( 찡긋 )
 구조화 질의어, 관계형 데이터베이스 관리 시스템(RDBMS)의 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어이다. 
> 본 문서는 MYSQL을 기준으로 한다.

##데이터베이스 생성과 삭제 그리고 접속 
### 데이터 베이스 접속
터미널을 열어 다음 명령어를 이용해 접속한다.
```BASH
mysql -u {user name} -p {database}
```

> -p 는 password옵션을 뜻한다.

###데이터베이스 생성
```SQL
CREATE DATABASE {KUKEU} ;
```

###데이터베이스 삭제
```SQL
DROP DATABASE {KUKEU};
```

###데이터베이스 사용
```SQL
USE {KUKEU};
```

##기본적인 TABLE 생성과 삭제
### TABLE생성
```SQL
CREATE TABLE {KUKEU_TABLE} (
{column datatype,}
id integer NOT NULL,
taste varchar(50) NOT NULL,
box varchar(50),
PRIMARY KEY(id) 
);
```

###테이블 확인
```SQL
DESC {KUKEU};
```

###테이블 리스트 확인
```SQL
SHOW TABLES;
```


###테이블 삭제
```SQL
DROP TABLE {KUKEU};
```

###테이블 비워내기
```SQL
TRUNCATE {KUKEU};
```

##기본적인 CRUD Query
 
 