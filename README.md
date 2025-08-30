# CarGForce Monitor - 汽车G值与方向监测应用

![CarGForce Monitor](https://img.shields.io/badge/Version-1.0.0-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Platform](https://img.shields.io/badge/Platform-Web%20Mobile-lightgrey.svg)
![HTTPS](https://img.shields.io/badge/HTTPS-Required-red.svg)

一个基于Web的汽车运动监测应用，使用手机内置传感器实时监测车辆运动过程中的加速度（G值）和方向变化。

![应用预览]
<img width="2825" height="1672" alt="image" src="https://github.com/user-attachments/assets/d1c92ffc-6b1f-4dad-9f7b-89d8899d6c43" />


## 目录

- [功能特点](#功能特点)
- [HTTPS要求](#https要求)
- [技术栈](#技术栈)
- [快速开始](#快速开始)
- [使用说明](#使用说明)
- [API参考](#api参考)
- [项目结构](#项目结构)
- [开发指南](#开发指南)
- [常见问题](#常见问题)
- [贡献指南](#贡献指南)
- [许可证](#许可证)
- [更新日志](#更新日志)

## 功能特点

- 📊 **实时三轴加速度监测**：实时显示X（左右）、Y（前后）、Z（垂直）三个方向的加速度值
- 🧭 **方向指示器**：可视化显示车辆当前方向角度（0-360°）
- 📈 **数据图表**：实时绘制加速度变化曲线，记录历史数据
- ⚖️ **重力补偿**：自动校准并减去重力影响，提供更精确的运动加速度测量
- 📱 **响应式设计**：适配各种手机屏幕尺寸
- 🔧 **传感器校准**：提供简单易用的传感器校准功能
- 💾 **数据持久化**：可选的数据记录和导出功能

## HTTPS要求

**本项目强制要求使用HTTPS协议提供服务**。

### 为什么需要HTTPS？

1. **安全上下文要求**：现代浏览器要求DeviceMotion和DeviceOrientation API只能在安全上下文(HTTPS)中使用
2. **用户隐私保护**：HTTPS加密传输保护用户的传感器数据不被窃取
3. **浏览器策略**：大多数浏览器已限制非HTTPS网站访问传感器API

### 部署要求

- 生产环境必须使用有效的SSL证书
- 开发环境可使用localhost(浏览器视localhost为安全来源)或自签名证书
- 所有资源必须通过HTTPS加载，包括外部库(Chart.js等)

### 本地开发HTTPS设置

```bash
# 使用mkcert创建本地证书
mkcert -install
mkcert localhost

# 使用Node.js启动HTTPS服务器
npm install -g http-server
http-server -S -C localhost.pem -K localhost-key.pem

# 或使用Live Server with HTTPS
npm install -g live-server
live-server --https=/path/to/cert.pem --https=/path/to/key.pem
```

## 技术栈

- **HTML5** - 页面结构和内容
- **CSS3** - 响应式布局和动画效果
- **JavaScript (ES6+)** - 传感器数据处理和业务逻辑
- **Chart.js** - 数据可视化图表
- **DeviceMotion API** - 访问设备加速度计
- **DeviceOrientation API** - 访问设备方向传感器

## 快速开始

### 在线使用

1. 在手机浏览器中打开 [应用链接](https://your-domain.com/car-gforce-monitor)
2. 允许应用访问设备传感器权限
3. 按照提示进行传感器校准
4. 开始监测汽车运动数据

### 本地运行

```bash
# 克隆项目
git clone https://github.com/your-username/CarGForce-Monitor.git

# 进入项目目录
cd CarGForce-Monitor

# 生成SSL证书（开发环境）
mkcert localhost

# 启动本地HTTPS服务器
http-server -S -C localhost.pem -K localhost-key.pem

# 在手机浏览器中访问显示的HTTPS地址
```

## 使用说明

### 基本使用

1. 在手机浏览器中打开应用页面
2. 将手机固定在汽车内（建议使用手机支架）
3. 点击"开始校准"按钮，按照提示将手机平放在水平表面上
4. 校准完成后，点击"开始监测"按钮
5. 开始驾驶，观察实时加速度和方向变化

### 校准说明

为了获得准确的加速度测量值，请务必进行重力校准：

1. 将手机平放在水平表面上（屏幕朝上）
2. 点击"开始校准"按钮并保持手机静止3秒
3. 校准完成后，应用将自动减去重力影响
4. 校准后，手机静止时各轴加速度应显示接近0G

### 界面说明

- **X轴**：左右方向的加速度（左转为负，右转为正）
- **Y轴**：前后方向的加速度（加速为正，减速为负）
- **Z轴**：垂直方向的加速度（上升为正，下降为负）
- **合成加速度**：三轴加速度的矢量合成值
- **方向指示器**：显示车辆当前方向（0°=北，90°=东，180°=南，270°=西）

## API参考

### DeviceMotion Event

应用使用DeviceMotion API获取加速度数据：

```javascript
window.addEventListener('devicemotion', (event) => {
  const acceleration = event.accelerationIncludingGravity;
  // 处理加速度数据
});
```

### DeviceOrientation Event

应用使用DeviceOrientation API获取设备方向数据：

```javascript
window.addEventListener('deviceorientation', (event) => {
  const direction = event.alpha; // 0-360度
  // 处理方向数据
});
```

## 项目结构

```
CarGForce-Monitor/
├── index.html          # 主页面文件
├── styles/             # 样式文件目录
│   └── main.css        # 主要样式文件
├── scripts/            # 脚本文件目录
│   ├── app.js         # 主应用逻辑
│   ├── sensors.js     # 传感器处理模块
│   └── charts.js      # 图表处理模块
├── assets/            # 资源文件目录
│   └── icons/         # 应用图标
├── localhost.pem      # 开发环境SSL证书（不提交到Git）
├── localhost-key.pem  # 开发环境SSL私钥（不提交到Git）
└── README.md          # 项目说明文件
```

## 开发指南

### 环境要求

- 现代浏览器（Chrome 60+、Firefox 55+、Safari 11+）
- 支持DeviceMotion和DeviceOrientation API的设备
- HTTPS环境（生产或开发）

### 构建说明

本项目为纯前端应用，无需构建过程。直接部署HTML、CSS和JS文件即可。

### 自定义修改

- 修改`styles/main.css`可调整应用外观
- 修改`scripts/charts.js`可调整图表样式和行为
- 调整校准采样时间和算法可改变校准精度
- 可添加数据导出功能保存历史记录

## 常见问题

### Q: 为什么必须使用HTTPS？

A: 现代浏览器出于安全考虑，要求传感器API只能在安全上下文(HTTPS)中使用。这是浏览器的安全策略，无法绕过。

### Q: 本地开发如何设置HTTPS？

A: 可以使用mkcert工具生成本地证书，然后使用支持HTTPS的本地服务器。具体步骤见[HTTPS要求](#https要求)部分。

### Q: 为什么手机静止时Z轴显示1G？

A: 这是正常现象，因为加速度计测量的是包括重力在内的所有加速度。应用已添加重力补偿功能，校准后静止时显示接近0G。

### Q: 点击"开始监测"没有反应怎么办？

A: 请检查浏览器是否请求了运动传感器权限，并确保已授权。在Android设备的Chrome设置中，检查"网站设置"中的"运动传感器"权限。

### Q: 数据不准确怎么办？

A: 请尝试重新校准传感器，确保校准时手机放置在水平表面上并保持静止。

### Q: 哪些设备支持此应用？

A: 大多数现代智能手机都支持，包括iOS和Android设备。某些较旧设备可能不支持传感器API。

## 贡献指南

我们欢迎任何形式的贡献！请遵循以下步骤：

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个Pull Request

## 许可证

本项目采用MIT许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 更新日志

### v1.0.0 - 2023-11-05
- 初始版本发布
- 实现基本加速度和方向监测功能
- 添加重力补偿校准功能
- 实现实时数据图表显示
- 强制HTTPS要求

---

**注意**：驾驶时请勿操作手机，确保行车安全。本应用仅供参考，不应用于精确的工程测量。

如有问题或建议，欢迎提交Issue或联系开发者。
