---
title: 关于 | About
description: 网络安全研究员 | 渗透测试工程师 | 技术分享者
aliases:
  - about-us
  - contact
license: CC BY-NC-ND
menu:
    main: 
        weight: -90
        params:
            icon: user
---

<div class="about-container">

## 👋 你好，我是 Slimer

欢迎来到我的个人博客！我是一名专注于网络安全领域的技术研究员和渗透测试工程师。

### 🎯 专业领域

- **Web安全攻防** - 深入研究各种Web漏洞的发现与防护
- **内网渗透测试** - 企业内网安全评估与红队演练
- **漏洞研究** - 关注最新安全漏洞，进行深度分析
- **安全工具开发** - 开发实用的安全工具和脚本
- **应急响应** - 安全事件分析与处置

### 🛠️ 技术栈

- **编程语言**: Python, JavaScript, PHP, Java, C/C++
- **安全工具**: Burp Suite, Nmap, Metasploit, Cobalt Strike
- **操作系统**: Linux, Windows, macOS
- **云平台**: AWS, Azure, 阿里云
- **数据库**: MySQL, PostgreSQL, MongoDB

### 📚 学习与分享

我热衷于技术分享，在这个博客中记录我的学习心得和实践经验：

- 🔍 **渗透测试实战** - 分享真实的渗透测试案例
- 🎯 **靶场练习** - 各种CTF和靶场的解题思路
- 🛡️ **安全防护** - 蓝队视角的安全防护策略
- 🔧 **工具使用** - 安全工具的使用技巧和开发经验
- 📖 **技术研究** - 最新安全技术的研究与分析

### 🏆 认证与荣誉

- 持有多个网络安全相关认证
- 参与过多个企业级安全项目
- 在多个安全会议分享过研究成果

### 📞 联系方式

如果你对我的文章有任何疑问，或者想要交流技术问题，欢迎通过以下方式联系我：

- **GitHub**: [SSlimes](https://github.com/SSlimes)
- **Bilibili**: [个人空间](https://space.bilibili.com/274685458)
- **邮箱**: 通过GitHub或Bilibili私信联系

### 💡 关于这个博客

这个博客使用 [Hugo](https://gohugo.io/) 构建，采用 [hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack) 主题，并进行了个性化定制。

---

<div class="about-footer">
<p>感谢你的访问！希望我的分享能对你有所帮助 🚀</p>
</div>

</div>

<style>
/* 全局容器样式 */
.about-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px;
  line-height: 1.8;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 24px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1), 0 4px 20px rgba(255, 105, 180, 0.08);
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.5);
}

/* 鼠标悬浮效果 */
.about-container:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 50px rgba(0, 0, 0, 0.15), 0 6px 30px rgba(255, 105, 180, 0.12);
}

/* 顶部渐变装饰条 */
.about-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #ff69b4, #ff1493, #8a2be2);
  border-radius: 24px 24px 0 0;
}

/* 装饰元素 */
.about-container::after {
  content: '';
  position: absolute;
  top: -100px;
  right: -100px;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(255, 105, 180, 0.05) 0%, transparent 70%);
  border-radius: 50%;
  z-index: 0;
}

/* 主要标题样式 */
.about-container h2 {
  color: var(--accent-color);
  margin-top: 40px;
  margin-bottom: 25px;
  font-size: 2.2em;
  font-weight: 700;
  text-align: center;
  position: relative;
  z-index: 1;
}

/* 第一个标题特殊样式 */
.about-container h2:first-of-type {
  margin-top: 20px;
  color: #ff69b4;
  font-size: 2.5em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.05);
  background: linear-gradient(90deg, #ff69b4, #ff1493, #8a2be2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  padding-bottom: 10px;
}

/* 二级标题样式 */
.about-container h3 {
  color: var(--card-text-color-main);
  margin-top: 40px;
  margin-bottom: 20px;
  font-size: 1.5em;
  font-weight: 600;
  position: relative;
  z-index: 1;
  padding-left: 15px;
  border-left: 4px solid #ff69b4;
}

/* 列表样式 */
.about-container ul {
  margin: 20px 0;
  padding-left: 20px;
  position: relative;
  z-index: 1;
}

/* 列表项样式 */
.about-container li {
  margin: 12px 0;
  padding: 12px 18px;
  background: rgba(255, 255, 255, 0.8);
  border-left: 3px solid #ff69b4;
  border-radius: 8px;
  color: var(--card-text-color-secondary);
  transition: all 0.3s ease;
  list-style-type: none;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  position: relative;
  overflow: hidden;
}

/* 列表项装饰 */
.about-container li::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 4px;
  height: 100%;
  background: linear-gradient(to bottom, #ff69b4, #ff1493);
  border-radius: 4px 0 0 4px;
}

/* 列表项悬停效果 */
.about-container li:hover {
  background: rgba(255, 255, 255, 0.95);
  transform: translateX(8px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

/* 段落样式 */
.about-container p {
  color: var(--card-text-color-secondary);
  margin: 18px 0;
  font-size: 1.05em;
  position: relative;
  z-index: 1;
  line-height: 1.8;
}

/* 引用样式 */
.about-container blockquote {
  background: rgba(255, 105, 180, 0.1);
  border-left: 4px solid #ff69b4;
  border-radius: 12px;
  margin: 30px 0;
  padding: 25px 30px;
  box-shadow: var(--shadow-l1);
  position: relative;
  z-index: 1;
  transition: transform 0.3s ease;
}

/* 引用悬停效果 */
.about-container blockquote:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
}

/* 引用段落样式 */
.about-container blockquote p {
  margin: 0;
  font-weight: bold;
  color: var(--card-text-color-main);
  font-size: 1.1em;
  line-height: 1.6;
}

/* 页脚样式 */
.about-footer {
  text-align: center;
  margin-top: 60px;
  padding: 30px;
  background: linear-gradient(135deg, rgba(255, 105, 180, 0.1), rgba(138, 43, 226, 0.1));
  border-radius: 15px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
  position: relative;
  z-index: 1;
  border: 1px solid rgba(255, 255, 255, 0.6);
}

/* 页脚段落样式 */
.about-footer p {
  margin: 0;
  font-size: 1.2em;
  color: var(--accent-color);
  font-weight: 600;
  background: linear-gradient(90deg, #ff69b4, #ff1493, #8a2be2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* 链接样式 */
.about-container a {
  color: #ff69b4;
  text-decoration: none;
  transition: all 0.3s ease;
  font-weight: 500;
  position: relative;
  padding: 0 2px;
}

/* 链接悬停效果 */
.about-container a:hover {
  color: #ff1493;
}

/* 链接下划线动画 */
.about-container a::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -2px;
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #ff69b4, #ff1493);
  transition: width 0.3s ease;
}

.about-container a:hover::after {
  width: 100%;
}

/* 分隔线样式 */
.about-container hr {
  border: none;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(255, 105, 180, 0.3), transparent);
  margin: 40px 0;
  position: relative;
  z-index: 1;
}

/* 专业领域和技术栈卡片网格布局 */
.about-skills-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
  margin: 25px 0;
  position: relative;
  z-index: 1;
}

/* 技能卡片样式 */
.skill-card {
  background: rgba(255, 255, 255, 0.85);
  padding: 25px;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.6);
}

/* 技能卡片悬停效果 */
.skill-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.1);
  background: rgba(255, 255, 255, 0.95);
}

/* 技能卡片标题样式 */
.skill-card h4 {
  color: var(--card-text-color-main);
  margin-top: 0;
  margin-bottom: 15px;
  font-size: 1.2em;
  font-weight: 600;
}

/* 技能卡片列表样式 */
.skill-card ul {
  margin: 0;
  padding-left: 15px;
}

/* 技能卡片列表项样式 */
.skill-card li {
  margin: 8px 0;
  padding: 6px 12px;
  background: none;
  border-left: 2px solid #ff69b4;
  border-radius: 0 6px 6px 0;
  box-shadow: none;
}

.skill-card li:hover {
  transform: translateX(3px);
  background: rgba(255, 105, 180, 0.05);
}

/* 响应式设计 */
@media (max-width: 768px) {
  .about-container {
    padding: 25px;
    margin: 15px;
    border-radius: 20px;
  }
  
  .about-container h2 {
    font-size: 1.8em;
  }
  
  .about-container h2:first-of-type {
    font-size: 2em;
  }
  
  .about-container h3 {
    font-size: 1.3em;
    margin-top: 30px;
  }
  
  .about-container blockquote {
    padding: 20px;
  }
  
  .about-footer {
    padding: 25px 20px;
    margin-top: 40px;
  }
  
  .about-skills-grid {
    grid-template-columns: 1fr;
  }
  
  .skill-card {
    padding: 20px;
  }
}

/* 暗色模式适配 */
@media (prefers-color-scheme: dark) {
  .about-container {
    background: rgba(18, 18, 18, 0.9);
    border: 1px solid rgba(40, 40, 40, 0.5);
  }
  
  .about-container::after {
    background: radial-gradient(circle, rgba(255, 105, 180, 0.03) 0%, transparent 70%);
  }
  
  .about-container li {
    background: rgba(30, 30, 30, 0.8);
    border-left: 3px solid #ff69b4;
  }
  
  .about-footer {
    background: linear-gradient(135deg, rgba(255, 105, 180, 0.05), rgba(138, 43, 226, 0.05));
    border: 1px solid rgba(40, 40, 40, 0.6);
  }
  
  .skill-card {
    background: rgba(30, 30, 30, 0.85);
    border: 1px solid rgba(40, 40, 40, 0.6);
  }
  
  .skill-card:hover {
    background: rgba(30, 30, 30, 0.95);
  }
}
</style>
</style>
