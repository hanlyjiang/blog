---
title: 【Gson使用笔记】Gson 自定义TypeAdapter实现自定义解析需求
date: 2017-08-27 01:23:16
tags: Gson, Json解析
category: Java
---

##【2017-08-27日更新】可以更改为更加简单的实现方式
### 1. 数据结构更改为如下形式
```java
public class Dataset {

    String title;

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    private Map<String, String> child = new LinkedHashMap<>();

    public Map<String, String> getChild() {
        return child;
    }

    public void setChild(Map<String, String> child) {
        this.child = child;
    }
}
```
### 2. 直接使用默认的Gson对象解析

### 3. 测试代码
```java

   String datasetJson = "{\n" +
            "  \"title\":\"这是标题\",\n" +
            "  \"child\":{\n" +
            "    \"name\":\"小宝宝\",\n" +
            "    \"age\":\"1\",\n" +
            "    \"gender\":null\n" +
            "  }\n" +
            "}";

    String datasetJson2 = "{\n" +
            "  \"title\":\"这是标题\",\n" +
            "  \"child\":null\n" +
            "}";
    @Test
    public void test_不自定义TypeAdapter() {
        Gson gson = new GsonBuilder().create();
        Dataset2 dataset = gson.fromJson(datasetJson,Dataset2.class);
        Assert.assertEquals("这是标题",dataset.getTitle());
        Assert.assertEquals(3,dataset.getChild().size());
        Assert.assertEquals("小宝宝",dataset.getChild().get("name"));
        Assert.assertEquals("1",dataset.getChild().get("age"));
        Assert.assertNull(dataset.getChild().get("gender"));
        System.out.println(dataset.getTitle());
    }

    @Test
    public void test_不自定义TypeAdapter无数据() {
        Gson gson = new GsonBuilder().create();
        Dataset2 dataset = gson.fromJson(datasetJson2,Dataset2.class);
        Assert.assertEquals("这是标题",dataset.getTitle());
        Assert.assertNull(dataset.getChild());
        System.out.println(dataset.getTitle());
    }
```



## 需求：将一个Json对象解析过程中平展为Map对象
* 目标格式如下：
```json
{
  "title":"这是标题",
  "child":{
    "name":"小宝宝",
    "age":"1",
    "gender":null
  }
}
```

* 需要将child 部分解析为一个Map 对象，Map 的Key 为此处的 name、age、gender，值就是相应的值

## 解决：使用Gson解析，自定义TypeAdapter来自定义解析过程

### 1. 更改数据结构如下：
```java
public class Dataset {

    String title;
    Child child;

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public Child getChild() {
        return child;
    }

    public void setChild(Child child) {
        this.child = child;
    }

    public static class Child {

        private Map<String,String> map = new LinkedHashMap<>();

        public Map<String, String> getMap() {
            return map;
        }

    }

}
```

### 2. 自定义解析`Child`的类型适配器
```java
public class DatasetTypeAdapter extends TypeAdapter<Dataset.Child> {

    @Override
    public void write(JsonWriter out, Dataset.Child value) throws IOException {
    }

    @Override
    public Dataset.Child read(JsonReader reader) throws IOException {
        System.out.println("read called");
        Dataset.Child child = new Dataset.Child();
        JsonToken token = reader.peek();
        if (token.equals(JsonToken.NULL)) { //应对 对象为  null
            reader.skipValue();
            return null;
        }
        if (token.equals(JsonToken.BEGIN_OBJECT)) {
            reader.beginObject();
            String key = null;
            String value = null;
            while (!reader.peek().equals(JsonToken.END_OBJECT)) {
                if (reader.peek().equals(JsonToken.NAME)) {
                    key = reader.nextName();
                    if (reader.peek().equals(JsonToken.NULL)) {
                        value = "无";
                        reader.skipValue();
                    } else {
                        value = reader.nextString();
                    }
                    System.out.println(key + ":" + value);
                    child.getMap().put(key, value);
                }
            }
            reader.endObject();
        }
        return child;
    }
}

```

### 3. 如何将自定义的TypeAdapter加入到Gson解析过程
```java
// 使用如下方法创建gson对象
Gson gson = new GsonBuilder().registerTypeAdapter(Dataset.Child.class, new DatasetTypeAdapter()).create();
```

### 4. 测试
```java
    String datasetJson = "{\n" +
            "  \"title\":\"这是标题\",\n" +
            "  \"child\":{\n" +
            "    \"name\":\"小宝宝\",\n" +
            "    \"age\":\"1\",\n" +
            "    \"gender\":null\n" +
            "  }\n" +
            "}";

    String datasetJson2 = "{\n" +
            "  \"title\":\"这是标题\",\n" +
            "  \"child\":null\n" +
            "}";

    @Test
    public void test_TypeAdapter() {
        Gson gson = new GsonBuilder().registerTypeAdapter(Dataset.Child.class, new DatasetTypeAdapter()).create();
        Dataset dataset = gson.fromJson(datasetJson,Dataset.class);
        Assert.assertEquals("这是标题",dataset.getTitle());
        Assert.assertEquals(3,dataset.getChild().getMap().size());
        Assert.assertEquals("小宝宝",dataset.getChild().getMap().get("name"));
        Assert.assertEquals("1",dataset.getChild().getMap().get("age"));
        Assert.assertEquals("无",dataset.getChild().getMap().get("gender"));
        System.out.println(dataset.getTitle());
    }

    @Test
    public void test_TypeAdapterNull() {
        Gson gson = new GsonBuilder().registerTypeAdapter(Dataset.Child.class, new DatasetTypeAdapter()).create();
        Dataset dataset = gson.fromJson(datasetJson2,Dataset.class);
        Assert.assertEquals("这是标题",dataset.getTitle());
        Assert.assertEquals(null,dataset.getChild());
    }
```


