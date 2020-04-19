---
layout: post
title: 基于反射机制实现动态操纵Javabean
categories: Java
description: 基于反射机制实现动态操纵Javabean
keywords: Java
---

#### 源码

```java
package org.fanlychie.util;
import java.util.Map;
import java.util.HashMap;
import java.lang.reflect.Field;
import java.lang.reflect.Modifier;
/**
 * 动态 Bean
 *
 * @author 范忠云（fanlychie）
 */
public class DynaBean {
    // Bean 对象
    private Object bean;

    // Bean 类型
    private Class&lt;?&gt; beanClass;

    // Bean 对象的非静态属性对照表
    private Map&lt;String, Field&gt; beanProps;

    /**
     * 实例化一个动态 Bean
     *
     * @param beanClass
     *            Bean 类型
     */
    public DynaBean(Class&lt;?&gt; beanClass) {
        this.beanClass = beanClass;
        this.beanProps = getDeclaredFieldsMap(beanClass);
    }

    /**
     * 实例化一个 Bean 对象
     */
    public void newBeanInstance() {
        try {
            bean = beanClass.newInstance();
        } catch (Throwable e) {
            throw new RuntimeException(e);
        }
    }
    /**
     * 获取属性的类型
     *
     * @param name
     *            属性名称
     * @return 返回属性的类型
     */
    public Class&lt;?&gt; getFieldType(String name) {
        Field field = beanProps.get(name);
        if (field == null) {
            throw new RuntimeException(castExceptionMessage(name));
        }
        return field.getType();
    }

    /**
     * 设置属性的值
     *
     * @param name
     *            属性名称
     * @param value
     *            属性的值
     */
    public void setFieldValue(String name, Object value) {
        Field field = beanProps.get(name);
        if (field == null) {
            throw new RuntimeException(castExceptionMessage(name));
        }
        try {
            field.set(bean, value);
        } catch (Throwable e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 获取属性的值
     *
     * @param bean
     *            对象
     * @param name
     *            属性名称
     * @return 返回对象中属性的值
     */
    public Object getFieldValue(Object bean, String name) {
        Field field = beanProps.get(name);
        if (field == null) {
            throw new RuntimeException(castExceptionMessage(name));
        }
        try {
            return field.get(bean);
        } catch (Throwable e) {
            throw new RuntimeException(e);
        }
    }
    /**
     * 获取 Bean 的实例
     *
     * @return 返回 Bean 的实例
     */
    public Object getBean() {
        return bean;
    }

    /**
     * 获取类声明的非静态属性表
     *
     * @param beanClass
     *            类
     * @return 返回类声明的非静态属性表
     */
    private Map&lt;String, Field&gt; getDeclaredFieldsMap(Class&lt;?&gt; beanClass) {
        // 获取类声明的属性集合
        Field[] fields = beanClass.getDeclaredFields();
        Map&lt;String, Field&gt; map = new HashMap&lt;String, Field&gt;();
        // 迭代属性集合
        for (Field field : fields) {
            // 剔除静态属性
            if ((field.getModifiers() &amp; Modifier.STATIC) != Modifier.STATIC) {
                // 强行设置成可访问
                field.setAccessible(true);
                map.put(field.getName(), field);
            }
        }
        return map;
    }

    /**
     * 异常信息
     *
     * @param name
     *            属性名称
     * @return 返回异常信息
     */
    private String castExceptionMessage(String name) {
        return String.format("Can not found property \"%s\" in class %s", name,
                beanClass.getSimpleName());
    }

}
```

#### 测试对象

```java
public class Person {
    private String name;

    private String sex;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSex() {
        return sex;
    }
    public void setSex(String sex) {
        this.sex = sex;
    }
    @Override
    public String toString() {
        return "Person [name=" + name + ", sex=" + sex + "]";
    }

}
```

#### 测试一

```java
public static void main(String[] args) {

    List&lt;Person&gt; persons = new ArrayList&lt;Person&gt;();

    // 创建动态 Bean 实例
    DynaBean dynaBean = new DynaBean(Person.class);

    // 创建一个 Bean 的实例
    dynaBean.newBeanInstance();
    // 设置 Bean 属性的值
    dynaBean.setFieldValue("name", "张三");
    dynaBean.setFieldValue("sex", "男");
    // 取出 Bean 对象
    persons.add((Person) dynaBean.getBean());
    // 创建一个 Bean 的实例
    dynaBean.newBeanInstance();
    // 设置 Bean 属性的值
    dynaBean.setFieldValue("name", "李四");
    dynaBean.setFieldValue("sex", "女");
    // 取出 Bean 对象
    persons.add((Person) dynaBean.getBean());

    System.out.println(persons);

}
```

#### 测试结果

```java
[Person [name=张三, sex=男], Person [name=李四, sex=女]]
```

#### 测试二

```java
public static void main(String[] args) {
    List&lt;Person&gt; persons = new ArrayList&lt;Person&gt;();
    Person person1 = new Person();
    person1.setName("张三");
    person1.setSex("男");
    persons.add(person1);
    Person person2 = new Person();
    person2.setName("李四");
    person2.setSex("女");
    persons.add(person2);
    // 创建动态 Bean 实例
    DynaBean dynaBean = new DynaBean(Person.class);
    for (Person person : persons) {
        System.out.println(String.format("[name=%s, sex=%s]",
                // 获取 Bean 对象的 name 属性的值
                dynaBean.getFieldValue(person, "name"),
                // 获取 Bean 对象的 sex  属性的值
                dynaBean.getFieldValue(person, "sex")));
    }
}
```

#### 测试结果

```java
[name=张三, sex=男]
[name=李四, sex=女]
```
