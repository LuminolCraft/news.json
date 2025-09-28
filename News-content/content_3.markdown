# 测试标题 1 (H1)
## 测试标题 2 (H2)
### 测试标题 3 (H3)

这是普通的段落文本，**加粗**和 *斜体* 效果，~~删除线~~ 也支持。

- 无序列表项 1
- 无序列表项 2
  - 嵌套列表项
- 无序列表项 3

1. 有序列表项 1
2. 有序列表项 2
3. 有序列表项 3

> 这是一个块引用，引用一段文字来突出显示。
[这是一个链接](https://luminolsuki.moe/)

`inline code` 示例，以及以下的多行代码块：

```java
package org.ncc.JoinQuitMessage;

import org.bukkit.Bukkit;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.event.Listener;
import org.bukkit.plugin.java.JavaPlugin;

public final class JoinQuitMessage extends JavaPlugin implements Listener {
    public static JavaPlugin instance;

    @Override
    public void onLoad() {
        getLogger().info("JoinQuitMessage插件已成功加载！");
    }

    public boolean onCommand(CommandSender sender, Command command, String label, String[] args) {
        if (sender.hasPermission("joinquitmessage.admin")){// 判断输入的指令是否是/joinquitmessage
            ConfigManager.reloadConfig();
            sender.sendMessage("JoinQuitMessage配置文件已成功重新加载！");
            return true; // 返回true防止返回指令的usage信息
        }
        return false;
    }

    @Override
    public void onEnable() {
        instance = this;
        Bukkit.getPluginManager().registerEvents(new PlayerJoin(), this);    //注册事件
        Bukkit.getPluginManager().registerEvents(new PlayerQuit(), this);
        ConfigManager.initConfig();
        ConfigManager.loadConfig();
        if(!Bukkit.getPluginManager().isPluginEnabled("PlaceholderAPI")){
            getLogger().warning("PlaceHolderAPI is needed for further features.");
        }
        getLogger().info("JoinQuitMessage插件已成功启用！");
        // Plugin startup logic
    }

    @Override
    public void onDisable() {
        // Plugin shutdown logic
        getLogger().info("JoinQuitMessage插件已成功禁用！");
    }
}

```
```java
public static void reloadConfig(CommandSender sender) {
        config = YamlConfiguration.loadConfiguration(configFile);
        if (Bukkit.getPluginManager().getPlugin("PlaceholderAPI") != null) {
            new papiHook().register();
        } else {
            Location.getInstance().getLogger().warning("未找到 PlaceholderAPI 插件, 无法注册变量.");
        }
        getConfigValue(config);
        sender.sendMessage(Component.text("Reload successfully!", NamedTextColor.GREEN));
    }

    public static void getConfigValue(FileConfiguration conf) {
        CACHE_OUTDATED_RATE = conf.getInt("cache-outdated-rate", 24);
        CHECK_INTERVAL_MINUTES = conf.getInt("check-interval-minute", 10);
        QPS = conf.getInt("qps", 15);
        RETRY_COUNT_DROP = conf.getInt("retry-count-drop", 5);

        COUNTRY_REPLACEMENT = LocationType.valueOf(conf.getString("replacement.country","UNKNOWN"));
        PROVINCE_REPLACEMENT = LocationType.valueOf(conf.getString("replacement.province","UNKNOWN"));
        ISP_REPLACEMENT = LocationType.valueOf(conf.getString("replacement.isp","UNKNOWN"));
        DISTRICT_REPLACEMENT = LocationType.valueOf(conf.getString("replacement.district","UNKNOWN"));
        CITY_REPLACEMENT = LocationType.valueOf(conf.getString("replacement.city","UNKNOWN"));

        replacementKey = conf.getStringList("replacement.key");
    }

```
```kotlin
val entityTypeSet = setOf(
        EntityType.CREEPER,
        EntityType.PRIMED_TNT,
        EntityType.MINECART_TNT,
        EntityType.SMALL_FIREBALL,
        EntityType.WITHER_SKULL,
        EntityType.WITHER,
        EntityType.ENDER_CRYSTAL
    )

    override fun onEnable() {
        if (!configFile.exists()) {
            if (!configFile.parentFile.exists()) {
                configFile.parentFile.mkdirs()
            }
            configFile.createNewFile()
            tmpB = true
        }
        defaultConfigMap.forEach { (key, value) ->
            run {
                if (config.get(key) == null) {
                    config.set(key, value)
                    tmpB = true
                } else {
                    currentConfigMap[key] = config.getBoolean(key, value)
                }
            }
        }
        if (tmpB) {
            saveConfig()
        }
        Bukkit.getPluginManager().registerEvents(this, this)
    }
```

```yml
name: JoinQuitMessage
version: '${project.version}'
main: org.ncc.JoinQuitMessage.JoinQuitMessage
api-version: '1.21'
folia-supported: true
softdepend:
  - PlaceHolderAPI

commands:
  joinquitmessage:
    description: "使插件重载配置文件"
    usage: /joinquitmessage #指令的用法 当onCommand()方法返回false时提示这里的内容
    aliases: [ jqm ] #指令的多种形式 意为可以用 jqm 来触发/joinquitmessage这个指令
    permission: "joinquitmessage.admin" #指令所需要的权限（权限节点默认op）
    permission-message: "你配用吗你就用" #当输入者无上方权限时提示该信息
```

```java
  public static void loadConfig() {
      config = YamlConfiguration.loadConfiguration(configFile);
      joinMessage = config.getStringList("join-message").isEmpty() ? defaultJoinMessage : config.getStringList("join-message");
      quitMessage = config.getStringList("quit-message").isEmpty() ? defaultQuitMessage : config.getStringList("quit-message");
  }

  public static void reloadConfig() {
      initConfig();
      loadConfig();
  }
```

```css
 @keyframes pulse {
    0% { box-shadow: 0 0 0 0 rgba(39, 174, 96, 0.7); }
    70% { box-shadow: 0 0 0 8px rgba(39, 174, 96, 0); }
    100% { box-shadow: 0 0 0 0 rgba(94, 40, 77, 0); }
}
.online::before {
    content: '';
    position: absolute;
    left: 0;
    top: 50%;
    transform: translateY(-50%);
    width: 13px;
    height: 13px;
    background: var(--online-color);
    border-radius: 50%;
    animation: pulse 2s infinite;
}
@keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-10px); }
}
@media (max-width: 480px) {
    .hero-title {
        font-size: 2rem;
    }
    
    .server-status-card {
        padding: 20px;
    }
    
    .feature-card,
    .server-card {
        padding: 30px 20px;
    }
}
```

```html
<div class="hero-visual">
                <div class="server-status-card">
                    <div class="status-indicator">
                        <div class="status-dot"></div>
                        <div class="status-text">服务器在线</div>
                    </div>

<div class="team-card">
                    <img src="https://q1.qlogo.cn/g?b=qq&nk=1928325064&s=0" alt="Narcssu" class="team-avatar">
                    <h3 class="team-name">Narcssu</h3>
                    <p class="team-role">网站开发主理人</p>
                    <div class="team-links">
                        <a class="team-link" href="https://github.com/NARCSSU" target="_blank" rel="noopener noreferrer" aria-label="GitHub">
                            <i class="fab fa-github"></i>
                        </a>
                        <a class="team-link" href="mailto:goofygazer@gmail.com" aria-label="Email">
                            <i class="fas fa-envelope"></i>
                        </a>
                    </div>
                </div>
```

```JavaScript
// 初始化平滑滚动
    initSmoothScroll() {
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function(e) {
                e.preventDefault();
                
                const targetId = this.getAttribute('href');
                const targetElement = document.querySelector(targetId);
                
                if (targetElement) {
                    window.scrollTo({
                        top: targetElement.offsetTop - 60,
                        behavior: 'smooth'
                    });
                }
            });
        });
    }
```

---
