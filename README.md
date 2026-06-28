# SecLab Simulation Rules

`seclab-sim-rules` 是 SecLab 协议仿真规则库，负责维护可导入主控的协议仿真规则资产，并生成独立发布的 `.slrp` 规则包。

规则库只负责规则资产、YAML 审计、Protobuf 载荷生成、签名和发布产物。主控负责规则导入、实例生命周期、端口调度和审计日志。

## 当前库存

当前规则包包含 30 条规则：

| 分类 | 数量 | 说明 |
| --- | --- | --- |
| `cve_sim` | 20 | 真实 CVE 仿真规则。 |
| `vuln_sim` | 4 | Redis 泛漏洞仿真规则。 |
| `honeypot` | 5 | HTTP、Redis 和邮件协议诱捕规则。 |
| `test_env` | 1 | HTTP 测试环境规则。 |

## 目录结构

```text
rules/
├── http/
│   ├── cve/
│   ├── honeypot/
│   └── test-env/
├── database/
│   └── redis/
└── mail/
    ├── smtp/
    ├── pop3/
    └── imap/
```

规则目录必须和 `category` 对齐。规则 ID 使用 6 位数字，`[1, 999_999]` 由官方规则库使用，`1_000_000+` 预留给用户自定义规则。

## 规则字段

每条规则使用 YAML 描述，必填字段包括：

- `id`
- `defaultPort`
- `name`
- `nameEn`
- `category`
- `vendor`
- `product`
- `severity`
- `references`
- `tags`
- `ruleVersion`
- `descriptionZh`
- `descriptionEn`
- `protocol`
- `configYaml`

`severity` 只允许 `critical`、`high`、`medium`、`low`、`info`。`references` 至少包含一个 HTTPS 来源链接。

## 构建与审计

```bash
cargo fmt
cargo test
cargo build --release
```

规则审计会检查：

1. YAML 字段完整性。
2. 规则 ID 分区。
3. 协议目录和分类一致性。
4. 协议配置 schema。
5. 规则总数和核心分类数量。

## 规则包

主控通过导入以下格式的规则包消费规则库：

```text
seclab-sim-rules-<version>.slrp
```

规则包包含规则清单、Protobuf 载荷、签名和版本声明。`min_seclab_version` 用于声明最低主控版本，`ruleset_format_version` 用于声明规则包格式版本。

## 发布

规则包通过 `Publish Simulation Rules` workflow 发布到 GitHub Release。

发布标签格式：

```text
sim-rules/v<version>
```

示例：

```text
sim-rules/v0.1.0-alpha.1
```

发布前需要在 GitHub Secrets 中配置：

- `SECLAB_SIGNING_PRIVATE_KEY`：规则包签名私钥文件内容。
- `SECLAB_SIGNING_PRIVATE_KEY_PASSWORD`：签名私钥密码；未加密私钥可留空。

## 契约

规则库和主控必须保持 Protobuf 字段、规则包格式、版本声明和 ID 分区一致。契约文档位于：

```text
docs/design/SecLab主控与规则库兼容性契约.md
```
