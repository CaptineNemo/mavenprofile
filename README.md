# profile的使用
测试maven profile，根据mvn -P传递的值激活不同profile，打包不同的配置文件
e.g.:
```
mvn clean compile -Pdev
```
————>激活id为dev的profile  
————>其内的properties生效  
————>POM resource插件引用的变量值生效  
————>打包不同的配置文件
# settings.xml也可以配置profile
可以为一个profile配置properties，比较常用的方法是为不同的profile配置不同的仓库，当这个profile激活时，使用配置的仓库参与maven的生命周期。  
这样做的背景是，在实际项目中，常采用Maven私服，每次拉取install等都是和私服交互，且还区分环境，如snapshot环境私服，prod环境私服。如果每次都是在POM中配置太繁琐（每个项目都要配置一遍），所以采用在根节点settings.xml中配置的办法。
# 注意POM和settings.xml的默认激活profile不要冲突
POM和settings.xml都可以使用<profile>——<activation>来配置默认激活的profile，e.g.
```
<profile>
    <id>dev</id>
    <properties>
        <profiles.active>dev</profiles.active>
    </properties>
    <activation>
        <activeByDefault>true</activeByDefault>   <!--默认激活dev-->
    </activation>
</profile>
```
settings.xml还有一种方式————activeProfiles
```
<activeProfiles>
	<activeProfile>dev</activeProfile>
</activeProfiles>
```
为了防止产生误解（两个文件中配置active的人都以为自己的配置生效了），应尽量避免二者配置的默认profile冲突。
