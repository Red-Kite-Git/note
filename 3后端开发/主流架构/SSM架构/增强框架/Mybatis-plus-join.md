# Mybatis-plus-join

## 一、概念

MyBatis-plus-join 是一个基于 [MybatisPlus]() 的增强工具，它在 MyBatisPlus 的基础上再次进行了扩展和优化

## 二、创建项目（Maven项目）

### 1、引入依赖（pom.xml）

```xml
<!-- mybatis-plus-join 依赖 -->
<dependency>
    <groupId>com.github.yulichang</groupId>
    <artifactId>mybatis-plus-join-boot-starter</artifactId>
    <version>1.5.2</version>
</dependency>
```

### 2、添加配置信息（application.yaml）

依然使用MybatisPlus的配置信息

### 3、添加注解(实体类)

与MybatisPlus无异

### 4.创建MyBatisPlus的配置类

与MybatisPlus无异

### 5.创建Mapper接口与映射Mapper.xml文件

Mapper接口文件`extends MPJBaseJoin<T>`（联表的Mapper接口文件也需要）

>Tip：因为`MPJBaseJoin<T> extends BaseMapper<T>, JoinMapper<T>`所以MyBatisPlus的部分不受影响

```java
@Mapper
public interface Mapper extends MPJBaseMapper<SmbmsUser> {
	
}
```

Mapper.xml文件	与MybatisPlus无异

### 6.创建Service接口与ServiceImpl实现类

Service接口与实现类 与MybatisPlus无异

ServiceImpl实现类中联表查询时 ▼

```java
@Service("service")
public class ServiceImpl extends ServiceImpl<M extends BaseMapper<T>, T> implements SmbmsUserService {
	@Override
    public SmbmsBill getBillWithProPlus(Long id) {
        MPJLambdaWrapper<SmbmsBill> wrapper =
                //查询订单
                JoinWrappers.lambda(SmbmsBill.class)
                        //关联供应商
                        .selectAssociation(SmbmsProvider.class, SmbmsBill::getSmbmsProvider)
                        //结果集字段
                        .selectAll(SmbmsBill.class)
                        .selectAll(SmbmsProvider.class)
                        //联表方式和ON条件
                        .leftJoin(SmbmsProvider.class, SmbmsProvider::getId, SmbmsBill::getProviderid)
                        //WHERE条件
                        .eq(SmbmsBill::getId, id);
        
        return smbmsBillMapper.selectJoinOne(wrapper);
    }
}
```

### 7.创建Controller类

调用业务层方法实现新增操作
