# Phira LSPosed 模块

**最新正式版：[v0.6.5](https://github.com/yskzctx/phira-lsposed/releases/tag/v0.6.5)**

> **未经过真机 LSPosed 验收。**

运行时附加，**不修改游戏 APK、不改签名、不重打包**。保留宿主登录、联网和原版上传流程，不绕过服务器、账号、购买或反作弊校验。

| 产物 | 目标 |
|---|---|
| `Phira-Lab-LSPosed-0.8.3-arm64-v0.6.5-TEST.apk` | Phira 0.8.3(40) arm64 |

覆盖安装后强停目标游戏再打开。

---

## 功能

### 判定保护（事后改判，不接管输入）

| 保护等级 | 效果 |
|---|---|
| 辅助 S | 低于保底分数时把判定改好 |
| 辅助 FC | Bad / Miss 改成 Perfect，Good 保留 |
| 辅助 AT | 任何非 Perfect 都改成 Perfect |

判定产生后改判，**不替你按键**。

### 结算页强制显示

- 分数、准确率、P/G/B/M、最大连击
- 结算符号：不干预 / Φ（全 Perfect）/ FC（无 Bad、Miss）
- Good 早/晚：可选开关

### 其他

- **结算时间误差**：目标 ±ms
- **Phira 历史记录**：目标 RKS
- **恢复历史成绩**
- **音符皮肤**：标准 Phira 音符图包
- **自制章节**：Phira 原生支持自制谱，此入口打开本地章节库

---

## 实现要点

### 保护不依赖可疑偏移

保护逻辑原本整个包在 `if (inner == observed_judge + 0x58)` 里。`0x58` 是判定对象上的硬编码字段偏移——**如果对当前 Phira 版本不对，分支永远不进，保护静默失效**。

决定要不要改判只需要判定值本身，所以改判逻辑已移出偏移检查，偏移仅用于模块自己的计数记账。

### 诊断日志

保护路径有有界诊断（只打 4 次，不刷屏）：

```
adb logcat -s PhiraLab | grep guard.diag
```

- `rescued>0` → 改判在工作
- `rescued=0` → 判定没被改
- `offsetMatched=0` → 偏移不匹配，但不影响改判

---

## 构建

```bash
python build_module.py \
    --sdk <Android SDK> --ndk <NDK> --xposed-api <api-82.jar> \
    --keystore <jks> --alias <alias> --password-file <txt> \
    --out <输出目录>
```

---

## 相关仓库

- [Phigros 模块](https://github.com/yskzctx/phigros-lsposed)
- [Phigros 离线修改版](https://github.com/yskzctx/phigros-offline-lab)

## 免责声明

见 [DISCLAIMER.md](DISCLAIMER.md)。
