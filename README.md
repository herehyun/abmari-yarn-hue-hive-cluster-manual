<img width="869" alt="스크린샷 2023-07-13 오후 4 30 18" src="https://github.com/herehyun/manuals/assets/82385436/335f1fb5-2d16-4675-9bf0-b97a7b889397"># 목차

[1. Ambari](#1-ambari)
> [1.0. Ambari란](#10-ambari란)

> [1.1. Ambari UI 접속](#11-ambari-ui-접속)

> [1.2. Services - HDFS - Summary](#12-services-hdfs-summary)

> [1.3. Services - HDFS - config](#13-services-hdfs-config)

> [1.4. Services - YARN - SUMMARY](#14-services-yarn-summary)

[2. YARN](#2-yarn)
> [2.1. YARN의 역할](#21-yarn의-역할)

> [2.2. YARN의 구성 요소](#22-yarn의-구성-요소)

> [2.3. Services - YARN - Configs](#23-services-yarn-configs)

[3. Sqoop 사용](#3-sqoop-사용)
> [3.1. sqoop 명령어](#31-sqoop-명령어)

[4. HUE](#4hue)
> [4.0. HUE란](#40-hue란)

> [4.1. HUE 접속 계정](#41-hue-접속-계정)

> [4.2. HUE](#42-hue)

> [4.3. HUE권한 부여](#43-HUE권한-부여)

[5. pyspark job 제출](#5-pyspark-job-제출)
> [5.1. spark3-submit으로 pyspark3로 실행 할시 이점](#51-spark3-submit으로-pyspark3로-실행-할시-이점)

[6. Hive/Delta](#6-hive-delta)
> [6.1. DESCRIBE HISTORY](#61-describe-history)

> [6.2. CREATE DELTA DATA](#62-create-delta-data)

> [6.3. HUE에 있는 DELTA 데이터 읽어오기](#63-hue에-있는-delta-데이터-읽어오기)

> [6.4. WRITE TO A TABLE](#64-write-to-a-table)

>> [6.4.1. APPEND](#641-append)

>> [6.4.2. OVERWRITE](#642-overwrite)

>> [6.4.3. HUE에 있는 데이터 업데이트](#643-hue에-있는-데이터-업데이트

> [6.5. DELTA HISTORY로 과거 테이블로 복원하는 방법](#65-delta-history로-과거-테이블로-복원하는-방법)

> [6.6. TABLE SCHEMA 포맷 확인하는법](#66-table-schema-포맷-확인하는법)

> [6.7. CREATE TABLE 하는방법](#67-create-table-하는방법)

>> [6.7.1. PARTITION이 없는경우](#671-partition이-없는경우)

>> [6.7.2. PARTITION이 있는경우](#672-partition이-있는경우)

>> [6.7.3. COMMENT가 있는 경우](#673-comment가-있는-경우)

[7. spark jdbc 사용](#7-spark-jdbc-사용)

> [7.1 jdbc의 목적](#71-jdbc의-목적)

[8. 클러스터](#8-클러스터)

> [8.1 클러스터 접속 권한 부여](#81-클러스터-접속-권한-부여)

> [8.2 Thrift Server 확인](#82-thrift-server-확인)

> [8.3 GPU 사용량 확인하기](#83-gpu-사용량-확인-하기)

> [8.4 MariaDB 사용법](#84-mariadb-사용법)

> [8.5 GitLab](#85-gitlab)

> [8.6 ftp계정추가](#86-ftp계정-추가)


----
# 1. Ambari

```
URL : url정보
user : admin
password : admin
```
## 1.0. Ambari란
  Ambari는 Hadoop 클러스터에 대하여 프로비저닝, 모니터링, 그 외 다른 관리 작업을 수행 할 수 있는 프로젝트

## 1.1. Ambari UI 접속
  * Dashboard 탭
    + 설치된 서비스들의 상태를 지표 및 그래프로 확인할 수 있습니다.

<div align="center">
<img width="854" alt="스크린샷 2023-07-13 오후 12 32 54" src="https://github.com/herehyun/manuals/assets/82385436/48808bb7-b6db-439c-a10f-6b2429706620">
</div>

* YARN Memory : 노드별 사용 메모리 sum

  + 전체 노드 메모리사용량의 합을 나타냅니다. 현재는 4.0TB 가 MAXIMUM
 
  + 각 노드별 사용량 확인은

<div align="center">
<img width="847" alt="스크린샷 2023-07-13 오후 12 52 34" src="https://github.com/herehyun/manuals/assets/82385436/a8dc4130-2c16-4b3c-af71-523bd9593d4c">
</div>
<div>&darr;</div>
<div align="center">
<img width="847" alt="스크린샷 2023-07-13 오후 12 53 02" src="https://github.com/herehyun/manuals/assets/82385436/0e3d6a57-646e-4a11-a8d4-8059855e8d2e">
</div>
<div>&darr;</div>
<div align="center">
<img width="854" alt="스크린샷 2023-07-13 오후 12 36 32" src="https://github.com/herehyun/manuals/assets/82385436/0aeeea4a-7ada-4c55-8182-587ee990a503">
</div>

* HDFS Disk Usage : HDFS 디스크 사용량 sum

  + HDFS(Hadoop 분산 파일 시스템)에서 사용하는 디스크 공간의 양에 대한 정보를 제공하고 클러스터를 관리하고 데이터 저장 및 처리에 사용할 수 있는 디스크 공간이 충분한지 확인해줌
 
 ### Log 디스크 공간이 부족한 경우 조치사항
   1. log위치 ambari -> Airflow -> Advanced airflow-logging-site -> Base log floder : /var/log/airflow
   2. airflow log 삭제

**2-1.** 폴더 크기 : 

    du -sh /var/log/airflow

    [613M /var/log/airflow]

**2-2.** du -hd1 /var/log/airflow

    [5.5M    /var/log/airflow/dag_id=DAT_ST_TEST1

    32M      /var/log/airflow/dag_id=DAG_ST_TEST2

    12M      /var/log/airflow/dag_id=DAG_ST_TEST3

    ...

    ]

**2-3.** 전 노드 airflow log size 조사

  dn1 205M

  dn2 189M

  dn3 212M

  dn4 227M

  dn5 670M

  dn6 613M

  dn7 918M

  dn8 941M

  dn9 924M

  mn2 225G

**2-4.**기준 : domain로그는 수동 삭제, 대신 확인하는 쉘 생성

    mn2 직전 한달치만 남기고 다 삭제 - 한달에 한번 실행

    NGIOS_UTIL/airflowLogDelete.sh

    로그 폴더에서 60일 보다 오래된 모든 폴더 삭제.

    0 0 * * * sh /usr/hmg/2.2.1.0-552/airflow/dags/NGIOS_UTIL/airflowLogDelete.sh

  + 각 노드별 사용량 확인은 Hosts - Disk Usage 에 직접 들어가서 확인

<div align="center">
<img width="847" alt="스크린샷 2023-07-13 오후 12 52 04" src="https://github.com/herehyun/manuals/assets/82385436/00fc26ab-17f0-4054-9340-30a40a9bc6ad">
</div>

* CPU Usage : 노드별 CPU사용량 sum

  1. 클러스터 성능 모니터링 : 관리자는 클러스터에 있는 각 노드의 CPU 사용량을 모니터링하여 클러스터가 얼마나 효과적으로 데이터를 처리하고 작업을 실행하는지파악 가능.
  2. 병목 현상 식별 : CPU 사용량이 높은 노드를 식별함으로써 관리자는 병목 현상을 해결하고 성능개선 가능.

  + 각 노드별 사용량 확인은 Hosts - Summary - Host metric

 ----
 ## 1.2. Services - HDFS - Summary

   * hdfs 구성 및 로그, Config정보 관리


<div align="center">
<img width="847" alt="스크린샷 2023-07-13 오후 1 03 49" src="https://github.com/herehyun/manuals/assets/82385436/ecbf4f1d-5a69-42d2-a3ed-130669eb2ffb">
</div>

  * DATANODES : 각 노드 별 상태 확인 및 노드 HOST정보 연결
   
----
## 1.3. Services - HDFS - config

  * NameNode/DataNode Java Heap Size 설정(튜닝포인트 : sparkJob 성능 튜닝없이 부스팅 할 경우 사용)
    + DataNode Java Heap Size :
      1. 성능 향상 : Datanode Java 힙에 더 많은 메모리를 할당하면 메모리에서 더 많은 데이터를 처리할 수 있으므로 Datanode 프로세스의 성능을 잠재적으로 향상
      2. 안정성 향상 : Datanode 프로세스의 메모리가 부족하고 메모리 부족 오류가 발생하는 경우 Java 힙 크기를 늘리면 프로세스를 안정화하고 충돌을 방지.
      3. 리소스 할당 : 데이터노드 프로세스에 대한 Java 힙 크기를 조정하여 노드에서 리소스 할당을 최적화하고 데이터노드가 작업을 효과적으로 수행하기에 충분한 메모리를 갖도록 할 수 있음.
    + 너무 많은 메모리를 할당하면 가비지 수집 시간 증가 또는 다른 프로세스와의 메모리 경합과 같은 노드 또는 클러스터에서 다른 문제가 발생할 수 있으므로 Datanode프로세스에 대한 Java 힙 크기 설정은 주의해서 수행해야 함
    + 운영 할 시 긴급으로 heap size를 늘려서 속도를 개선 할 수는 있지만 위험성이 크므로 긴급시에만 사용.

<div align="center">
<img width="848" alt="스크린샷 2023-07-13 오후 1 18 26" src="https://github.com/herehyun/manuals/assets/82385436/20382b46-9a29-45c6-9c78-7be1ed825ca9">
</div>

----
## 1.4. Services - YARN - SUMMARY

# 2. YARN

## 2.1.YARN의 역할

1. <mark>리소스 관리</mark> : YARN은 Hadoop 클러스터에서 실행되는 다양한 어플리케이션에 CPU 및 메모리와 같은 리소스 할당.

2. <mark>작업 예약</mark> : YARN은 애플리케이션이 제출한 작업(예: MapReduce 작업, spark 작업 )이 클러스터 내의 적절한 노드에서 실행되도록 예약.

3. <mark>확장성</mark>: YARN을 사용하면 클러스터에 더 많은 노드를 추가하여 Hadoop이 수평으로 확장할 수 있으므로 더 많은 수의 애플리케이션과 더 큰 데이터 세트 처리 가능.

4. <mark>다중 처리 프레임워크 지원</mark>: MapReduce 처리 모델과 밀접하게 결합된 Hadoop 1.0과 달리 YARN은 다양한 데이터 처리 프레임워크
(예: Apache Spark, Apache Flink 및 Apache Tez)를 통합하여 Hadoop에서 실행할 수 있음.

5. <mark>내결함성</mark> : YARN은 클러스터에서 실행되는 애플리케이션 및 작업의 상태를 모니터링.
   작업이 실패하거나 노드가 응답하지 않는 경우 YARN은 다른 노드에서 작업을 다시 예약하여 오류가 나도 애플리케이션이 계속 실행되도록 할 수 있음.

## 2.2. YARN의 구성 요소

1. ResourceManager: 클러스터 리소스를 관리하고 애플리케이션을 예약하는 중앙 구성 요소.
   ResourceManager는 다양한 경쟁 응용 프로그램 간의 리소스 할당을 중재하고 리소스가 효율적으로 사용되도록 함.

2. NodeManager: Hadoop 클러스터의 각 작업자 노드에서 실행되는 노드별 에이전ㅌ.
   NodeManager는 노드에서 컨테이너(격리된 실행 환경)를 관리하고 리소스 사용량을 모니터링하며 노드의 상태를 ResourceManager에 보고하는 역할을 함.

* pyspark log 확인하는 방법:

<div align="center">
<img width="848" alt="스크린샷 2023-07-13 오후 1 38 57" src="https://github.com/herehyun/manuals/assets/82385436/9ec455b9-3d94-475b-9681-210d62c14133">
</div>
<div align="center">&darr;</div>
<div align="center">
<img width="848" alt="스크린샷 2023-07-13 오후 1 39 21" src="https://github.com/herehyun/manuals/assets/82385436/b5429f0c-4fa2-40fd-a3f3-dc0dd38fd96b">
</div>
<div align="center">&darr;</div>
<div align="center">
<img width="848" alt="스크린샷 2023-07-13 오후 1 39 58" src="https://github.com/herehyun/manuals/assets/82385436/d555dc88-4d4e-4f69-bd9f-9dc4d061fd88">
</div>

* hdfs log 용량 확인하는 방법
hdfs dfs -du -h /user/mbsadmin

* Memory Used : 현재 사용중인 메모리
* Memory Total : 전체 사용할 수 있는 메모리(현재 : 4.0TB)
* Memory Reserved : 사용 예정인 메모리 + 현재 사용중인 메모리 > 전체 사용할 수 있는 메모리 이면은 Reserved에서 대기하고 있음.
* Vcoresd Used : 현재 사용중인 코어 수
* Vcores Total : 전체 사용할 수 있는 코어 수(현재 : 576)
* Vcores Reserved : 사용 예정인 코어 수 + 현재 사용중인 코어 수 > 전체 사용할 수 있는 코어 수 이면 Reserved에서 대기 하고있음.

* Allocated CPU Vcores : 현재 할당된 코어 수
* Allocated Memory : 현재 할당된 메모리
* Reserved CPU Vcores : 사용 예정인 코어 수가 전체 사용할 수 있는 코어 수 보다 크면 Reserved에서 대기 하고 있음.
* Reserved Memory : 사용 예정인 메모리가 전체 사용할 수 있는 메모리 보다 크면 Reserved에서 대기 하고 있음.
* % of Cluster : 전체 4.0TB에서 얼마나 사용하는지 퍼센트로 나누어져서 표현.

<div align="center">
<img width="848" alt="스크린샷 2023-07-13 오후 3 49 50" src="https://github.com/herehyun/manuals/assets/82385436/b0ddaf5b-fe25-41be-8483-1e2badedf5b0">
</div>
<div align="center">
&darr;
</div>
<div align="center">
<img width="868" alt="스크린샷 2023-07-13 오후 3 50 21" src="https://github.com/herehyun/manuals/assets/82385436/c2bcc5d9-1d4b-48f8-ad25-54ee2275cd13">
</div>
<div align="center">
&darr;
</div>
<div align="center">
JOBS에서 최근 프로그램 실행 이력을 파악할 수 있고, 사용 하지 않는 세션은 죽여도 된다.
</div>

* yarn kill 방법 : yarn application -kill [application 명]
* ex) yarn application -kill application_123491123_1234
<!-- <div align="center">
UI에 들어가서 다양한 정보를 얻을 수 있음
</div> -->

<details markdown="1">
<summary>1. JOBS</summary>
1. JOBS
- spark 애플리케이션이 실행하는  모든 작업을 나타냄
- 각 Job은 일련의 Stage로 구성되어 있으며, Job에서 실행되는 Stage의 수와 진행 상황등을 모니터링 할 수 있음

  * <mark>로그 파일 확인</mark> : Spark 애플리케이션의 로그 파일을 확인하여 문제가 발생한 원인을 파악할 수 있음. 예를 들어, 오류 메시지나 예외가 발생한 경우 로그 파일에서 자세한 정보를 확인할 수 있음.
  * <mark>메모리 사용량 모니터링</mark> : 메모리 사용량이 너무 높으면 애플리케이션이 느려지거나 실패할 수 있음. 따라서, Spark UI에서 메모리 사용량을 모니터링 하여 애플리케이션의 성능을 개선할 수 있음
  * <mark>Executor 개수 조정</mark> : Executor의 개수를 조정하여 애플리케이션의 성능을 개선할 수 있음. Executor의 갯수가 너무 적으면 애플리케이션의 처리속도가 느려질수 있고, Executor의 갯수가 너무 많으면 클러스터의 부하가 커지면서 애플리케이션의 성능이 저하될 수 있음. 따라서, Executor의 개수를 적절하게 조정하여 애플리케이션의 성능을 최적화 할 수 있음
  * <mark>모니터링</mark> : Spark 애플리케이션에서는 GC(Garbage Collection)의 발생 빈도와 시간이 매우 중요합니다. GC가 너무 자주 발생하거나 GC시간이 오래걸리면 애플리케이션의 처리 속도가 느려지거나 실패 할 수 있음. 따라서, GC모니터링을 통해 애플리케이션의 성능을 개선할 수 있음.
  * <mark>캐시 사용 여부 확인</mark> : Spark에서는 RDD나 DataFrame등을 캐시하여 동일한 연산을 반복할 때 성능을 개선할 수 있음. 따라서, 캐시를 적절하게 사용하여 애플리케이션의 처리 속도를 개선할 수 있음. Spark UI에서 캐시 사용여부를 확인할 수 있음.
</details>

<detils markdown="1">
<summary>2. STAGES</summary>
2. STAGES
- Spark 애플리케이션이 실행하는 모든 Stage를 나타냄. 각 Stage는 RDD(Distributed Resilient Dataset)를 처리하고, 새로운 RDD를 생성하는 연산을 수행.
- Stage에서 수행되는 Task들은 동일한 데이터셋을 사용하여 동시에 실행. Stages 탭에서는 각 Stage의 ID, 상태, 수행시간, 메모리 사용량등의 정보를 제공

  * <mark>Stage의 종류 파악</mark> : Spark에서는 다양한 종류의 Stage가 있음. Shuffle Stage, Result Stage, Cache Stage등이 있음. 각각의 Stages는 다른 종류의 작업들을 처리하므로 Stage의 종류를 파악 가능

  * <mark>Stage의 수와 크기 파악</mark> : Spark UI에서는 Stage의 수와 크기를 확인 할 수있음. Stage의 수가 많고 크기가 큰 경우, 클러스터에서의 부하가 커져서 애플리케이션의 처리 속도가 느려질 수 있음. 따라서, Stage의 수와 크기를 파악하여 애플리케이션의 처리 속도를 최적화할 수 있음

  * <mark>Stage의 의존성 파악</mark> : Stage간에는 의존성이 있음. 즉, 이전 Stage의 결과를 다음 Stage에서 사용하는 경우가 많음. Stage 간의 의존성을 파악하여 애플리케이션의 처리 흐름을 이해할 수 있음.

  * <mark>Shuffle 수행 여부 파악</mark> : Shuffle은 데이터를 섞는 작업. Shuffle이 수행되는 경우, 클러스터에서의 부하가 커져서 애플리케이션의 처리 속도가 느려질 수 있음. 따라서 Shuffle이 수행되는 Stage를 파악하여 애플리케이션의 처리 속도를 개선할 수 있음.

  * <mark>Stage의 수행 시간 모니터링</mark> : Stage의 수행 시간이 너무 긴 경우, 애플리케이션의 처리속도가 느려질 수 있음. 따라서, Stage의 수행시간을 모니터링 하여 애플리케이션의 처리 속도를 개선할 수 있음. Spark UI에서 Stage의 수행시간을 확인할 수 있음
</details>

<details markdown="1">
<summary>3. STORAGE</summary>
3. STORAGE
- Spark 애플리케이션이 사용하는 모든 메모리 및 디스크 스토리지에 대한 정보를 제공. 이 탭에서는 RDD의 크기, 캐싱 여부, 메모리와 디스크 스토리지의 사용량 등을 확인. 또한, Storage Level을 설정하여 RDD를 어떻게 저장할지 결정할 수 있음.

  * <mark>스토리지 레벨 확인</mark> : Spark에서는 메모리와 디스크에 데이터를 저장. 메모리는 빠르지만 제한된 용량을 가지고 있고, 디스크는 용량은 더 크지만 느릴수 있음.

  * <mark>메모리 사용량 파악</mark> : Spark에서 메모리 사용량이 많아지면 성능이 떨어질 수 있음. STORAGE 섹션에서는 메모리 사용량에 대한 정보를 확인할 수 잇음. 메모리 사용량이 높으면, 스파크 애플리케이션의 성능 문제가 발생할 가능성이 높음

  * <mark>디스크 사용량 파악</mark> : 디스크는 메모리보다 용량이 크지만, 더 느림. STORAGE섹션에서 디스크 사용량에대한 정보를 확인할 수 있음. 디스크 사용량이 높으면, 디스크 I/O병목 현상이 발생할 가능성이 높으므로, 성능 개선을 위해 디스크 용량을 늘리는 등의 방법을 고려해야함.

  * <mark>데이터 파티셔닝 확인</mark> : Spark에서는 데이터를 파티션으로 나누어 처리. STORAGE 섹션에서 파티션 수와 각 파티션의 크기를 확인할 수 있음. 데이터 파티셔닝은 데이터 처리 성능에 매우 중요하므로, 파티션 수와 크기를 최적화하는 것이 중요합니다.

  * <mark>캐시 데이터 확인</mark>: Spark에서는 데이터를 캐시하여 재사용할 수 있음. STORAGE 섹션에서 캐시된 데이터 크기와 갯수를 확인할 수 있음. 캐시된 데이터가 많아지면 메모리 사용량이 늘어나므로, 캐시 데이터를 적절히 관리하여 메모리 사용량을 줄이는 것이 중요.
</details>

<details markdown="1">
<summary>4. ENVIRONMENT<</summary>
4. ENVIRONMNET
- Spark 애플리케이션의 환경정보를 제공. 이탭에서는 Spark의 설정 값, JVM 환경, 로그 파일등을 확인. Spark애플리케이션을 싱행하는 클러스터의 구성과 운영체제등의 정보도 확인 할 수 있음.

  * <mark> 자원 할당 및 사용량 파악</mark> : ENVIRONMENT 섹션에서는 애플리케이션이 사용할 수 있는 자원 및 할당 된 자원에 대한 정보를 제공. 메모리, CPU, 디스크 및 네트워크 등 자원 사용에 대한 정보를 파악하면 애플리케이션의 성능을 최적화할 수 잇음.

  * <mark>설정 값 확인</mark> : Spark의 환경 설정 값은 애플리케이션의 성능에 매우 중요. ENVIRONMNET 섹션에서 설정 값에 대한 정보를 확인가능. 설정 값이 잘못 구성되면 애플리케이션의 성능에 영향을 미치므로, 올바른 설정 값으로 구성하는 것이 중요.

  * <mark>라이브러리 및 의존성 확인</mark> : 애플리케이션의 성능에 영향을 미치는 라이브러리 및 의존성 정보를 확인할 수 있음. ENVIRONMNET 섹션에서는 사용되는 라이브러리 및 의존성에 대한 정보를 제공.

  * <mark>스파크 버전 확인</mark> : ENVIRONMENT 섹션에서는 Spark 버전 정보도 확인 가능.
  
  * <mark>JVM 설정 확인</mark> : JVM은 Spark 애플리케이션 실행에 매우 중요. ENVIRONMENT 섹션에서 JVM설정 값을 확인할 수 있음. JVM 설정 값을 최적화하면 애플리케이션의 성능을 최적화할 수 있음.
</details>

----

## 2.3. Services - YAN - Configs

  * <mark>ResourceManager가 각 노드별 메모리/CPU 및 컨테이너의 사이즈를 통제함</mark>
<img width="869" alt="스크린샷 2023-07-13 오후 4 30 18" src="https://github.com/herehyun/manuals/assets/82385436/8a16e6e8-a90d-4f7c-8319-0f84a24934ab">

  + Memory : ResourceManager로 각 노드 별 메모리를 설정/대시보드에서 보이는 전체 가용량을 늘링 경우 각 노드별 가용량의 sum을 증가시켜야함 (개인 노드별로는 설정 <mark>불가능</mark>)
    + 현재 전체 4.0TB로 세팅되어있음.

----
## 3. Sqoop 사용

* Sqoop : RDBMS(Relational Database Management System)과 Haddop 사이의 데이터 이전을 쉽게 할 수 있도록 지원

## 3.1. Sqoop 명령어
--> 마스터노드에서 실행
```
sqoop import --connect jdbc:oracle:thin:[ip주소] \
--username "gios_test" \
--passowrd "gios_test" \
--connection-manager "org.apache.sqoop.manager.OracleManager" \
--query "SELECT * FROM GIOS_TEST.TEST_TABLE WHERE DATE = 20230223 \
--target-dir /user/hive/warehouse/NGIOS_TEST.db/NEW_TABLE \
--delete-target-dir \
--fields-terminated-by '\007' \
--lines-terminated-by '\n' \
--input-escaped-by ',';
```

----

# 4. HUE

## 4.0. HUE란
  - 데이터 분석가, 개발자 및 명령줄 인터페이스에 익숙하지 않은 기타 사용자를 위한 그래픽 사용자 인터페이스(GUI)를 제공하는 오픈 소스 웹 기반 인터페이스
  - Hadoop내의 파일을 관리하는데 사용함

## 4.1. Hue 접속 계정

```
URL : url정보
user : mbsadmin
password : mbsadmin
```

## 4.2. <mark>HUE</mark>

  * <h2>접속 하는 방법</h2>
<div align="center">
<img width="835" alt="스크린샷 2023-07-13 오후 4 48 54" src="https://github.com/herehyun/manuals/assets/82385436/99b41da6-cb5f-4f9a-b637-9e90ddf5b75d">
</div>
<div align="center"> &darr; </div>
<div align="center">
<img width="851" alt="스크린샷 2023-07-13 오후 4 49 20" src="https://github.com/herehyun/manuals/assets/82385436/ad90dae5-edcf-4fa8-9113-ade2384883e0">
</div>
<div align="center"> &darr; </div>
<div align="center">
<h2>원하는 파일 위치로 접속</h2>
</div>
<div align="center"> & darr;</div>
<div align="center">
<img width="856" alt="스크린샷 2023-07-13 오후 4 50 09" src="https://github.com/herehyun/manuals/assets/82385436/9d380e68-b365-43c9-b23a-b23214ef0b16">
</div>

## 4.3. <mark>디렉토리 권한 부여 방법</mark>

- 디렉토리 권한 부여 방법
1. hdfs로 유저 변경(하둡 super user계정)
2. 권한 부여
   2-1. 단일 유저 권한 부여 : hadoop fs -setfacl -R -m user:{유저명}:rwx {권한 변경할 디렉토리 경로}

   2-2. 유저 그룹 권한 부여 : hadoop fs -setfacl -R -m group:{그룹명}:rwx {권한 변경할 디렉토리 경로}

   - 유저 속한 그룹 확인 방법: groups {유저명}

   - mbsadmin 및 dt 유저가 포함된 유저 그룹: mbs
3. 권한 부여 확인 : hadoop fs -getfacl {권한 변경한 디렉토리 경로}

----
# 5. pyspark job 제출

## 5.1. spark3-submit으로 pyspark3로 실행 할시 이점

<mark>spark-submit</mark> 명령어:

- spark-submit을 활용하면 대화형 shell에서 개발한 프로그램을 운영용 어플리케이션으로 전환가능.
- spark-submit명령은 어플리케이션 코드를 클러스터에 전송해 실행시키는 역할을 수행
- 전송된 어플리케이션은 종료되거나 에러가 발생할 때까지 실행.
- Spark-submit 실행시 옵션 지정을 통해 필요한 자원과 실행 방식을 지정 가능.

```
spark3-submit
--master yarn
--driver-memory 5G
--executor-memory 10G
--num-executor 10
--executor-cores 4
--conf "spark.kryoserializer.buffer.max=512m"
--conf "spark.dynamicAllocation.enabled=false"
--conf "spark.memory.storageFraction=01."
--conf "spark.memory.fraction=0.8" /usr/hmg/2.2.1.0-552/airflow/dags/main.py
--job PKG_TEST.PR_TEST
--conf /usr/hmg/2.2.1.0-552/airflow/dags/NGIOS_CONFIG/DAG_TEST/test_config.json
```

* --<mark>master yarn</mark> : 사용할 클러스터 관리자를 지정. Hadoop에서 일반적으로 사용되는 YARN으로 설정
* --<mark>driver-memory</mark> : Spark 애플리케이션의 기본 기능을 실행하는 드라이버 프로세스에 할당할 메모리 양을 지정. 실행자가 실행하는 작업을 조정하는 역할.
* --<mark>num-executors</mark> : 애플리케이션에 대해 실행할 실행기 프로세스 수를 지정. executor가 사용하는 총 메모리 양은 executor-memory에 num-executors를 곱하여 결정됨
* --<mark>executor-cores</mark> : 매개변수는 각 실행기에 할당할 CPU 코어 수를 지정. 기본적으로 각 실행기에는 하나의 코어가 할당되지만 워크로드가 여러 코어에서 병렬화될 수 있는 경우 이 매개변수를 사용하여 코어 수를 늘릴 수 있음.
* --<mark>conf "spark.kryoserializer.buffer.max=512m"</mark>: Kryo 직렬 변환기 버퍼의 최대 크기를 512MB로 구성. Kryo직렬 변환기는 spark에서 효율적인 처리 및 저장을 위해 객체와 데이터를 직렬화하는 데 사용.
* --<mark>conf "spark.dynamicAllocation.enabled=false"</mark> : 실행기 리소스의 동적 할당을 비활성화. 동적 할당을 통해 Spark는 워크로드에 따라 실행기 및 해당 리소스의 수를 조정할 수 있지만 경우에 따라 오버헤드 및 속도 저하가 발생할 수 있음.
*  --<mark>conf "spark.memory.storageFraction=0.1"</amrk>: 실행기 메모리의 저장 부분을 0.1로 설정. 이 비율은 캐시된 데이터 및 임시 셔플 파일을 저장하는데 사용되는 메모리 양을결정.
*  --<mark>conf "spark.memory.fraction=0.8"</mark>: Spark에 할당되는 총 메모리의 비율을 설정. 이예에서 메모리의 80%는 Spark에 할당되고 나머지 20%는 다른 프로세스에 할당.

----
# 6. Hive/Delta

## 6.1. DESCRIBE HISTORY

```
spark.sql(f""" DESCRIBE HISTORY delta.'/user/hive/warehouse/NGIOS_TEST.db/table_name`""").show(truncate=False)
```

## 6.2. CREATE DELTA DATA
```
data = spark.range(1,100)
data.write.format("delta").load("tmp/delta_table")
```

## 6.3. HUE에 있는 DELTA 데이터 읽어오기
```
df = spark.read.format("delta").load("tmp/delta_table")
df.show()
```

## 6.4. WRITE TO A TABLE

### 6.4.1. APPEND
```
df.write.format("delta").mode("append").save("tmp/delta_table2")
df.write.format("delta").mode("append").saveAsTable("ngios_test.delta_table")
```

### 6.4.2. OVERWRITE
```
df.write.format("delta").mode("overwrite").save("tmp/delta_table2")
df.write.format("delta").mode("overwrite").saveAsTable("ngios_test.delta_table")
```

### 6.4.3. HUE에 있는 데이터 업데이트
```
data = spark.range(5,10)
data.write.format("delta").mode("overwrite").save("tmp/delta_table")
```

## 6.5. DELTA HISTORY로 과거 테이블로 복원하는 방법

```
# >>> Define Variables
# - rest_tblnm : 테이블 명
# - rest_ver   : 복원할 version ID

rest_tblnm = "BK_TABLE"
rest_ver = 1

# >>> Select Version Data
df_restore = spark.read.format("delta").option("versaionAsOf", rest_ver).load("/user/hive/warehouse/NGIOS_TEST.db/" + rest_tblnm)

# df_restore.count()
# df_restore.show()

# >>> Create TempView
df_erstore.createOrReplaceTempView("R_"  + rest_tblnm)

# spark.sql(f""" select * from """ + "R_" + rest_tblnm).show()

# >>> Insert Overwrite
spark.sql("f""" insert overwrite NGIOS_TEST.""" + rest_tblnm + " select * from R_" + rest_tblnm)

spark.sql(f""" select * from NGIOS_TEST.""" + rest_tblnm).count()
```

<h3> &uarr; 필요한 version을 고르고 그 버전의 snapshot을 임시테이블로 만들고 그 snapshot만든 임시테이블로 기존 테이블을 overwrite하는 형식이다.</h3>

## 6.6. TABLE SCHEMA 포맷 확인하는법
```
spark.sql(f"" desc formatted NGIOS_TEST.TEST_TABLE""").show(100)
```

## 6.7 CREATE TABLE 하는 방법

### 6.7.1. PARTITON이 없는 경우
```
CREATE EXTERNAL TABLE IF NOT EXISTS NGIOS_TEST.TEST_TABLE ( NAME string,
  ID string,
  NO decimal(38,7),
  CREAETE_DTTM timestamp
) USING DELTA
LOCATION '/user/hive/warehouse/NGIOS_TEST.db/TEST_TABLE'
```

### 6.7.2. PATITION이 있는 경우
```
CREATE EXTERNAL TABLE IF NOT EXISTS NGIOS_TEST.TEST_TABLE ( NAME string,
  ID string,
  NO decimal(38,7),
  CREAETE_DTTM timestamp
) USING DELTA
LOCATION '/user/hive/warehouse/NGIOS_TEST.db/TEST_TABLE'
PARTITIONED BY (NAME)
```

### 6.7.3. COMMENT가 있는 경우
```
CREATE EXTERNAL TABLE IF NOT EXISTS NGIOS_TEST.TEST_TABLE (
  NAME string COMMENT "이름",
  ID string COMMENT "아이디",
  NO decimal(38,7) COMMENT "번호",
  CREAETE_DTTM timestamp COMMENT "생성시간"
) USING DELTA
LOCATION '/user/hive/warehouse/NGIOS_TEST.db/TEST_TABLE'
PARTIOTNED BY (NAME)
COMMENT "테스트 테이블"
```

* <mark>외부 테이블</mark> (<mark>External</mark> Table) : 외부 테이블(External Table)은 이미 HDFS에 존재하는 원본 데이터를 기반으로 테이블을 만들기 때문에 스키마만 정해주면 됨. 파일이 HDFS 상에 이미 있을 떄 외부 테이블을 사용하고, 테이블이 삭제 되더라도 파일은 남아 있음.
* <mark>내부 테이블</mark> (<mark>Internal</mark> or Managed Table) : 관리형 테이블을 생성하면 파일이 기본 위치인 /user/hive/warehouse/databasename.db/tablename/에 저장됨. 외부 테이블과는 다르게, 관리 테이블 또는 파티션이 삭제 (drop)되면 해당 테이블 또는 파티션과 연관된 데이터 및 메타 데이터가 삭제됨.


----
## 7. spark jdbc 사용

### 7.1 jdbcdml ahrwjr
* Spark JDBC의 목적은 Spark 애플리케이션이 JDBC(Java Database Connectivity) API를 사용하여 관계형 데이터베이스에서 데이터를 읽고 쓸 수 있도록 하는것
```
# ex) etl_conf.py
# _etl.py

# mysql
MYSQL_HOST = 'ip주소'
MYSQL_PORT = port주소
MYSQL_USER = 'sql_test'
MYSQL_PASSWoRD = 'sql_password'
MYSQL_DATABAES = 'sql_test'

# oracle
ORACLE_USER = 'oracle_test'
ORACLE_PASSWORD = 'oracle_paswd'
ORACLE_DSN = 'oracle주소'
```
# oacle spark read jdbc
# 1. select etl_old_table,partition_col from sql_test.etl_table_info

df = spark.read.format("jdbc")\
     .option("url", f"jdbc:oracle:thin:@{ORACLE_DSN}")\
     .option("user", ORACLE_USER)\
     .option("passowrd", ORACLE_PASSWORD)\
     .option("dbtable", "test_table")\
     .option("fetchsize", 100000).load()

     # 저장로직은 똑같음.
     .write
     .format("csv") #csv/elta/parquet
     .mode("overwrite") #overwrite/append
     .option("delimiter", "\007")
     .save("/user/hive/warehouse/NGIOS_TEST.db/TEST_TABLE")

     # oracle spark read jdbc

```

----
# 8. 클러스터
----
## 8.1 클러스터 접속 권한 부여































