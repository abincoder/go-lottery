## 抽奖活动服务端 - Golang

### 6 种抽奖小栗子
> 关键点：奖品类型，数量有限，中奖概率，发奖规则  
> 并发安全性问题：互斥锁，队列，CAS递减（atomic）  
> 优化：通过三列减少单个集合的大小

- [年会抽奖](./_demo/1annualMeeting)
- [彩票刮奖](./_demo/2ticket)
- [微信摇一摇](./_demo/3wechatShake)
- [支付宝集福卡](./_demo/4alipayFu)
- [微博抢红包](./_demo/5weiboRedPacket)
- [大转盘](./_demo/6wheel)

### 数据设计 - MySQL
#### 奖品表 - gift
| 字段 | 属性 | 说明 |
| --- | --- | --- |
| id | int,pk,auto_increment | 主键 |
| title | varchar(255) | 奖品名称 |
| prize_num | int | 奖品数量：0 无限；>0 限量；<0 无奖品 |
| left_num | int | 剩余奖品数量 |
| prize_code | varchar(50) | 0-9999 标识 100%，0-0 表示万分之一的中奖概率 |
| prize_time | int | 发奖周期：D 天 |
| img | varchar(255) | 奖品图片 |
| display_order | int | 位置序号：小的排在前面 |
| gtype | int | 奖品类型：0 虚拟币；1 虚拟券；2 实物-小奖；3 实物-大奖 |
| gdata | varchar(255) | 扩展数据：如虚拟币数量 |
| time_begin | int | 开始时间 |
| time_end | int | 结束时间 |
| prize_data | mediumtext | 发奖计划：[[时间 1，数量 1]，[时间 2，数量 2]] |
| prize_begin | int | 发奖周期的开始 |
| prize_end | int | 发奖周期的结束 |
| sys_status | smallint | 状态：0 正常；1 删除 |
| sys_created | init | 创建时间 |
| sys_updated | int | 修改时间 |
| sys_ip | varchar(50) | 操作人 IP |

#### 优惠券 - code
| 字段 | 属性 | 说明 |
| --- | --- | --- |
| id | int,pk,auto_increment | 主键 |
| gift_id | int | 奖品 ID，关联 gift 表 |
| code | varcahr(255) | 虚拟券编码 |
| sys_created | int | 创建时间 |
| sys_udpated | int | 更新时间 |
| sys_status | smallint | 状态：0 正常；1 作废；2 已发放 |

#### 抽奖记录表 - result
| 字段 | 属性 | 说明 |
| --- | --- | --- |
| id | int,pk,auto_increment | 主键 |
| gift_id | int | 奖品 ID，关联 gift 表 |
| gift_name | varchar(255) | 奖品名称 |
| gift_type | int | 奖品类型：同 gift.gtype |
| uid | int | 用户 ID |
| username | varchar(50) | 用户名 |
| prize_cdoe | int | 抽奖编号（4 位的随机数） |
| gift_data | varchar(50) | 获奖信息 |
| sys_created | int | 创建时间 |
| sys_ip | varchar(50) | 用户抽奖的 IP |
| sys_status | smallint | 状态：0 正常；1 删除；2 作弊 |

#### 用户黑名单表 - black_user
| 字段 | 属性 | 说明 |
| --- | --- | --- |
| id | int,pk,auto_increment | 主键 |
| username | varchar(50) | 用户名 |
| black_time | int | 黑名单限制到期时间 |
| real_name | varchar(50) | 联系人 |
| mobile | varchar(50) | 手机号 |
| address | varchar(255) | 联系地址 |
| sys_created | int | 创建时间 |
| sys_udpated | int | 更新时间 |
| sys_ip | varchar(50) | 用户抽奖的 IP |

#### IP 黑名单表 - black_ip
| 字段 | 属性 | 说明 |
| --- | --- | --- |
| id | int,pk,auto_increment | 主键 |
| ip | varchar(50) | IP 地址 |
| black_time | int | 黑名单限制到期时间 |
| sys_created | int | 创建时间 |
| sys_udpated | int | 更新时间 |

#### 用户每日次数表 - user_day
| 字段 | 属性 | 说明 |
| --- | --- | --- |
| id | int,pk,auto_increment | 主键 |
| uid | int | 用户 ID |
| day | int | 日期：如 20180725 |
| num | int | 次数 |
| sys_created | int | 创建时间 |
| sys_udpated | int | 更新时间 |

未完待续。。。

如有错误，务必指出，谢谢 ^.^ !

