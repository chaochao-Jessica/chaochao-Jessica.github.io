---
title: react为什么用hooks
date: 2021-02-22 14:10:43
tags:
---
·Component非UI逻辑复用困难。| 需要使用高阶组件hell复用状态。useState状态更易复用
·组件的生命周期函数不适合side effect逻辑的管理。  | hook更易扩展和组合，
·不友好的Class Component。 | event handler 需要绑定this
