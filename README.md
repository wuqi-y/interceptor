# API 拦截器配置面板

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.4.2-blue.svg)](https://github.com/wuqi-y/interceptor)
[![Tampermonkey](https://img.shields.io/badge/Tampermonkey-Compatible-green.svg)](https://www.tampermonkey.net/)

> 🎯 功能强大的浏览器 API 拦截工具，支持 XHR 和 Fetch 请求拦截，提供可视化配置面板


## ✨ 主要功能

- 🎯 **双模式拦截**
  - 完全替换模式：直接替换整个响应内容
  - 部分修改模式：通过脚本灵活修改响应数据

- 🌐 **全局 Header 注入**
  - 统一管理请求头
  - 支持域名过滤
  - 可独立启用/禁用

- 🔒 **域名级别隔离**
  - 每个域名独立配置
  - 互不干扰
  - 方便多站点管理

- 📦 **配置导入导出**
  - 支持单域名配置导出/导入
  - 支持所有域名批量导出/导入
  - JSON 格式，方便分享和备份

- 🎨 **可视化配置面板**
  - 直观的 UI 操作
  - 实时预览
  - 支持拖拽悬浮按钮

- 📱 **响应式设计**
  - 完美适配 PC 端
  - 完美适配移动端
  - 触摸友好

- 🔄 **智能更新**
  - 自动检查更新
  - 多源降级机制
  - 网页内通知提示

- 🛡️ **安全可靠**
  - 只在顶层窗口运行
  - 不影响 iframe
  - 本地存储，隐私安全

## 🚀 快速安装

### 方法1：一键安装（推荐）

点击下方链接直接安装（需先安装 Tampermonkey）：

**jsDelivr CDN（全球加速，推荐）：**
```
https://cdn.jsdelivr.net/gh/wuqi-y/interceptor@main/simple_interceptor_panel.user.js
```

**GitHub Raw（国内可能较慢）：**
```
https://github.com/wuqi-y/interceptor/raw/main/simple_interceptor_panel.user.js
```

### 方法2：手动安装

1. 安装 [Tampermonkey](https://www.tampermonkey.net/) 浏览器扩展
   - [Chrome 商店](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
   - [Edge 商店](https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd)
   - [Firefox 商店](https://addons.mozilla.org/firefox/addon/tampermonkey/)

2. 复制 [`simple_interceptor_panel.user.js`](simple_interceptor_panel.user.js) 的内容

3. 在 Tampermonkey 管理面板中点击 ➕ 创建新脚本

4. 粘贴代码并保存（`Ctrl+S`）

## 📖 使用说明

### 基本操作

1. **打开配置面板**
   - 点击页面右下角的 ⚙️ 悬浮按钮

2. **移动悬浮按钮**
   - 长按按钮 200ms 后可拖动
   - 位置会自动保存

3. **启用拦截器**
   - 在面板顶部勾选"拦截器总开关"
   - 启用后需刷新页面生效

4. **添加拦截规则**
   - 填写规则名称
   - 填写匹配URL（支持部分匹配）
   - 选择拦截模式（完全替换/部分修改）
   - 输入响应数据或修改脚本
   - 点击"添加规则"

### 拦截模式详解

#### 1. 完全替换模式

直接替换整个响应内容，适合 Mock 固定数据。

**示例：Mock 用户信息**

```javascript
// 规则名称：Mock 用户信息
// 匹配 URL：/api/user/info
// 模式：完全替换
// 响应数据：
{
  "code": 200,
  "success": true,
  "data": {
    "id": 12345,
    "name": "测试用户",
    "email": "test@example.com",
    "vip": true,
    "balance": 9999.99
  },
  "message": "成功"
}
```

#### 2. 部分修改模式

使用 JavaScript 脚本修改原始响应，适合动态调整。

**示例：修改商品价格**

```javascript
// 规则名称：修改商品价格
// 匹配 URL：/api/product/detail
// 模式：部分修改
// 修改脚本：

// 修改价格为 0.01
modified.data.price = 0.01;
modified.data.discountPrice = 0.01;

// 修改库存
modified.data.stock = 999;

// 添加新字段
modified.data.isSpecial = true;

// 访问原始数据
console.log('原始价格:', originalData.data.price);

// 访问请求信息
console.log('请求URL:', requestInfo.url);
console.log('请求方法:', requestInfo.method);
```

**可用变量：**
- `modified` - 修改后的数据对象（修改它）
- `originalData` - 原始响应数据（只读）
- `requestInfo` - 请求信息对象
  - `requestInfo.url` - 请求URL
  - `requestInfo.method` - 请求方法（GET/POST等）
  - `requestInfo.timestamp` - 请求时间戳

### 全局 Header 配置

在"全局Header配置"区域可以添加需要注入的请求头：

**示例：添加认证 Token**

```
Header名称: Authorization
Header值: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
域名过滤: api.example.com
```

- **Header名称**：HTTP 请求头的名称
- **Header值**：请求头的值
- **域名过滤**：
  - 留空 = 对所有域名生效
  - 填写域名 = 只对指定域名生效
  - 支持多个域名，用逗号分隔

**常用 Header 示例：**

| Header名称 | 用途 | 示例值 |
|-----------|------|--------|
| `Authorization` | 身份认证 | `Bearer token123` |
| `X-Token` | 自定义Token | `abc123xyz` |
| `User-Agent` | 浏览器标识 | `Mozilla/5.0...` |
| `Referer` | 来源页面 | `https://example.com` |
| `X-Requested-With` | AJAX标识 | `XMLHttpRequest` |

### 配置管理

#### 当前域名操作

- **📤 导出当前域名**
  - 导出当前网站的配置为 JSON 文件
  - 文件名格式：`interceptor-domain.com-2025-01-15.json`

- **📥 导入到当前域名**
  - 从文件或文本导入配置
  - 会覆盖当前域名的所有配置

- **🔄 重置当前域名**
  - 清空当前网站的所有配置
  - 操作不可恢复，谨慎使用

#### 全局操作

- **📦 导出所有域名**
  - 导出所有已配置域名的数据
  - 适合整体备份和迁移
  - 文件名格式：`interceptor-all-domains-2025-01-15.json`

- **📥 导入所有域名**
  - 批量导入多个域名的配置
  - 会覆盖所有已存在的配置

- **⚠️ 清空所有配置**
  - 删除所有域名的配置
  - 双重确认，防止误操作

#### 从文本导入

支持直接粘贴 JSON 配置文本：

1. 点击文本框下方的"📥 从文本导入"按钮
2. 粘贴配置 JSON
3. 自动识别单域名或全局配置
4. 确认导入

### 已配置的域名

查看和管理所有已配置的域名：

- **查看配置信息**
  - 域名状态（启用/禁用）
  - 规则数量
  - Header数量

- **📋 复制配置**
  - 复制域名配置到剪贴板
  - 可在其他域名的"从文本导入"处粘贴使用

- **🗑️ 删除域名**
  - 删除指定域名的所有配置
  - 不能删除当前域名的配置

### 更新管理

- **🔍 检查更新**
  - 手动检查是否有新版本
  - 多源降级（jsDelivr → GitHub Raw → GitHub）
  - 发现新版本会显示通知

- **ℹ️ 更新信息**
  - 查看当前版本
  - 上次检查时间
  - 下次自动检查时间

**自动更新机制：**
- 每 24 小时自动检查一次
- 在浏览器启动后 3 秒执行
- 发现新版本会弹出通知

### 存储管理

- **👁️ 查看存储**
  - 查看所有存储的数据
  - 数据会复制到剪贴板
  - 控制台显示详细信息

- **📤 导出存储**
  - 导出所有原始存储数据
  - 包含所有配置和设置
  - JSON 格式

- **🗑️ 清空存储**
  - 清空所有存储数据
  - 双重确认，防止误操作

## 🎯 使用场景

### 场景1：前端开发 Mock 数据

**需求**：测试用户信息展示

```javascript
// 规则：Mock VIP用户
// URL：/api/user/info
{
  "code": 200,
  "data": {
    "vip": true,
    "vipLevel": 5,
    "vipExpire": "2099-12-31"
  }
}
```

### 场景2：调试特殊状态

**需求**：测试错误处理

```javascript
// 规则：模拟服务器错误
// URL：/api/submit
{
  "code": 500,
  "success": false,
  "message": "服务器内部错误"
}
```

### 场景3：绕过前端限制

**需求**：解除下载限制

```javascript
// 规则：解除下载限制
// URL：/api/download/check
// 模式：部分修改
modified.data.canDownload = true;
modified.data.remainCount = 999;
```

### 场景4：测试权限功能

**需求**：测试管理员界面

```javascript
// 规则：提升用户权限
// URL：/api/user/permission
// 模式：部分修改
modified.data.role = 'admin';
modified.data.permissions = ['read', 'write', 'delete', 'admin'];
```

### 场景5：API 接口联调

**需求**：修改请求头进行跨域调试

```
Header名称：Origin
Header值：https://trusted-domain.com
域名过滤：api.example.com
```

### 场景6：性能测试

**需求**：模拟大量数据

```javascript
// 规则：返回大量列表数据
// URL：/api/list
// 模式：部分修改
modified.data.list = Array(1000).fill(null).map((_, i) => ({
  id: i,
  name: `Item ${i}`,
  value: Math.random() * 100
}));
```

## ⚙️ 高级技巧

### 1. 规则优先级

当多个规则匹配同一 URL 时，只有一个规则会生效：

- 启用其中一个规则时，其他相同匹配的规则会自动禁用
- 确保同一 URL 只有一个规则生效
- 避免冲突和混淆

### 2. URL 匹配技巧

支持部分匹配，推荐使用：

```javascript
// ✅ 推荐：使用关键路径
/api/user/info

// ✅ 推荐：包含参数
/api/product/detail?id=

// ✅ 推荐：匹配域名
https://api.example.com/

// ⚠️ 不推荐：过于宽泛
/api/
```

### 3. 脚本调试技巧

在部分修改模式中使用 `console.log` 调试：

```javascript
// 查看原始数据结构
console.log('原始数据:', originalData);

// 查看请求信息
console.log('请求:', requestInfo);

// 修改前后对比
console.log('修改前:', modified.data.price);
modified.data.price = 0.01;
console.log('修改后:', modified.data.price);

// 条件修改
if (originalData.data.price > 100) {
  modified.data.price = 99.99;
  console.log('已降价');
}
```

### 4. 配置复用

**方法1：导出后分享**
1. 配置好规则后导出
2. 将 JSON 文件分享给其他人
3. 其他人导入即可使用

**方法2：跨域名复制**
1. 在"已配置的域名"列表中点击"复制配置"
2. 切换到目标域名
3. 在"从文本导入"处粘贴

### 5. 控制台调试接口

在浏览器控制台中使用调试接口：

```javascript
// 查看所有存储的键
__interceptor_debug.listKeys()

// 查看所有域名配置
__interceptor_debug.getValue('interceptor_all_domain_configs')

// 查看按钮位置
__interceptor_debug.getValue('interceptor_float_btn_position')

// 修改按钮位置
__interceptor_debug.setValue('interceptor_float_btn_position', {
  bottom: 100,
  right: 100
})

// 导出所有数据
var backup = __interceptor_debug.exportAll()
console.log(backup)

// 清空所有存储
__interceptor_debug.clearAll()
```

### 6. 正则匹配（进阶）

虽然目前是字符串匹配，但可以通过脚本实现正则：

```javascript
// 在修改脚本中判断
if (/\/api\/product\/\d+/.test(requestInfo.url)) {
  modified.data.price = 0.01;
}
```

## 🐛 常见问题

### Q1: 拦截器不生效？

**解决方法：**
1. 确认拦截器总开关已启用（绿色）
2. 确认规则已启用（蓝色边框）
3. 确认 URL 匹配正确（查看控制台日志）
4. 刷新页面使配置生效
5. 检查是否在 iframe 中（脚本不在 iframe 中运行）

### Q2: 看不到悬浮按钮？

**解决方法：**
1. 检查脚本是否已启用
2. 检查页面是否在 iframe 中
3. 尝试刷新页面
4. 检查是否被其他元素遮挡
5. 在控制台执行：`document.getElementById('interceptor-float-btn')`

### Q3: 配置丢失了？

**解决方法：**
1. 配置存储在浏览器本地，清除浏览器数据会丢失
2. 建议定期导出配置备份
3. 使用"查看存储"检查数据是否还在
4. 如果数据还在，尝试刷新页面

### Q4: 无法导入配置？

**解决方法：**
1. 确认 JSON 格式正确（使用 JSON 校验工具）
2. 确认文件编码为 UTF-8
3. 尝试从文本导入而不是文件导入
4. 查看控制台的错误信息

### Q5: 部分修改脚本报错？

**解决方法：**
1. 检查 JavaScript 语法是否正确
2. 确认使用的变量名正确（`modified`, `originalData`, `requestInfo`）
3. 使用 `console.log` 查看数据结构
4. 查看浏览器控制台的错误信息
5. 参考文档中的示例代码

### Q6: Header 注入不生效？

**解决方法：**
1. 确认"启用全局Header注入"已勾选
2. 确认 Header 已启用（绿色背景）
3. 确认域名过滤正确（留空或匹配当前域名）
4. 刷新页面使配置生效
5. 在开发者工具 Network 中查看请求头

### Q7: 更新检查失败？

**解决方法：**
1. 检查网络连接
2. 尝试关闭代理/VPN
3. 脚本会自动尝试多个更新源
4. 可以手动访问 GitHub 仓库下载新版本

### Q8: 跨域请求授权提示？

**解决方法：**
1. 点击"始终允许"
2. 这是 Tampermonkey 的安全机制
3. 只会提示一次
4. 或者在脚本元数据中已添加 `@connect` 权限

## 📋 更新日志

### v1.4.2 (2025-01-15)

**新增功能**
- ✨ 新增：网页内通知系统，替代系统通知
- ✨ 新增：使用 fetch 检查更新，避免跨域授权提示
- ✨ 新增：存储管理功能（查看/导出/清空）
- ✨ 新增：更新信息查看功能

**优化改进**
- 💄 优化：更新检查流程和用户提示
- 💄 优化：通知显示时机和动画效果
- 💄 优化：配置面板布局和样式

**Bug 修复**
- 🐛 修复：点击取消后复选框状态异常
- 🐛 修复：拖拽方向反向问题
- 🐛 修复：点击拖拽按钮无法打开面板

### v1.4.1 (2025-01-14)

**新增功能**
- ✨ 新增：域名级别配置隔离
- ✨ 新增：可拖拽悬浮按钮
- ✨ 新增：导出/导入所有域名配置
- ✨ 新增：弹窗式 JSON 编辑器
- ✨ 新增：规则冲突自动检测

**优化改进**
- 💄 优化：配置面板 UI 设计
- 💄 优化：移动端适配
- 💄 优化：配置导入导出流程

**Bug 修复**
- 🐛 修复：iframe 多次触发问题
- 🐛 修复：空配置域名保存问题
- 🐛 修复：长 JSON 编辑问题

### v1.4.0 (2025-01-10)

- 🎉 初始版本发布
- 🎯 支持 XHR 和 Fetch 拦截
- 🌐 支持全局 Header 注入
- 📦 支持配置导入导出
- 🎨 可视化配置面板

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

### 提交 Issue

- 🐛 Bug 报告：请详细描述复现步骤
- 💡 功能建议：说明使用场景和期望效果
- ❓ 使用问题：提供错误信息和环境信息

### 提交 PR

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

### 开发规范

- 代码风格：遵循项目现有代码风格
- 注释：关键逻辑添加中文注释
- 测试：确保功能正常工作
- 文档：更新相关文档

## 📄 许可证

本项目基于 [MIT License](LICENSE) 开源。

## 💬 问题反馈

- **GitHub Issues**: [提交问题](https://github.com/wuqi-y/interceptor/issues)
- **讨论区**: [参与讨论](https://github.com/wuqi-y/interceptor/discussions)

## ⭐ Star History

如果这个项目对你有帮助，请给个 Star ⭐

[![Star History Chart](https://api.star-history.com/svg?repos=wuqi-y/interceptor&type=Date)](https://star-history.com/#wuqi-y/interceptor&Date)

## 🙏 鸣谢

感谢所有贡献者和使用者的支持！

## 📚 相关资源

- [Tampermonkey 官网](https://www.tampermonkey.net/)
- [Greasyfork](https://greasyfork.org/)
- [Tampermonkey 文档](https://www.tampermonkey.net/documentation.php)

---

<div align="center">

Made with ❤️ by [wuqi-y](https://github.com/wuqi-y)

[⬆ 回到顶部](#api-拦截器配置面板)

</div>
