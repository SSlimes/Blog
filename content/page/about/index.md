---
title: 关于| About
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



<div class="about-footer">
<p>感谢你的访问！希望我的分享能对你有所帮助 🚀</p>
</div>

</div>

<style>
.about-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 30px;
  line-height: 1.7;
  background: var(--card-background);
  border-radius: 20px;
  box-shadow: var(--shadow-l2);
  position: relative;
}

.about-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #ff69b4, #ff1493);
  border-radius: 20px 20px 0 0;
}

.about-container h2 {
  color: var(--accent-color);
  margin-top: 40px;
  margin-bottom: 25px;
  font-size: 2.2em;
  font-weight: 700;
  text-align: center;
}

.about-container h2:first-of-type {
  margin-top: 20px;
  color: #ff69b4;
  font-size: 2.5em;
}

.about-container h3 {
  color: var(--card-text-color-main);
  margin-top: 35px;
  margin-bottom: 20px;
  font-size: 1.4em;
  font-weight: 600;
}

.about-container ul {
  margin: 20px 0;
  padding-left: 20px;
}

.about-container li {
  margin: 12px 0;
  padding: 8px 15px;
  background: rgba(255, 105, 180, 0.05);
  border-left: 3px solid #ff69b4;
  border-radius: 0 8px 8px 0;
  color: var(--card-text-color-secondary);
  transition: all 0.3s ease;
}

.about-container li:hover {
  background: rgba(255, 105, 180, 0.1);
  transform: translateX(5px);
}

.about-container p {
  color: var(--card-text-color-secondary);
  margin: 18px 0;
  font-size: 1.05em;
}

.about-container blockquote {
  background: rgba(255, 105, 180, 0.1);
  border-left: 4px solid #ff69b4;
  border-radius: 0 10px 10px 0;
  margin: 30px 0;
  padding: 25px 30px;
  box-shadow: var(--shadow-l1);
}

.about-container blockquote p {
  margin: 0;
  font-weight: bold;
  color: var(--card-text-color-main);
  font-size: 1.1em;
  line-height: 1.6;
}

.about-footer {
  text-align: center;
  margin-top: 50px;
  padding: 30px;
  background: rgba(255, 105, 180, 0.1);
  border-radius: 15px;
  box-shadow: var(--shadow-l1);
}

.about-footer p {
  margin: 0;
  font-size: 1.2em;
  color: var(--accent-color);
  font-weight: 600;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .about-container {
    padding: 20px;
    margin: 10px;
    border-radius: 15px;
  }
  
  .about-container h2 {
    font-size: 1.8em;
  }
  
  .about-container h3 {
    font-size: 1.2em;
  }
  
  .about-container blockquote {
    padding: 20px;
  }
  
  .about-footer {
    padding: 25px 20px;
  }
}
</style>
