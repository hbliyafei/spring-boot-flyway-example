# flyway 自动构建 SQL


## 使用 `maven` 插件运行

### 1. 添加 `maven` 插件
```xml
<plugin>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-maven-plugin</artifactId>
    <version>5.2.4</version>
    <configuration>
        <driver>com.mysql.jdbc</driver>
        <url>jdbc:mysql://localhost:3306/flyway?createDatabaseIfNotExist=true</url>
        <user>root</user>
        <password>123456</password>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
    </dependencies>
</plugin>
```

### 2. 配置 `flyway` 参数

```properties
spring.flyway.enabled=true
spring.flyway.locations=classpath:/db/migration         ## .sql 文件保存路径
spring.flyway.schemas=flyway                            ## 连接的数据库
spring.flyway.baseline-on-migrate=true              
spring.flyway.sql-migration-prefix=V                    ## sql 文件的前缀[默认为V]
spring.flyway.sql-migration-separator=__                ## sql 文件连接符[默认__]（两个下划线）
spring.flyway.sql-migration-suffixes=.sql               ## sql 文件后缀[默认为 .sql]

```

### 3. 添加`sql` 文件
V1__Create_person_table.sql
```sql
create table PERSON (
    ID int not null,
    NAME varchar(100) not null
);

```
V2__Add_people.sql
```sql
insert into PERSON (ID, NAME) values (1, 'Axel');
insert into PERSON (ID, NAME) values (2, 'Mr. Foo');
insert into PERSON (ID, NAME) values (3, 'Ms. Bar');
```

### 运行 `flyway`
    mvn flyway:migrate
    
### 检查数据库
    `flyway_schema_history`
    `PERSON`
    表


## 启动项目时运行  `flyway`

### 1. 新建一个数据库[flyway]

### 2. 添加 `maven` Jar包
```xml
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
    <version>5.2.4</version>
</dependency>
```

### 3. 配置 `flyway` 参数
```properties
spring.flyway.enabled=true
spring.flyway.locations=classpath:/db/migration         ## .sql 文件保存路径
spring.flyway.schemas=flyway                            ## 连接的数据库
spring.flyway.baseline-on-migrate=true              
spring.flyway.sql-migration-prefix=V                    ## sql 文件的前缀[默认为V]
spring.flyway.sql-migration-separator=__                ## sql 文件连接符[默认__]（两个下划线）
spring.flyway.sql-migration-suffixes=.sql               ## sql 文件后缀[默认为 .sql]

```

### 4. 添加`sql` 文件
V1__Create_person_table.sql
```sql
create table PERSON (
    ID int not null,
    NAME varchar(100) not null
);

```
V2__Add_people.sql
```sql
insert into PERSON (ID, NAME) values (1, 'Axel');
insert into PERSON (ID, NAME) values (2, 'Mr. Foo');
insert into PERSON (ID, NAME) values (3, 'Ms. Bar');
```

### 5. 运行项目
    直接运行 Application.java run 方法
    
### 6.检查数据库
    `flyway_schema_history`
    `PERSON`
    表


maven plugs 的方式运行可以直接创建库，项目运行自动更新sql的必须先创建数据库才能使用。
