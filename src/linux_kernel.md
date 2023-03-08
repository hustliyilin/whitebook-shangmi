# Linux 内核国密支持

Linux 内核的算法往往是被开发者和用户忽略的一个基础算法库，Linux
内核结构复杂组件众多，随着部分用户态加解密机制下沉到内核，内核对加解密的需求还是非常丰富的，其次由于算法自身的无依赖，跨语言和跨平台的特点，内核有自身的一套完整的Crypto API 架构，并实现了非常丰富的算法，为内核中诸多的安全子系统提供支撑和服务。

Linux 内核Crypto API提供了以下的算法类型：

* 对称加密算法
* 带认证的 AEAD 算法
* 非对称密码算法
* 消息摘要，包括带密钥的消息摘要
* 随机数生成器
* 用户空间接口

内核Crypto API 提供单块密码和消息摘要的实现。 此外，内核加密 API 提供了许多可与单块密码和消息摘要结合使用的“模板”。 模板包括各类分组链接模式、HMAC机制等。

单块密码和消息摘要可以直接由调用者使用，也可以与模板一起调用以形成多块的分组密码或密钥消息摘要。

在具体的架构上，常用的算法是可以使用SIMD指令来优化，这一般会结合具体模式模板和单块密码，作为一个独立的算法注册到内核的 Crypto 子系统中，比如结合模式的 SM4-CTR 算法，在x86和arm64架构上，都是作为一个独立算法做过优化的，在其它架构平台上，可以选择使用 sm4 和 ctr 模板结合动态生成的`ctr(sm4)`算法。

通过`/proc/crypto`文件可以看到内核中所有支持的算法。

本小节内容会详细介绍内核中国密算法的支持情况以及结合内核国密的诸多应用场景。

{{#template template/footer.md}}