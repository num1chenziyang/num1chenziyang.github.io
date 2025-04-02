---
title: about
date: 2025-03-31 21:56:41
type: about
comments: false
---
# Man！！！

---

由于Markdown在静态站点中无法直接展示动态效果（需渲染后查看），这里提供可直接粘贴到`about.md`的**完整可执行代码**与对应的**本地运行效果说明**：

---

### 玩法一：动态ASCII头像
**完整代码**（直接粘贴到Hexo的about.md）：
````markdown
<div style="text-align:center;animation: float 3s ease-in-out infinite">
<pre>
  ╭(╯^╰)╮ 
<( 挠头码农 )>
  ╰(  ´ ▽ ` )╯
</pre>
</div>
<style>
@keyframes float {
  0% { transform: translateY(0px); }
  50% { transform: translateY(-20px); }
  100% { transform: translateY(0px); }
}
</style>
````
**实际效果**：ASCII头像会持续上下浮动，类似这样：
<div style="text-align:center;animation: float 3s ease-in-out infinite">
<pre>
  ╭(╯^╰)╮ 
<( 挠头码农 )>
  ╰(  ´ ▽ ` )╯
</pre>
</div>
<style>
@keyframes float {
  0% { transform: translateY(0px); }
  50% { transform: translateY(-20px); }
  100% { transform: translateY(0px); }
}
</style>

---

### 玩法二：程序员技能进度条
**完整代码**：
```markdown
### 我的超能力 🦸
**熬夜写BUG** ██████████ 100%  
**复制Stackoverflow** ████████░░ 80%  
**喝咖啡手不抖** █░░░░░░░░░ 10%  
**记住同事名字** ██████░░░░ 60%  
**准时下班** ░░░░░░░░░░ 0%

<small>（警告：数据经过艺术化处理，可能与现实存在114514%偏差）</small>
```
**实际效果**：生成带字符进度条和吐槽文案：
```
我的超能力 🦸
熬夜写BUG ██████████ 100%  
复制Stackoverflow ████████░░ 80%  
喝咖啡手不抖 █░░░░░░░░░ 10%  
记住同事名字 ██████░░░░ 60%  
准时下班 ░░░░░░░░░░ 0%
```

---

### 玩法三：点击有惊喜彩蛋
**完整代码**（依赖浏览器JS支持）：
```markdown
<span style="cursor:pointer;border-bottom:1px dashed red" onclick="this.innerHTML='🎉恭喜发现隐藏成就：<b>摸鱼大师</b>！🎮<br>成就进度：■■□□□ 40%'">🔍 戳这里领红包</span>
```
**实际效果**：点击文字后实时变成：
```
🎉恭喜发现隐藏成就：摸鱼大师！🎮
成就进度：■■□□□ 40%
```

---

### 玩法四：字符画简历
**完整代码**：
```markdown
<pre>
简历生成中...▓▓▓▓▓▓▓▓▓▓ 90%
╔════════════╗
║  职业：   ║
║ 赛博吟游诗人 ║
║   aka.    ║
║BUG饲养员  ║
╚════════════╝
</pre>
```
**实际效果**：固定宽度的字符画方框：
```
简历生成中...▓▓▓▓▓▓▓▓▓▓ 90%
╔════════════╗
║  职业:   诗人          ║
║  爱好:                    ║
║                              ║
║                              ║
╚════════════╝
```

---

**注意事项**：
1. 动态效果（如浮动动画、点击事件）需浏览器支持
2. Next主题默认支持内联CSS/JS，若遇到样式冲突可在主题配置中调整
3. 字符画建议用`<pre>`标签包裹以保留空格格式
