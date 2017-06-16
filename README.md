## Gson使用说明

* [添加引用](#添加引用)
* [gson的使用](#gson的使用)
* [其他](#其他)
    * [SerializedName注解](#serializedname注解)
    * [Expose注解](#expose注解)

#### 添加引用

    <p><code>build.gradle的dependencies中添加compile 'com.google.code.gson:gson:2.7'，
    也可以引用其他的版本</p></code>

```
dependencies {
    compile 'com.google.code.gson:gson:2.7'
}
```

#### gson的使用
    <p><code>首先谈谈我对gson的理解，实体类Model，json字符串jsonStr。
    转换关系可以有：</p></code>
```
    /**
     * 实体类转换成json字符串
     * @param model
     * @return
     */
    public static String modelToJson(Model model){
        String jsonStr = new GsonBuilder().create().toJson(model);
        return jsonStr;
    }

    /**
     * json字符串转换成实体类
     * @param jsonStr
     * @return
     */
    public static Model jsonToModel(String jsonStr){
        Model model = new GsonBuilder().create().fromJson(jsonStr, Model.class);
        return model;
    }

    /**
     * json字符串转换成实体类集合
     * @param jsonStr
     * @return
     */
    public static List<Model> jsonToModels(String jsonStr){
        List<Model> models = new GsonBuilder().create().fromJson(jsonStr, new TypeToken<List<Model>>(){}.getType());
        return models;
    }
```

    <p><code>常用的转换有这三种，
    实体类转换成Json字符串，Json字符串转换成实体类，Json字符串转换成实体类集合
    当然类可以相互嵌套</p></code>
```
public class ProductResultModel {
    private int code;//返回信息code码
    private String msg;//返回信息
    private List<ProductModel> products;//商品列表
    public class ProductModel{
        private String name;//商品名称
        private String price;//商品价格
    }
}
```
    例如接收返回结果先判断是否成功，然后获取实体类集合。

#### 其他
    
###### SerializedName注解
    该注解能指定该字段在JSON中对应的字段名称
```
public class Box {

  @SerializedName("w")
  private int width;

  @SerializedName("h")
  private int height;

  @SerializedName("d")
  private int depth;

}
```

###### Expose注解
    该注解能够指定该字段是否能够序列化或者反序列化，默认的是都支持（true）。
```
public class Account {

  @Expose(deserialize = false)
  private String accountNumber;

  @Expose
  private String iban;

  @Expose(serialize = false)
  private String owner;

  @Expose(serialize = false, deserialize = false)
  private String address;

  private String pin;
}
```
    需要注意的通过 builder.excludeFieldsWithoutExposeAnnotation()方法是该注解生效。
```
final GsonBuilder builder = new GsonBuilder();
builder.excludeFieldsWithoutExposeAnnotation();
final Gson gson = builder.create();
```