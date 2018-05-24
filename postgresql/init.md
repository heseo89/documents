# 설치
```
$ sudo apt-get install postgresql
$ sudo apt-get install pgadmin3
```

# 계정 생성
```
$ sudo su - postgres
$ psql -d postgres -U postgres

postgres=# create user hwaeunseo with password '$password';
CREATE ROLE

postgres=# create databas test;
CREATE DATABASE
```

#  외부 접속 허용
```
$ sudo vim /etc/postgresql/9.3/main/postgresql.conf
- # listen_address='localhost'
+ listen_address='*' //  외부 접속 모두 허용

$ sudo vim /etc/postgresql/9.3/main/pg_hba.conf
- host    all             all             127.0.0.1/32            md5
+ host    all             all             0.0.0.0/32              md5

$ sudo /etc/init.d/postgresql restart
```


## 명령어
1. 접속 방법
```
$ psql              // postgres DB에 postgres 롤로 접속
$ psql test         //  test 디비에 postgres 롤로 접속
$ psql -d test      //  test 디비에 postgres 롤로 접속
$ psql postgres -U username   // postgres DB 에 username 롤로 접속
$ psql -d postgres -U username    // postgres DB 에 username 롤로 접속
```

2. 기본 명령어
```
=# \q   종료(Ctrl + d)
=# \d   테이블, 인덱스, 시퀄스, 뷰 목록
=# \dt  컬럼 목록
=# \dS  시스템 테이블 목록
=# \dv  뷰 목록
=# \ds  시퀀스 목록
=# \dl  DB 목록
=# \du  롤(사용자) 목록
=# \dn  스키마 목록
=# \c {db_name}   다른 DB에 접속
=# \c {db_name} {username}  다른 DB에 username으로 접속
=# \db  테이블 스페이스 목록

=# \d                         /*테이블 목록 보기*/
=# \d {table_name}    /*지정된 테이블의 컬럼 목록 보기*/
=# \dS                      /*시스템테이블 목록 보기/
=# \dv                       /*뷰 목록 보기*/
=# \ds                      /*시퀀스 목록 보기*/
=# \l                         /*데이터베이스 목록 보기*/
=# \q                        /*PSQL 종료*/
=# \du                      /*롤 목록 보기*/
=# \dn                      /*스키마 목록 보기*/
```

![alt text](http://cfile25.uf.tistory.com/image/245DD64E51D66C9222CA50)
참고 : http://www.kim-taesuk.com/25
