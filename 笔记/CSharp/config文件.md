## App.config

放在入口文件（Program.cs）同一个目录。

注意，是当前启动项目的入口目录，有多个项目的需要注意，放错位置不会报错，但是会读不到数据

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <startup></startup>

  <appSettings>
    <add key="hour" value="16"/>
    <add key="minutes" value="48"/>
    <add key="directory" value="C:\\DownsVisitLog"/>
    <add key="file" value="lastTime"/>
    <add key="accessPath" value="C:\\Users\\brain\\Desktop\\downs\\db_prisca4rpt.mdb"/>
    <add key="accessPassword" value="prisca"/>
    <add key="db2ConnectionString" value="Database=db2Test;Uid=neusoft;Pwd=neusoft;"/>
  </appSettings>
</configuration>
```

ConfigUtils

操作config文件

```
public static class ConfigUtils
    {
        //静态构造,不能实例化
        static ConfigUtils() { }
        
        /// <summary>
        /// 获取AppSettings配置节中的Key值
        /// </summary>
        /// <param name="keyName">Key's name</param>
        /// <returns>Key's value</returns>
        public static string GetAppSettingsKeyValue(string keyName)
        {
            return ConfigurationManager.AppSettings[keyName];
        } 

        /// <summary>
        /// 获取ConnectionStrings配置节中的值
        /// </summary>
        /// <returns></returns>
        public static string GetConnectionStringsElementValue()
        {
            ConnectionStringSettings settings = System.Configuration.ConfigurationManager.ConnectionStrings["connectionString"];
            return settings.ConnectionString;
        } 

        /// <summary>
        /// 保存节点中ConnectionStrings的子节点配置项的值
        /// </summary>
        /// <param name="elementValue"></param>
        public static void ConnectionStringsSave(string ConnectionStringsName, string elementValue)
        {
            System.Configuration.Configuration config =
            ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
            config.ConnectionStrings.ConnectionStrings["connectionString"].ConnectionString = elementValue;
            config.Save(ConfigurationSaveMode.Modified);
            ConfigurationManager.RefreshSection("connectionStrings");
        } 

        /// <summary>
        /// 判断appSettings中是否有此项
        /// </summary>
        private static bool AppSettingsKeyExists(string strKey, Configuration config)
        {
            foreach (string str in config.AppSettings.Settings.AllKeys)
            {
                if (str == strKey)
                {
                    return true;
                }
            }
            return false;
        } 
        
        /// <summary>
        /// 保存appSettings中某key的value值
        /// </summary>
        /// <param name="strKey">key's name</param>
        /// <param name="newValue">value</param>
        public static void AppSettingsSave(string strKey, string newValue)
        {
            System.Configuration.Configuration config =
             ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
            if (AppSettingsKeyExists(strKey, config))
            {
                config.AppSettings.Settings[strKey].Value = newValue;
                config.Save(ConfigurationSaveMode.Modified);
                ConfigurationManager.RefreshSection("appSettings");
            }
        }
    }
```

