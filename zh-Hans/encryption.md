# 加密

我们想保守秘密。与其把所有的数据保存在我们的脑子里或保卫一个物理副本，我们可以对这些数据进行加密，使其以某种对不能解密的人没有意义的形式存在。

在本章中，我们主要讨论两种类型的加密。**对称加密**（symmetric encryption）和**非对称加密**（asymmetric encryption）。我们将处理原始数据、加密数据和**密钥**（key）。密钥是用于转换数据的参数。理想情况下，加密的数据应该看起来像随机数据，而用不同（甚至相似）的密钥加密的数据应该也没有任何相似之处。这样的加密算法已经由科学家设计好了。

## 对称加密

对称加密有一个密钥。这个密钥用于加密和解密数据。

对称加密适用于预先共享密钥的存储和内部网络。如果密钥与未加密的数据一起被共享，那么加密*总是*可以被连接承担者破解。

### 典型工作流程

- Alice 和 Bob 知道密钥。
- Alice 拥有原始数据。
- Alice 用密钥对原始数据进行加密。
- Alice 将加密后的数据发送给 Bob。
- Bob 用密钥解密了数据。
- Bob 拥有原始数据。

## 非对称加密

非对称加密有一对密钥。当其中一个密钥被用来加密数据时，只有另一个密钥可以被用来解密。请记住，发送方知道原始数据。

通常情况下，一个密钥是公开的，被称作公钥；另一个密钥是保密的，被称作私钥。这种设计允许安全的通信。

### 理想工作流程

- 每个人都知道其他人的公钥。
- Alice 拥有原始数据。
- Alice 用 Bob 的公钥对原始数据进行加密。
- Alice 将加密后的数据发送给 Bob。
- Bob 用他的私钥对数据进行解密。
- Bob 拥有原始数据。

请注意，Alice 并没有用自己的私钥进行加密，否则任何能接触到加密数据的人都能解密它。

然而，在现实世界中，要知道每个人的公钥是不现实的。密钥必须在连接开始时共享。在这种情况下，连接承担者总是可以进行**中间人**（man-in-the-middle，MITM）攻击。下一节将说明这种攻击是如何进行的。目前世界上有一种广泛使用的（而且是**可靠**的）防止MITM攻击的方法，我们将在「签名和 CA」一章中讨论这个问题。


### 具有实时密钥共享和中间人攻击的工作流程

- Alice 拥有原始数据。
- Alice 要求 Bob 提供他的公钥，这个请求最后落到了 Charles 的手里。
- Charles 要求 Bob 提供他的公钥。鲍勃用他的公钥回应。
- Charles 用他自己的公钥回应 Alice。
- Alice 用 Charles 的公钥对原始数据进行加密。
- Alice 将加密后的数据发送给 Bob，最后落到了 Charles 的手中。
- Charles 用他的私钥对数据进行解密。
- Charles 拥有原始数据。
- Charles 可以选择修改原始数据。
- Charles 用 Bob 的公钥对原始数据进行加密。
- Bob 用他的私钥对数据进行解密。
- Bob 拥有（可能被 Charles 修改过的）原始数据。

## 不安全因素

暴力破解意味着不断尝试用不同的密钥进行解密，直到找到正确的密钥。为了最大限度地防止这种情况，请使用尽可能随机的密钥。即使大多数程序使用你的密码的加密哈希值作为密钥，但请记住，攻击者可以尝试流行的密码和单词组合，所以在设置密码时要遵循典型建议。

如果有任何方法可以比使用暴力更快地解密数据（计算成本更低），那么一种加密算法就被认为是被破解了的。然而，这些可以在不同的层次上进行，有些比其他的更能加快尝试的速度。