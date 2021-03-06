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

## 大型重构

**特点**: 耗时长，不能让系统运行两个月重构，一步一步来，团队建立大型重构共识

**重要性**: 对前期理解不全的设计进一步修正，保证系统的健壮

### Tease Apart Inheritance (梳理并分解继承体系)

**描述**: 某个继承体系同时承担两项责任。建立两个继承体系，并通过委托关系让其中一个可以调用另一个

**动机**: 混乱的继承体系导致代码重复及难以理解

**判断标准**: 判断一个继承体系是否承担两项及以上责任，继承体系某个特定层级上的所有类，子类名称都以相同的形容词开始

**做法**: 

1. 识别继承体系承担的不同责任，建立二维表，以坐标标出不同任务（每次处理一个维度），重复运用本重构处理两个及以上维度
2. 在当前继承体系保留重要责任，将另一个责任迁移到另一个继承体系
3. 用Extract Class从当前超类中将不重要的责任提炼到一个新类，并在原超类中增加新类的实例变量
4. 对原继承体系中的每个子类，创建新类的一个子类。在原继承体系的子类中，将上一步所添加的实例变量初始化为新建子类的实例
5. 针对原继承体系中的每个子类，用Move Method将其中的行为迁移到对应的新建子类中
6. 当原继承体系中的某个子类不再有任何代码时，将它删除
7. 重复上述步骤，直到原继承体系中的所有子类都被处理过为止。观察新继承体系，看是否可能实施其他重构手法，如：Pull Up Method或Pull Up Field

**范例**: 

![Tease Apart Inheritance](./asset/image/refactor/tease-apart-inheritance.png "Tease Apart Inheritance")

### Convert Procedural Design to Objects（将过程化设计转化为对象设计）

**描述**: 你手上有些传统过程化风格的代码。将数据记录变成对象，将大块的行为分成小块，并将行为移入相关对象之中

**动机**: 面对一些过程化风格的代码所带来的的问题，并因此希望他们变得更面向对象

**做法**: 

1. 针对每一个记录类型，将其转变为只含访问函数的哑数据对象。（数据库表）
2. 针对每一处过程化风格，将该出的代码提炼到一个独立类中。（类->Singleton，函数->static）
3. 针对每一段长长的程序，实施Extract Method及其他相关重构将他分解。再以Move Method将分解后的函数分别移到它所相关的哑数据类中。
4. 重复上述步骤，直到原始类中的所有函数都被移除。如果原始类是一个完全过程化的类，将它拿掉将大快人心。

**范例**: 

![Convert Procedural Design to Objects](./asset/image/refactor/convert-procedural-design-to-objects.png "Convert Procedural Design to Objects")

### Separate Domain from Presentation（将领域和表述/显示分离）

**描述**: GUI类中包含领域逻辑，将领域逻辑分离出来，为它们建立独立的领域类。

**动机**: MVC很好的将 展现层 和 领域逻辑 分离，但面对大多数C/S结构的程序业务逻辑放到了展示逻辑中。

**做法**: 

1. 为每个窗口建立一个领域类
2. 如果窗口内有一张表格，为行新建一个类，再以窗口类中的集合包含所有行对象
3. 检查窗口的数据。如果数据只被UI用，保留；如果数据被领域逻辑使用，不显示在窗口，Move Field将它搬到领域类中；如果数据同时被UI和领域逻辑使用，则实施Duplicate Observed Data，将它同时放在两处，并保持两处之间的同步。
4. 检查展现类中的逻辑。实施Extract Method将展现逻辑从领域逻辑中分离。一旦隔离了领域逻辑，再运用Move Method将它移到领域类。
5. 以上步骤完成后，得到两组分离的类：展现类处理GUI，领域类处理业务逻辑。领域类不够严谨时进一步重构

**范例**: 

![Separate Domain from Presentation](./asset/image/refactor/separate-domain-from-presentation.png "Separate Domain from Presentation")

### Extract Hierarchty（提炼继承体系）

**描述**: 一个类一部分工作是以大量条件表达式完成  建立继承体系，以一个子类表示一种特殊情况

**动机**: 

渐进式设计，初期一个类实现一个概念，随方案演变，一个类实现多个不同概念，一开始很简单，几周或几周后，加一两个标记或测试，使其它环境也可以使用这个类，一年以后，这个类一团糟，变量、条件到处都是
遇到瑞士军刀类，需要一个好策略将各功能梳理并分开，前提条件逻辑在对象生命周期保持不变
Extract Hierarchty是大型重构，一两天不能完成，可能需要几周或几个月，可先进行一些简单重构，当理解更深时在来完成Extract Hierarchy

**做法**: 

**A. （无法确定哪些地方会变化）**：
1. 鉴别一种变化情况
2. 新建一个子类，对原类实施Replace Constructor with Factory Method。再修改工厂函数，使他适当子类实例
3. 将含有条件的逻辑函数，一次一个，逐一复制到子类，在明确情况下，简化这些函数。
4. 重复上述过程，将所有变化情况分离，直到超累声明为抽象类
5. 删除超类中被子类复写的函数体，并将它声明为抽象函数

**B.（明确原类的变化情况）**
1. 针对原类每种变化建立一个子类
2. 用Replace Constructor with Factory Method将原类构造函数转变为工厂函数，针对每种情况返回适当子类实例（每种变化情况以类型码表示，用Replace Type Code with Subclasses；每种变化情况在对象生命周期不同阶段有不同体现是用Replace Type Code with State/Strategy）
3. 对带条件逻辑的函数，实施Replace Conditional with Polymorphism

**范例**: 

![Extract Hierarchty](./asset/image/refactor/extract-hierarchty.png "Extract Hierarchty")