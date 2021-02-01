# 项目背景

![WeDPR](https://wedpr-lab.readthedocs.io/zh_CN/latest/_static/images/wedpr_logo.png)

WeDPR是一系列**即时可用场景式**隐私保护高效解决方案套件和服务（参见[WeDPR白皮书](https://mp.weixin.qq.com/s?__biz=MzU0MDY4MDMzOA==&mid=2247483910&idx=1&sn=7b647dec9f046f1e6f94d103897f7efb&scene=19#wechat_redirect)），由微众银行区块链团队自主研发。方案致力于解决业务数字化中隐私不“隐”、共享协作不可控等隐私保护风险痛点，消除隐私主体的隐私顾虑和业务创新的合规壁垒，助力基于隐私数据的核心价值互联和新兴商业探索，营造公平、对等、共赢的多方数据协作环境，达成数据价值跨主体融合和数据治理的可控平衡。

WeDPR具备以下特色和优势：

- **场景式解决方案**：已基于具有共性的场景需求，提炼出公开可验证密文账本、多方密文决策、多方密文排名、多方密文计算、多方安全随机数生成、选择性密文披露等高效技术方案框架模板，可应用于支付、供应链金融、跨境金融、投票、选举、榜单、竞拍、招标、摇号、抽检、审计、隐私数据聚合分析、数字化身份、数字化资质凭证、智慧城市、智慧医疗等广泛业务场景。
- **即时可用**：高性能、高易用、跨平台跨语言实现、不依赖中心化可信服务、不依赖可信硬件、支持国密算法标准、隐私效果公开可验证，5分钟一键构建示例应用。
- **透明可控**：隐私控制回归属主，杜绝数据未授权使用，在『数据可用而不可见』的基础上，进一步实现数据使用全程可监管、可追溯、可验证。

WeDPR全面拥抱开放，将陆续开源一系列核心算法组件，进一步提升系统安全性的透明度，提供更透明、更可信的隐私保护效果。WeDPR-Lab就是这一系列开源的**核心算法组件**的集合。

为便于开发者**仅对WeDPR-Lab中的密码算法组件进行选择性使用**，我们将WeDPR-Lab中涉及的所有密码算法组件进行拆分、迁移，重新独立包装形成一个新的密码模块仓库WeDPR-Lab-Crypto。

首先，我们将WeDPR-Lab Core v1.3.0之前版本中的所有密码算法进行拆分，重新加入WeDPR-Lab-Crypto，拆分部分具体包括：

1. **核心密码算法组件**

    （1）基础编解码；

    （2）公钥加解密算法：ECIES加解密；

    （3）哈希算法；

    （4）签名及验证；

    （5）离散对数系统的零知识证明算法：加和证明及验证、乘积证明及验证；

    （6）零知识范围证明及验证。

2. **FFI接口**，支持交叉编译跨语言、跨平台所调用的FFI适配接口。

在此基础上，本次WeDPR-Lab Crypto v1.0.0又新增以下密码算法及其对应的FFI接口：

1.	国密SM2签名及验证算法

国密SM2为我国自主设计，基于椭圆曲线密码（Elliptic Curve Cryptography, ECC）的公钥密码算法，包括SM2-1椭圆曲线数字签名算法，SM2-2椭圆曲线密钥交换协议，SM2-3椭圆曲线公钥加密算法。

WeDPR-Lab Crypto v1.0.0新增的是其中的SM2-1椭圆曲线数字签名算法。

2.	国密SM3

国密SM3为我国自主设计的密码杂凑算法，也称消息摘要算法。适用于数字签名、消息认证码的生成与验证、随机数的生成等。

3.	基于椭圆曲线的可验证随机函数VRF(Verifiable Random Functions)

VRF包含：
    （1）密钥生成算法
    （2）证明生成算法
    （3）证明验证算法

VRF能够实现：只有私钥的持有者才能计算哈希，但任何拥有公钥的人都可以验证哈希的正确性。可用于随机数的生成，在区块链共识机制中具有广泛应用。

4.	FFI接口，支持交叉编译跨语言、跨平台所调用的FFI适配接口。

（说明：由于在进行密码算法组件迁移过程中存在接口变动，所以WeDPR-Lab-Crypto v1.0.0与WeDPR-Lab Core v1.3.0之前版本的密码算法可能存在部分接口不兼容的情况，未来我们会持续进行修复、更新。）

欢迎社区伙伴参与WeDPR-Lab的共建，一起为可信开放数字新生态的构建打造坚实、可靠的技术底座。

## 安装

### 安装Rust环境

安装nightly版本的Rust开发环境，可参考[Rust官方文档](https://www.rust-lang.org/zh-CN/tools/install)。
```bash
rustup default nightly
```

### 下载WeDPR-Lab-Crypto源代码

使用git命令行工具，执行如下命令。

```bash
git clone https://github.com/WeBankBlockchain/WeDPR-Lab-Crypto.git
```

## 编译

WeDPR-Lab-Crypto支持灵活的定制化算法选择，用户可根据业务场景、实际需求选择对应的密码算法。

譬如，若用户只希望使用“ECIES加解密算法”的Java调用接口，则只需在WeDPR-Lab Crypto的Java FFI目录下，打开ECIES加解密算法对应的特性进行编译，即进行如下条件编译即可：

```bash
cd ffi/ffi_java/ffi_java_crypto/ && cargo build --features "wedpr_f_base64, wedpr_f_ecies_secp256k1" --no-default-features
```

**../ffi目录下只有ffi_c与ffi_java会涉及条件编译，编译方法如下：**
（以ffi_c目录下的条件编译为例，ffi_java与之类似）

1. 进入ffi_c目录：

```bash
cd ffi/ffi_c/ffi_c_crypto
```

2. 查看当前目录下的Cargo.toml中的[features]，明确本目录所有的条件编译选项。

3. 使用cargo build进行编译时，默认打开了所有条件编译选项，编译完成后，即生成本目录下所有密码算法的调用接口。

4. 若只需使用部分密码算法的调用接口，则开启该密码算法对应的条件编译选项，编译时使用：

```bash
cargo build --features "一个或多个feature名"
```

其中，选择"一个或多个feature名"时，注意：

|    注意事项    |                         条件编译选项                         |
| :------------: | :----------------------------------------------------------: |
| 互斥条件编译项 |                 wedpr_f_base64, wedpr_f_hex                  |
| 必选条件编译项 |                 wedpr_f_base64或wedpr_f_hex                  |
| 可选条件编译项 | wedpr_f_ecies_secp256k1， wedpr_f_signature_secp256k1， wedpr_f_hash_keccak256， wedpr_f_signature_sm2， wedpr_f_hash_sm3， wedpr_f_vrf_curve25519 |

## 接口文档

### 生成并查看Rust SDK接口文档

在本项目的根目录（即`WeDPR-Lab-Crypto`目录）中，运行如下命令。

```bash
cargo doc --no-deps
```

以上命令将根据代码中的注释，在`target/doc`子目录中，生成的SDK接口文档。

进入`target/doc`文档目录后，会看到所有SDK相关的包名（包含WeDPR-Lab-Crypto和其他依赖包），进入其中任意一个包名的目录，用网页浏览器打开其中的`index.html`文件，便可查看WeDPR-Lab-Crypto相关的接口说明。

## 其他相关文档

- [WeDPR方案白皮书](https://mp.weixin.qq.com/s?__biz=MzU0MDY4MDMzOA==&mid=2247483910&idx=1&sn=7b647dec9f046f1e6f94d103897f7efb&scene=19#wechat_redirect)
- [WeDPR-Lab用户文档](https://wedpr-lab.readthedocs.io/zh_CN/latest/index.html)

## 项目贡献

- 点亮我们的小星星(点击项目右上方Star按钮)
- 提交代码(Pull Request)，参考我们的代码[贡献流程](./CONTRIBUTING.md)
- [提问和提交BUG](https://github.com/WeBankBlockchain/WeDPR-Lab-Core/issues)
