# Changelog

格式遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，并遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [Unreleased]

### Changed in alpha development

- 重写规则包 v1，增加 `schema_version: 1` 与具名端点声明，并要求导入端实际验证 Ed25519/minisign 签名。
- 增加 Telnet、MySQL、PostgreSQL、SMB 和 LDAP 基础诱捕规则；格式版本仍保持 v1。
- 增加同时声明 `dns-tcp` 与 `dns-udp` 端点的 DNS 诱捕规则，默认主机端口为 1053，容器内保持 53/TCP 与 53/UDP。

### Added

- 首次发布 SecLab 协议仿真规则库。
- 提供 HTTP、Redis、SMTP、POP3 和 IMAP 协议仿真规则。
- 提供 CVE 仿真、泛漏洞仿真、诱捕规则和测试环境规则分类。
- 提供 YAML 规则审计、Protobuf 载荷生成和 `.slrp` 规则包交付能力。
- 提供规则包版本声明、最低主控版本声明和规则 ID 分区约束。
