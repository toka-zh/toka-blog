---
title: "ETH"
date: 2022-05-20
tags: ["BlockChain"]
categories: ["BlockChain"]
---

相较于比特币,以太坊的出块时间大幅度降低到了十几秒,并且设计了基于GHOST协议的共识机制,挖矿使用的minign puzzle,比特币的基于算力,导致了挖矿机器的专业化,以太坊的 memory hard mining puzzle,proof of work(工作量证明) 挖矿,将来改为 proof of stake(权益证明) 按照持有量证明,除此之外 以太坊增加了对智能合约的支持 smart contract 

比特币实现了去中心化的货币是数字黄金

bitcoin decentralized currency 

ethereum decentralized contract

去中心化合约的优点

* 不可篡改,只能按照既定规则执行
* 流通性强,跨国交易方便



账户

基于账户模型

account-based ledger

double spenging attack 双花攻击  付款者是恶意节点,花过的币再花一遍

replay attack 重放攻击 收款者是恶意节点,接收的钱再收一遍



优点 可以避免double spending attack

缺点 可能遭受replay attack

避免方式:添加交易次数计数器nonce,节点除了维护账户的balance,还要维护账户的nonce,每次收到账户发起的交易,nonce+1



eth有两类账户

* externally owned account 外部账户 使用公私钥控制(普通账户)
  * balance 账户余额
  * nonce 计数器
* smart contract account 合约账户 不通过公私钥对控制,不能主动发起交易
  * balance
  * nonce
  * code
  * storage



状态树