---
title: 使用UUID
tags:
  - UUID
  - Java
categories: Java
date: 2016-02-21 16:46:20
updated:
comments:
permalink:
---


# 什么是UUID

UUID含义是通用唯一识别码 (Universally Unique Identifier)，这是一个软件建构的标准，也是被开源软件基金会 (Open Software Foundation, OSF) 的组织在分布式计算环境 (Distributed Computing Environment, DCE) 领域的一部份。UUID 的目的，是让分布式系统中的所有元素，都能有唯一的辨识资讯，而不需要透过中央控制端来做辨识资讯的指定。如此一来，每个人都可以建立不与其它人冲突的 UUID。在这样的情况下，就不需考虑数据库建立时的名称重复问题。目前最广泛应用的 UUID，即是微软的 Microsoft's Globally Unique Identifiers (GUIDs)，而其他重要的应用，则有 Linux ext2/ext3 档案系统、LUKS 加密分割区、GNOME、KDE、Mac OS X 等等。
</br>
UUID由以下几部分的组成：当前日期和时间(UUID的第一个部分与时间有关，如果你在生成一个UUID之后，过几秒又生成一个UUID，则第一个部分不同，其余相同)，时钟序列，全局唯一的IEEE机器识别号（如果有网卡，从网卡获得，没有网卡以其他方式获得），UUID的唯一缺陷在于生成的结果串会比较长。

<!--more-->

# 生成UUID

生成UUID的java实现非常简单：
```java
import java.util.UUID;

public class UUIDTest {
    public static void main(String[] args){
        UUID uuid = UUID.randomUUID();
        String result = uuid.toString();
        System.out.println(result);
    }
}
```
上面程序生成的UUID: 1d421c2c-b6d7-4f9b-8456-99b6b36199ec

**实际情况下，我们通常不需要中间的横杠**

以下是完整的实现和测试

```java
public class UUIDTest {
    public static String getUUID() {
        UUID uuid = UUID.randomUUID();
        String str = uuid.toString();
        // 去掉"-"符号
        String temp = str.substring(0, 8) + str.substring(9, 13) + str.substring(14, 18) + str.substring(19, 23) + str.substring(24);
        return str+","+temp;
    }
    //获得指定数量的UUID
    public static String[] getUUID(int num) {
        if (num < 1) {
            return null;
        }
        String[] strings = new String[num];
        for (int i = 0; i < num; i++) {
            strings[i] = getUUID();
        }
        return strings;
    }

    public static void main(String[] args) {
        String[] strings = getUUID(10);
        for (int i = 0; i < strings.length; i++) {
            System.out.println("UUID["+i+"]=====>"+strings[i]);
        }
    }
}
```

**测试结果**

```
UUID[0]=====>09a1512e-dc98-49fd-bc0d-ee65f42a59fa,09a1512edc9849fdbc0dee65f42a59fa
UUID[1]=====>545886f2-4de0-4297-8005-4c25de5923ff,545886f24de0429780054c25de5923ff
UUID[2]=====>36cb776c-8349-4106-9a11-26e3b3ad3a64,36cb776c834941069a1126e3b3ad3a64
UUID[3]=====>9096e155-0a28-4e43-b556-676a2b196f07,9096e1550a284e43b556676a2b196f07
UUID[4]=====>0b70094c-5502-490b-aa86-a73955b9389b,0b70094c5502490baa86a73955b9389b
UUID[5]=====>5b0483a1-7e7b-4f29-a0a9-fcf55136a31c,5b0483a17e7b4f29a0a9fcf55136a31c
UUID[6]=====>bbd13a23-b2c7-4074-9be3-1c15e9950f98,bbd13a23b2c740749be31c15e9950f98
UUID[7]=====>c2f170f1-3c15-4efb-99f5-710848d446f4,c2f170f13c154efb99f5710848d446f4
UUID[8]=====>a69c0a50-294c-45bb-ab31-5a70989c94b8,a69c0a50294c45bbab315a70989c94b8
UUID[9]=====>865a18ed-ac81-41ef-a79f-95d0ed3c829e,865a18edac8141efa79f95d0ed3c829e
```
