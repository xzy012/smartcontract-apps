# Solidity 智能合约开发流程
到目前为止我开发了不少部署在主网的智能合约，现在总结一下 Solidity 合约的开发流程。

## 需求&功能
确定客户需求，规划合约功能，一般来说这是归产品经理干的事情。但是由于智能合约的特殊性，智能合约开发也要参与智能合约功能的规划，最好能直接和客户沟通需求。因为智能合约需要运行在区块链上，所以智能合约实现功能和传统的软件有很大区别，不懂这点的客户会提出一些他以为实现简单的功能。在项目开始阶段就要果断砍掉不适合做在链上的功能，只留下必须使用智能合约实现的功能，不然后续的功能调整和互相扯皮能磨掉整个项目团队的时间和信心。

## 接口&数据
功能确定之后，就需要确定实现功能的接口。这里可能需要你和前后端的同学进行协调，但最终还是以你为准，因为智能合约功能的实现和调整成本最大，所以优先级也是最高的。做完之后接下来要确定存储数据的结构和位置，这需要使用最低的 gas 成本来存储项目数据，如果你认真学过 solidity 的数据结构，相信这并不难。

## 实现&优化
这里不得不提 openzeppelin 这个 solidity 库，常用的智能合约功能和接口都包括，可以大大节约智能合约开发的成本。如果你需要实现的功能就在里面，那么 cv 大法就是最佳选择，不要重复造轮子。如果你要实现的功能不在里面，那么其次的方法是在网上找相似功能的智能合约代码然后做改造。最后网上找不到，实在没办法了才应该自己想一个解决方案。这里我并不是鼓励偷懒，而是要求你慎重，智能合约的 bug 往往无法修正，即使用可升级合约修正了，巨大的损失也无法挽回。如果你技术高超，那么大可以在合约的gas 优化上下功夫。说到合约的 gas 优化，这门学问非常深奥，涉及 evm 的底层原理，这里就不多说了，有兴趣的可以去看 solidity 内联汇编。我其实并不推崇完全使用汇编写的合约，因为有的智能合约需要开源，最好让同行容易看懂，希望合约大佬们能在容易理解和 gas 优化下取个平衡。

## 测试&检查
测试是 Solidity 智能合约开发中最费时的阶段，根据项目需求可以分为本地测试、主网仿真测试和测试网测试。本地测试是指使用本地的区块链节点对合约功能进行测试。主网仿真测需要使用本地节点 fork 主网环境，再对合约功能进行测试。测试网测试需要在区块链官方指定的测试网络进行，一般是在前后端完成之后进行联调时使用。测试一般需要保证测试覆盖率在 95% 以上，确保所有的功能和报错都正确。除了测试，对合约进行常规 bug 的检查也十分必要，比如重入、溢出、可见性等，这需要了解常见的智能合约可利用 bug。

## 配置&部署
在不同的区块链网络上部署智能合约的配置数据不同，这就需要在配置文件上做专门的区分。部署时使用自动化部署脚本可以实现一键部署，并把部署结果写入 SDK 配置文件，可以大大提高合约的更新效率。

## SDK&文档
由于前后端往往不懂智能合约的配置和调用，因此需要智能合约开发提供给他们一套用来和合约对接的 sdk。sdk里要提供事先约定好的接口和数据，并给出对象初始化的数据。如果对方完全没有 dapp 的开发经验，往往还要教授 metamask 或者区块链节点的使用方法。智能合约作为项目数据的核心，良好的说明文档也是必不可少的，这不仅能降低前后端对接的成本，还能在以后的项目迭代中起到指引的作用。

## 结束

最后放一个我自己目前使用的 Solidity 智能合约框架，整个框架整合了hardhat和foundry，能够几乎实现上面提到的所有功能，希望对大家有用 https://github.com/33357/hardhat-foundry-example