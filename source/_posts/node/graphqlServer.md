---
layout: _posts
title: graphql
date: 2018-09-03 09:31:48
tags: 
- graphql
categories: 
- graphql
description: graphql 服务端相关技术
---

## 一、关于`GraphQL`
GraphQL 既是一种用于 API 的查询语言也是一个满足你数据查询的运行时。GraphQL 对你的 API 中的数据提供了一套易于理解的完整描述，使得客户端能够准确地获得它需要的数据，而且没有任何冗余，也让 API 更容易地随着时间推移而演进，还能用于构建强大的开发者工具。
#### [英文官网](https://graphql.org/)
#### [中文官网](http://graphql.cn/)
---
## 二、服务端`GraphQL Server`
### [关于 `GraphQL` 服务器方案介绍](https://mdluo.com/2018-05-03/an-intro-to-graphql-server/)
- #### [apollo-server](https://github.com/apollographql/apollo-server)
开源的 node.js GraphQL 服务器，提供了多种封装方案，可以很方便的基于 express、koa、AWS Lambda、Azure functions 等 Web 服务器和云环境中运行 GraphQL 服务。
- #### [apollo-engine](https://engine.apollographql.com/)
SaaS 平台，可以与 apollo-server 集成，也可以集成到现有的其他语言的 GraphQL Server。提供缓存、错误追踪、性能追踪等服务，有免费和付费的方案。
- #### [graphql-yoga](https://github.com/graphcool/graphql-yoga)
对 apollo-server 和相关的封装，类似于 create-react-app 的这种脚手架，可以快速方便的搭建 GraphQL 服务器，同时还默认集成了 graphql-playground 这个交互式的工具，比 GraphiQL 功能更完善。但是 graphql-yoga 和 apollo-server 并无本质区别，只提供对外的 GraphQL 接口，不关心底层数据的来源。
- #### [prisma](https://github.com/graphcool/prisma)
Prisma 的核心功能是直接管理 GraphQL 的类型结构与数据库表和字段直接的映射，同时提供对外的 GraphQL 接口。此外还提供速率限制、访问认证、日志等其他功能。与数据库的连接有两种模式：主动模式是完全管理数据库的 Schema 和数据结构迁移；被动模式是不管理数据库，仅对数据库做读写操作。Prisma 目前只有 MySQL 的主动模式，已经把自身和数据库用 docker 封装，可以很方便的部署。目前的问题就是只支持 docker 化的 MySQL 数据库的主动模式，所以只适合搭建全新的系统。不支持其他数据库以及云数据库（还在开发中）
  - [官方 node.js 教程，基于 prisma ](https://www.howtographql.com/graphql-js/0-introduction/)
- #### [Prisma Cloud](https://www.prisma.io/cloud)
提供 Prisma 的托管服务，有免费和付费方案，除了把上面提到的 prisma 快捷部署到云服务器之外，还提供了 Web 控制面板，提供查看数据等功能。

### 关于 `apollo-server-koa` 实现 GranphQL 服务器端
#### apollo-server 实现 GraphQL Schema 的两种方式
- #### 1、单独定义 `typeDefs` 和 `resolvers`
```
const { ApolloServer, gql } = require('apollo-server');

// The GraphQL schema
const typeDefs = gql`
  type Query {
    "A simple type for getting started!"
    hello: String
  }
`;

// A map of functions which return data for the schema.
const resolvers = {
  Query: {
    hello: () => 'world',
  },
};

const server = new ApolloServer({
  typeDefs,
  resolvers,
});
```
为了方便，我通过 单数据库表\单文件 的方式定义各自的 `typeDefs` 和 `resolvers`，再整合所有文件，如：pub_cata_def.js
```
import { connectSql, disconnectSql, authorize } from '../util'

const typeDefs = {
  Query: `
    pubCataDef(id: ID, cataid: String): [PubCataDef!]!
  `,
  Mutation:`
  `,
  Type:`
    type PubCataDef {
      id: ID!,
      groupid: String
    }
  `
}

const resolvers = {
  Query: {
    // read
    pubCataDef(obj, args, ctx){
      return getPubCataDef(args, 'read', ctx);
    }
  },
  Mutation:{

  },
  PubCataDef: {
    id: (root) => root.id,
    groupid: (root) => root.groupid
  }
}

module.exports = {
  typeDefs,
  resolvers
}
```
schema.js
```
import { ApolloServer } from 'apollo-server-koa';

import { objectFactory } from '../util';

// models
import pub_cata_def from './pub_cata_def';

const serverConf = objectFactory([
  pub_cata_def
])

export const server = new ApolloServer({
  ...serverConf,
  context: ({ ctx }) => ctx
});
```
util.js
```

// 深度合并对象
export const objectFactory = (array) => {
  let typeDefsAry = [], resolversAry = [];
  array.forEach(item => {
    typeDefsAry.push(item.typeDefs)
    resolversAry.push(item.resolvers)
  })
  let typeDefs = objectAssign(typeDefsAry, 'typeDefs');
  let resolvers = objectAssign(resolversAry, 'resolvers');

  typeDefs = gql`
    type Query {
      ${typeDefs.Query}
    }
    type Mutation{
      ${typeDefs.Mutation}
    }
    ${typeDefs.Type}
  `
  return {typeDefs, resolvers}
}
export const objectAssign = (ary, type) => {
  return ary.reduce(function(a, b){
    return type === 'typeDefs' ? mergeTypeDefs(a, b) : deepObjectMerge(a, b)
  })
}
const deepObjectMerge = (FirstOBJ, SecondOBJ) => {
  for (var key in SecondOBJ) {
      FirstOBJ[key] = FirstOBJ[key] && FirstOBJ[key].toString() === "[object Object]" ?
          deepObjectMerge(FirstOBJ[key], SecondOBJ[key]) : FirstOBJ[key] = SecondOBJ[key];
  }
  return FirstOBJ;
}
const mergeTypeDefs = (FirstOBJ, SecondOBJ) => {
  let Query = `${FirstOBJ.Query}${SecondOBJ.Query}`,
      Mutation = `${FirstOBJ.Mutation}${SecondOBJ.Mutation}`,
      Type = `${FirstOBJ.Type}${SecondOBJ.Type}`;

  FirstOBJ = { Query, Mutation, Type };
  return FirstOBJ;
}
```
这种方式的优点在于，定义 `typeDefs` 和 `resolvers` 时比较方便，缺点在于写起来麻烦

- #### 2、使用 Schema Definition Language 定义 schema
```
const { ApolloServer, gql } = require('apollo-server');
const { GraphQLSchema, GraphQLObjectType, GraphQLString } = require('graphql');

// The GraphQL schema
const schema = new GraphQLSchema({
  query: new GraphQLObjectType({
    name: 'Query',
    fields: {
      hello: {
        type: GraphQLString,
        description: 'A simple type for getting started!',
        resolve: () => 'world',
      },
    },
  }),
});

const server = new ApolloServer({ schema });

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`);
});
```
这种方式的优点在于使用 `sequelize-auto` 根据数据库生成的 `model` ，与 `schema` 的 `type` 结构类似，且不需要手动定义数据库 `model`，实现全自动化生成 `schema` 的 `query` ，`mutation` 可根据业务需求手动实现。

#### 使用 `koa`、`apollo-server`、`sequelize` 实现的 GraphQL 服务端
- #### 安装相关包 `package`
```
npm i koa koa-router apollo-server-koa babel-cli babel-preset-env babel-preset-stage-0 graphql sequelize sequelize-auto
```
- #### `.babelrc`
```
{
  "presets": [
  	"stage-0",
    ["env", {
      "targets": {
        "node": "current"
      }
    }]
  ]
}
```
- #### `package.json`
```
"scripts": {
  "start": "./node_modules/.bin/babel-node ./src/app.js",
}
```
- #### `src/app.js`
```
import Koa from 'koa';
import Router from 'koa-router';
import { ApolloServer } from 'apollo-server-koa';

import schema from './schema'

const server = new ApolloServer({
  schema,
  context: ({ ctx }) => ctx
});

const app = new Koa();
server.applyMiddleware({ app, path: '/graphql' })

const router = new Router();
router.get('/', (ctx, next) => {
  ctx.body = 'hello, welcome to graphql'
})

app
  .use(router.routes())
  .use(router.allowedMethods())

app.listen({ port: 4000 }, () => {
  console.log(`🚀  Server is running at http://localhost:4000/${server.graphqlPath}`);
});
```
- #### 根据数据库表结构自动生成 Model，'sequelize/sequelizeAuto.sh'
```
../node_modules/sequelize-auto/bin/sequelize-auto -o "../models" -d test_sandbox -h 10.0.0.175 -u sandbox -p 3306 -x sandbox2018 -e mysql
```
- #### 根据生成的 Model 实现数据库表查询
```
import fs from 'fs';
import path from 'path';
import sequelize from '../sequelize';

let db = {};
let basename  = path.basename(module.filename);

fs
  .readdirSync(__dirname)
  .filter(function(file) {
    return (file.indexOf('.') !== 0) && (file !== basename) && (file.slice(-3) === '.js');
  })
  .forEach(function(file) {
    let model = sequelize['import'](path.join(__dirname, file));
    let name = model.name.replace(/^\w|_\w/g, function(letter){return letter.replace('_', '').toUpperCase()}) + 'Model';
    db[name] = model;
  });

module.exports = db;
```
- #### 根据生成的 Model 实现对应表的 schema ，并生成 index.js 输出所有 schema 的合集，'schema/auto_schema'
```
import fs from 'fs';
import path from 'path';

// 生成 index.js
let modelImport = '';
let query = '';
// 辅助生成index.js
let fileLen = 0;

fs
  .readdirSync(path.resolve(__dirname, '../models'))
  .filter(function(file) {
    if(file.indexOf('.') !== 0 && file !== 'index.js' && file !== 'auto_schema.js' && file.slice(-3) === '.js'){
      fileLen++;
      return file;
    };
  })
  .forEach(file => {
    fs.readFile(path.resolve(__dirname, '../models/' + file), {encoding: 'utf-8'}, (err, data) => {
      if(err)throw err;
      // PubCataDef
      let ModelName = file.replace('.js', '').replace(/^\w|_\w/g, function(letter){return letter.replace('_', '').toUpperCase()});
      // pubCataDef
      let modelName = ModelName.replace(/^\w/, (letter) => { return letter.toLocaleLowerCase() })

      console.log(ModelName);
      let typeStr = data.replace(/[\S\s]+'\w+', |, \{[\S\s]+$|allowNull:\s\w+(,)?|unique:\s\w+(,)?|defaultValue: '([\w\[\]\._\u4e00-\u9fa5]*)'|defaultValue: \w+\.\w+\('\w+'\)|primaryKey: \w+(,)?|autoIncrement: \w+(,)?|,(?=\))|/g, '');
      let keys = typeStr.match(/\w+: \{/g);
      let i = 0;
      typeStr = typeStr.replace(/DataTypes\.\w+(\(\w+\))?(\.\w+)?,/g, function(match){
        let str;
        if(match.includes('INT')){
          str = 'GraphQLInt,'
        }else{
          str = 'GraphQLString,'
        }
        str += `
      resolve (root) {
        return root.${keys[i].replace(': {', '')};
      }`
        i++;
        str.replace(/\n+/g, '');
        return str
      });
      let result = `
import {
  GraphQLObjectType,
  GraphQLString,
  GraphQLInt,
  GraphQLNonNull,
  GraphQLList,
} from 'graphql';

import { ${ModelName}Model } from '../models'

const Type = new GraphQLObjectType({
  name: '${ModelName}',
  description: '${ModelName}',
  fields: () => {
    return ${typeStr}
  }
});

export const ${modelName} = {
  type: new GraphQLList(Type),
  args: {
    id: {
      type: GraphQLInt
    }
  },
  resolve: (root, args) => {
    const offset = args.offset || 0;
    const limit = args.first || 10;
    delete args.offset;
    delete args.first;
    return ${ModelName}Model.findAll({ where: args, offset, limit });
  }
}
`
      // 写入文件
      let isExist = fs.existsSync(path.resolve(__dirname, `./${modelName}.js`));
      
      fs.writeFileSync(path.resolve(__dirname, `./${isExist ? '/s/' + modelName : modelName}.js`), result, 'utf8');

      fileLen--;
modelImport +=`import { ${modelName} } from './${modelName}'
`
query+=`
      ${modelName},`;
      if(fileLen === 0){
        console.log(modelImport);
        let schema = `
import {
  GraphQLObjectType,
  GraphQLSchema,
} from 'graphql';

${modelImport}

const Query = new GraphQLObjectType({
  name: 'Query',
  description: 'Root query object',
  fields: () => {
    return {
      ${query}
    };
  }
});

const Mutation = new GraphQLObjectType({
  name: 'Mutations',
  description: 'Functions to set stuff',
  fields: () => {
    return {
      
    }
  }
})

const Schema = new GraphQLSchema({
  query: Query,
  mutation: Mutation
});

export default Schema;
`
        fs.writeFileSync(path.resolve(__dirname, `./index.js`), schema, 'utf8');
      }
    })
  })
```
- `pachage.json`
```
"scripts": {
  "schema": "./node_modules/.bin/babel-node ./src/schema/auto_schema.js"
}
```
```
npm run schema
```