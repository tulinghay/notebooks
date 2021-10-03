## 泛型

反射类

基本数据类型不可做为泛型传入

类型通配符、以及其上下限

类型擦除、无限制类型擦出、有限制类型擦出、桥接方法（保持接口和类的实现关系）

可以声明带泛型的数组引用，但是不能直接创建带泛型的数组对象

可以通过java.lang.reflect.Array的newInstance(Class<T>, int)创建T[]数组

## 反射

```java
Class cls=Class.forName("Dog");
//创建类实例
Object object = cls.newInstance();
//获取方法对象
Method method=cls.getMethod("setName")
method.invoke(object);
//getField不能得到私有变量
Field name=cls.getField("name");
System.out.println(name.get(object));
//无参构造器
Contructor contructor=cls.getConstructor();
//有参构造器
Contructor contructor=cls.getConstructor(String.class);


//通过反射更新结果，将对象名为columnName的属性更新值为columnValue
//Order为需要赋值的对象
Field field=Order.class.getDeclaredField(columnName);
//反射调用优化，取消访问检查
field.setAccessible(true);
field.set(order,columnValue);

//获取class对象的方法
Class cls=Class.forName("com.huang.Dog");//编译阶段
Class cls=Dog.class;//加载阶段
Class cls=dog.getClass();//运行时阶段
//通过获取类加载器来获取类
ClassLoader classLoader=dog.getClass().getClassLoader();
Class cls=classloader.loadClass("com.huang.Dog")

//基本数据类型对应的包装类可以通过TYPE获取class
Class<Integer> type=Integer.TYPE

    
//静态加载，编译时加载类，new创建对象，子类加载父类也加载，调用类中的静态成员
//动态加载，运行时加载类，反射，运行时不调用代码，不会报错
//创建对象
Class cls=Class.forName("com.huang.Dog");
Object object=cls.newInstance();
//public构造方法创建对象
Constructor contructor=cls.getConstructor(String.class);
Object object=constructor.newInstance("huangda");
//private构造方法构造对象
Constructor contructor=cls.getDeclaredConstructor(int.class,String.class);
//爆破
contructor.setAccessible(true);
Object object=constructor.newInstance(12,"huangda");

```

![image-20210915090741982](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210915090741982.png)

![image-20210915091011545](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210915091011545.png)



### java版本迭代

```java
switch
泛型
```





## 文件File





### 反射

```java
//通过反射更新结果，将对象名为columnName的属性更新值为columnValue
//Order为需要赋值的对象
Field field=Order.class.getDeclaredField(columnName);
field.setAccessible(true);
field.set(order,columnValue);
```



```java
//指定response的content-type为text/plain，也可以指定为json等
@Produces("text/plain")
//指定访问路径为demo
@Path("demo")
```



### 域的作用范围：

ServletContext设置了属性之后，只要web没有停止，则在整个web中都可通过获取context来获取到属性

```java
servletContext.setAttribute("type","one");
servletContext.gettAttribute("type");
```

### http协议（超文本传输协议）

指客户端和服务器之间通信时需要遵守的规则，http中的数据称之为报文。

#### 请求的http协议格式

- GET请求
  - 请求行：
    - 请求方式：	GET
    - 请求的资源路径：/login.html
    - 请求的协议版本：HTTP/1.1
  - 请求头：
    - Accept：告诉服务器客户端可以接受的数据类型
    - Accept-Language：zh-CN/en_US
    - User-Agent：用户代理，一般用浏览器访问就是浏览器信息
    - Accept-Encoding：服务器可接受的数据编码压缩格式
    - Host：服务器的IP和端口
    - Connection：告诉服务器请求连接如何处理
      - Keep-Alive：告诉服务器保持一段时间连接后断开
      - Closed：马上关闭
- POST请求
  - 请求行：
    - 请求方式：POST
    - 请求资源路径：
    - 请求协议版本号：
  - 请求头：
    - Accept：告诉服务器客户端可以接受的数据类型
    - Accept-Language：zh-CN/en_US
    - Referer：表示请求发起时，浏览器地址栏中的地址从哪来
    - User-Agent：用户代理，一般用浏览器访问就是浏览器信息
    - Content-Type：表示发送的数据类型
    - Content-Length:发送数据的长度
    - Cache-Control：表示如何控制缓存
      - no-cache：无缓存
    - 空行:
  - 请求体：发送给服务器的数据



GET请求：

1. form标签 method=get
2. a标签
3. link标签引入css
4. script标签引入js
5. img引入图片
6. iframe引入html页面
7. 在浏览器中输入地址后敲回车

POST请求：

1. form标签中 method=post

#### 响应码

```
200			请求成功
302			请求重定向
404			请求服务器已收到，但页面未找到
500			服务器内部错误
```

#### MIME类型数据说明

``` 
超文本标记语言文本 .html,.html text/html 
普通文本 .txt text/plain 
RTF文本 .rtf application/rtf 
GIF图形 .gif image/gif 
JPEG图形 .jpeg,.jpg image/jpeg 
au声音文件 .au audio/basic 
MIDI音乐文件 mid,.midi audio/midi,audio/x-midi 
RealAudio音乐文件 .ra, .ram audio/x-pn-realaudio 
MPEG文件 .mpg,.mpeg video/mpeg 
AVI文件 .avi video/x-msvideo 
GZIP文件 .gz application/x-gzip 
TAR文件 .tar application/x-tar
```



doPost请求参数中存在乱码，则需要在获取参数前设置编码格式：

```java
//设置请求体编码格式为utf-8
req.setCharacterEncoding("utf-8");
String username = req.getParameter("username");
```

跳转转发

```java
//跳转到根目录下的next.html，根目录下的next.html
req.getRequestDispatcher("/next.html").forward(req,resp);
```

设置基础目录

```java
	这样做就相当于设置了当前路径，而不是浏览器的访问路径
    <base href="http://localhost:8080/Test1/a/b/c.html">
```

文件流的使用：字节流和字符流只能用一个，不能同时使用

### 乱码问题解决：

1.

```java
//服务器端设置编码为utf-8
resp.setCharacterEncoding("utf-8");
//通过响应头，设置浏览器也为utf-8
resp.setHeader("Content-Type","text/html; charset-UTF-8");

//或者下面一步到位,但是下面这种方法需要在获取流之前使用
resp.setContentType("text/html; charset-UTF-8");
```

```java
//自定义过滤器解决乱码
//xml中配置过滤器
 package com.fuck.web.filter;
 
 import java.io.IOException;
 import java.io.UnsupportedEncodingException;
 import java.util.Map;
 
 import javax.servlet.Filter;
 import javax.servlet.FilterChain;
 import javax.servlet.FilterConfig;
 import javax.servlet.ServletException;
 import javax.servlet.ServletRequest;
 import javax.servlet.ServletResponse;
 import javax.servlet.http.HttpServletRequest;
 import javax.servlet.http.HttpServletRequestWrapper;
 
 /**
  * 解决get和post请求 全部乱码
  * 
  * @author seawind
  * 
  */
 public class GenericEncodingFilter implements Filter {
 
     @Override
     public void destroy() {
     }
 
     @Override
     public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
         // 转型为与协议相关对象
         HttpServletRequest httpServletRequest = (HttpServletRequest) request;
         // 对request包装增强
         HttpServletRequest myrequest = new MyRequest(httpServletRequest);
         response.setContentType("text/html;charset=utf-8");
         chain.doFilter(myrequest, response);
     }
 
     @Override
     public void init(FilterConfig filterConfig) throws ServletException {
     }
 
 }
 
 // 自定义request对象
 class MyRequest extends HttpServletRequestWrapper {
 
     private HttpServletRequest request;
 
     private boolean hasEncode;
 
     public MyRequest(HttpServletRequest request) {
         super(request);// super必须写
         this.request = request;
     }
 
     // 对需要增强方法 进行覆盖
     @Override
     public Map getParameterMap() {
         // 先获得请求方式
         String method = request.getMethod();
         if (method.equalsIgnoreCase("post")) {
             // post请求
             try {
                 // 处理post乱码
                 request.setCharacterEncoding("utf-8");
                 return request.getParameterMap();
             } catch (UnsupportedEncodingException e) {
                 e.printStackTrace();
             }
         } else if (method.equalsIgnoreCase("get")) {
             // get请求
             Map<String, String[]> parameterMap = request.getParameterMap();
             if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                 for (String parameterName : parameterMap.keySet()) {
                     String[] values = parameterMap.get(parameterName);
                     if (values != null) {
                         for (int i = 0; i < values.length; i++) {
                             try {
                                 // 处理get乱码
                                 values[i] = new String(values[i].getBytes("ISO-8859-1"), "utf-8");
                             } catch (UnsupportedEncodingException e) {
                                 e.printStackTrace();
                             }
                         }
                     }
                 }
                 hasEncode = true;
             }
             return parameterMap;
         }
 
         return super.getParameterMap();
     }
 
     @Override
     public String getParameter(String name) {
         Map<String, String[]> parameterMap = getParameterMap();
         String[] values = parameterMap.get(name);
         if (values == null) {
             return null;
         }
         return values[0]; // 取回参数的第一个值
     }
 
     @Override
     public String[] getParameterValues(String name) {
         Map<String, String[]> parameterMap = getParameterMap();
         String[] values = parameterMap.get(name);
         return values;
     }
 
 }

```

```xml
<!--tomcat配置文件解决乱码-->
<Connector port="8080" maxThreads="150" minSpareThreads="25" maxSpareThreads="75"
    enableLookups="false" redirectPort="8443" acceptCount="100"
    connectionTimeout="20000" disableUploadTimeout="true" URIEncoding="UTF-8" />
```

```xml
<!--spring配置乱码-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>/</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/</url-pattern>
    </filter-mapping>
```

#### spring解决json乱码问题

**单个方法通过RequestMapping解决json乱码**

```java
//RequestMapping设置produces utf-8解决json中文乱码
@RequestMapping(value="/hello",produces="application/json;charset=utf-8")
```

**spring.xml文件中全局配置解决json乱码**

```xml
	
<!--spring设置json乱码-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">


    <context:component-scan base-package="com.huang.controller"/>
    <mvc:default-servlet-handler/>
    <!--设置json乱码，目前会报错-->
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
    <!--设置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```



### 请求重定向1

服务器升级页面重定向，在请求头设置了status和Location后

	1. 浏览器会再发出Location地址的请求，地址栏地址变为Location中的地址
	2. 不共享request域中的数据：
	3. 无法访问WEB-INF下的数据
	4. 可访问工程外的资源（http://www.baidu.com）

```java
//设置响应状态码302,代表重定向
resp.setStatus(302);
//设置请求头
resp.setHeader("Location","http://localhost:8080/Test1/response2");
```

### 请求重定向2

```java
resp.sendRedirect("http://localhost:8080")
```



# EL表达式

```html
<!--写java代码块-->
<%
   	Person person = new Person();
   	person.name="hdt";
   pageContext.setAttribute("p",person)
%>
    <!--用上面的属性p来使用对象 ,调用元素，获取类的成员变量则必须声明成员的get方法-->
    输出：${p.name}
```



### 数据库数据导出到excel提供下载

POI导出excel的三种workbook的区别

三种workbook分别是HSSFWorkbook、XSSFWorkbook、SXSSFWorkbook

**第一种：HSSFWorkbook**

针对EXCEL 2003版本，扩展名为.xls，此种的局限就是导出的行数最多为65535行。因为导出行数受限，不足7万行，所以一般不会发送内存溢出(OOM)的情况

**第二种：XSSFWorkbook**

这种形式的出现是由于第一种HSSF的局限性产生的，因为其导出行数较少，XSSFWorkbook应运而生，其对应的是EXCEL2007+ ，扩展名为.xlsx ，最多可以导出104万行，不过这样就伴随着一个问题–OOM内存溢出。因为使用XSSFWorkbook创建的book sheet row cell 是存在内存中的，并没有持久化到磁盘上，那么随着数据量的增大，内存的需求量也就增大。那么很有可能出现 OOM了，那么怎么解决呢？

**第三种:SXSSFWorkbook** 　poi.jar 3.8+

SXSSFWorkbook可以根据行数将内存中的数据持久化写到文件中。

此种的情况就是设置最大内存条数，比如设置最大内存量为5000行， new SXSSFWookbook(5000)，当行数达到 5000 时，把内存持久化写到文件中，以此逐步写入，避免OOM。这样就完美解决了大数据下导出的问题

#### 使用SXSSFWorkbook处理，对.xlsx类型文件下载

```java
    @UserLoginToken
    @RequestMapping(value = "/excelall", method = RequestMethod.GET)
    @ResponseBody
    public void downloadExcel(HttpServletResponse response) throws IOException {

        //创建SXSSFWorkbook 对象
        SXSSFWorkbook workbook = new SXSSFWorkbook(1000);
        Sheet sheet = workbook.createSheet("信息表");
		
        //获取list对象，下面将list对象写入excel中
        List<Promember> downloadData = promemberService.findByBadge(username);

        //设置导出的文件名称
        String fileName = "项目成员表"  + ".xlsx";//设置要导出的文件的名字
        
        //当前数据写入的位置行数，行头写在0行
        int rowNum = 1;
		
        //设置表格的标题
        String[] headers = { "项目成员工号", "项目成员名称", "项目校内标号", "金额", "是否是负责人"};
        
        //创建第一行表头的写入位置，row，cell
        Row titleRow = sheet.createRow(0);

        //在excel表中添加表头
        for(int i=0;i<headers.length;i++){
            Cell cell = titleRow.createCell(i);
            cell.setCellValue(headers[i]);

        }

        //在表中存放查询到的数据放入对应的列
        for (Promember promember : downloadData) {
            Row row1 = sheet.createRow(rowNum);
            row1.createCell(0).setCellValue(promember.getPromembadge());
            row1.createCell(1).setCellValue(promember.getPromemname());
            row1.createCell(2).setCellValue(promember.getPid());
            row1.createCell(2).setCellValue(promember.getFee());
            row1.createCell(2).setCellValue(promember.getIscharge());
            rowNum++;
        }

        response.setContentType("application/octet-stream");
        //文件命中包含中文，所以需要对filename进行转编码
        response.setHeader("Content-disposition", "attachment;filename=" +new String(fileName.getBytes("utf-8"),"ISO8859-1") );
        response.flushBuffer();
        workbook.write(response.getOutputStream());
    }
```

#### 使用HSSFWorkbook处理，对.xls类型文件下载

```java
    @UserLoginToken
    @RequestMapping(value = "/excelallnotx", method = RequestMethod.GET)
    @ResponseBody
    public void downloadAllClassmate(HttpServletResponse response) throws IOException {

        String token= TokenUtil.getToken();
        // System.out.println("token:"+token);
        String username=TokenUtil.getTokenUsername(token);

        HSSFWorkbook workbook = new HSSFWorkbook();
        HSSFSheet sheet = workbook.createSheet("信息表");

        List<Awardorgandep> downloadData = awardorgandepService.findByBadge(username);

        String fileName = "科研奖励"  + ".xls";//设置要导出的文件的名字
        //新增数据行，并且设置单元格数据

        int rowNum = 1;

        String[] headers = { "评奖组织单位", "获奖项目名称", "奖励名称类型", "获奖等级",
                "获奖时间（以证书日期为准）", "获奖人员姓名（按顺序）", "完成单位名称（按顺序）", "证书编号",
                "个人证书","单位证书"};
        //headers表示excel表中第一行的表头

        HSSFRow row = sheet.createRow(0);
        //在excel表中添加表头

        for(int i=0;i<headers.length;i++){
            HSSFCell cell = row.createCell(i);
            HSSFRichTextString text = new HSSFRichTextString(headers[i]);
            cell.setCellValue(text);
        }

        //在表中存放查询到的数据放入对应的列
        for (Awardorgandep awardorgandep : downloadData) {
            HSSFRow row1 = sheet.createRow(rowNum);
            row1.createCell(0).setCellValue(awardorgandep.getAwardunit());
            row1.createCell(1).setCellValue(awardorgandep.getAwardname());
            row1.createCell(2).setCellValue(awardorgandep.getAwardnametype());
            row1.createCell(3).setCellValue(awardorgandep.getAwardlevel());
            row1.createCell(4).setCellValue(awardorgandep.getAwarddate());
            row1.createCell(5).setCellValue(awardorgandep.getAwardstaffname());
            row1.createCell(6).setCellValue(awardorgandep.getCompletedepname());
            row1.createCell(7).setCellValue(awardorgandep.getCertno());
            row1.createCell(8).setCellValue(awardorgandep.getPersoncerti());
            row1.createCell(9).setCellValue(awardorgandep.getUnitcerti());
            rowNum++;
        }

        response.setContentType("application/octet-stream");
        response.setHeader("Content-disposition", "attachment;filename=" + fileName);
        response.flushBuffer();
        workbook.write(response.getOutputStream());
    }
```

### 将excel文件导入到数据库中            springdata-FileManager

```xml
<!--连接数据库需要-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.23</version>
</dependency>
<!--解析excel文件需要-->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>3.17</version>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>3.17</version>
</dependency>
<!--multiPat文件-->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
<!--druid数据库连接-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.22</version>
</dependency>

```



```java
package com.example.springdata.controller;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.fastjson.JSONObject;
import org.apache.commons.io.FileUtils;
import org.apache.commons.io.IOUtils;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.sql.SQLException;
import java.util.*;

/*
需添加依赖
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>

 */

/**
 * 上传文件，并给出下载链接
 */
@Controller
public class UploadFile {

    //文件存储在根目录下的files目录下
    private static String baseDir = "files/";

    /**
     * 文件上传
     * @param inputFile
     * @param
     * @param httpServletRequest
     * @return
     */
    @PostMapping("/upload")
    @ResponseBody
    public JSONObject uploadcert(@RequestParam("file") MultipartFile inputFile, HttpServletRequest httpServletRequest) {
        //文件的存储目录--
        String destPath="E:\\personLearning\\javaweb\\project\\springdata\\temp\\";
        JSONObject jsonObject = new JSONObject();
        //生成唯一的UUID与源文件拼接生成的新文件名
        String name = UUID.randomUUID().toString() + "-" + inputFile.getOriginalFilename();

        try {
            //方法一
            //传入目标存储文件，这里的存储路径是相对于根目录下
            FileUtils.copyToFile(inputFile.getInputStream(), new File(baseDir + name));
            //方法二
            //此方法在给定了目标存储文件的绝对路径的情况下使用，由于版本问题transferTo会添加一个temp临时路径，报错文件 不存在
            //String fileName = inputFile.getOriginalFilename();
            //构建目标文件
            //File dest = new File(destPath + fileName);
            //如果文件夹不在则创建文件夹
            //if (!dest.getParentFile().exists()) {
            //    dest.getParentFile().mkdirs();
            //}
            //inputFile.transferTo(dest);
            //======================================================================================
        } catch (IOException e) {
            e.printStackTrace();
            return getJson("上传失败","202");
        }
        // 生成下载链接
        String url=httpServletRequest.getScheme()+"://"+httpServletRequest.getServerName()+":"+httpServletRequest.getServerPort()+"/";

        name=url.substring(0, url.indexOf("/",9)+1)+"download/"+name;
        System.out.println(name);

        if (inputFile.isEmpty()) {
            jsonObject.put("Msg", "上传失败");
            jsonObject.put("code", 202);
            jsonObject.put("data", "");
            jsonObject.put("total","");
            return jsonObject;
        }
        jsonObject.put("Msg", "上传成功");
        jsonObject.put("code", 200);
        jsonObject.put("data", "");
        jsonObject.put("total","");
        return jsonObject;
    }

    /**
     * 下载上传的文件
     * @param response
     * @param name 传入需要下载的文件名
     * @throws IOException
     */
    @GetMapping("/download/{name}")
    public void download(HttpServletResponse response, @PathVariable String name) throws IOException {
        File file = new File(baseDir + name);
        IOUtils.copy(FileUtils.openInputStream(file), response.getOutputStream());
    }

    /**
     * 将excel上传到sql中
     * @param multipartFile
     * @return
     * @throws IOException
     * @throws SQLException
     */
    @PostMapping("/todatabase")
    @ResponseBody
    public JSONObject toDataBase(@RequestParam MultipartFile multipartFile) throws IOException, SQLException {
        //连接数据库
        DruidDataSource dataSource=new DruidDataSource();
        dataSource.setUrl("jdbc:mysql://localhost:3306/test?useSSL=false&allowPublicKeyRetrieval=true&&serverTimezone = GMT");
        dataSource.setUsername("root");;
        dataSource.setPassword("qwe");
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");

        JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
        String sql="insert into `temp`(name,password,age) values(?,?,?)";

        Workbook workbook=null;
        String fileName=multipartFile.getOriginalFilename();
        InputStream inputStream=new ByteArrayInputStream(multipartFile.getBytes());

        if(fileName==null || fileName.lastIndexOf(".")==-1){
            return getJson("文件名不符合要求","404");
        }

        if(".xlsx".equals(fileName.substring(fileName.lastIndexOf('.')))){
            workbook=new XSSFWorkbook(inputStream);
        }else if(".xls".equals(fileName.substring(fileName.lastIndexOf('.')))){
            workbook=new HSSFWorkbook(inputStream);
        }else{
            return getJson("文件名不符合要求","404");
        }

        if(workbook!=null){
            //获取第一个sheet
            Sheet sheet=workbook.getSheetAt(0);
            //获取表中最大行数
            int rowNum=sheet.getPhysicalNumberOfRows();
            //获取第一行，默认为标题行
            Row row=sheet.getRow(0);
            //获取列数
            int column=row.getPhysicalNumberOfCells();
            //数据库中的字段名称
            String[] header={"name","password","age"};
            //判断excel的标题与数据库中的字段是否匹配，默认标题都是String类型
            for(int i=0;i<column;i++){
                if(!header[i].equals(row.getCell(i).getStringCellValue())){
                    return getJson("上传失败:表单列名不匹配","205");
                }
            }
            List list=new ArrayList<>();

            for (int i = 1; i < rowNum; i++) {
                row = sheet.getRow(i);
                Object cellData=null;
                if (row != null) {
                    for (int j = 0; j < column; j++) {
                        cellData = getCellFormatValue(row.getCell(j));
                        list.add(cellData);
                    }
                    jdbcTemplate.update(sql,list.toArray());
                }
            }
        }
        return  getJson("上传成功","200");
    }

    //获取cell中不同类型的数据
    public Object getCellFormatValue(Cell cell){
        Object cellValue=null;
        switch (cell.getCellTypeEnum()){
            case NUMERIC:{
                cellValue=cell.getNumericCellValue();
                break;
            }
            case STRING:{
                cellValue=cell.getStringCellValue();
                break;
            }
            case FORMULA:{
                //判断cell是否为日期格式
                if (DateUtil.isCellDateFormatted(cell)) {
                    //转换为日期格式YYYY-mm-dd
                    cellValue = cell.getDateCellValue();
                } else {
                    //数字
                    cellValue = String.valueOf(cell.getNumericCellValue());
                }
                break;
            }
            default:
                cellValue="";
        }
        return cellValue;
    }

    public JSONObject getJson(String msg,String code){
        JSONObject jsonObject=new JSONObject();
        jsonObject.put("msg",msg);
        jsonObject.put("code",code);
        return jsonObject;
    }
}

```



# Filter

今日内容：



![image-20210421102008660](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210421102008660.png)

1. 在java类中继承Filter接口，实现接口中的方法，主要在doFilter中实现逻辑，在web.xml文件中完成filter的配置。

每次拦截处理逻辑后，如果需要接着让用户往下访问资源，则需要执行下面方法：

```java
//用户接着往下访问用户的目标资源
filterchain.doFilter(servletRequest,servletResponse)
```

2. 怎样修改tomcat在服务器的日志乱码问题，

查看ideal的编码，然后找到tomcat安装目录下的conf文件夹中的logging.properties文件，修改 如下：

java.util.logging.ConsoleHandler.encoding = UTF-8

后重启服务器执行即可

3.  创建工程后默认生成了工程目录名称，修改了tomcat配置中的url后报错无法访问

3. 由于tomcat内部config默认设置，导致工程打开默认识别webapp下面的index.jsp页面，如果需要修改的话，则在web.xml中增加下面代码

   ```xml
   <welcome-file-list>
       <welcome-file>index1.jsp</welcome-file>
   </welcome-file-list>
   ```



filter的FilterChain.doFilter()方法，该方法在实现Filter接口的doFilter方法中调用默认参数filterChain类的doFilter方法使用：

![image-20210422105233213](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210422105233213.png)

### pom文件配置

过滤资源，资源过滤

```xml
<!--静态资源导出问题-->    
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

maven包

```xml
<!--@stereotype.Controller-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.5</version>
</dependency>
<!--自动getter setter-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.18</version>
</dependency>
<!--json-->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.4</version>
</dependency>
<!--junit-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
    <scope>test</scope>
</dependency>
<!--mysql-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.23</version>
</dependency>
<!--数据库连接池-->
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.5.2</version>
</dependency>
<!--servlet -jsp-->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
</dependency>
<!--web.xml中的DispatcherServlet-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>

<!--mybatis-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.6</version>
</dependency>
<!--spring\spring.xml中的mvc配置-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.5</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.5</version>
</dependency>

<!--上传文件-->

```

### Spring架构搭建

spring.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--扫描位置，让其内的注解生效，，由IOC容器管理-->
    <context:component-scan base-package="com.huang.controller"/>
    <!--使用注解加载映射器和解析器，省略配置步骤-->
    <mvc:annotation-driven/>
    <!--让springmvc不使用视图解析器解析静态资源-->
    <mvc:default-servlet-handler/>
	<!--配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
    
    <!--文件上传配置-->
    <bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="multipartResolver">
        <!--请求的编码格式必须和jsp的pageencoding属性一直，以便正确读取表单的内容，默认为IOS-8859-1-->
        <property name="defaultEncoding" value="utf-8"/>
        <!--文件大小上线，单位字节，10485760=10m-->
        <property name="maxUploadSize" value="10485760"/>
        <property name="maxInMemorySize" value="40960"/>
    </bean>
    <!--设置json乱码，目前导入这个配置会报错-->
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
    <!--设置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.huang.controller.Interception"/>
        </mvc:interceptor>
    </mvc:interceptors>
</beans>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置DispatcherServlet，自动分发servlet-->
    <servlet>
        <servlet-name>mvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>mvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <!--编码设置-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>/</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/</url-pattern>
    </filter-mapping>
</web-app>

```

### 注解

方法注释

```java
@RequestMapping		可配置地址，通过method参数配置访问的方法是post、get、delete、update等
@PostMapping		可配置地址，设置方法为post
@GetMapping 		可配置地址，设置方法为get
@ResponseBody		表示返回为字符串，不进行视图解析器解析，只传递json可以用这个
```

类注释

```java
@Controller			配置表示属于controller
@RestController		配置表示该类的所有返回都属于字符串，不进行视图解析，只传递json可以用这个
```

- @Autowired：自动装配，通过类型，名字
  - 如果@Autowired不能唯一自动装配，则需要通过@Qualifier(value="xxx")来进行指定
- @Nullable ：参数标记了这个注解啧说明字段可为null
- @Resource：自动装配，通过名字类型



### FastJson

导包

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```

### 拦截器 Interception

**1. 类实现接口HandlerInterceptor**

```java
public class Interception implements HandlerInterceptor {
	
    //返回false表示拦截
    //True表示不拦截
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```

**2. spring.xml文件中添加**

```xml
    <mvc:interceptors>
        <mvc:interceptor>
            <!--/**表示根目录下的所有路径（包括子路径）都拦截-->
            <mvc:mapping path="/**"/>
            <bean class="com.huang.controller.Interception"/>
        </mvc:interceptor>
    </mvc:interceptors>
```



### 文件上传与下载

1. pom.xml

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.2</version>
</dependency>
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```

2. 配置bean

```xml
<!--文件上传配置-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!--设置默认编码格式-->
    <property name="defaultEncoding" value="UTF-8"/>
    <!--设置单个文件最大上传大小，以字节位单位-->
    <property name="maxUploadSizePerFile" value="5242880"/>
    <!--设置所有文件最大上传大小，以字节位单位-->
    <property name="maxUploadSize" value="5242880"/>
</bean>
```

3. java代码实现，用的是commons-io依赖

```java
package com.example.springdata.controller;

import com.alibaba.fastjson.JSONObject;
import org.apache.commons.io.FileUtils;
import org.apache.commons.io.IOUtils;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.util.UUID;

/*
需添加依赖
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>

 */

/**
 * 上传文件，并给出下载链接
 */
@Controller
public class UploadFile {

    //文件存储在根目录下的files目录下
    private static String baseDir = "files/";

    /**
     * 文件上传
     * @param inputFile
     * @param
     * @param httpServletRequest
     * @return
     */
    @PostMapping("/upload")
    @ResponseBody
    public JSONObject uploadcert(@RequestParam("file") MultipartFile inputFile, HttpServletRequest httpServletRequest) {
        //文件的存储目录--
        String destPath="E:\\personLearning\\javaweb\\project\\springdata\\temp\\";
        JSONObject jsonObject = new JSONObject();
        //生成唯一的UUID与源文件拼接生成的新文件名
        String name = UUID.randomUUID().toString() + "-" + inputFile.getOriginalFilename();

        try {
            //方法一
            //传入目标存储文件，这里的存储路径是相对于根目录下
            FileUtils.copyToFile(inputFile.getInputStream(), new File(baseDir + name));
            //方法二
            //此方法在给定了目标存储文件的绝对路径的情况下使用，由于版本问题transferTo会添加一个temp临时路径，报错文件 不存在
            //String fileName = inputFile.getOriginalFilename();
            //构建目标文件
            //File dest = new File(destPath + fileName);
            //如果文件夹不在则创建文件夹
            //if (!dest.getParentFile().exists()) {
            //    dest.getParentFile().mkdirs();
            //}
            //inputFile.transferTo(dest);
            //======================================================================================
        } catch (IOException e) {
            e.printStackTrace();
            jsonObject.put("Msg", "上传失败");
            jsonObject.put("code", 202);
            jsonObject.put("data", "");
            jsonObject.put("total","");
            return jsonObject;
        }
        // 生成下载链接
        String url=httpServletRequest.getScheme()+"://"+httpServletRequest.getServerName()+":"+httpServletRequest.getServerPort()+"/";

        name=url.substring(0, url.indexOf("/",9)+1)+"download/"+name;
        System.out.println(name);

        if (inputFile.isEmpty()) {
            jsonObject.put("Msg", "上传失败");
            jsonObject.put("code", 202);
            jsonObject.put("data", "");
            jsonObject.put("total","");
            return jsonObject;
        }
        jsonObject.put("Msg", "上传成功");
        jsonObject.put("code", 200);
        jsonObject.put("data", "");
        jsonObject.put("total","");
        return jsonObject;
    }

    /**
     * 下载上传的文件
     * @param response
     * @param name 传入需要下载的文件名
     * @throws IOException
     */
    @GetMapping("/download/{name}")
    public void download(HttpServletResponse response, @PathVariable String name) throws IOException {
        File file = new File(baseDir + name);
        IOUtils.copy(FileUtils.openInputStream(file), response.getOutputStream());
    }
}

```







### 问题



问题1：

```shell
新建一个javaenterprise项目，无法访问webapp下的png图片资源，jpg图片可正常访问。
```

问题2：

```
新建一个filter，在web.xml文件中配置了filter后报错如下：
21-Apr-2021 20:12:07.394 信息 [RMI TCP Connection(3)-127.0.0.1] org.apache.jasper.servlet.TldScanner.scanJars 至少有一个JAR被扫描用于TLD但尚未包含TLD。 为此记录器启用调试日志记录，以获取已扫描但未在其中找到TLD的完整JAR列表。 在扫描期间跳过不需要的JAR可以缩短启动时间和JSP编译时间。
21-Apr-2021 20:12:07.413 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.catalina.core.StandardContext.startInternal 一个或多个筛选器启动失败。完整的详细信息将在相应的容器日志文件中找到
21-Apr-2021 20:12:07.413 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.catalina.core.StandardContext.startInternal 由于之前的错误，Context[/demo]启动失败
```

问题3：

```
ThreadLocal的使用
```

# SpringBoot

springboot的自动装配配置都在spring-boot-autoconfigure-2.2.0.RELEASEjar包下

![image-20210902113307259](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210902113307259.png)

### pom.xml

pom.xml文件设置了starter-web后才作为一个web项目，通过springinitialize新建项目，未勾选springweb时添加如下依赖

```xml
<!--starter-web后才作为一个web项目-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--thymeleaf-->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
</dependency>
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-java8time</artifactId>
</dependency>
<!--druid数据库连接诶-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.22</version>
</dependency>
<!--log4j日志-->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.12</version>
</dependency>
```

在pom配置了一个父工程的依赖，springboot几乎提前导入需要的jar包

### application.properties

```properties
#设置端口
server.port=8081
#设置访问目录,localhost:8080/huang
server.servlet.context-path=/huang
#关闭thymeleaf缓存引擎
spring.thymeleaf.cache=false
#设置日期格式为yyyy-MM-dd，默认为yyyy/MM/dd
spring.mvc.date-format=yyyy-MM-dd

#数据库连接设置
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC&&useSSL=false&&autoReconnect=true&failOverReadOnly=false 
spring.datasource.username=root
spring.datasource.password=qwe
#数据库源设置，这里设置为DruidDataSource
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
#连接池的配置信息
spring.datasource.initialSize=5
spring.datasource.minIdle=5
spring.datasource.maxActive=20
# 配置获取连接等待超时的时间
spring.datasource.maxWait=60000
# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
spring.datasource.timeBetweenEvictionRunsMillis=60000
# 配置一个连接在池中最小生存的时间，单位是毫秒
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=SELECT 1 FROM DUAL
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false
# 打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.poolPreparedStatements=true
spring.datasource.maxPoolPreparedStatementPerConnectionSize=20
# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall,log4j
# 通过connectProperties属性来打开mergeSql功能；慢SQL记录
spring.datasource.connectionProperties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000

```

**通过注解将配置在yaml文件中的值赋值给类，以及jsr303校验属性验证**

1. yaml文件中配置person类的属性,yaml文件中可以使用EL表达式

```yaml
person:
	name: huang
	age: 23
	birth: 2019/12/12
	maps:{key1: val1,key2: val2}
	email: huang@163.com

```

2. 创建类时，加上注解

```java
@Component//注册bean，设置为容器中的组件，才能使用ConfigurationProperties功能
@ConfigurationProperties(prefix="person")//赋值yaml文件中的person对象
@Validated//jsr303校验
public class Person{
    private String name;
    private Integer age;
    private Date birth;
    private Map<String,Object> maps;
    @Email(message="自定义错误提示")//设置校验，赋值不符合邮箱格式会报错
    private String email;
    Person(){}
    Person(String name,Integer age,Date birth,Map<String,Object> maps,String email){
        this.name=name;
        this.age=age;
        this.birth=birth;
        this.maps=maps;
        this.email=email;
    }
}
```

3. 使用

```java
@Autowired
Person person= new Person()
```

### 使用properties配置文件获取值

1. 配置文件huang.properties

```properties
name=huang
```

2. 配置类

```java
@configurationProperties("classpath:huang.properties")
public class Person{
    @Value("name")
    private String name;
}
```

获取一个值时，使用properties；多个使用yaml



从properties中加载配置信息

1. 配置properties

```xml
url=jdbc:mysql://localhost:3306/test?useSSL=false&allowPublicKeyRetrieval=true&&serverTimezone = GMT
driverClass=com.mysql.cj.jdbc.Driver
user=root
password=qwe
```

2. java实现

```java
public void test() throws IOException, ClassNotFoundException, SQLException {
    InputStream is=JDBC.class.getClassLoader().getResourceAsStream("jdbc.properties");
    Properties pros=new Properties();
    pros.load(is);
    String user=pros.getProperty("user");
    String password=pros.getProperty("password");
    String url=pros.getProperty("url");
    String driverClass=pros.getProperty("driverClass");

    Class.forName(driverClass);

    Connection conn=DriverManager.getConnection(url,user,password);
    System.out.println(conn);
}
```



### yaml文件可放置的位置

file:./config/（工程根目录下创建config下）  file:./(工程根目录下)     classpath:./config/   classpath:./（类路径下，一般是resources下）

优先级也如上顺序，第一个最高，如有重复变量，则取优先级高的

yaml/yml实现多环境配置：https://www.cnblogs.com/tudou1179006580/p/14875313.html

### thymeleaf使用

1. pom.xml文件中导入依赖

   ```xml
   <!--thymeleaf-->
   <dependency>
       <groupId>org.thymeleaf</groupId>
       <artifactId>thymeleaf-spring5</artifactId>
   </dependency>
   <dependency>
       <groupId>org.thymeleaf.extras</groupId>
       <artifactId>thymeleaf-extras-java8time</artifactId>
   </dependency>
   ```

2. html文件中引入`<html lang="en" xmlns:th="http://thymeleaf.org">`

3. 用法

   ```java
   变量表达式：${...}
   选择表达式：*{...}
   消息表达式：#{...}
   链接表达式：@{...}
   片段表达式：~{...}
   ```

### AOP

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

给指定注释添加OperateLog方法，如果是整个包执行过程，则不需要@annotation

```java
package com.example.demo.config;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;
@Component
@Aspect
public class OperateLog {
    //execution : * 返回任意结果
    // com.example.demo.api.*.*(..)  包名com.example.demo.api下的所有类，所有方法，方法参数任意
    // 且方法带有OperateLog注释
    @Pointcut("execution (* com.example.demo.api.*.*(..)) && @annotation(com.example.demo.annotation.OperateLog))")
    public void log(){
    }
    @Before("log()")
    public void insertLogBefore(){
        System.out.println("insertLogBefore");
    }
    @After("log()")
    public void insertLogAfter(){
        System.out.println("insertLogAfter");
    }
}
```

### 拦截器Interceptor 过滤器Filter 监听器Listener

**过滤器定义方法一**：使用@WebFilter注解实现，最简单，创建添加@WebFilter注解的AnnotationFilter类，然后在springbootApplication中**<font color=red>添加@SpringBootApplication注解</font>**

```java
package com.example.demo.config;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

/**
 * @Author William
 * @Date 2021/9/28 10:37
 * @Version 1.0
 */
@WebFilter(filterName = "annotation",urlPatterns = "/*")
public class AnnotationFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("annotationFilter success");
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}
```

定义拦截器

```java
package com.example.demo.interceptor;

import com.alibaba.fastjson.JSON;
import com.auth0.jwt.JWT;
import com.auth0.jwt.JWTVerifier;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.exceptions.JWTDecodeException;
import com.auth0.jwt.exceptions.JWTVerificationException;
import com.example.demo.annotation.PassToken;
import com.example.demo.annotation.UserLoginToken;
import com.example.demo.entity.ResultUtil;
import com.example.demo.entity.User;
import com.example.demo.service.UserService;
import com.example.demo.util.TokenUtil;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.lang.reflect.Method;


/**
 * @author jinbin
 * @date 2018-07-08 20:41
 */
public class AuthenticationInterceptor implements HandlerInterceptor {
    @Autowired
    UserService userService;
    private static Logger log = LoggerFactory.getLogger(AuthenticationInterceptor.class);
	//可以通过handler获取到方法注释，参数，方法名等信息
    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object handler) throws Exception {
        //如果不是映射到方法直接通过
        if (!(object instanceof HandlerMethod)) {
            return true;
        }
        HandlerMethod handlerMethod = (HandlerMethod) object;
        Method method = handlerMethod.getMethod();

        //如果映射是HandlerMethod，则可获取到方法信息
        if(handler instanceof HandlerMethod){
            HandlerMethod handlerMethod = (HandlerMethod) handler;
            log.info("当前拦截的方法为：{}",handlerMethod.getMethod().getName());
            log.info("当前拦截的方法参数长度为：{}",handlerMethod.getMethod().getParameters().length);
            log.info("当前拦截的方法为：{}",handlerMethod.getBean().getClass().getName());
            System.out.println("开始拦截---------");
            String uri = request.getRequestURI();
            System.out.println("拦截的uri："+uri);
        }

        //检查有没有需要用户权限的注解UserLoginToken
        if (method.isAnnotationPresent(UserLoginToken.class)) {
            String token = TokenUtil.getToken();
            log.info("当前用户的token:" + token);
			//获取UserLoginToken注解
            UserLoginToken userLoginToken = method.getAnnotation(UserLoginToken.class);
            if (userLoginToken.required()) {
                return true;
            }
        }
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {

    }
}
```

过滤器定义方法二：

```java
package com.example.demo.interceptor;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpServletResponseWrapper;
import java.io.IOException;

/**
 * @Author William
 * @Date 2021/9/28 9:46
 * @Version 1.0
 */
public class GeneralFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("GeneralFilter success");
        HttpServletRequest httpServletRequest = (HttpServletRequest)servletRequest;
        HttpServletResponseWrapper wrapper = new HttpServletResponseWrapper((HttpServletResponse) servletResponse);
        if(httpServletRequest.getRequestURI().indexOf("/index") != -1
        ) {
            filterChain.doFilter(servletRequest, servletResponse);
        }else {
            wrapper.sendRedirect("/logintest");
        }
    }
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}
```



configuration配置Interceptor  Filter

```java
package com.example.demo.config;

import com.example.demo.interceptor.AuthenticationInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.format.FormatterRegistry;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.validation.MessageCodesResolver;
import org.springframework.validation.Validator;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.HandlerMethodReturnValueHandler;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.config.annotation.*;

import java.util.List;

/***
 * 新建Token拦截器
* @Title: InterceptorConfig.java 
* @author MRC
* @date 2019年5月27日 下午5:33:28 
* @version V1.0
 */
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(authenticationInterceptor())
                .addPathPatterns("/**")
				.excludePathPatterns("/download/")
				//.excludePathPatterns("/logintest")
				.excludePathPatterns("/login");    // 拦截所有请求，通过判断是否有 @LoginRequired 注解 决定是否需要登录
    }
    @Bean
    public AuthenticationInterceptor authenticationInterceptor() {
        return new AuthenticationInterceptor();
    }
	@Bean
	FilterRegistrationBean filterRegist(){
		FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
		filterRegistrationBean.setFilter(new GeneralFilter());
		filterRegistrationBean.addUrlPatterns("/*");
		System.out.println("_______________________执行一次filterRegist");
		return filterRegistrationBean;
	}
}
```

### 消息国际化配置

resources文件夹下创建i18n文件夹，在其下创建login.properties、login_zh_CN.properties、login_en_US.properties文件，通过可视化创建不同语言的属性后，在配置文件中添加文件位置`spring.messages.basename=i18n.login` ;最后在html页面中添加`th:text="#{login.tip}";

详情见：https://www.bilibili.com/video/BV1PE411i7CV?p=22&spm_id_from=pageDriver

1. 系统语言默认是根据浏览器请求头的Accept-language的信息，生成对应的LocaleResolver，返回给系统执行；可以自定义一个LocaleResoler来根据请求中的参数来实现语言切换。请求可以设置为：`<a class="btn btn-sm" th:href="@{/index.html(lang='zh_CN')}">中文</a>`

```java
package com.example.springboot.config;

import org.springframework.web.servlet.LocaleResolver;
import org.thymeleaf.util.StringUtils;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Enumeration;
import java.util.Locale;

public class IndexLocaleResolver implements LocaleResolver {
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        String lang= (String) httpServletRequest.getParameter("lang");
        //先获取默认的localeResolver
        Locale locale=Locale.getDefault();
        //如果链接里有参数则生成locale
        System.out.println("==========="+lang);
        if(!StringUtils.isEmpty(lang)){
            String[] split=lang.split("_");
            locale=new Locale(split[0],split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
    }
}
```

2. 在config中配置bean

```java
package com.example.springboot.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class IndexConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }

    //自定义的国际化bean加入
    @Bean
    public LocaleResolver localeResolver(){
        return new IndexLocaleResolver();
    }
}
```

### 404 500配置

在resources目录下的static下创建error文件夹，然后创建404.html、505.html即可

### 前端框架网站

bootstrap、layui、semantic-ui

### 文件上传

1. application.properties文件配置

```properties
#设置单个文件大小
spring.servlet.multipart.max-file-size= 50MB
#设置单次请求文件的总大小
spring.servlet.multipart.max-request-size= 50MB
```

2. controller配置

```java
    private static String baseDir = "files/";

    /**
     * 文件上传
     * @param file
     * @param certno
     * @param httpServletRequest
     * @return
     */
    @UserLoginToken
    @PostMapping("/uploadgrant")
    @ResponseBody
    public Object uploadcert(@RequestParam("file") MultipartFile file, @RequestParam String applyseq, HttpServletRequest httpServletRequest) {

        JSONObject jsonObject = new JSONObject();
        //生成唯一的UUID与源文件拼接
        String name = UUID.randomUUID().toString() + "-" + file.getOriginalFilename();
        File outFile = new File(baseDir + name);
        try {
            FileUtils.copyToFile(file.getInputStream(), outFile);
        } catch (IOException e) {
            e.printStackTrace();
            jsonObject.put("Msg", "上传失败");
            jsonObject.put("code", 202);
            jsonObject.put("data", "");
            jsonObject.put("total","");
            return jsonObject;
        }
        // String url=httpServletRequest.getHeader("REFERER");

        String url=httpServletRequest.getScheme()+"://"+httpServletRequest.getServerName()+":"+httpServletRequest.getServerPort()+"/";
        System.out.println("name:"+name);
        System.out.println("url:"+url);

        name=url.substring(0, url.indexOf("/",9)+1)+"download/"+name;
        String username= TokenUtil.getTokenUsername();

        if (file.isEmpty()) {
            jsonObject.put("Msg", "上传失败");
            jsonObject.put("code", 202);
            jsonObject.put("data", "");
            jsonObject.put("total","");
            return jsonObject;
        }
        //try {
        //    System.out.println("文件大小为："+file.getSize()+":"+file.getBytes());
        //} catch (IOException e) {
        //    e.printStackTrace();
        //}
        //org.apache.tomcat.util.http.fileupload.impl.FileSizeLimitExceededException: The field file exceeds its maximum permitted size of 1048576 bytes
        System.out.println("开始上传");

        String fileName = file.getOriginalFilename();
        String filePath = "temp";

        File dest = new File(new File(filePath).getAbsolutePath() + "/" + fileName);
        try {

            if (!dest.getParentFile().exists()) {
                dest.getParentFile().mkdirs();
            }

            file.transferTo(dest);

            LOGGER.info("连接数据库:");

            DBConnection db = new DBConnection();

            String sql;
            sql ="update patentgrant set remark='" +name+"' where ifnull(applyseq,'null')='"+applyseq+"'";

            LOGGER.info("sql:"+sql);
            db.update(sql);

            jsonObject.put("Msg", "上传成功");
            jsonObject.put("code", 200);
            jsonObject.put("data", "");
            jsonObject.put("total","");
            return jsonObject;
        } catch (IOException e) {
            LOGGER.error(e.toString(), e);
        }
        jsonObject.put("Msg", "上传失败");
        jsonObject.put("code", 202);
        jsonObject.put("data", "");
        jsonObject.put("total","");
        return jsonObject;
    }

```

### JDBC连接数据库获取数据

```java
package com.example.springdata.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Map;

@RestController
public class JDBController {
    @Autowired
    JdbcTemplate jdbcTemplate;

    /**
     * 查询所有数据
     * 自动查询表中所有字段
     * jdbctemplate自动有事物管理
     * @return
     */
    @GetMapping("/list")
    public List<Map<String ,Object>> userList(){
        String sql="select * from student";
        List<Map<String ,Object>> listmap=jdbcTemplate.queryForList(sql);
        return listmap;
    }

    @PostMapping("/add")
    public String add(){
        String sql="insert into test.student values(1,'23',12)";
        jdbcTemplate.update(sql);
        return "ok";
    }

    /**
     * 原生jdbc实现的另一种方式
     * @return
     */
    @PostMapping("/update")
    public String update(){
        String sql="update test.student set name=? where id=1";
        Object[] objects= new Object[2];
        objects[0]="huang";
        objects[1]=12;
        jdbcTemplate.update(sql,objects);
        return "ok";
    }

    /**
     * 原生jdbc的另一种实现方式
     * @param id
     * @return
     */
    @PostMapping("/update/{id}")
    public String delete(@PathVariable("id") int id){
        String sql="delete from test.student where id = ?";
        jdbcTemplate.update(sql,id);
        return "ok";
    }
}
```

### Druid

1. 添加druid配置

```properties
#连接池的配置信息
spring.datasource.initialSize=5
spring.datasource.minIdle=5
spring.datasource.maxActive=20
# 配置获取连接等待超时的时间
spring.datasource.maxWait=60000
# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
spring.datasource.timeBetweenEvictionRunsMillis=60000
# 配置一个连接在池中最小生存的时间，单位是毫秒
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=SELECT 1 FROM DUAL
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false
# 打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.poolPreparedStatements=true
spring.datasource.maxPoolPreparedStatementPerConnectionSize=20
# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall,log4j
# 通过connectProperties属性来打开mergeSql功能；慢SQL记录
spring.datasource.connectionProperties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000
#是否启用StatFilter默认值true
spring.datasource.druid.web-stat-filter.enabled=true
#多个白名单IP以逗号分隔
druid.monitor.allow=127.0.0.1
#多个黑名单IP以逗号分隔
druid.monitor.deny=0.0.0.0
#druid监控管理界面登录帐号
druid.monitor.loginUsername=admin
#druid监控管理界面登录密码
druid.monitor.loginPassword=qwe
#是否开启重置功能
druid.monitor.resetEnable=false
```

2. 设置java代码，后可以通过访问localhost:8080进行访问，通过configuration配置filter

```java
package com.example.springdata;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.support.http.StatViewServlet;
import com.alibaba.druid.support.http.WebStatFilter;
import org.omg.CosNaming.BindingIterator;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.Map;

@Configuration
public class DruidConfig {
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }
    //后台监控
    //由于springboot内置了servlet容器，所以没有web.xml，使用ServletRegistrationBean来替代
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean<StatViewServlet> bean=new ServletRegistrationBean<>(new StatViewServlet(),"/druid/*");

        return bean;
    }
    //配置过滤器
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean=new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());;

        //设置过滤的请求
        Map<String,String> initParameters=new HashMap<>();
        initParameters.put("exclusions","*.js,*.css,/druid/*");
        bean.setInitParameters(initParameters);

        return bean;
    }
}

```

### spring整合mybatis

1. pom.xml配置

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>
```

2. application.properties配置

```properties
#类型包
mybatis.type-aliases-package=com.kuang.pojo
#mapper.xml包
mybatis.mapper-locations=classpath:mybatis/mapper*.xml
```

3. 分别创建对应的user.java，userMapper.java（接口），userMapper.xml

实体类

```java
package com.kuang.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class Department {

    private Integer id;
    private String departmentName;

}
```

Mapper.java接口类

```java
//@Mapper : 表示本类是一个 MyBatis 的 Mapper
@Mapper
@Repository
public interface DepartmentMapper {

    // 获取所有部门信息
    List<Department> getDepartments();

    // 通过id获得部门
    Department getDepartment(Integer id);

}
```

mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.kuang.mapper.DepartmentMapper">

    <select id="getDepartments" resultType="Department">
       select * from department;
    </select>

    <select id="getDepartment" resultType="Department" parameterType="int">
       select * from department where id = #{id};
    </select>

</mapper>
```

Controller

```java
@RestController
public class DepartmentController {
    
    @Autowired
    DepartmentMapper departmentMapper;
    
    // 查询全部部门
    @GetMapping("/getDepartments")
    public List<Department> getDepartments(){
        return departmentMapper.getDepartments();
    }

    // 查询全部部门
    @GetMapping("/getDepartment/{id}")
    public Department getDepartment(@PathVariable("id") Integer id){
        return departmentMapper.getDepartment(id);
    }
    
}
```

### springSecurity

1. pom.xml配置

```xml

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<!--security整合thymeleaf-->
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity4</artifactId>
    <version>3.0.4.RELEASE</version>
</dependency>
```

2. html中添加命名空间

```html
xmlns="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4"
```

### Shiro

1. pom.xml依赖

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.7.1</version>
</dependency>

<!--shiro整合thymeleaf-->
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.0.0</version>
</dependency>
```



### Swagger

1. pom.xml导包

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>3.0.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>3.0.0</version>
</dependency>
```

2. 开启swagger

```java
package cn.luischen.config;

import org.springframework.context.annotation.Configuration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
    
}
```



### 异步

1. 给需要异步执行的方法加上@Async
2. 给SpringbootApplication主类开启异步方法@EnableAsync



### 邮件

1. pom.xml

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

application.properties

```properties
spring.mail.username=1443652986@qq.com
#从qq邮箱开启pop3/smtp服务获取password
spring.mail.password=xxxxxxxxx
spring.mail.host=smtp.qq.com
#腾讯邮箱需要 开启加密验证
spring.mail.properties.mail.smtp.ssl.enable=true
```



```java
@Autowired
JavaMailSenderImpl mailSender;
//简单的邮件 
void contextLoads(){
    SimpleMailMessage mailMessage=new SimpleMailMessage();
   	mailMessage.setSubject("主题");
    mailMessage.setText("内容");
    mailMessage.setTo("接收人");
    mailMessage.setFrom("发送人");
    mailSender.send(mailMessage);
}
//复杂的邮件
void contextLoads2() throws MessagingException{
    MimeMessage mimeMessage = mailSender.createMimeMessage();
    //组装
    MimeMessageHelper helper=new MimeMessageHelper(mimeMessage,true);
    //正文
    helper.setSubject("主题");
    //第二个参数是否开启html模式
    helper.setText("内容",ture);
    //附件
    helper.addAttachment("1.jpg",new File("文件路径"));
    helper.addAttachment("2.jpg",new File("文件路径"));
    
    helper.setTo("1443652986@qq.com");
    helper.setFrom("1443652986@qq.com");
    mailSender.send(mimeMessage);
}
```



### 定时任务

1. //TaskScheduler
   //TaskExecutor
   @EnableScheduling//在SpringbootApplication主类中开启定时功能的注解
   @Scheduled 	//什么时候执行

```java
package com.example.springdata.service;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Service;

@Service
public class ScheduledService {
    //注解中写入cron表达式
    @Scheduled(cron = "")
    public void hello(){
        System.out.println("hello ,本次执行了");
    }
}
```

# MyBatisPlus

​	1. pom.xml

```xml
<!--mybatisplus导入-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3.3</version>
</dependency>
```

2. 配置数据库连接

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC&&useSSL=false&&autoReconnect=true&failOverReadOnly=false 
spring.datasource.username=root
spring.datasource.password=qwe
#配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

3. 编写mapper，继承BaseMapper类，泛型为对应的Dao

   ```java
   package com.example.springdata.mapper;
   
   import com.baomidou.mybatisplus.core.mapper.BaseMapper;
   import com.example.springdata.dao.Temp;
   import org.apache.ibatis.annotations.Mapper;
   import org.apache.ibatis.annotations.Select;
   
   import java.util.List;
   
   
   @Mapper
   public interface TempMapper extends BaseMapper<Temp> {
       
   }
   ```

   

4. 

### 数据库字段自动填充数据

https://mp.baomidou.com/guide/auto-fill-metainfo.html

1. 创建字段赋予 @TableField 注解

```java
package com.example.springdata.dao;

import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.annotation.TableField;
import lombok.Data;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;

import java.util.Date;

@Data
@Entity
public class Temp {
    @Id
    @Column(name = "id")
    private Integer id;

    @Column(name = "name")
    private String name;

    @Column(name = "password")
    private String password;

    @Column(name = "age")
    private Integer age;

    @TableField(fill = FieldFill.INSERT)
    private Date createTime;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}
```

2. 编写处理器

```java

```







































































## 问题

1. 新建springboot项目后，切换maven仓库直接报错，pom文件以及springbootapplication注解也报错

   `等待程序加载完成即可`

2. springboot中使用@Configuration来自定义视图解析器，如下，访问地址localhost:8080/

   ```java
   package com.example.springboot.config;
   
   import org.springframework.context.annotation.Configuration;
   import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
   import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
   
   @Configuration
   public class IndexConfig implements WebMvcConfigurer {
       @Override
       public void addViewControllers(ViewControllerRegistry registry) {
           registry.addViewController("/").setViewName("index");
           registry.addViewController("/index.html").setViewName("index");
       }
   }
   ```

   需要导入thymeleaf依赖,不然会报错

   ```XML
   <dependency>
       <groupId>org.thymeleaf</groupId>
       <artifactId>thymeleaf-spring5</artifactId>
   </dependency>
   <dependency>
       <groupId>org.thymeleaf.extras</groupId>
       <artifactId>thymeleaf-extras-java8time</artifactId>
   </dependency>
   ```

3. ```tex
   ***************************
   APPLICATION FAILED TO START
   ***************************
   
   Description:
   
   Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.
   
   Reason: Failed to determine a suitable driver class
   
   
   Action:
   
   Consider the following:
   	If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
   	If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).
   ```

   设置@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})，避免自动配置数据库

4. springboot访问接口全无反应，日志中有：Using generated security password: 3d2b89bf-8a18-4a64-b303-61ede15c53fa；

   ```xml
   原因是添加了security
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-security</artifactId>
   </dependency>
   ```

5. 报错如下，问题原因为将@SpringBootApplication注释了：

```JAVA
Caused by: org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
```

6. 检查配置文件中是否配置了datasource连接

```tex
***************************
APPLICATION FAILED TO START
***************************

Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class


Action:

Consider the following:
	If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
	If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).

```































































































