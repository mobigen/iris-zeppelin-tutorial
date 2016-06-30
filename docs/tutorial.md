# IRIS with Zeppelin Tutorial

본 튜토리얼은 Zeppelin 노트북을 활용하여 `Spark on IRIS`와 `IRIS Interpreter`를 사용하는 방법에 대해 설명합니다.

## Zeppelin Interpreter 생성 방법
웹 브라우저를 통해 Zeppelin이 설치된 서버로 접속합니다. 접속이 완료되면 아래와 같은 메인 페이지가 보입니다.

![Zeppelin 메인 페이지](https://raw.githubusercontent.com/mobigen/iris-zeppelin-tutorial/master/docs/images/001.zeppelin_main.png)

상단의 Interpreter 탭으로 이동하여 원하는 추가 설정을 합니다.

Zeppelin 브라우저의 인터프리터 탭으로 이동 후, Create 버튼을 클릭합니다.
Name과 Interpreter를 선택한 후, Properties에 아래와 같은 형식으로 Property를 입력합니다.
(RPC 서버가 실행되고 있는 호스트의 아이피, 포트)

|name|value|
|---|---|
|zeppelin.iris.rpc.host|localhost|
|zeppelin.iris.rpc.port|4400|

![Zeppelin 인터프리터](https://raw.githubusercontent.com/mobigen/iris-zeppelin-tutorial/master/docs/images/002.zeppelin_interpreter.png)

## Spark on IRIS 사용 방법

상단의 Notebook 탭으로 이동하여 Spark on IRIS를 사용합니다.

![Zeppelin 노트북](https://raw.githubusercontent.com/mobigen/iris-zeppelin-tutorial/master/docs/images/003.zeppelin_notebook.png)

## 주의사항
**IrisContext를 이용해 생성한 데이터프레임을 `registerTempTable` 메소드로 등록할 경우, Zeppelin SQL 인터프리터(%sql)에서 테이블 명을 찾을 수 없습니다.** 만약 SQL 인터프리터를 사용해야 한다면 반드시 SQLContext를 이용해서 데이터프레임을 생성해야 합니다.

### 사용 가능 예
```
val df = sqlContext.read.format("com.mobigen.spark.iris").option("table", "nation").load()
df.registerTempTable("nation")
```
```
%sql
select * from nation
```

### 사용 주의 예
아래와 같이 IrisContext를 사용하여 데이터프레임을 `registerTempTable` 메소드로 등록할 경우, Zeppelin에서는 내부적으로 ZeppelinContext를 사용하므로, 테이블 명을 찾을 수 없습니다.
```
val irisContext = new IrisContext(sc)
val df = irisContext.iris("nation")
df.registerTempTable("nation")
```
```
%sql
select * from nation
```

## IRIS Interpreter 사용 방법
노트북에서 `%iris`를 붙인 후, 사용하고자 하는 iplus 명령어 혹은 쿼리를 수행합니다.