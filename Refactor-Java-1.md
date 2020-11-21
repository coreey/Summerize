# 重构（第一版）

### 电子书：[Download](./ebook/重构_改善既有代码的设计.pdf)

-----

## 定义

**重构（名词）**：对软件内部结构的一种调整，目的是在不改变软件可观察行为的前提下，提高其可理解性，降低其修改成本。

**重构（动词）**：使用一系列重构手段，在不改变软件可观察行为的前提下，调整其结构。

## 目的
1. 重构改进软件设计
2. 重构使软件更容易理解
3. 重构帮助找到bug
4. 重构提高变成速度

## 何时重构

三次法则：事不过三

1. 添加功能是重构
2. 修补错误时重构
3. 审核代码是重构

## 重构难题

1. 数据库
2. 修改接口（不要过早发布接口。请修改你的代码所有策略，是重构更顺畅。）
3. 难以通过重构手法完成的设计改动
4. 何时不该重构（代码不能稳定运行时；项目结尾时）

## 代码坏味道

1. Dduplicate Code -- 重复代码
2. Long Method -- 过长函数
3. Large Class -- 过大的类
4. Long Parameter list -- 过长参数列表
5. Divergent Change -- 发散式变化
6. Shotgun Surgery -- 霰弹式修改
7. Feature Envy -- 依恋情结
8. Data Clumps -- 数据泥团
9. Primitive Obsession -- 基本类型偏执
10. Switch Statements -- Switch惊悚现身
11. Parallel Inheritance hierarchies -- 平行继承体系
12. Lazy Class -- 冗赘类
13. Speculative Generality -- 夸夸其谈未来性
14. Temporay Field -- 令人迷惑的暂时字段
15. Message Chains -- 过度耦合的消息链
16. Middle Man -- 中间人
17. Inappropriate Intimacy -- 狎昵关系
18. Alternative Classes with Different Interfaces -- 异曲同工的类
19. Incomplete Library Class -- 不完美的类库
20. Data Class -- 单纯的数据类
21. Refused Bequest -- 被拒绝的馈赠
22. Comments -- 过多的注解