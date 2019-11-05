---
title: (1020)SQL과 ORM
date: 2019-10-20 10:11:86
category: TIL
---

SQL의 정의와 ORM으로 발전한 과정을 공부한 내용을 정리한 글이다

## SQL

Structured Query Language 이다  
즉, 관계형 데이터베이스 관리 시스템의 data를 관리하기 위해  
설계된 특수 목적의 programming language 이다  
data 의 CRUD 와 data model 의 schema 생성과 수정  
database object 에 access 하기 위해 고안되었다

### SQL 의 한계

일반적인 SQL database 를 사용하게 되면  
하나의 object 에 대한 data 를 불러오기 위해  
수많은 join query 를 작성하게 된다  
이 때 app이 확장하여 schema 가 복잡해질수록  
작성해야하는 sql query 가 더욱 많아진다

## ORM

Object Relational Model 이다  
SQL 의 한계를 극복하기 위해 고안된 framework이다

1. 필요한 data를 불러오는 query들의 조합을 객체 단위로 미리 생성해 놓는다
2. 해당 객체(query들의 조합)를 만들어내는 query를 사용한다

이런 query를 API로 만들어 활용할 수 있게 도와주는 framework이다  
대표적인 ORM 으로는 sequelize 가 있다

### Sequelize 적용 example

1. DB(postgres)와 sequelize를 연결한다
   ```js
   var sequelize = new Sequelize(
     'postgres://sequelize:1234@localhost/sequelize'
   )
   var sequelize = new Sequelize('sequelize', 'sequelize', '1234', {
     host: 'localhost',
     dialect: 'postgres',
   })
   ```
2. Model define 하여 table의 schema 를 표현한다  
   이후 실제 DB에 이 schema를 반영하기 위해서는 import 해야한다
   ```js
   sequelize.define(
     'Publisher',
     {
       pub_id: {
         type: DataTypes.INTEGER,
         primaryKey: true,
         autoIncrement: true,
       },
       name: { type: DataTypes.STRING(32), allowNull: false },
       established_date: { type: DataTypes.DATE, defaultValue: DataTypes.NOW },
     },
     {
       classMethods: {},
       tableName: 'publisher',
       freezeTableName: true,
       underscored: true,
       timestamps: false,
     }
   )
   ```
3. import 하여 model define 한 것을 실제 DB에 반영한다, object 생성하여 저장.(memory 공간에 올려 caching)
   ```js
   var fs = require('fs')
   var path = require('path')
   var Sequelize = require('sequelize')
   var sequelize = new Sequelize(
     'postgres://sequelize:1234@localhost/sequelize'
   )
   var db = {}
   db['Publisher'] = sequelize.import(path.join(\_\_dirname, 'publisher.js'))db.sequelize = sequelize
   db.Sequelize = Sequelize
   module.exports = db
   ```
