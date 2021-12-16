# ������ 2: �������������� ������ �� Gradle

## ��������
��� ����������� ����������� ��������� ��������������, ��� ��������� �������. 

����������� �������������� ������ ����� ������������ �� �����������:

db - ������ ������ � ����� ������

api - ������ ������ � web

service - ���� ��������

## ����������

��� ������ �������� ������������� ����� - ��� ��� ������ �� �������.

����� ������� 3 ����� � ���������� �������: db, api, service.

� ������ �� ���������� ������� build.gradle c ����������:

```groovy
plugins {
    id 'java'
}

group 'ru.netology'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {

}
``` 

����� ���������� ����� ������ � �������, ��������� � ����� ������� � settings.gradle ����� ��������� ������:

```groovy
include 'db'
include 'service'
include 'api'
``` 

����� �������� ��������������� ������� ��������� ��������� ������ ����� �����.
 
��� ����������� ������ db � ������  service ������� �����������:

```groovy
dependencies {
    implementation project(":db")
}
```  

��� ����������� ������ service � ������ api ������� �����������:

```groovy
dependencies {
    implementation project(":db")
    implementation project(":service")
}
```

������ ����� ������� ������, �������� �������: 

```shell script
gradle build
``` 

��� ����������� ������ ����������� � ������� db �������� ������.

```java
public class DbSetting {

    private String name;
    private String password;

    public DbSetting(String name, String password) {
        this.name = name;
        this.password = password;
    }

}
```

```java
public class MyEntity {

    private UUID id;
    private String name;

    public MyEntity(String name) {
        this.name = name;
    }

    public UUID getId() {
        return id;
    }

    public void setId(UUID id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return new StringBuilder().append("MyEntity{").append("id=").append(id).append(", name='").append(name).append('\'').append('}').toString();
    }
}
```

```java
public class Db {

    private DbSetting dbSetting;
    private MyEntity myEntity;

    public Db(DbSetting dbSetting) {
        this.dbSetting = dbSetting;
        myEntity = new MyEntity("first");
        myEntity.setId(UUID.randomUUID());
    }

    public DbSetting getDbSetting() {
        return dbSetting;
    }

    public MyEntity getMyEntity() {
        return myEntity;
    }

    public void setMyEntity(MyEntity myEntity) {
        this.myEntity = myEntity;
    }
}
```

� ������� Service �������� �����:

```java
public class MyService {

    private DbSetting dbSetting = new DbSetting("name", "password");
    private String name = "myService";
    private Db db = new Db(dbSetting);

    public String getName() {
        return name;
    }

    public MyEntity setMyEntity(MyEntity myEntity) {
        myEntity.setId(UUID.randomUUID());
        db.setMyEntity(myEntity);
        return myEntity;
    }

    public MyEntity getMyEntity() {
        return db.getMyEntity();
    }
}
```

� ������� api �������� �����:

```java
public class Main {

    public static void main(String[] args) {
        MyService myService = new MyService();
        System.out.println(myService.getMyEntity());
        System.out.println(myService.setMyEntity(new MyEntity("second")));
        System.out.println(myService.getMyEntity());
    }
}
```

����� �������� ������ ������� ����� ��������� ������ ���. 

� ���������� ���������� ������� ������� �� �������� ��������� ������ � ����������� �� ����������������