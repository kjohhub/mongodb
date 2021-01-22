# Mongo DB를 활용한 NoSQL의 이해

## 	1. MongoDB 및 HBase의 개요
###	1.1 RDBMS & NoSQL

#### 1.1.1 NoSQL의 장점

* **클라우드 컴퓨팅 환경에 적합하다**
  - Open Source이다
  - 하드웨어 확장에 유연한 대처가 가능하다
  - RDBMS에 비해 저렴한 비용으로 분산처리와 병렬처리가 가능하다
* **유연한 데이터 모델이다.**
  * 비정형 데이터 구조 설계로 설계 비용 감소
  * 관계형 데이터 베이스의 Relation 과 Join 구조를 Linking과 Embedded로 구현하여 성능이 빠르다.
* **Big Data 처리에 효과적이다.**
  * Memory Mapping 기능을 통해 Read/Write가 빠르다
  * 전형적인 OS와 Hardware에 구축할 수 있다.
  * 기존 RDB와 동일하게 데이터 처리가 가능하다.

#### 1.1.2 NoSQL의 제품군

* **Key-Value Database** : Riak, Voldmort, Tokyo
* **BigTable Database** : `Hbase`, Casandra, Hypertable
* **Document Database** : `MongoDB`, CoughDB
* **Graph Database** : AllegroGraph, Sones



### 1.2 NoSQL 적용사례
* **Disney** : 다양한 Game, Media Data 관리 시스템에 적용
* **GYT** : 비디오/ 오디오 Content Mangement System에 적용
* **Forbes** : 원고 자동 수집 및 발생 시스템에 적용
* **Shutterfly** : 대용량 데이터의 트랜잭션 처리 비용, 성능 이슈
* **Foursquare** : GeoSpatial Index 기능 활용
* **National Archives** : 미국 국가 기록원, 콘텐츠 정보 관리  시스템에 적용



### 1.3 MongoDB & HBASE
* **HDFS (Hadoop Distriputed File System)**
  * `저비용 하드웨어`를 이용한 빅데이터의 효율적인 처리를 위한 `분산파일 시스템`
  * `아파치 너츠` 웹 검색엔진 프로젝트를 위한 하부구조로 만들어졌으며, `아파치 루씬`의 일부분인 `아파치 하둡` 프로젝트에 의해 시작
  * 수평적 확장을 통한 시스템의 가용성을 극대화시킬수 있으며, 이 기종간의 하드웨어와 소프트웨어 플랫폼의 `이식성이 뛰어남`
  * [Apache Project Information](https://projects.apache.org/project.html?hadoop)
* **Map Reduce**
  * 구글에서 `분산 컴퓨팅을 지원하기 위한 목적으로 제작`
  * 페타 데이터 이상의 `클러스터 환경에서 병렬처리 하기 위해 개발`되었으며 MAP과 REDUCE 함수로 구성
  * `Map 함수`를 통해 Input 데이터(Key, Value)를 `여러개의 작은 Split-Peace로 분산 입력`하고, `Reduce 함수`를 통해 중복 데이터를 제거한 후 사용자가 원하는 형태로 `데이터를 집계`
  * 구글 MapReduce를 기반으로 Hadoop MapReduce가 설계 되었으며, `Hadoop HDFS를 통해 수집 저장된 빅데이터의 효과적인 분석 처리` 위한 프로그램 모델을 의미



## 	2. MongoDB 설치및 데이터 처리

### 2.1 MongoDB

* **MongoDB란?**
  * `JSON Type`의 데이터 저장 구조를 제공
  * `Sharing(분산)/Replica(복제) 기능을 제공
  * MapReduce(분산/병렬처리) 기능을 제공
  * CRUD (Create, Read, Update, Delete) 위주의 다중 트랜잭션 처리도 가능
  * Memory Mapping 기술을 기반으로 Big Data 처리에 탁월한 성능을 제공
* 설치환경 및 지원 드라이버
  * 설치가능 플랫폼 :  Windows, Linux, Unix Solaris, Mac OS
  * 지원 언어 : C / C#/ C++, Java / Java Script, Perl /PHP/Python



### 2.2 MongoDB 설치와 환경 설정

* 다운로드 및 설치

  https://www.mongodb.com/
  
* 환경 설정

  C:\Program Files\MongoDB\Server\4.0\bin 추가
  
  ![image-20210122180938195](C:\Users\kjoh\AppData\Roaming\Typora\typora-user-images\image-20210122180938195.png)
  
  

### 2.3 MongoDB 시작과 종료

* 데몬 시작

  ```
  mkdir c:\mongodb\test				<- 디렉토리 생성
  mongod --dbpath c:\mongodb\test		<- 데몬 시작
  ```


* 서버 접속

  ```
  mongo localhost:27017
  ```

* 서버 종료

  ```
  use admin 							<- adimn DB로 이동
  db.shutdownServer() 				<- DB 종료
  exit
  ```

* Logout

  ```
  db.logout()
  ```


* Status

  ```
  db.stats()
  {
      "db" : "show",          <-DB명
      "collections" : 0,      <-컬렉션 수
      "views" : 0,            <-뷰어의 수
      "objects" : 0,          <-오브젝트 수
      "avgObjSize" : 0,       <-오브젝트의 평균 크기
      "dataSize" : 0,         <-데이터 크기
      "storageSize" : 0,      <-저장공간의 크기
      "numExtents" : 0,       <- 총 익스턴트 수
      "indexes" : 0,          <- 인덱스 수
      "indexSize" : 0,        <- 인덱스 크기
      "fsUsedSize" : 0,       <- 사용된 파일 크기
      "fsTotalSize" : 0,      <- 총 파일 크기
      "ok" : 1                <- 문장의 실행 상태(정상:1,  실패:0) 
  }
  ```

* host Info

  ```
  db.hostInfo()
  {
      "system" : {
      "currentTime" : ISODate("2021-01-22T04:22:15.935Z"),
      "hostname" : "DESKTOP-6OT4NIC",
      "cpuAddrSize" : 64,
      "memSizeMB" : NumberLong(3992),
      "memLimitMB" : NumberLong(3992),
      "numCores" : 4,
      "cpuArch" : "x86_64",
      "numaEnabled" : false
      },
      "os" : {
      "type" : "Windows",
      "name" : "Microsoft Windows 10",
      "version" : "10.0 (build 19041)"
      },
      "extra" : {
      "pageSize" : NumberLong(4096),
      "physicalCores" : 2
      },
      "ok" : 1
  }
  ```

  

### 2.4 MongoDB 데이터 처리

>RDBMS의 Table에 해당되는 데이터 구조를 컬렉션(Collection)이라고 표현
>
>Collection을 생성할때 구성요소(필드명, 데이터타입, 길이 등)가 결정되어 있지 않더라도 데이터 저장 구조를 생성
>
>**Non Capped Collection** : 디스크 공간이 허용하는 범위 내에서 데이터를 계속적으로 저장
>
>**Capped Collection** : 제한된 공간을 모두 사용하면 기존 공간을 재사용 (ex: 로그 데이터)

* Collection 생성

  ```
  > use SALES															<- SALES DB 생성 및 이동(첫 번째 컬렉션 생성 시 DB 자동 생성)
  > db.createCollecction(“employees”, {capped:true, size:100000})		<- Collection 생성
  > show collections
  > db.employees.stats() 												<- Collection의 현재 상태 및 정보 분석
  > db.employees.renameCollection(“emp”) 								<- 해당 Collection 이름 변경
  > db.emp.drop() 													<- 해당 Collection 삭제
  ```

* 데이터 입력

  ```
  > m={ename:"smith"}
  > n={empno:1001}
  > db.thing.save(m)
  > db.thing.save(n)
  > db.thing.insert({empno:1102, ename : "king"})
  ```

* 데이터 수정

  ```
  > db.thing.update({empno:1103}, {$set:{ename:"standford", dept:"research"}})
  > db.thing.update({empno:1104}, {$set:{ename:"John", dept:"inventory"}})
  > db.thing.update({empno:1105}, {$set:{enmae:"Joe", dept:"accounting"}})
  > db.thing.update({empno:1106}, {$set:{ename:"King", dept:"research"}})
  > db.thing.update({empno:1107}, {$set:{ename:"adams", dept:"personel"}})
  ```

* 데이터 삭제

  ```
  > db.thing.remove({ename:"King"})
  > db.thing.drop()			
  ```

* 데이터 조회

  ```
  > db.thing.find()
  ```







## 	3. MongoDB 인덱스와 관리

## 	4. MongoDB 튜닝과 백업/복구

## 	5. MongoDB 설계와 데이터 모델링









