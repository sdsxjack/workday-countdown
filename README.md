# Workday Countdown Dashboard / 工作日倒计时看板

A lightweight, secure, and standalone self-hosted dashboard designed for precisely tracking statutory deadlines based on workdays (excluding weekends). 
一个轻量、安全、单文件自托管的工作日法定期限精准倒计时面板，专门用于跟踪排除周末后的法定办理时限。

It features client-side cryptography for security over plain HTTP networks, built-in task archiving, and seamless runtime language switching.
支持纯 HTTP 局域网环境下的前端哈希加密鉴权、已办结事务归档管理以及全站动态中英文一键切换。

---

## Features / 功能特性

* ⏱️ **Statutory Workday Calculation / 法定时限精准推算**
  Automatically excludes weekends (Saturdays and Sundays) and starts calculation from the day following the received date.
  自动排除周六、周日周末时间，严格按照“签收次日为起始日”的法定标准进行顺延计算。

* 🌐 **Dynamic Bilingual Interface / 原生中英文双语切换**
  Built-in dynamic runtime language switching (EN/ZH) that remembers your choice across sessions via local storage.
  全站文本支持纯前端一键无缝语言切换，利用浏览器本地缓存（Local Storage）记忆您的语言偏好，无需重复选择。

* 🔐 **Zero-Knowledge Password Authentication / 零信任前端密码鉴权**
  Uses a pure JavaScript local implementation of the SHA-256 algorithm. Password hashes are generated on the client side before submission.
  采用纯 JS 逻辑实现的 SHA-256 算法。密码在浏览器本地完成哈希加密后才向后端提交，杜绝了明文密码在普通 HTTP 局域网被抓包的风险。

* 📁 **Archive Management / 状态归档留存**
  Toggle between "Pending" and "Archived" views. Mark tasks as "Followed Up" to hide them from the main dashboard, with options to revert or permanently delete.
  支持“进行中”与“已办结”双页面分流。点击“已跟进”后事项自动从主页隐藏移入归档，并支持一键移回主页或永久删除。

* 🗄️ **Zero-Configuration SQLite / 零配置 SQLite 数据库**
  Powered by a single PHP file and an embedded SQLite database (`tasks.db`), featuring seamless database auto-migration for zero-loss updates.
  数据纯本地化托管，系统初次运行会自动检查并无损升级现有数据库（`tasks.db`），老数据绝不丢失。

* 📱 **Responsive Layout / 响应式双语面板**
  Embedded custom CSS grid supporting a live-clock display and optimized mobile viewing.
  完全内嵌所有 CSS 样式，自带右上角动态时钟，完美适配手机与网页端。

---

## Quick Start with Docker / 部署指南 (以飞牛 NAS 等 Docker 环境为例)

The easiest way to run this on a NAS or Linux server is using a lightweight Nginx+PHP bundle:
推荐使用极度精简的二合一集成镜像进行单容器部署：

1. **Prepare Directories / 准备代码文件**: 
   Create a directory on your host (e.g., `/vol1/docker/workday-countdown`) and place `index.php` inside it.
   在 NAS 上新建文件夹（如 `/vol1/docker/workday-countdown`），将 `index.php` 放入其中。

2. **Permissions / 数据库权限（核心注意点）**: 
   Create an empty file named `tasks.db` in that folder and grant it read/write permissions for all users (`chmod 777 tasks.db`).
   在同级目录下手动新建一个名为 `tasks.db` 的空白文件，并修改**属性/权限**赋予其“所有人可读写”权限，防止 PHP 写入报错。

3. **Run Container / 创建 Docker 容器**:
   ```bash
   docker run -d \
     --name workday-countdown \
     --restart always \
     -p 60001:80 \
     -v /vol1/docker/workday-countdown:/var/www/html \
     trafex/php-nginx:latest
