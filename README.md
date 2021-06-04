# ueditor 修改文件路径

## 配置文件 application.properties 中添加配置

```
#对应 ueditor config 文件中 physicsPath
ueditor.physicsPath=/home/
#对应 ueditor 图片路径前缀
ueditor.imagePath=/ueditor/jsp/upload/image/
```

![](../assets/images/ueditor1.png)

## 修改 ueditor 配置文件

为方便本地调试，已修改 [ueditor.jar](#jar) 包，**添加了配置`physicsPath`** 和 **对应 application.properties 实现多配置文件**

ueditor 配置文件`config.json`中添加配置
"physicsPath":"/home/", /_上传文件路径_/

项目配置文件 properties 和 config 对应关系

- `ueditor.physicsPath` 和 config 中 `physicsPath` 对应
- `ueditor.imagePath` 和 config 中 `imageUrlPrefix + imagePathFormat` 对应

![](../assets/images/ueditor2.png)

## 虚拟路径的映射

创建 WebAppConfiguration 类

```java
@Configuration
public class WebAppConfiguration extends WebMvcConfigurerAdapter {
/\*\*
_ Spring Boot 中有默认的静态资源访问路径，浏览器也不容许访问项目目录外的资源文件
_ 添加一些虚拟路径的映射
_ 设置静态资源路径和上传文件的路径
_/
@Value("${ueditor.physicsPath}")
String physicsPath;

    @Value("${ueditor.imagePath}")
    String imagePath;

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        if (StringUtils.isNotEmpty(physicsPath)) {
            registry.addResourceHandler(imagePath + "**").addResourceLocations("file:" + physicsPath + imagePath);
        }
        super.addResourceHandlers(registry);
    }

}
```

## <div id="jar">修改的 ueditor.jar</div>

替换 `ueditor.jar`

下载地址[ueditor-1.1.2.jar](https://github.com/AIxiuxiu/ueditor/blob/main/ueditor-1.1.2.jar)
