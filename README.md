# domain-driven-design-notes
>  《领域驱动设计：软件核心复杂性应对之道》学习笔记

之前关于 DDD 学的并不系统，只是在通过一些文档来浅尝辄止，借助书来具体学一下。

| ![](https://image-1252109614.cos.ap-beijing.myqcloud.com/2023/04/27/6449571f3525f.png) | ![](https://image-1252109614.cos.ap-beijing.myqcloud.com/2023/04/27/64495726ed049.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |

# 第一部分 运用领域模型

## 第 1 章 消化知识

> Digesting Knowledge

- 第一章讨论了有效建模的要素，强调在软件开发过程中领域知识的重要性，以及如何通过与领域专家的紧密合作来提炼出领域模型。
- 本章还探讨了持续学习的重要性，指出在软件开发过程中不断地调整和完善领域模型是关键。
- 最后，章节讨论了知识丰富的设计和深层模型，提倡将领域知识融入设计中以提高软件的质量。

## 第 2 章 交流与语言的使用

> Communication and the Use of Language

- 第二章介绍了通用语言（Ubiquitous Language）的概念，强调在团队中建立一种共同的、针对特定领域的语言以提高沟通效率。
- 本章讨论了“大声地”建模的方法，以便更好地理解和捕捉领域知识。
- 接下来，介绍了一个团队需要使用一种统一的语言来确保沟通的顺畅。
- 最后，讨论了文档和图的使用，提倡在开发过程中使用适当的文档和图形工具来辅助沟通和理解。

## 第 3 章 绑定模型和实现

> Binding Model and Implementation

- 第三章介绍了模型驱动设计（Model-Driven Design）的概念，强调领域模型在整个软件开发过程中的核心地位。
- 本章还讨论了建模范式和工具支持，以帮助开发团队实现模型驱动设计。
- 接下来，作者阐述了揭示主旨的重要性，即模型对于用户的价值。
- 最后，介绍了实践者模型（Hands-on Modeler）的概念，强调开发人员需要积极参与模型的创建和维护。

![1682528724949.png](https://image-1252109614.cos.ap-beijing.myqcloud.com/2023/04/27/644959ca32a7e.png)

# 第二部分 模型驱动设计的构造块

## 第 4 章 分离领域

### Layered Architecture 模式

> 分层架构（Layered Architecture）是一种软件设计模式，它将应用程序划分为多个层次，每个层次负责特定的功能。这种架构旨在降低系统的复杂性、提高模块化程度、增强可维护性和可扩展性。
>
> 在分层架构中，各层次之间遵循一定的依赖关系，通常情况下，上层依赖于下层，下层不知道上层的存在。这种依赖关系有助于保持层次的独立性和解耦。

![LAYERED_ARCHITECTURE](https://image-1252109614.cos.ap-beijing.myqcloud.com/2023/04/27/644958cf5a179.png)

1. **用户界面层**（或表示层）：负责向用户（可能是另一个计算机系统或人）显示信息和解释用户指令。
2. **应用层**：定义软件要完成的任务，协调领域层中的领域对象来解决问题。这一层对业务意义重大，同时也是与其他系统的应用层交互的渠道。应用层应尽量简单，不包含业务规则或知识，只负责协调任务和分配工作。
3. **领域层**（或模型层）：负责表达业务概念、业务状态信息和业务规则。这一层控制并使用反映业务情况的状态，是业务软件的核心。
4. **基础设施层**：为其他层提供通用技术能力，如为应用层传递消息，为领域层提供持久化机制，为用户界面层绘制屏幕组件等。此外，基础设施层还可以通过架构框架支持四个层次间的交互模式。

为复杂的应用程序划分层次，使每一层具有内聚性且仅依赖于下层。采用标准架构模式，与上层进行松散连接。将所有与领域模型相关的代码放在一个层中，与用户界面层、应用层和基础设施层的代码分开。领域对象关注表达领域模型，而无需考虑显示、存储问题或管理应用任务。这使得模型富有意义、结构清晰，能捕捉基本业务知识并有效使用。

- 层与层之间的依赖关系应当是单向的，上层可以通过调用下层元素的公共接口进行交互。为了实现层之间的通信，可以采用架构模式，例如回调模式、观察者模式以及最早将用户界面层与应用层和领域层相连的MVC（模型-视图-控制器）框架。

### THE SMART UI "反模式"

"智能UI（The Smart UI）"反模式是一种软件设计反模式，它将业务逻辑与用户界面（UI）紧密耦合在一起。在这种设计中，大部分业务逻辑和领域逻辑都直接嵌入到用户界面层，导致代码难以维护、测试和重用。

智能UI反模式的问题：

1. **耦合度高**：由于业务逻辑和领域逻辑与用户界面紧密耦合，当需要修改业务规则或需求时，很可能涉及到大量的UI代码更改。这使得维护和扩展变得困难。
2. **代码重用性低**：在智能UI反模式中，业务逻辑通常与特定的UI组件耦合，导致这些逻辑无法在其他UI组件或系统中重用。
3. **测试难度增加**：**当业务逻辑与用户界面混合在一起时，编写针对这些逻辑的单元测试变得更加困难。用户界面通常涉及许多与显示和交互相关的细节，这些细节与业务逻辑无关，但在测试过程中可能引入额外的复杂性。
4. **难以适应变化**：业务需求和界面设计可能会随着时间的推移而发生变化。在智能UI反模式下，由于业务逻辑与界面紧密耦合，对于这些变化的适应会变得更加困难。

要避免智能UI反模式，可以采用分层架构，如 Model-View-Controller（MVC）或 Model-View-ViewModel（MVVM）等设计模式。这些模式将业务逻辑与用户界面分离，使得代码更易于维护、测试和重用。在领域驱动设计（DDD）中，特别强调领域模型和业务逻辑的独立性，将它们与用户界面和基础设施层进行解耦。这有助于实现更加清晰、可扩展和易于维护的软件系统。

## 第 5 章 软件中所表示的模型

在不削弱模型驱动设计能力的前提下对实现做出折中，需要重新组织基本元素并理解它们。本章从如何设计和简化关联开始，强调模型中详细实现决策的重要性。讨论主要关注模型本身，同时仔细考察模型选择与实现问题之间的关系。本章主要关注三种模式：

- ENTITY（实体）
- VALUE OBJECT（值对象）
- SERVICE（服务）

实体和值对象之间的根本区别在于它们所表示的事物。**实体表示具有连续性和标识的事物**，而**值对象用于描述某个事物的某种状态的属性**。明确地选择这两种模式中的一个有助于减少歧义并得到健壮的设计。

领域中的一些方面适合用动作或操作来表示，这比用对象表示更清楚，这些方面最好用服务来表示。服务应客户端请求来完成某事，在领域中也可以使用服务，当对软件必须实现的某项无状态的活动进行建模时，可以将该活动作为一项服务。

有时为了将对象存储在关系数据库中，我们必须对对象模型做一些折中改变，虽然这会影响它的纯度。本章给出一些指导原则，以便在处理这种复杂局面时保持正确的方向。

最后，MODULE（模块）的讨论有助于理解每个设计决策都应该在深入理解领域中的某些深层知识之后做出。高内聚、低耦合的思想应用于概念本身。在模型驱动设计中，模块是模型的一部分，它们应该反映领域中的概念。

本章将所有这些软件模型的构造块组织在一起，这些都是传统思想，但将这些思想组织到模型驱动开发的上下文中，可以帮助开发人员创建符合领域驱动设计主要原则的详细组件，从而解决更大的模型和设计问题。掌握这些基本原则可以帮助开发人员在被迫做出折中设计时把握正确的方向。

### 关联

关联使得对象间的建模与实现交互更复杂。对于每个可遍历关联，软件中都存在具有相同属性的机制。关联模型具有两个含义：抽象现实世界中对象间的关系，以及软件中对象指针或数据库查询等实现。例如，一对多关联可以用集合实现，也可以通过访问方法查询数据库。设计中需指定一种遍历机制，使其与模型关联一致。

实际生活中存在许多复杂的多对多关联。为降低实现和维护的复杂性，可以采用以下方法：

1. 规定遍历方向：指定单向遍历可以减少相互依赖性，简化设计。
2. 添加限定符：有效地减少多重关联，将多对多关联简化为一对多关联。
3. 消除不必要的关联：清除对当前工作或模型对象基本含义不重要的关联。

通过对关系进行约束，可以使设计更实用，表达领域的深入理解。例如，将国家与总统之间的双向关系简化为单向关联，使Person类独立于President概念。通过深入理解领域，可以得到限定的关系，如将多对多关联简化为一对一关系。限定遍历方向可以简化实现，并突出双向关联的重要性。当双向关联是领域的语义特征或应用程序功能要求时，需保留双向关联。

### 模式:ENTITY(又称为REFERENCE OBJECT)

**实体 (Entity, 又称为参照对象 Reference Object)** 是一个概念，通常用于描述计算机编程、数据库管理、知识表示和其他领域中的一个独立的、可辨识的单位。实体可以代表现实世界的对象、概念、事件等，它们具有一定的属性和行为。

为了更好地理解实体，我们可以从以下几个方面来进行解释：

1. **属性 (Attributes)**：实体具有一组描述其特性的属性。例如，在面向对象编程中，一个“汽车”实体可能具有颜色、型号、制造商等属性。
2. **行为 (Behaviors)**：实体可能具有一些行为或操作，这些操作可以执行某些任务或修改实体的状态。例如，汽车实体可以有一个启动引擎的行为。
3. **关系 (Relationships)**：实体之间可以存在不同类型的关系。例如，一个“员工”实体可能与一个“公司”实体具有从属关系。
4. **标识符 (Identifier)**：实体通常具有唯一标识符，以便在各种上下文中引用它们。这可以是一个数字、字符串或其他类型的标识。

在不同领域和语境下，实体的具体含义和表示方式可能会有所不同。例如，在数据库管理中，实体可以表示为一张表中的一行记录；在面向对象编程中，实体可以是一个具有属性和方法的类的实例。总的来说，实体是一种用于表示和处理现实世界中的对象、概念等的抽象概念。
