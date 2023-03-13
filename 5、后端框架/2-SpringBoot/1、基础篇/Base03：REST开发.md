## 1、<span style="color:brown">REST简介：</span>

**1.1、概述：**

![image-20221005224146863](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/REST%E6%A6%82%E8%BF%B0.png)

**1.2、如何区分REST风格？**

![image-20221005224328145](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/REST%E9%A3%8E%E6%A0%BC%E5%8C%BA%E5%88%86.png)

## 2、<span style="color:brown">案例分析：</span>

### <span style="color:orange">根据REST风格对资源进行访问</span>称为RESTful

**2.1、设定HTTP请求动作：**

<img src="https://raw.githubusercontent.com/root-bine/image/main/Typora-image/RESTful%E4%B9%8B%E8%AE%BE%E5%AE%9Ahttp%E8%AF%B7%E6%B1%82.png" alt="image-20221005231209318" style="zoom: 80%;" />

**2.2、设定请求参数：**

![image-20221005231425663](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/RESTful%E4%B9%8B%E8%AE%BE%E5%AE%9A%E8%AF%B7%E6%B1%82%E5%8F%82%E6%95%B0.png)

**2.3、接受参数的注解：**

|       类别        |                             用途                             |
| :---------------: | :----------------------------------------------------------: |
| **@RequestParam** |               用于接收**url地址传参/表单传参**               |
| **@RequestBody**  | 用于接收前端的参数（**json数据**），当<u>使用post请求</u>时，会将参数放在`request body`中用于接收 |
| **@PathVariable** |      用于接收**路径参数**，使用`{参数名称}`描述路径参数      |

```scss
语法：@RequestParam(value=”参数名”,required=”true/false”,defaultValue=””)
 
value：参数名[可省略]
 
required：是否包含该参数，默认为true，表示该请求路径中必须包含该参数，如果不包含就报错。
 
defaultValue：默认参数值，如果设置了该值，required=true将失效，自动为false,如果没有传该参数，就使用默认值
```

**2.4、代码简化：**

![image-20221005235020940](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/RESTController%E5%BA%94%E7%94%A8.png)



<span style="color:red">**注意**</span>：@ResponseBody将<u>*方法的返回值*</u>，以特定的格式写入到`response body`区域，进而将数据返回给客户端！！！

![image-20221005235455208](https://raw.githubusercontent.com/root-bine/image/main/Typora-image/%40XxxxMapping%E7%9A%84%E5%9B%9B%E7%A7%8D%E8%AF%B7%E6%B1%82.png)