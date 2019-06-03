---
title: "mongodb connection과 연결여부 monitoring"
classes: wide
categories:
  - mongodb
tags:
  - mongoose
date: 2019-06-03 19:00:00 -0600
---

간단한 코드 기록용  

```javascript
let mongoose  = require('mongoose')

let conn1     = mongoose.createConnection('mongodb://localhost:5555/db1', {useNewUrlParser: true})
let conn2     = mongoose.createConnection('mongodb://localhost:5556/db2', {useNewUrlParser: true})

conn1.on('connected', () =>
{
    console.log("connected to mongodb conn1")
});
conn2.on('connected', () =>
{
    console.log("connected to mongodb conn2")
});
conn1.on('disconnected', () =>
{
    console.log("disconnected with mongodb conn1")
});
conn2.on('disconnected', () =>
{
    console.log("disconnected with mongodb conn2")
});

```
