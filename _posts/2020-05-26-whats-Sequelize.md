---
title: "Sequelize : How to manage DB"
date: 2020-05-21 21:30:30 +0900
categories: Knowledges
comments: true
published: false
---

I was aware of these _ORM_ solutions(or frameworks), and heard about them from various engineers, which basically was an anthem. It is obvious Object-relational mapping makes Object oriented programming much easier(as it's name is self-explanatory), and also makes us to get rid of awful SQLs out of codes.

But then, why didn't I use it yet? That may have various answers, but generally having an ORM framework means another application on the go, and that kind of luxury was not my choice to take. In a sense of the cost of maintenance, resource, and the time to dig this thing about to make it work was not in my career.

But I got a full-stack coding challenge from anonymous, and I just thought: 'If I'm building a completely new application, why don't I take this chance to try ORM?'. That's how I met and started with [Sequelize][sequelize]. I can't review a whole scale but YES it was an _amazing experience_.

## Tips from experience

Sequelize is Node based application. Which is a very good start, because you just need to execute only _npm_ command to use this ORM platform. (First problem of maintenance solved)

```shell
npm install sequelize sequelize-cli
```

First tip: It is okay to install it on global, but try to manage it locally. I never found sequelize too heavy to seperate it as different container from the application.

### seqelize-cli

When I use database, first thing I do is create a database and tables. Assuming that you have all the data structure decided, try to use sequelize cli commands to build models, migrations from preset, and fix it as a code.

```shell
node_modules/.bin/sequelize-cli init
```

With _init_ command, you create sequelize's

```shell
node_modules/.bin/sequelize model:generate --name Reviews --attributes reviewer_id:integer,reviewee_id:integer,score:integer
```

For example, I just generated 

[sequelize]: https://sequelize.org/
