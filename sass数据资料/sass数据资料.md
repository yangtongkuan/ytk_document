## sass数据资料设计

Minbo: 表做隔离，不同类型的用户，用不同的表。万一将来还有其他的用户类型呢，也好扩展，功能也独立性高。这里，我当时有一个小的功能：如果租户不是自己的登录名，而是用系统直接分配的用户名（有固定规则），登录上来，直接查租户表即可。如果绑定了个人的信息，比如邮箱什么的，主用户表则增加一条记录id信息映射。当然，还有更简单的做法，让用户登录时选择自己的身份类型即可（系统用户，租户等等）。是不是同一个db库，都可以，做好数据同步/异步写就好，也可以做隔离，也可以不隔离。如果担心彼此影响到，就做隔离，比如系统用户登录不了了，但租户可以正常登录。

#### 1. sass中租户的设计

整体系统设计 系统中只有多个租户信息 其中租户包括一些简单的信息 例如租户名 负责人 最重要的是到期时间等信息

```sql
CREATE TABLE `sys_tenant` (
  `tenant_id` BIGINT (20) NOT NULL AUTO_INCREMENT,
  `tenant_name` VARCHAR (50) DEFAULT NULL COMMENT '租户名称',
  `manager_name` VARCHAR(50) DEFAULT NULL COMMENT '负责人',
  `manager_phone` VARCHAR(20) DEFAULT NULL COMMENT '联系电话',
  `prov` VARCHAR (20) DEFAULT NULL COMMENT '省',
  `city` VARCHAR (20) DEFAULT NULL COMMENT '市',
  `county` VARCHAR (20) DEFAULT NULL COMMENT '区县',
  `address` VARCHAR (64) DEFAULT NULL COMMENT '地址',
  `note` VARCHAR (200) DEFAULT NULL COMMENT '备注',
  `for_ever` INT(1) DEFAULT '0' COMMENT '是否长期有效',
  `expire_time` DATETIME DEFAULT NULL COMMENT '到期时间',
  `last_renewal_time` DATETIME DEFAULT NULL COMMENT '最后续期时间',
  `del_flag` INT(1) DEFAULT '0' COMMENT '是否删除',
  `del_user_id` BIGINT(20) DEFAULT NULL COMMENT '删除者id',
  `create_time` DATETIME DEFAULT NULL COMMENT '创建时间',
  `create_user_id` BIGINT (20) DEFAULT NULL COMMENT '创建者id',
  PRIMARY KEY (`tenant_id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4 COMMENT = '系统租户';
```

#### 2.  用户表的设计

```sql
CREATE TABLE `user_info` (
  `user_id` BIGINT (20) NOT NULL AUTO_INCREMENT,
  `tenant_id` BIGINT (20) NOT NULL COMMENT '租户id',
  `user_name` VARCHAR (50) DEFAULT NULL COMMENT '账号_系統生成',
  `name` VARCHAR(50) DEFAULT NULL COMMENT '昵称',
  `phone` VARCHAR(20) DEFAULT NULL COMMENT '绑定电话',
  `tel_phone` VARCHAR(20) DEFAULT NULL COMMENT '联系电话',
  `email` VARCHAR(64) DEFAULT NULL COMMENT '邮箱',
  `sign_photo` VARCHAR(128) DEFAULT NULL COMMENT '头像',
  `signature` VARCHAR(128) DEFAULT NULL COMMENT '个性签名',
  `prov` VARCHAR (20) DEFAULT NULL COMMENT '省',
  `city` VARCHAR (20) DEFAULT NULL COMMENT '市',
  `county` VARCHAR (20) DEFAULT NULL COMMENT '区县',
  `address` VARCHAR (64) DEFAULT NULL COMMENT '地址',
  `lock` TINYINT(1) DEFAULT '0' COMMENT '是否锁定',
  `del_flag` TINYINT(1) DEFAULT '0' COMMENT '是否删除',
  `del_user_id` BIGINT(20) DEFAULT NULL COMMENT '删除者id',
  `create_time` DATETIME DEFAULT NULL COMMENT '创建时间',
  `create_user_id` BIGINT (20) DEFAULT NULL COMMENT '创建者id',
  PRIMARY KEY (`user_id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4 COMMENT = '用户信息';
```

#### 3.  用户密码表

```mysql
CREATE TABLE `user_imp_info` (
  `user_imp_id` INT(10) NOT NULL AUTO_INCREMENT,
  `tenant_id` BIGINT(20) NOT NULL COMMENT '租户id',
  `user_id` BIGINT(20) NOT NULL COMMENT '用户id',
  `passwd` VARCHAR(128) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '密码',
  `salt` VARCHAR(128) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '盐值',
  `note` VARCHAR(256) COLLATE utf8mb4_unicode_ci DEFAULT '' COMMENT '备注',
  `delFlag` INT(1) DEFAULT '0' COMMENT '是否已删除(1已删除，0未删除)',
  `create_user_id` BIGINT(20) DEFAULT NULL COMMENT '创建者id',
  `create_time` DATETIME DEFAULT NULL COMMENT '创建日期  141231212423423时间戳毫秒级标识',
  `del_user_id` BIGINT(20)  DEFAULT NULL COMMENT '删除者id',
  PRIMARY KEY (`user_imp_id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci

```

