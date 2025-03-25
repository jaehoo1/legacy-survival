# iBatis → MyBatis
java SQL Mapper Framework 로는 iBatis와 MyBatis가 있다. [iBatis](ibatis.apache.org)는 2010년 이후 개발이 중단되었고, 이후 [MyBatis](mybatis.org)로 프로젝트가 이관되었다.  
레거시 프로젝트에서 iBatis를 사용하다 MyBatis로 변경하게 되었다.

AS-IS `pom.xml`
```xml
    ...
    <org.springframework-version>3.2.1.RELEASE</org.springframework-version>
    ...

    <!-- spring - iBatis -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-ibatis</artifactId>
        <version>2.0.8</version>
    </dependency>
```

TO-BE `pom.xml`
```xml
    ...
    <org.springframework-version>4.3.22.RELEASE</org.springframework-version>
    ...

    <!-- spring - MyBatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.19</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.2</version>    <!-- 프로젝트 환경(Spring 4)과 맞는 버전 사용 -->
    </dependency>
```

## 쿼리 전체 수정
iBatis든 MyBatis든 쿼리를 직접 .xml 매퍼 파일에 작성을 해야한다(SQL 주도 개발). 헌데, 제공되는 동적 SQL 요소 문법 자체가 다르기 때문에 전체 쿼리(mapper.xml)와 설정들을 수정해줘야 한다.  
(쿼리가 한 두개 되면 상관 없겠다만, 쿼리 매퍼의 수가 많다면 엄청난 작업량을 필요로한다..)
|<center>iBATIS</center>|<center>MyBatis</center>|
|---|---|
|`<!DOCTYPE sqlMap PUBLIC "-//ibatis.apache.org//DTD SQL Map 2.0//EN" "http://ibatis.apache.org/dtd/sql-map-2.dtd">`|`<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">`|
|`<sqlMap namespace="">`|`<mapper namespace="">`|
|`parameterClass`|`parameterType`|
|`resultClass`|`resultType`|
|`<isEqual>`, `<isNotEqual>`, `<isNull>`, `<isNotNull>`, `<isEmpty>`, `<isNotEmpty>`|`<if test=''>`|
|`<iterate property="" conjunction=""> #variable[]#`|`<foreach collection="" item="" separator=""> #{variable}`|
|`<dynamic prepend="WHERE">`, `<dynamic prepend="SET">`, `...`|`<where>`, `<set>`, `...`|
|`prepend="AND"`, `prepend="OR"`|쿼리에 직접 `AND`, `OR` 삽입|
|`removeFirstPrepend="true"`|`<where>`, `<set>`, `...` 에서 자동으로 처리|
|`#variable#`|`#{variable}`|
|`$variable$`|`${variable}`|

## ibatis2mybatis

## mybatis-2-spring