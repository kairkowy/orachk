### ORAchk 유틸리티 소개 및 사용법

#### 1.개요

ORAchk는 오라클 데이터베이스 소프트웨어 및 하드웨어 컴포넌트 요소에 대한 상태점검 프레임워크로써 알려진 문제에 대한 스캔 및 보고서 출력을 제공하는 유틸리티이다.

점검 대상

- 오라클 데이터베이스
- 오라클 그리드 인프라스트럭처
- 오라클 RAC
- MAA 검증
- 업그레이드 준비 검증 등

#### 2.설치전 확인, 준비 사항

##### 설치 및 실행 계정
오라클 클러스터웨어가 설치된 경우에는 Oracle Grid Infrastructure Home 설치 유저(Owner)로 설치, 실행하면된다.
오라클 클러스터웨어를 사용하지 않는 경우는 Root 계정(오라클 권고)으로 설치 또는 오라클 데이터베이스 홈 유저(owner) 계정으로 설치하면 된다.

##### SSH 접속 허용

RAC 환경인 경우 모든 노드의 헬스 체크를 위한 파일 복사와 실행이 원격으로 실행되기 때문에 모든 노드의 SSH 접속, 실행이 가능해야 한다.
SSH가 불가능할 경우에는 RAC 노드를 포함한 일부 명령 실행이 제한되어 실행된 부분만 결과 보고서를 얻을 수 있다.

##### root 패스워드
ORAchk가 실행될때 root 패스워드를 필요로 한다. ORAchk 실행때 프롬프트에 맞게 각 노드의 root 계정의 패스워드를 입력해주면 된다.

##### 스토리지 용량
소프트웨어 패키지는 수MB, 보고서는 건단 4MB 정도의 스토리지가 필요하다. 패키지와 보고서 보관을 고려하여 수GB 용량이면 충분하다.

##### 파이썬 패키지
이 유틸리티는 Python 코드로 제작 되어 있다. 따라서 Python 3.6 이상의 패키지 및 러이브러리를 필요로한다.

#### 3.설치 방법

##### 최신 버전 다운로드
ORAchk 유틸리티는 오라클 소프트웨어 설치 패키지에 포함되어 있고 오라클 소프트웨어 설치 떄 자동 설치된다. 초ㅚ신 버전의 ORAchk은 Oracle software 사이트에서 다운로드 사용을 권장한다. 오라클 12.1 버전 이후에는 Autonomous Health Framework(AHF) 패키지에 ORAchk 유틸리티를 포함시켜 제공된다.
RPM으로도 설치를 할 수 있으니 상세한 방법은 오라클 소프트웨어 패키지에 포함되어 있는 User Guide를 참고바란다.

##### ORAchk 위치
GRID 소프트웨어 설치가 안된 경우는 $ORACLE_HOME/suptool에 위치한다.

```shell
[racdb1:/u01/app/oracle/product/19c/db_1/suptools/orachk]>ls
Apex5_CollectionManager_App.sql   doc          rules.dat
CollectionManager_App.sql         exadiscover  sample_user_defined_checks.xml
ORAchk_Health_Check_Catalog.html  lib          templates
UserGuide.txt                     orachk       user_defined_checks.xsd
bash                              orachk.bat   web
build                             orachk.pyc
collections.dat                   readme.txt
```
GRID 소프트웨어 설치된 경우는 $GRID_HOME/suptools에 위치한다.
```shell
[racdb1:/u01/app/19c/grid/suptools/orachk]>ls
Apex5_CollectionManager_App.sql   build            orachk      sample_user_defined_checks.xml
CollectionManager_App.sql         collections.dat  orachk.bat  templates
ORAchk_Health_Check_Catalog.html  doc              orachk.pyc  user_defined_checks.xsd
UserGuide.txt                     exadiscover      readme.txt  web
bash                             lib              rules.dat
```

#### 4.실행

ORAchk 실행은 실행 파일 위치에서 ./orachk 를 실행한다. 실행 동안에 실행 환경 및 root 패스워드와 관련한 질문이 있다.
세부 내용은 아래 샘플 내용을 참고바란다.

```shell
racdb1:/u01/app/19c/grid/suptools/orachk]>./orachk

Running orachk
----------------------------------------------------------
PATH                             : /u01/app/oracle/product/19c/db_1/suptools/orachk
VERSION                          : 18.4.0_20181129
COLLECTIONS DATA LOCATION        : /u01/app/oracle/orachk/
----------------------------------------------------------

Clusterware stack is running from /u01/app/19c/grid. Is this the correct Clusterware Home?[y/n][y] y

Checking ssh user equivalency settings on all nodes in cluster for oracle

Node rac2 is configured for ssh user equivalency for oracle user

Searching for running databases . . . . .

.  .
List of running databases registered in OCR

1. racdb
2. None of above

Select databases from list for checking best practices. For multiple databases, select 1 for All or comma separated number like 1,2 etc [1-2][1]. 1
.  .  .  .
.  .  .

Checking Status of Oracle Software Stack - Clusterware, ASM, RDBMS

.  .  . . . .
.  .  . . . .  .  .  .  .  .  .  .  .  . . . .  .  .  .  .  .  .  .
-------------------------------------------------------------------------------------------------------
                                                 Oracle Stack Status
-------------------------------------------------------------------------------------------------------
  Host Name       CRS Installed  RDBMS Installed    CRS UP    ASM UP  RDBMS UP    DB Instance Name
-------------------------------------------------------------------------------------------------------
       rac1                       Yes          Yes          Yes      Yes      Yes              racdb1
       rac2                       Yes          Yes          Yes      Yes      Yes              racdb2
-------------------------------------------------------------------------------------------------------


Copying plug-ins

. .
.  .  .  .  .  .


188 of the included audit checks require root privileged data collection . If sudo is not configured or the root password is not available, audit checks which require root privileged data collection can be skipped.

1. Enter 1 if you will enter root password for each  host when prompted

2. Enter 2 if you have sudo configured for oracle user to execute root_orachk.sh script

3. Enter 3 to skip the root privileged collections

4. Enter 4 to exit and work with the SA to configure sudo  or to arrange for root access and run the tool later.

Please indicate your selection from one of the above options for root access[1-4][1]:-  1


Is root password same on all Database nodes?[y/n][y] n

.


Enter root password  rac1 :-
Verifying root password ...
.


Enter root password  rac2 :-
Verifying root password ...


*** Checking Best Practice Recommendations ( PASS / WARNING / FAIL ) ***

.

Collections and audit checks log file is
/u01/app/oracle/orachk/orachk_rac1_racdb_071724_093311/log/orachk.log

Starting to run orachk in background on rac2

============================================================
                   Node name - rac1
============================================================

 Collecting - ASM Disk Groups
 Collecting - ASM Disk I/O stats
 Collecting - ASM Diskgroup Attributes
 Collecting - ASM disk partnership imbalance
 Collecting - ASM diskgroup attributes
 Collecting - ASM diskgroup usable free space
 Collecting - ASM initialization parameters
 Collecting - Active sessions load balance for racdb database
 Collecting - Archived Destination Status for racdb database
 Collecting - Cluster Interconnect Config for racdb database
 Collecting - Database Archive Destinations for racdb database
 Collecting - Database Files for racdb database
 Collecting - Database Instance Settings for racdb database
 Collecting - Database Parameters for racdb database
 Collecting - Database Properties for racdb database
 Collecting - Database Registry for racdb database
 Collecting - Database Sequences for racdb database
 Collecting - Database Undocumented Parameters for racdb database
 Collecting - Database Undocumented Parameters for racdb database
 Collecting - Database Workload Services for racdb database
 Collecting - Dataguard Status for racdb database
 Collecting - Files not opened by ASM
 Collecting - Log Sequence Numbers for racdb database
 Collecting - Percentage of asm disk  Imbalance
 Collecting - Process for shipping Redo to standby for racdb database
 Collecting - RDBMS Feature Usage for racdb database
 Collecting - Redo Log information for racdb database
 Collecting - Standby redo log creation status before switchover for racdb database
 Collecting - /proc/cmdline
 Collecting - /proc/modules
 Collecting - CPU Information
 Collecting - CRS active version
 Collecting - CRS oifcfg
 Collecting - CRS software version
 Collecting - CSS Reboot time
 Collecting - CSS disktimout
 Collecting - Cluster interconnect (clusterware)
 Collecting - Clusterware OCR healthcheck
 Collecting - Clusterware Resource Status
 Collecting - Disk I/O Scheduler on Linux
 Collecting - DiskFree Information
 Collecting - DiskMount Information
 Collecting - Huge pages configuration
 Collecting - Interconnect network card speed
 Collecting - Kernel parameters
 Collecting - Linux module config.
 Collecting - Maximum number of semaphore sets on system
 Collecting - Maximum number of semaphores on system
 Collecting - Maximum number of semaphores per semaphore set
 Collecting - Memory Information
 Collecting - NUMA Configuration
 Collecting - Network Interface Configuration
 Collecting - Network Performance
 Collecting - Network Service Switch
 Collecting - OS Packages
 Collecting - OS version
 Collecting - Operating system release information and kernel version
 Collecting - Oracle Executable Attributes
 Collecting - Patches for Grid Infrastructure
 Collecting - Patches for RDBMS Home
 Collecting - Shared memory segments
 Collecting - Table of file system defaults
 Collecting - Voting disks (clusterware)
 Collecting - number of semaphore operations per semop system call

Preparing to run root privileged commands  rac1.


 Collecting - Broadcast Requirements for Networks
 Collecting - CRS Opatch version
 Collecting - CRS user time zone check
 Collecting - Custom rc init scripts (rc.local)
 Collecting - Disk Information
 Collecting - Grid Infastructure user shell limits configuration
 Collecting - Interconnect interface config
 Collecting - Network interface stats
 Collecting - ORAchk Daemon/Scheduler configuration
 Collecting - Root user limits
 Collecting - Verify no database server kernel out of memory errors
 Collecting - root time zone check
 Collecting - slabinfo
 Collecting - umask setting for GI owner


Data collections completed. Checking best practices on rac1.
------------------------------------------------------------

 INFO =>     Important Automatic Storage Management (ASM) Notes and Technical White Papers
 INFO =>     Oracle Data Pump Best practices.
 WARNING =>  Linux swap configuration does not meet recommendation
 WARNING =>  physical memory is not sufficient
 INFO =>     Important Storage Minimum Requirements for Grid & Database Homes
 INFO =>     Oracle asm filter driver is not loaded
 WARNING =>  There are some application objects with STALE statistics. for racdb
 WARNING =>  All the nodes not have active roles in a Flex Cluster.
 INFO =>     Hidden database initialization parameters should not be set per best practice recommendations for racdb
 INFO =>     Most recent ADR incidents for /u01/app/oracle/product/19c/db_1
 INFO =>     Oracle GoldenGate failure prevention best practices
 INFO =>     oracleasm (asmlib) module is loaded
 WARNING =>  Shell limit hard nofile for GI is NOT configured according to recommendation
 WARNING =>  Shell limit hard nproc for GI is NOT configured according to recommendation
 WARNING =>  OCR and OCR backup locations are the same path
 CRITICAL => The RMAN snapshot control file location is not shared on all database nodes in the cluster for racdb
 WARNING =>  OCR is not being backed up daily
 CRITICAL => ORAchk Daemon/Scheduler is not configured correctly
 WARNING =>  Some user sessions lack proper failover mode (BASIC) and method (SELECT) for racdb
 INFO =>     CSS misscount is not set to the default value of 30
 WARNING =>  All voting disks are not online
 INFO =>     Number of SCAN listeners is not equal to the recommended number of 3.
 CRITICAL => Operating system hugepages count does not satisfy total SGA requirements
 WARNING =>  NIC bonding is not configured for interconnect
 WARNING =>  NIC bonding is NOT configured for public network (VIP)
 WARNING =>  RAC interconnect network card speed does not meet recommendation
 WARNING =>  CSS reboot time is not set to the default value of 3
 INFO =>     Jumbo frames (MTU >= 9000) are not configured for interconnect
 WARNING =>  NTP is not running with correct setting
 WARNING =>  All disk groups should have compatible.rdbms attribute set to recommended values
 WARNING =>  All disk groups should have compatible.advm attribute set to recommended values
 FAIL =>     Database parameter DB_BLOCK_CHECKSUM is not set to recommended value on racdb1 instance
 FAIL =>     Database parameter DB_LOST_WRITE_PROTECT is not set to recommended value on racdb1 instance
 WARNING =>  Database parameter DB_BLOCK_CHECKING on PRIMARY is NOT set to the recommended value. for racdb
 FAIL =>     Flashback on PRIMARY is not configured for racdb
 INFO =>     Operational Best Practices
 INFO =>     Database Consolidation Best Practices
 INFO =>     Computer failure prevention best practices
 INFO =>     Data corruption prevention best practices
 INFO =>     Logical corruption prevention best practices
 INFO =>     Database/Cluster/Site failure prevention best practices
 INFO =>     Client failover operational best practices
 WARNING =>  fast_start_mttr_target should be greater than or equal to 300. on racdb1 instance
 INFO =>     Information about hanganalyze and systemstate dump
 WARNING =>  Duplicate objects were found in the SYS and SYSTEM schemas for racdb
 WARNING =>  Remote listener is not set to SCAN name for racdb
 FAIL =>     Table AUD$[FGA_LOG$] should use Automatic Segment Space Management for racdb
 INFO =>     Database failure prevention best practices
 FAIL =>     Primary database is not protected with Data Guard (standby database) for real-time data protection and availability for racdb
 WARNING =>  Local listener init parameter is not set to local node VIP for racdb
 INFO =>     Parallel Execution Health-Checks and Diagnostics Reports for racdb
 WARNING =>  vm.min_free_kbytes should be set as recommended.
 WARNING =>  One or more diskgroups from v$asm_diskgroups are not registered in clusterware registry
 FAIL =>     FRA space management problem file types are present without an RMAN backup completion within the last 7 days. for racdb
 INFO =>     Oracle recovery manager(rman) best practices
 WARNING =>  RMAN controlfile autobackup should be set to ON for racdb
 WARNING =>  Linux Disk I/O Scheduler should be configured to Deadline
 WARNING =>  Consider investigating changes to the schema objects such as DDLs or new object creation for racdb
 WARNING =>  Consider increasing the value of the session_cached_cursors database parameter for racdb
 WARNING =>  Consider investigating the frequency of SGA resize operations and take corrective action for racdb
Best Practice checking completed. Checking recommended patches on rac1
--------------------------------------------------------------------------------
Collecting patch inventory on CRS HOME /u01/app/19c/grid
Collecting patch inventory on ASM HOME /u01/app/19c/grid
Collecting patch inventory on ORACLE_HOME /u01/app/oracle/product/19c/db_1
--------------------------------------------------------------------------------
0 Recommended CRS patches for  from /u01/app/19c/grid on rac1
--------------------------------------------------------------------------------
Patch#   CRS  ASM    RDBMS RDBMS_HOME                              Patch-Description            
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
0 Recommended RDBMS patches for 190000 from /u01/app/oracle/product/19c/db_1 on rac1
--------------------------------------------------------------------------------
Patch#   RDBMS    ASM     type                Patch-Description
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
              Clusterware patches summary report
--------------------------------------------------------------------------------
Total patches  Applied on CRS Applied on RDBMS Applied on ASM
--------------------------------------------------------------------------------
0              0              0                0
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
              RDBMS homes patches summary report
--------------------------------------------------------------------------------
Total patches  Applied on RDBMS Applied on ASM ORACLE_HOME
--------------------------------------------------------------------------------
0              0              0                /u01/app/oracle/product/19c/db_1
--------------------------------------------------------------------------------

Copying results from rac2 and generating report. This might take a while. Be patient.

.
============================================================
                   Node name - rac2
============================================================

 Collecting - /proc/cmdline
 Collecting - /proc/modules
 Collecting - CPU Information
 Collecting - CRS active version
 Collecting - CRS oifcfg
 Collecting - CRS software version
 Collecting - Cluster interconnect (clusterware)
 Collecting - Disk I/O Scheduler on Linux
 Collecting - DiskFree Information
 Collecting - DiskMount Information
 Collecting - Huge pages configuration
 Collecting - Interconnect network card speed
 Collecting - Kernel parameters
 Collecting - Linux module config.
 Collecting - Maximum number of semaphore sets on system
 Collecting - Maximum number of semaphores on system
 Collecting - Maximum number of semaphores per semaphore set
 Collecting - Memory Information
 Collecting - NUMA Configuration
 Collecting - Network Interface Configuration
 Collecting - Network Performance
 Collecting - Network Service Switch
 Collecting - OS Packages
 Collecting - OS version
 Collecting - Operating system release information and kernel version
 Collecting - Oracle Executable Attributes
 Collecting - Patches for Grid Infrastructure
 Collecting - Patches for RDBMS Home
 Collecting - Shared memory segments
 Collecting - Table of file system defaults
 Collecting - number of semaphore operations per semop system call

Preparing to run root privileged commands  rac2.

 Collecting - Broadcast Requirements for Networks
 Collecting - CRS Opatch version
 Collecting - CRS user time zone check
 Collecting - Disk Information
 Collecting - Grid Infastructure user shell limits configuration
 Collecting - Interconnect interface config
 Collecting - Network interface stats
 Collecting - ORAchk Daemon/Scheduler configuration
 Collecting - Root user limits
 Collecting - Verify no database server kernel out of memory errors
 Collecting - root time zone check
 Collecting - slabinfo
 Collecting - umask setting for GI owner

Data collections completed. Checking best practices on rac2.
------------------------------------------------------------

 WARNING =>  Linux swap configuration does not meet recommendation
 WARNING =>  physical memory is not sufficient
 INFO =>     Important Storage Minimum Requirements for Grid & Database Homes
 INFO =>     Oracle asm filter driver is not loaded
 WARNING =>  All the nodes not have active roles in a Flex Cluster.
 FAIL =>     Hidden ASM initialization parameters should not be set per best practice recommendations
 INFO =>     Hidden database initialization parameters should not be set per best practice recommendations for racdb
 INFO =>     Most recent ADR incidents for /u01/app/oracle/product/19c/db_1
 INFO =>     Oracle GoldenGate failure prevention best practices
 INFO =>     oracleasm (asmlib) module is loaded
 WARNING =>  Shell limit hard nofile for GI is NOT configured according to recommendation
 WARNING =>  Shell limit hard nproc for GI is NOT configured according to recommendation
 FAIL =>     ASM parameter CLUSTER_INTERCONNECTS is not set to the recommended value
 WARNING =>  OCR and OCR backup locations are the same path
 CRITICAL => The RMAN snapshot control file location is not shared on all database nodes in the cluster for racdb
 INFO =>     Number of SCAN listeners is not equal to the recommended number of 3.
 CRITICAL => Operating system hugepages count does not satisfy total SGA requirements
 WARNING =>  NIC bonding is not configured for interconnect
 WARNING =>  NIC bonding is NOT configured for public network (VIP)
 WARNING =>  RAC interconnect network card speed does not meet recommendation
 INFO =>     Jumbo frames (MTU >= 9000) are not configured for interconnect
 WARNING =>  NTP is not running with correct setting
 FAIL =>     Database parameter DB_BLOCK_CHECKSUM is not set to recommended value on racdb2 instance
 FAIL =>     Database parameter DB_LOST_WRITE_PROTECT is not set to recommended value on racdb2 instance
 WARNING =>  Database parameter DB_BLOCK_CHECKING on PRIMARY is NOT set to the recommended value. for racdb
 WARNING =>  fast_start_mttr_target should be greater than or equal to 300. on racdb2 instance
 WARNING =>  Remote listener is not set to SCAN name for racdb
 WARNING =>  Local listener init parameter is not set to local node VIP for racdb
 WARNING =>  vm.min_free_kbytes should be set as recommended.
 WARNING =>  ASM processes parameter is not set to recommended value
 WARNING =>  Linux Disk I/O Scheduler should be configured to Deadline
Best Practice checking completed. Checking recommended patches on rac2
--------------------------------------------------------------------------------
Collecting patch inventory on CRS HOME /u01/app/19c/grid
Collecting patch inventory on ASM HOME /u01/app/19c/grid
Collecting patch inventory on ORACLE_HOME /u01/app/oracle/product/19c/db_1
--------------------------------------------------------------------------------
0 Recommended CRS patches for  from /u01/app/19c/grid on rac2
--------------------------------------------------------------------------------
Patch#   CRS  ASM    RDBMS RDBMS_HOME                              Patch-Description            
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
0 Recommended RDBMS patches for 190000 from /u01/app/oracle/product/19c/db_1 on rac2
--------------------------------------------------------------------------------
Patch#   RDBMS    ASM     type                Patch-Description
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
              Clusterware patches summary report
--------------------------------------------------------------------------------
Total patches  Applied on CRS Applied on RDBMS Applied on ASM
--------------------------------------------------------------------------------
0              0              0                0
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
              RDBMS homes patches summary report
--------------------------------------------------------------------------------
Total patches  Applied on RDBMS Applied on ASM ORACLE_HOME
--------------------------------------------------------------------------------
0              0              0                /u01/app/oracle/product/19c/db_1
--------------------------------------------------------------------------------


------------------------------------------------------------
                      CLUSTERWIDE CHECKS
------------------------------------------------------------

------------------------------------------------------------
Detailed report (html) -  /u01/app/oracle/orachk/orachk_rac1_racdb_071724_093311/orachk_rac1_racdb_071724_093311.html

UPLOAD [if required] - /u01/app/oracle/orachk/orachk_rac1_racdb_071724_093311.zip

```

#### 5.보고서

보고서는 $ORACLE_BASE/orachk 디렉토리에 출력된다. 디렉토리 구조를 가진 HTML 파일과 외부로 추출할 수 있는 zip 파일로 출력된다.
