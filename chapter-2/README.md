# Chapter 2 重构的法则


上一章节的例子应该大体上让你明白了什么是重构。现在是时候让我们来回顾一下关于重构的那些法则了。

## 定义什么是重构

就像其他软件开发的术语一样，“重构”这个词也经常会被不准确地使用。“重构”可以被用作名词和动词，定义如下：

> **Refactoring (noun)**: 是对软件内部结构的一种改变，这种改变可以使代码更容易被理解和更改，并且不能改变软件对外的行为。

> **Refactoring (verb)**:对软件进行一系列的重构（名词），并且不改变软件对外的行为。

在过去的几十年间，许多业界人员都会错误地使用“重构”这个词，他们认为任何对代码的清理都能被称为是“重构”。事实上重构是由一系列小的重构组成的，重要的是每次你的重构都足够的小，因为足够小，所以你的软件还是能够正常运行并且很容易进行，这些小的步骤最后会成为一个大的重构，

> 如果有人和你说他把代码重构坏了，那么你应该很确定他不是在重构。

我使用 “restructuring” 这个词来指代对 code base 的任何 reorganizing 或者 cleaning up ，并且把重构看作是 “restructuring” 的一种。第一次见我重构的人可能会认为这样效率很低，我进行很多很小的步骤来重构，而不是一次性修改很大一部分。但最终正是这些小步骤让我能够更快地进行下去，因为我根本不用花费很多时间去 debugging。

在我的定义里，我提到了“软件对外的行为”，这里的意思简单来说就是在重构完之后程序应该和重构完之前有一样的功能。并不是说程序内部运行和重构之前一模一样，比如调用栈，性能等等。这里的功能应该是指用户所真正关心的功能。

重构和性能优化非常相似，因为它们都不应该改变程序的功能。区别在于彼此的目的：重构总是为了“使代码更容易被理解和更改”，这可能使程序变慢但也可能变快。对性能优化而言，我只考虑怎么使程序变快，有时不可避免的会需要难以理解的代码。

## 两顶帽子

Kent Beck 对此有一个 two hats 的比喻。当你使用重构来开发软件的时候，你把时间分配在两个不同的方面：增加新功能和重构。当我增加新功能的时候，我不应该改变原有的代码；我只是在增加新的功能。我通过增加新的测试来衡量我的工作进程。当我重构的时候，我不应该增加新的功能；我只是在 restructure 我的代码。我不用增加任何的测试（除非我发现之前有遗漏）。

当我自己开发软件的时候，我发现我经常换着戴这两顶“帽子”。我刚开始是想增加新的功能，但有时候会发现如果我重新组织代码，会更容易达到我增加新功能的目的。所以我换帽子到“重构”了。一旦代码有了更好的组织结构，我再换帽子到“增加功能”，并且增加功能。一旦新功能增加好了，我可能又意识到我写的代码实在太难理解了，所以我又换帽子并且重构并且重构。整个过程可能只有十分钟，但是我总是有意识的知道我在干什么。

## 为什么要重构？

我不会说“重构可以解决软件的所有问题”，它不是 “silver bullet”（银弹）。但它是一种珍贵的控制代码的工具，并且可以或者说应该被广泛使用。

## 重构提高软件的设计

如果没有重构，软件的结构会有衰变的倾向。随着开发者为了各种短期目的改变代码（常常没有很好的理解架构），代码会渐渐地失去原有的结构，并且这会有一种累积的效果，代码越是难读懂，开发者越是会乱改，于是代码的结构衰变的越快。定期的重构有助于维护代码的结构。

设计不良的代码通常会需要更多的代码来做同一件事，经常是因为在很多地方散落了重复的代码。因此改善设计的一个重要方面就是要避免重复代码。并不是因为减少了代码数量系统会变得那么的快而重要，减少重复代码会对你以后改变代码起巨大的作用。代码越多，越是难正确地去修改，因为你要理解更多的代码。我在一个地方修改了代码，但是系统并没有按我理解的去运行，这是因为在另一个地方也有同样的代码但我却没有修改。通过避免重复代码，我能够保证某部分代码 `says everything once and only once`，这正是良好设计的核心。

## 重构使软件更容易被理解

编程从许多意义上来说，其实是一种和计算机进行的沟通。我们写代码来告诉计算机该做什么，然后计算机准确地去做，不会多做也不会少。我们及时地减少我想让计算机做什么和我告诉计算机做什么之间的差距，编程其实就是关于如何准确地把我想做的事传达给计算机。但很有可能我们的代码会被其他用户使用，或许几个月后，另一个开发者会尝试着去阅读我的代码，并做一些他需要的变化。我们经常忽视了别人可能会阅读并修改我们的代码，而这恰恰是最重要的。就算计算机要多进行几个时钟循环来编译那又怎么样呢？但如果你的代码需要另一个程序员花几周甚至几个月才能正确理解，那就真的很重要了。

问题在于当我们自己编程的时候，我们只想要实现好我们的需求，而根本不会去想未来的那个开发者。重构帮助我们使得代码可读性更强。在重构之前，我们的代码可以工作但没有很好的结构。花那么一点时间来重构可以使我们的代码更好的阐明我们真正的意图。

我并不是无私的为其他开发者来重构，其实很多情况下那个其他开发者正是我们自己。所以这就使得重构更加重要了。我个人是非常懒非常健忘的，我健忘的一个形式就是我从来不记得我之前写过的代码。事实上我刻意地不去记以前写过的代码，因为我怕我的小脑袋会爆炸。我把我要记的所有东西都放在了重构好的代码里，那样我可以少担心点我的脑细胞。

## 重构利于我发现 Bug

有助于理解代码也就同时意味着有助于发现 bug。我必须承认我不是一个特别善于发现 bug 的程序员。有的天才可以通过读一大段复杂的代码并发现其中的 bug，而我却办不到。然而我有自己的办法，我发现如果我重构代码，我会试着去很深入地了解我的代码，并且把这种理解又体现在重构的代码里。通过使程序的结构变得更加清晰，甚至是我都能发现之前根本发现不了的 bug！

这让我想起了 Kent Beck 经常提起的关于自己的一句话：“我不是个伟大的程序员，我只是个还不错的程序员多加了点伟大的习惯而已。”重构帮助我更有效地书写健壮的代码。

## 重构帮助我更快地编程

归根结底，所有之前提到的导致的最终结果：重构帮助我更快地编程。

这听起来似乎是违反直觉的。当我讨论重构的时候，人们会自然而然的明白这会提高软件质量，比如更好的内部设计，可读性，减少 bug。但当你花时间重构，你不是减慢了开发效率吗？

根据我和一些开发者接触后所得到的经验是，他们通常一开始会进行地很快，但慢慢的增加新功能会变得越来越慢。每个新的功能都需要更多的时间来理解当前的代码，理解怎么把新代码加入到老代码里。这还不算完，就算你成功加入了这个新功能，通常你都会花很多的时间来修复可能会出现的各种 bug。慢慢的，你的代码会看起来像被各种补丁所组成，要准确地理解简直像在进行考古工作。于是有的开发者就会觉得这样的开发状态还不如自己从头开始实现。

下面的图是一个很好的例子：

![image](https://github.com/byelaney/Refactoring-improving-the-design-of-existing-code/blob/master/chapter-2/img/p0049_01.jpg)

当然也有的开发者有不一样的故事。他们很快就能添加新功能因为之前的代码写的非常好：

![image](https://github.com/byelaney/Refactoring-improving-the-design-of-existing-code/blob/master/chapter-2/img/p0049_02.jpg)

这两者的区别就在于软件内部的质量。质量高的代码允许我很容易地就知道该怎么加代码，以及加在哪里来实现我要的新功能。好的模块化让我只要理解全部代码的一部分就能添加新功能。如果代码非常清晰，那么同时我引入 bug 的几率也会大大降低，就算我引入了 bug，debug 也会变得没那么困难。慢慢的，你的 code base 就会变成类似一个平台，允许各种开发者在上面添加功能。

我把这种现象称为 Design Stamina Hypothesis。通过我们对软件内部设计的努力，我们增加了软件的耐受力，这份耐受力能让我们的软件走的更快走的更远。尽管我没有严格的证明，但它和我，以及我所认识的成百上千的杰出工程师的经验是吻合的。

20 年前，约定俗成的做法是在写任何一行代码之前，就对软件进行良好的设计。但现在重构改变了一切，我们明白了我们可以一边开发一边重构，由于一开始的设计几乎不可能是非常完美的，这也越发体现了重构的重要性。

## 我们什么时候该重构？

我几乎每个小时都在重构，我总结了以下几点：

> **The Rule of Three**
>
> 以下是 Don Roberts 给我的建议: 你第一次要写一段代码的时候，你只管写就是了。你第二次要写一段代码（和第一次的代码做的事非常类似）的时候，你稍微瞟一眼之前的实现，但也不用想太多，只管写就行。你第三次碰到类似的需求时，你才进行重构。
>
> 如果你喜欢棒球：Three strikes, then you refactor。

### 预备的重构--使加入新功能变得简单

重构的最佳时机是在你需要给 code base 添加新功能之前。我是这样做的，我先查看已经存在的代码，并且尝试着如果我改变一点原来的结构，那么添加新功能会不会就变得很简单？有可能我要的功能有一个函数已经实现了，只是和我的要求有些细微的区别。如果我不去重构，我可能只是单纯的拷贝这个函数并且适当修改。

“就像我想往东前进 100 英里，但是往东需要穿过茂密的丛林。我可以先往北前进 20 英里开上高速公路，然后再以三倍于直接往东的速度往东前进。”（磨刀不误砍柴工）

同样的故事也发生在修 bug 的时候。当我发现了一个问题的 root cause，我同时发现如果我能重构一下（把 copy-paste 的代码整合，或者分离相关逻辑等等），我能真正把这个 bug 修复，整理了代码结构之后也有助于避免其他开发者陷入一样的弯路。