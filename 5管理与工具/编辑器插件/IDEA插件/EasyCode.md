# EasyCode

* 基于IntelliJ IDEA开发的代码生成插件，支持自定义任意模板（Java，html，js，xml）
* 只要是与数据库相关的代码都可以通过自定义模板来生成。支持数据库类型与java类型映射关系配置
* 支持同时生成生成多张表的代码。每张表有独立的配置信息。完全的个性化定义，规则由你设置

**文件  >  设置  >  其他设置  >  EasyCode**

▼ 导入模板  [EasyCodeConfig.json](..\..\resources\EasyCodeConfig.json) 

```json
{
  "author" : "makejava",
  "version" : "1.2.8",
  "userSecure" : "",
  "currTypeMapperGroupName" : "Default",
  "currTemplateGroupName" : "Default",
  "currColumnConfigGroupName" : "Default",
  "currGlobalConfigGroupName" : "Default",
  "typeMapper" : {
    "Default" : {
      "name" : "Default",
      "elementList" : [ {
        "columnType" : "varchar(\\(\\d+\\))?",
        "javaType" : "java.lang.String"
      }, {
        "columnType" : "char(\\(\\d+\\))?",
        "javaType" : "java.lang.String"
      }, {
        "columnType" : "text",
        "javaType" : "java.lang.String"
      }, {
        "columnType" : "decimal(\\(\\d+\\))?",
        "javaType" : "java.lang.Double"
      }, {
        "columnType" : "decimal(\\(\\d+,\\d+\\))?",
        "javaType" : "java.lang.Double"
      }, {
        "columnType" : "integer",
        "javaType" : "java.lang.Integer"
      }, {
        "columnType" : "int(\\(\\d+\\))?",
        "javaType" : "java.lang.Integer"
      }, {
        "columnType" : "bigint(\\(\\d+\\))?",
        "javaType" : "java.lang.Long"
      }, {
        "columnType" : "datetime",
        "javaType" : "java.util.Date"
      }, {
        "columnType" : "timestamp",
        "javaType" : "java.util.Date"
      }, {
        "columnType" : "boolean",
        "javaType" : "java.lang.Boolean"
      }, {
        "columnType" : "date",
        "javaType" : "java.util.Date"
      }, {
        "columnType" : "double",
        "javaType" : "java.lang.Double"
      }, {
        "columnType" : "tinyint(\\(\\d+\\))?",
        "javaType" : "java.lang.Short"
      }, {
        "columnType" : "double(\\(\\d+,\\d+\\))?",
        "javaType" : "java.lang.Double"
      }, {
        "columnType" : "bit(\\(\\d+\\))?",
        "javaType" : "java.lang.Short"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "time",
        "javaType" : "java.lang.String"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "int(10) unsigned zerofill",
        "javaType" : "java.lang.Integer"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "int(11) unsigned",
        "javaType" : "java.lang.String"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "bigint(20) unsigned",
        "javaType" : "java.lang.Long"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "smallint(6)",
        "javaType" : "java.lang.Short"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "json",
        "javaType" : "java.lang.String"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "int(10) unsigned",
        "javaType" : "java.lang.Integer"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "tinyint(3) unsigned",
        "javaType" : "java.lang.Integer"
      }, {
        "matchType" : "ORDINARY",
        "columnType" : "longtext",
        "javaType" : "java.lang.String"
      } ]
    }
  },
  "template" : {
    "MybatisPlus-Mixed" : {
      "name" : "MybatisPlus-Mixed",
      "elementList" : [ {
        "name" : "controller.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"Controller\")\n\n##保存文件（宏定义）\n#save(\"/controller\", \"Controller.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"controller\")\n\n##定义服务名\n#set($serviceName = $!tool.append($!tool.firstLowerCase($!tableInfo.name), \"Service\"))\n\n##定义实体对象名\n#set($entityName = $!tool.firstLowerCase($!tableInfo.name))\n\nimport com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;\nimport com.baomidou.mybatisplus.extension.api.ApiController;\nimport com.baomidou.mybatisplus.extension.api.R;\nimport com.baomidou.mybatisplus.extension.plugins.pagination.Page;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\nimport $!{tableInfo.savePackageName}.service.$!{tableInfo.name}Service;\nimport org.springframework.web.bind.annotation.*;\n\nimport javax.annotation.Resource;\nimport java.io.Serializable;\nimport java.util.List;\n\n##表注释（宏定义）\n#tableComment(\"表控制层\")\n@RestController\n@RequestMapping(\"$!tool.firstLowerCase($!tableInfo.name)\")\npublic class $!{tableName} extends ApiController {\n    /**\n     * 服务对象\n     */\n    @Resource\n    private $!{tableInfo.name}Service $!{serviceName};\n\n    /**\n     * 分页查询所有数据\n     *\n     * @param page 分页对象\n     * @param $!entityName 查询实体\n     * @return 所有数据\n     */\n    @GetMapping\n    public R selectAll(Page<$!tableInfo.name> page, $!tableInfo.name $!entityName) {\n        return success(this.$!{serviceName}.page(page, new QueryWrapper<>($!entityName)));\n    }\n\n    /**\n     * 通过主键查询单条数据\n     *\n     * @param id 主键\n     * @return 单条数据\n     */\n    @GetMapping(\"{id}\")\n    public R selectOne(@PathVariable Serializable id) {\n        return success(this.$!{serviceName}.getById(id));\n    }\n\n    /**\n     * 新增数据\n     *\n     * @param $!entityName 实体对象\n     * @return 新增结果\n     */\n    @PostMapping\n    public R insert(@RequestBody $!tableInfo.name $!entityName) {\n        return success(this.$!{serviceName}.save($!entityName));\n    }\n\n    /**\n     * 修改数据\n     *\n     * @param $!entityName 实体对象\n     * @return 修改结果\n     */\n    @PutMapping\n    public R update(@RequestBody $!tableInfo.name $!entityName) {\n        return success(this.$!{serviceName}.updateById($!entityName));\n    }\n\n    /**\n     * 删除数据\n     *\n     * @param idList 主键结合\n     * @return 删除结果\n     */\n    @DeleteMapping\n    public R delete(@RequestParam(\"idList\") List<Long> idList) {\n        return success(this.$!{serviceName}.removeByIds(idList));\n    }\n}\n"
      }, {
        "name" : "dao.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"Dao\")\n\n##保存文件（宏定义）\n#save(\"/dao\", \"Dao.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"dao\")\n\nimport java.util.List;\n\nimport com.baomidou.mybatisplus.core.mapper.BaseMapper;\nimport org.apache.ibatis.annotations.Param;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\n\n##表注释（宏定义）\n#tableComment(\"表数据库访问层\")\npublic interface $!{tableName} extends BaseMapper<$!tableInfo.name> {\n\n/**\n* 批量新增数据（MyBatis原生foreach方法）\n*\n* @param entities List<$!{tableInfo.name}> 实例对象列表\n* @return 影响行数\n*/\nint insertBatch(@Param(\"entities\") List<$!{tableInfo.name}> entities);\n\n/**\n* 批量新增或按主键更新数据（MyBatis原生foreach方法）\n*\n* @param entities List<$!{tableInfo.name}> 实例对象列表\n* @return 影响行数\n* @throws org.springframework.jdbc.BadSqlGrammarException 入参是空List的时候会抛SQL语句错误的异常，请自行校验入参\n*/\nint insertOrUpdateBatch(@Param(\"entities\") List<$!{tableInfo.name}> entities);\n\n}\n"
      }, {
        "name" : "entity.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##保存文件（宏定义）\n#save(\"/entity\", \".java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"entity\")\n\n##自动导入包（全局变量）\n$!autoImport\nimport com.baomidou.mybatisplus.extension.activerecord.Model;\nimport java.io.Serializable;\n\n##表注释（宏定义）\n#tableComment(\"表实体类\")\n@SuppressWarnings(\"serial\")\npublic class $!{tableInfo.name} extends Model<$!{tableInfo.name}> {\n#foreach($column in $tableInfo.fullColumn)\n    #if(${column.comment})//${column.comment}#end\n\n    private $!{tool.getClsNameByFullName($column.type)} $!{column.name};\n#end\n\n#foreach($column in $tableInfo.fullColumn)\n#getSetMethod($column)\n#end\n\n#foreach($column in $tableInfo.pkColumn)\n    /**\n     * 获取主键值\n     *\n     * @return 主键值\n     */\n    @Override\n    protected Serializable pkVal() {\n        return this.$!column.name;\n    }\n    #break\n#end\n}\n"
      }, {
        "name" : "mapper.xml.vm",
        "code" : "##引入mybatis支持\n$!{mybatisSupport.vm}\n\n##设置保存名称与保存位置\n$!callback.setFileName($tool.append($!{tableInfo.name}, \"Dao.xml\"))\n$!callback.setSavePath($tool.append($modulePath, \"/src/main/resources/mapper\"))\n\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n\n<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"$!{tableInfo.savePackageName}.dao.$!{tableInfo.name}Dao\">\n\n    <resultMap type=\"$!{tableInfo.savePackageName}.entity.$!{tableInfo.name}\" id=\"$!{tableInfo.name}Map\">\n#foreach($column in $tableInfo.fullColumn)\n        <result property=\"$!column.name\" column=\"$!column.obj.name\" jdbcType=\"$!column.ext.jdbcType\"/>\n#end\n    </resultMap>\n\n    <!-- 批量插入 -->\n    <insert id=\"insertBatch\" keyProperty=\"$!pk.name\" useGeneratedKeys=\"true\">\n        insert into $!{tableInfo.obj.parent.name}.$!{tableInfo.obj.name}(#foreach($column in $tableInfo.otherColumn)$!column.obj.name#if($velocityHasNext), #end#end)\n        values\n        <foreach collection=\"entities\" item=\"entity\" separator=\",\">\n        (#foreach($column in $tableInfo.otherColumn)#{entity.$!{column.name}}#if($velocityHasNext), #end#end)\n        </foreach>\n    </insert>\n    <!-- 批量插入或按主键更新 -->\n    <insert id=\"insertOrUpdateBatch\" keyProperty=\"$!pk.name\" useGeneratedKeys=\"true\">\n        insert into $!{tableInfo.obj.parent.name}.$!{tableInfo.obj.name}(#foreach($column in $tableInfo.otherColumn)$!column.obj.name#if($velocityHasNext), #end#end)\n        values\n        <foreach collection=\"entities\" item=\"entity\" separator=\",\">\n            (#foreach($column in $tableInfo.otherColumn)#{entity.$!{column.name}}#if($velocityHasNext), #end#end)\n        </foreach>\n        on duplicate key update\n         #foreach($column in $tableInfo.otherColumn)$!column.obj.name = values($!column.obj.name) #if($velocityHasNext), #end#end\n    </insert>\n\n</mapper>\n"
      }, {
        "name" : "service.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"Service\")\n\n##保存文件（宏定义）\n#save(\"/service\", \"Service.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"service\")\n\nimport com.baomidou.mybatisplus.extension.service.IService;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\n\n##表注释（宏定义）\n#tableComment(\"表服务接口\")\npublic interface $!{tableName} extends IService<$!tableInfo.name> {\n\n}\n"
      }, {
        "name" : "serviceImpl.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"ServiceImpl\")\n\n##保存文件（宏定义）\n#save(\"/service/impl\", \"ServiceImpl.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"service.impl\")\n\nimport com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;\nimport $!{tableInfo.savePackageName}.dao.$!{tableInfo.name}Dao;\nimport $!{tableInfo.savePackageName}.entity.$!{tableInfo.name};\nimport $!{tableInfo.savePackageName}.service.$!{tableInfo.name}Service;\nimport org.springframework.stereotype.Service;\n\n##表注释（宏定义）\n#tableComment(\"表服务实现类\")\n@Service(\"$!tool.firstLowerCase($tableInfo.name)Service\")\npublic class $!{tableName} extends ServiceImpl<$!{tableInfo.name}Dao, $!{tableInfo.name}> implements $!{tableInfo.name}Service {\n\n}\n"
      } ]
    },
    "MybatisPlus" : {
      "name" : "MybatisPlus",
      "elementList" : [ {
        "name" : "controller.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"Controller\")\n\n##保存文件（宏定义）\n#save(\"/controller\", \"Controller.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"controller\")\n\n##定义服务名\n#set($serviceName = $!tool.append($!tool.firstLowerCase($!tableInfo.name), \"Service\"))\n\n##定义实体对象名\n#set($entityName = $!tool.firstLowerCase($!tableInfo.name))\n\nimport com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;\nimport com.baomidou.mybatisplus.extension.api.ApiController;\nimport com.baomidou.mybatisplus.extension.api.R;\nimport com.baomidou.mybatisplus.extension.plugins.pagination.Page;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\nimport $!{tableInfo.savePackageName}.service.$!{tableInfo.name}Service;\nimport org.springframework.web.bind.annotation.*;\n\nimport javax.annotation.Resource;\nimport java.io.Serializable;\nimport java.util.List;\n\n##表注释（宏定义）\n#tableComment(\"表控制层\")\n@RestController\n@RequestMapping(\"$!tool.firstLowerCase($!tableInfo.name)\")\npublic class $!{tableName} extends ApiController {\n    /**\n     * 服务对象\n     */\n    @Resource\n    private $!{tableInfo.name}Service $!{serviceName};\n\n    /**\n     * 分页查询所有数据\n     *\n     * @param page 分页对象\n     * @param $!entityName 查询实体\n     * @return 所有数据\n     */\n    @GetMapping\n    public R selectAll(Page<$!tableInfo.name> page, $!tableInfo.name $!entityName) {\n        return success(this.$!{serviceName}.page(page, new QueryWrapper<>($!entityName)));\n    }\n\n    /**\n     * 通过主键查询单条数据\n     *\n     * @param id 主键\n     * @return 单条数据\n     */\n    @GetMapping(\"{id}\")\n    public R selectOne(@PathVariable Serializable id) {\n        return success(this.$!{serviceName}.getById(id));\n    }\n\n    /**\n     * 新增数据\n     *\n     * @param $!entityName 实体对象\n     * @return 新增结果\n     */\n    @PostMapping\n    public R insert(@RequestBody $!tableInfo.name $!entityName) {\n        return success(this.$!{serviceName}.save($!entityName));\n    }\n\n    /**\n     * 修改数据\n     *\n     * @param $!entityName 实体对象\n     * @return 修改结果\n     */\n    @PutMapping\n    public R update(@RequestBody $!tableInfo.name $!entityName) {\n        return success(this.$!{serviceName}.updateById($!entityName));\n    }\n\n    /**\n     * 删除数据\n     *\n     * @param idList 主键结合\n     * @return 删除结果\n     */\n    @DeleteMapping\n    public R delete(@RequestParam(\"idList\") List<Long> idList) {\n        return success(this.$!{serviceName}.removeByIds(idList));\n    }\n}\n"
      }, {
        "name" : "dao.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"Dao\")\n\n##保存文件（宏定义）\n#save(\"/dao\", \"Dao.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"dao\")\n\nimport com.baomidou.mybatisplus.core.mapper.BaseMapper;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\n\n##表注释（宏定义）\n#tableComment(\"表数据库访问层\")\npublic interface $!{tableName} extends BaseMapper<$!tableInfo.name> {\n\n}\n"
      }, {
        "name" : "entity.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##保存文件（宏定义）\n#save(\"/entity\", \".java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"entity\")\n\n##自动导入包（全局变量）\n$!{autoImport.vm}\nimport java.io.Serializable;\n\n##表注释（宏定义）\n#tableComment(\"表实体类\")\n@SuppressWarnings(\"serial\")\npublic class $!{tableInfo.name}{\n#foreach($column in $tableInfo.fullColumn)\n    #if(${column.comment})//${column.comment}#end\n\n    private $!{tool.getClsNameByFullName($column.type)} $!{column.name};\n#end\n\n#foreach($column in $tableInfo.fullColumn)\n#getSetMethod($column)\n#end\n\n}\n"
      }, {
        "name" : "service.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"Service\")\n\n##保存文件（宏定义）\n#save(\"/service\", \"Service.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"service\")\n\nimport com.baomidou.mybatisplus.extension.service.IService;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\n\n##表注释（宏定义）\n#tableComment(\"表服务接口\")\npublic interface $!{tableName} extends IService<$!tableInfo.name> {\n\n}\n"
      }, {
        "name" : "serviceImpl.java.vm",
        "code" : "##导入宏定义\n$!{define.vm}\n\n##设置表后缀（宏定义）\n#setTableSuffix(\"ServiceImpl\")\n\n##保存文件（宏定义）\n#save(\"/service/impl\", \"ServiceImpl.java\")\n\n##包路径（宏定义）\n#setPackageSuffix(\"service.impl\")\n\nimport com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;\nimport $!{tableInfo.savePackageName}.dao.$!{tableInfo.name}Dao;\nimport $!{tableInfo.savePackageName}.entity.$!{tableInfo.name};\nimport $!{tableInfo.savePackageName}.service.$!{tableInfo.name}Service;\nimport org.springframework.stereotype.Service;\n\n##表注释（宏定义）\n#tableComment(\"表服务实现类\")\n@Service(\"$!tool.firstLowerCase($tableInfo.name)Service\")\npublic class $!{tableName} extends ServiceImpl<$!{tableInfo.name}Dao, $!{tableInfo.name}> implements $!{tableInfo.name}Service {\n\n}\n"
      } ]
    },
    "spring-data-mongodb" : {
      "name" : "spring-data-mongodb",
      "elementList" : [ {
        "name" : "controller.java.vm",
        "code" : "##导入宏定义、设置包名、类名、文件名##导入宏定义\n$!{define.vm}\n#setPackageSuffix(\"controller\")\n#setTableSuffix(\"Controller\")\n#save(\"/controller\", \"Controller.java\")\n\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n##定义服务名\n#set($serviceSortType = $!tool.append($!tableInfo.name, \"Service\"))\n#set($serviceType = $!tool.append($!tableInfo.savePackageName, \".service.\", $serviceSortType))\n#set($serviceVarName = $!tool.firstLowerCase($serviceSortType))\n\n#set($entityShortType = $!tableInfo.name)\n#set($entityType = $!tableInfo.psiClassObj.getQualifiedName())\n#set($entityVarName = $!tool.firstLowerCase($!tableInfo.name))\n#set($pkType = $!pk.type)\n\nimport $pkType;\nimport $entityType;\nimport $serviceType;\nimport lombok.AllArgsConstructor;\nimport org.springframework.data.domain.Page;\nimport org.springframework.data.domain.Pageable;\nimport org.springframework.web.bind.annotation.GetMapping;\nimport org.springframework.web.bind.annotation.PostMapping;\nimport org.springframework.web.bind.annotation.RequestBody;\nimport org.springframework.web.bind.annotation.RequestMapping;\nimport org.springframework.web.bind.annotation.RestController;\n\n\n/**\n * $!{tableInfo.comment}控制层\n *\n * @author $!author\n * @since $!time.currTime()\n */\n@RestController\n@RequestMapping(\"/$!tool.firstLowerCase($!tableInfo.name)\")\n@AllArgsConstructor\npublic class $!{tableName} {\n\n\tprivate $serviceSortType $serviceVarName;\n\n\t/**\n\t * 获取$!{tableInfo.comment}列表(分页)\n\t */\n\t@GetMapping(\"/list\")\n\tpublic Page<$entityShortType> list(Pageable page) {\n\t\treturn null;\n\t}\n\n\t/**\n\t * 获取$!{tableInfo.comment}\n\t */\n\t@GetMapping(\"/get\")\n\tpublic $entityShortType get($!pk.shortType id) {\n\t\treturn ${serviceVarName}.findById(id);\n\t}\n\n\t/**\n\t * 添加$!{tableInfo.comment}\n\t */\n\t@PostMapping(\"/add\")\n\tpublic void add(@RequestBody $entityShortType $entityVarName) {\n\t\t${serviceVarName}.save($entityVarName);\n\t}\n\n\n\t/**\n\t * 修改$!{tableInfo.comment}\n\t */\n\t@PostMapping(\"/update\")\n\tpublic void update(@RequestBody $entityShortType $entityVarName) {\n\t\t${serviceVarName}.save($entityVarName);\n\t}\n\n\t/**\n\t * 删除$!{tableInfo.comment}\n\t */\n\t@PostMapping(\"/delete\")\n\tpublic void delete($!pk.shortType id) {\n\t\t${serviceVarName}.deleteById(id);\n\t}\n\n}\n"
      }, {
        "name" : "entity.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n\n##使用宏定义设置回调（保存位置与文件后缀）\n#save(\"/entity\", \".java\")\n\n##使用宏定义设置包后缀\n#setPackageSuffix(\"entity\")\n\n##使用全局变量实现默认包导入\n$!{autoImport.vm}\nimport java.io.Serializable;\n\n##使用宏定义实现类注释信息\n#tableComment(\"实体类\")\npublic class $!{tableInfo.name} implements Serializable {\n    private static final long serialVersionUID = $!tool.serial();\n#foreach($column in $tableInfo.fullColumn)\n    #if(${column.comment})/**\n     * ${column.comment}\n     */#end\n\n    private $!{tool.getClsNameByFullName($column.type)} $!{column.name};\n#end\n\n#foreach($column in $tableInfo.fullColumn)\n##使用宏定义实现get,set方法\n#getSetMethod($column)\n#end\n\n}\n"
      }, {
        "name" : "repository.java.vm",
        "code" : "##导入宏定义、设置包名、类名、文件名##导入宏定义\n$!{define.vm}\n#setPackageSuffix(\"repository\")\n#setTableSuffix(\"Repository\")\n#save(\"/repository\", \"Repository.java\")\n\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n##实体类名、主键类名\n#set($entityShortType = $!tableInfo.name)\n#set($entityType = $!tableInfo.psiClassObj.getQualifiedName())\n#set($pkShortType = $!pk.shortType)\n#set($pkType = $!pk.type)\n\nimport $pkType;\nimport $entityType;\nimport org.springframework.data.mongodb.repository.MongoRepository;\n\n\n/**\n * $!{tableInfo.comment}持久层\n *\n * @author $!author\n * @since $!time.currTime()\n */\npublic interface $!{tableName} extends MongoRepository<$entityShortType, $pkShortType> {\n\n}\n"
      }, {
        "name" : "service.java.vm",
        "code" : "##导入宏定义、设置包名、类名、文件名##导入宏定义\n$!{define.vm}\n#setPackageSuffix(\"service\")\n#setTableSuffix(\"Service\")\n#save(\"/service\", \"Service.java\")\n\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n##实体类名、主键类名\n#set($entityShortType = $!tableInfo.name)\n#set($entityType = $!tableInfo.psiClassObj.getQualifiedName())\n#set($entityVarName = $!tool.firstLowerCase($!tableInfo.name))\n#set($pkShortType = $!pk.shortType)\n#set($pkType = $!pk.type)\n\nimport $pkType;\nimport $entityType;\nimport org.springframework.data.domain.Page;\nimport org.springframework.data.domain.Pageable;\nimport java.util.Collection;\nimport java.util.List;\n\n\n/**\n * $!{tableInfo.comment}业务层\n *\n * @author $!author\n * @since $!time.currTime()\n */\npublic interface $!{tableName} {\n\n    void save($entityShortType $entityVarName);\n\n    void deleteById($pkShortType id);\n\n    $entityShortType findById($pkShortType id);\n\n    List<$entityShortType> findById(Collection<$pkShortType> ids);\n\n    Page<$entityShortType> list(Pageable page);\n\n}\n"
      }, {
        "name" : "serviceImpl.java.vm",
        "code" : "##导入宏定义、设置包名、类名、文件名\n$!{define.vm}\n#setPackageSuffix(\"service.impl\")\n#setTableSuffix(\"ServiceImpl\")\n#save(\"/service/impl\", \"ServiceImpl.java\")\n\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n##业务层类名、持久层类名、实体名\n#set($serviceSortType = $!tool.append($!tableInfo.name, \"Service\"))\n#set($serviceType = $!tool.append($!tableInfo.savePackageName, \".service.\", $serviceSortType))\n#set($repositorySortType = $!tool.append($!tableInfo.name, \"Repository\"))\n#set($repositoryType = $!tool.append($!tableInfo.savePackageName, \".repository.\", $repositorySortType))\n#set($repositoryVarName = $!tool.firstLowerCase($!repositorySortType))\n#set($entityShortType = $!tableInfo.name)\n#set($entityType = $!tableInfo.psiClassObj.getQualifiedName())\n#set($entityVarName = $!tool.firstLowerCase($!tableInfo.name))\n#set($pkShortType = $!pk.shortType)\n#set($pkType = $!pk.type)\n\nimport $pkType;\nimport $entityType;\nimport $serviceType;\nimport $repositoryType;\nimport org.springframework.stereotype.Service;\nimport javax.annotation.Resource;\nimport org.springframework.data.domain.Page;\nimport org.springframework.data.domain.Pageable;\nimport java.util.Collection;\nimport java.util.List;\nimport java.util.stream.Collectors;\nimport java.util.stream.StreamSupport;\n\n\n/**\n * $!{tableInfo.comment}业务层\n *\n * @author $!author\n * @since $!time.currTime()\n */\n@Service\npublic class $!{tableName} implements $!serviceSortType {\n\n\t@Resource\n    private $repositorySortType $repositoryVarName;\n\n    @Override\n    public void save($entityShortType $entityVarName) {\n        $!{repositoryVarName}.save($entityVarName);\n    }\n\n    @Override\n    public void deleteById($pkShortType id) {\n        $!{repositoryVarName}.delete(id);\n    }\n\n    @Override\n    public $entityShortType findById($pkShortType id) {\n        return $!{repositoryVarName}.findOne(id);\n    }\n\n    @Override\n    public List<$entityShortType> findById(Collection<$pkShortType> ids) {\n        Iterable<$entityShortType> iterable = $!{repositoryVarName}.findAll(ids);\n        return StreamSupport.stream(iterable.spliterator(), false)\n                .collect(Collectors.toList());\n    }\n\n    @Override\n    public Page<$entityShortType> list(Pageable page) {\n        return $!{repositoryVarName}.findAll(page);\n    }\n\n}\n"
      } ]
    },
    "Default" : {
      "name" : "Default",
      "elementList" : [ {
        "name" : "controller.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n##使用宏定义设置回调（保存位置与文件后缀）\n#save(\"/controller\", \"Controller.java\")\n##使用宏定义设置包后缀\n#setPackageSuffix(\"controller\")\n##定义服务名\n#set($serviceName = $!tool.append($!tool.firstLowerCase($!tableInfo.name), \"Service\"))\n##定义实体对象名\n#set($entityName = $!tool.firstLowerCase($!tableInfo.name))\nimport com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\nimport $!{tableInfo.savePackageName}.service.$!{tableInfo.name}Service;\nimport org.springframework.web.bind.annotation.*;\nimport javax.annotation.Resource;\nimport java.io.Serializable;\nimport java.util.List;\nimport io.swagger.annotations.Api;\nimport io.swagger.annotations.ApiOperation;\nimport io.swagger.annotations.ApiImplicitParam;\nimport io.swagger.annotations.ApiImplicitParams;\nimport org.springframework.beans.BeanUtils;\nimport org.springframework.validation.annotation.Validated;\nimport com.baomidou.mybatisplus.core.metadata.IPage;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\nimport com.baomidou.mybatisplus.extension.plugins.pagination.Page;\nimport lombok.extern.slf4j.Slf4j;\n##表注释（宏定义）\n\n@RestController\n@RequestMapping(\"/$!tool.firstLowerCase($!tableInfo.name)\")\n@Api#if(${tableInfo.comment})(tags=\"${tableInfo.comment}模块\")#end\n@Validated\n@Slf4j\n@CrossOrigin\n@SuppressWarnings(\"all\")\npublic class $!{tableInfo.name}Controller {\n  @Resource\n  private $!{tableInfo.name}Service $!{serviceName};\n  \n    @GetMapping(\"/list\")\n    @ApiOperation(value = \"查询所有数据\")\n    @CrossOrigin\n    public List<$!tableInfo.name> selectAll() {\n        return $!{serviceName}.list();\n    }\n  \n  @GetMapping(\"/page\")\n  @ApiOperation(value = \"分页查询数据\")\n  @ApiImplicitParams({\n            @ApiImplicitParam(name = \"currentPage\", value = \"当前页\", defaultValue = \"1\", dataType = \"int\", paramType = \"query\"),\n            @ApiImplicitParam(name = \"pageSize\", value = \"每页显示条数\", defaultValue = \"5\", dataType = \"int\", paramType = \"query\")\n            })\n  @CrossOrigin        \n  public IPage<$!tableInfo.name> selectPage(@RequestParam(defaultValue = \"1\") int currentPage,\n                                              @RequestParam(defaultValue = \"5\") int pageSize) {\n    IPage<$!tableInfo.name>  page=new Page(currentPage,pageSize);\n   \n    return $!{serviceName}.page(page);\n  }\n\n  @GetMapping(\"/detail/{id}\")\n  @CrossOrigin\n  @ApiOperation(value = \"查看详细信息\")\n  @ApiImplicitParam(name = \"id\",value = \"主键\",paramType = \"path\",dataType = \"string\")\n  public $!tableInfo.name selectOne(@PathVariable Serializable id) {\n    return this.$!{serviceName}.getById(id);\n  }\n\n  @DeleteMapping(\"/del/{id}\")\n  @ApiOperation(value = \"根据主键删除\")\n  @ApiImplicitParam(name = \"id\",value = \"主键\",paramType = \"path\",dataType = \"string\")\n  @CrossOrigin\n  public boolean delete(@PathVariable String id) {\n    return this.$!{serviceName}.removeById(id);\n  }  \n}"
      }, {
        "name" : "dao.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n##使用宏定义设置回调（保存位置与文件后缀）\n#save(\"/mapper\", \"mapper.java\")\n##使用宏定义设置包后缀\n#setPackageSuffix(\"mapper\")\n##定义初始变量\n#set($tableName = $tool.append($tableInfo.name, \"Mapper\"))\n##设置回调\n$!callback.setFileName($tool.append($tableName, \".java\"))\n$!callback.setSavePath($tool.append($tableInfo.savePath, \"/mapper\"))\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\nimport com.baomidou.mybatisplus.core.mapper.BaseMapper;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\nimport org.apache.ibatis.annotations.Mapper;\n##表注释（宏定义）\n\n@Mapper\n@SuppressWarnings(\"all\")\npublic interface $!{tableName} extends BaseMapper<$!tableInfo.name> {\n \n}"
      }, {
        "name" : "entity.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n##使用宏定义设置回调（保存位置与文件后缀）\n#save(\"/entity\", \".java\")\n##使用宏定义设置包后缀\n#setPackageSuffix(\"entity\")\n##使用全局变量实现默认包导入\n$!{autoImport.vm}\nimport java.io.Serializable;\nimport com.baomidou.mybatisplus.annotation.*;\nimport com.baomidou.mybatisplus.extension.activerecord.Model;\nimport lombok.*;\nimport com.baomidou.mybatisplus.annotation.IdType;\nimport com.baomidou.mybatisplus.annotation.TableId;\nimport com.fasterxml.jackson.annotation.JsonIgnore;\nimport com.fasterxml.jackson.annotation.JsonInclude;\nimport com.fasterxml.jackson.annotation.JsonFormat;\n##使用宏定义实现类注释信息\n\n@Setter\n@Getter\n@AllArgsConstructor\n@NoArgsConstructor\n@Builder\n@TableName(value = \"$tool.hump2Underline($!{tableInfo.name})\")\n@JsonInclude(value = JsonInclude.Include.ALWAYS)\n@SuppressWarnings(\"all\")\npublic class $!{tableInfo.name} extends Model<$!{tableInfo.name}> implements Serializable {\n  #foreach($column in $tableInfo.fullColumn)\n  \n  #if(${column.obj.name}=='id')\n  #if(${column.comment}) /**${column.comment}*/#end\n  @TableId(type = IdType.ASSIGN_ID)\n  private String id; \n  #end\n  \n  #if(${column.obj.name}=='creationDate')\n  #if(${column.obj.comment}) /**${column.obj.comment}*/#end\n  @TableField(value = \"creationDate\",fill = FieldFill.INSERT)\n  @JsonIgnore\n  private Date creationDate; \n  #end\n  \n  #if(${column.obj.name}=='modifyDate')\n  #if(${column.obj.comment}) /**${column.obj.comment}*/#end\n  @TableField(value = \"modifyDate\",fill = FieldFill.UPDATE)\n  @JsonIgnore\n  private Date modifyDate;\n  #end\n  \n  #if(${column.obj.name}=='version')\n  #if(${column.comment}) /**${column.comment}*/#end\n  @Version\n  @TableField(value = \"version\",fill = FieldFill.INSERT)\n  private int version;\n  #end\n  \n  #if(${column.obj.name}=='deleted')\n  #if(${column.comment}) /**${column.comment}*/#end\n  @TableLogic\n  @JsonIgnore\n  @TableField(fill = FieldFill.INSERT)\n  private int deleted;\n  #end\n  \n  #if(${column.type}=='java.util.Date' && ${column.obj.name}!='modifyDate' && ${column.obj.name}!='creationDate')\n  #if(${column.comment}) /**${column.comment}*/#end\n  @TableField(value = \"$!{column.obj.name}\")\n  @JsonFormat(pattern = \"yyyy-MM-dd HH:mm:ss\",timezone = \"GMT+8\")\n  private $!{tool.getClsNameByFullName($column.type)} $!{tool.getJavaName($column.obj.name)}; #if(${column.comment})//${column.comment}#end\n  #end\n  \n  \n  #if(${column.name}!='id'&& ${column.name}!='deleted' && ${column.obj.name}!='version' && ${column.obj.name}!='modifyDate' && ${column.obj.name}!='creationDate' && ${column.type}!='java.util.Date')\n  #if(${column.obj.comment}) /**${column.obj.comment}*/#end\n  @TableField(value = \"$!{column.obj.name}\")\n  private $!{tool.getClsNameByFullName($column.type)} $!{tool.getJavaName($column.obj.name)}; \n  #end\n#end\n\n}"
      }, {
        "name" : "mapper.xml.vm",
        "code" : "##引入mybatis支持\n$!{mybatisSupport.vm}\n##设置保存名称与保存位置\n$!callback.setFileName($tool.append($!{tableInfo.name}, \"Mapper.xml\"))\n$!callback.setSavePath($tool.append($modulePath,\"/src/main/resources/mapper\"))\n\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n\n<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE mapper PUBLIC \"-//mybatis.org//DTD Mapper 3.0//EN\" \"http://mybatis.org/dtd/mybatis-3-mapper.dtd\">\n<mapper namespace=\"$!{tableInfo.savePackageName}.mapper.$!{tableInfo.name}Mapper\">\n\n</mapper>"
      }, {
        "name" : "service.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n##使用宏定义设置回调（保存位置与文件后缀）\n#save(\"/service\", \"Service.java\")\n##定义初始变量\n#set($tableName = $tool.append($tableInfo.name, \"Service\"))\n##定义初始变量\n#set($tableName = $tool.append($tableInfo.name, \"Service\"))\n##设置回调\n$!callback.setFileName($tool.append($tableName, \".java\"))\n$!callback.setSavePath($tool.append($tableInfo.savePath, \"/service\"))\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n#if($tableInfo.savePackageName)package $!{tableInfo.savePackageName}.#{end}service;\nimport com.baomidou.mybatisplus.extension.service.IService;\nimport $!{tableInfo.savePackageName}.entity.$!tableInfo.name;\n\n\npublic interface $!{tableName} extends IService<$!tableInfo.name> {\n \n}"
      }, {
        "name" : "serviceImpl.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n##使用宏定义设置回调（保存位置与文件后缀）\n#save(\"/service\", \"ServiceImpl.java\")\n##定义初始变量\n#set($tableName = $tool.append($tableInfo.name, \"ServiceImpl\"))\n##设置回调\n$!callback.setFileName($tool.append($tableName, \".java\"))\n$!callback.setSavePath($tool.append($tableInfo.savePath, \"/service/impl\"))\n##拿到主键\n#if(!$tableInfo.pkColumn.isEmpty())\n    #set($pk = $tableInfo.pkColumn.get(0))\n#end\n#if($tableInfo.savePackageName)package $!{tableInfo.savePackageName}.#{end}service.impl;\nimport com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;\nimport $!{tableInfo.savePackageName}.mapper.$!{tableInfo.name}Mapper;\nimport $!{tableInfo.savePackageName}.entity.$!{tableInfo.name};\nimport $!{tableInfo.savePackageName}.service.$!{tableInfo.name}Service;\nimport org.springframework.stereotype.Service;\nimport lombok.extern.slf4j.Slf4j;\n\n\n@Slf4j\n@Service(\"$!tool.firstLowerCase($tableInfo.name)Service\")\n@SuppressWarnings(\"all\")\npublic class $!{tableName} extends ServiceImpl<$!{tableInfo.name}Mapper, $!{tableInfo.name}> implements $!{tableInfo.name}Service {\n \n}"
      }, {
        "name" : "addForm.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n##保存文件（宏定义）\n#save(\"/form\", \"AddForm.java\")\n##包路径（宏定义）\n#setPackageSuffix(\"form\")\n$!autoImport\nimport io.swagger.annotations.ApiModel;\nimport io.swagger.annotations.ApiModelProperty;\nimport lombok.*;\nimport java.io.Serializable;\n\n@Data\n@ApiModel\n@SuppressWarnings(\"all\")\npublic class $!{tableInfo.name}AddForm implements Serializable {\n#foreach($column in $tableInfo.fullColumn)\n\n\n  #if(${column.obj.name}!='version' &&  ${column.obj.name}!='id' && ${column.obj.name}!='deleted'  && ${column.obj.name}!='modifyDate' && ${column.obj.name}!='creationDate' && ${column.type}!='java.util.Date')\n   #if(${column.comment}) /** ${column.comment} */ #end\n   @ApiModelProperty(value = \"${column.comment}\",dataType = \"$column.type\")\n   private $!{tool.getClsNameByFullName($column.type)} $!{tool.getJavaName($column.obj.name)}; \n  #end\n#end\n\n}"
      }, {
        "name" : "updateForm.java.vm",
        "code" : "##引入宏定义\n$!{define.vm}\n##保存文件（宏定义）\n#save(\"/form\", \"UpdateForm.java\")\n##包路径（宏定义）\n#setPackageSuffix(\"form\")\n$!autoImport\nimport java.io.Serializable;\nimport com.fasterxml.jackson.annotation.JsonFormat;\nimport lombok.*;\nimport java.util.Date;\nimport io.swagger.annotations.ApiModel;\nimport io.swagger.annotations.ApiModelProperty;\nimport org.springframework.format.annotation.DateTimeFormat;\n\n@Data\n@ApiModel\n@SuppressWarnings(\"all\")\npublic class $!{tableInfo.name}UpdateForm implements Serializable {\n#foreach($column in $tableInfo.fullColumn)\n  #if(${column.name}=='id')\n  /** 主键 */\n  @ApiModelProperty(name = \"id\",value = \"主键\",dataType = \"java.lang.String\")\n  private String id; \n  #end\n \n  #if(${column.name}=='version')\n  /** 版本 */\n  @ApiModelProperty(name=\"version\",value = \"乐观锁\",dataType = \"int\")\n  private int version;\n  #end\n\n  #if(${column.type}=='java.util.Date' && ${column.obj.name}!='modifyDate' && ${column.obj.name}!='creationDate')\n  @JsonFormat(pattern = \"yyyy-MM-dd HH:mm:ss\",timezone=\"GMT+8\")\n  #if(${column.comment}) /** ${column.comment} */ #end\n  @ApiModelProperty(value = \"${column.comment}\",dataType = \"$column.type\")\n  private $!{tool.getClsNameByFullName($column.type)} $!{tool.getJavaName($column.obj.name)};\n  #end\n  \n  #if(${column.obj.name}!='version' &&  ${column.obj.name}!='id' && ${column.obj.name}!='deleted'  && ${column.obj.name}!='modifyDate' && ${column.obj.name}!='creationDate' && ${column.type}!='java.util.Date')\n   #if(${column.comment}) /** ${column.comment} */ #end\n   @ApiModelProperty(value = \"${column.comment}\",dataType = \"$column.type\")\n   private $!{tool.getClsNameByFullName($column.type)} $!{tool.getJavaName($column.obj.name)}; \n  #end\n#end\n\n}"
      } ]
    }
  },
  "columnConfig" : {
    "Default" : {
      "name" : "Default",
      "elementList" : [ {
        "title" : "disable",
        "type" : "BOOLEAN",
        "selectValue" : ""
      }, {
        "title" : "support",
        "type" : "SELECT",
        "selectValue" : "add,edit,query,del,ui"
      } ]
    }
  },
  "globalConfig" : {
    "Default" : {
      "name" : "Default",
      "elementList" : [ {
        "name" : "autoImport.vm",
        "value" : "##自动导入包（仅导入实体属性需要的包，通常用于实体类）\n#foreach($import in $importList)\nimport $!import;\n#end"
      }, {
        "name" : "define.vm",
        "value" : "##（Velocity宏定义）\n\n##定义设置表名后缀的宏定义，调用方式：#setTableSuffix(\"Test\")\n#macro(setTableSuffix $suffix)\n    #set($tableName = $!tool.append($tableInfo.name, $suffix))\n#end\n\n##定义设置包名后缀的宏定义，调用方式：#setPackageSuffix(\"Test\")\n#macro(setPackageSuffix $suffix)\n#if($suffix!=\"\")package #end#if($tableInfo.savePackageName!=\"\")$!{tableInfo.savePackageName}.#{end}$!suffix;\n#end\n\n##定义直接保存路径与文件名简化的宏定义，调用方式：#save(\"/entity\", \".java\")\n#macro(save $path $fileName)\n    $!callback.setSavePath($tool.append($tableInfo.savePath, $path))\n    $!callback.setFileName($tool.append($tableInfo.name, $fileName))\n#end\n\n##定义表注释的宏定义，调用方式：#tableComment(\"注释信息\")\n#macro(tableComment $desc)\n/**\n * $!{tableInfo.comment}($!{tableInfo.name})$desc\n *\n * @author $!author\n * @since $!time.currTime()\n */\n#end\n\n##定义GET，SET方法的宏定义，调用方式：#getSetMethod($column)\n#macro(getSetMethod $column)\n\n    public $!{tool.getClsNameByFullName($column.type)} get$!{tool.firstUpperCase($column.name)}() {\n        return $!{column.name};\n    }\n\n    public void set$!{tool.firstUpperCase($column.name)}($!{tool.getClsNameByFullName($column.type)} $!{column.name}) {\n        this.$!{column.name} = $!{column.name};\n    }\n#end"
      }, {
        "name" : "init.vm",
        "value" : "##初始化区域\n\n##去掉表的t_前缀\n$!tableInfo.setName($tool.getClassName($tableInfo.obj.name.replaceFirst(\"book_\",\"\")))\n\n##参考阿里巴巴开发手册，POJO 类中布尔类型的变量，都不要加 is 前缀，否则部分框架解析会引起序列化错误\n#foreach($column in $tableInfo.fullColumn)\n#if($column.name.startsWith(\"is\") && $column.type.equals(\"java.lang.Boolean\"))\n    $!column.setName($tool.firstLowerCase($column.name.substring(2)))\n#end\n#end\n\n##实现动态排除列\n#set($temp = $tool.newHashSet(\"testCreateTime\", \"otherColumn\"))\n#foreach($item in $temp)\n    #set($newList = $tool.newArrayList())\n    #foreach($column in $tableInfo.fullColumn)\n        #if($column.name!=$item)\n            ##带有反回值的方法调用时使用$tool.call来消除返回值\n            $tool.call($newList.add($column))\n        #end\n    #end\n    ##重新保存\n    $tableInfo.setFullColumn($newList)\n#end\n\n##对importList进行篡改\n#set($temp = $tool.newHashSet())\n#foreach($column in $tableInfo.fullColumn)\n    #if(!$column.type.startsWith(\"java.lang.\"))\n        ##带有反回值的方法调用时使用$tool.call来消除返回值\n        $tool.call($temp.add($column.type))\n    #end\n#end\n##覆盖\n#set($importList = $temp)"
      }, {
        "name" : "mybatisSupport.vm",
        "value" : "##针对Mybatis 进行支持，主要用于生成xml文件\n#foreach($column in $tableInfo.fullColumn)\n    ##储存列类型\n    $tool.call($column.ext.put(\"sqlType\", $tool.getField($column.obj.dataType, \"typeName\")))\n    #if($tool.newHashSet(\"java.lang.String\").contains($column.type))\n        #set($jdbcType=\"VARCHAR\")\n    #elseif($tool.newHashSet(\"java.lang.Boolean\", \"boolean\").contains($column.type))\n        #set($jdbcType=\"BOOLEAN\")\n    #elseif($tool.newHashSet(\"java.lang.Byte\", \"byte\").contains($column.type))\n        #set($jdbcType=\"BYTE\")\n    #elseif($tool.newHashSet(\"java.lang.Integer\", \"int\", \"java.lang.Short\", \"short\").contains($column.type))\n        #set($jdbcType=\"INTEGER\")\n    #elseif($tool.newHashSet(\"java.lang.Long\", \"long\").contains($column.type))\n        #set($jdbcType=\"INTEGER\")\n    #elseif($tool.newHashSet(\"java.lang.Float\", \"float\", \"java.lang.Double\", \"double\").contains($column.type))\n        #set($jdbcType=\"NUMERIC\")\n    #elseif($tool.newHashSet(\"java.util.Date\", \"java.sql.Timestamp\", \"java.time.Instant\", \"java.time.LocalDateTime\", \"java.time.OffsetDateTime\", \"\tjava.time.ZonedDateTime\").contains($column.type))\n        #set($jdbcType=\"TIMESTAMP\")\n    #elseif($tool.newHashSet(\"java.sql.Date\", \"java.time.LocalDate\").contains($column.type))\n        #set($jdbcType=\"TIMESTAMP\")\n    #else\n        ##其他类型\n        #set($jdbcType=\"VARCHAR\")\n    #end\n    $tool.call($column.ext.put(\"jdbcType\", $jdbcType))\n#end\n\n##定义宏，查询所有列\n#macro(allSqlColumn)#foreach($column in $tableInfo.fullColumn)$column.obj.name#if($velocityHasNext), #end#end#end\n"
      } ]
    }
  }
}
```

