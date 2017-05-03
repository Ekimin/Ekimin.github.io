---
title: JDBC使用游标分页读取大量数据
tags:
  - Java
  - 数据库
categories: Java
date: 2015-12-25 00:00:00
updated:
comments:
permalink:
---


今天同步mysql中一张很大的表到另外一个数据库，折腾了半天写的程序报OOM。后来使用了limit限制每次读取量，新的问题又来了：程序跑到后面莫名卡死，而且效率低下。
查阅了文档，原来mysql中使用 limit X,Y 查询数据时，并不是从X开始查，而是从头开始读到X，然后返回X后Y条数据。当数据量大的时候仍然很慢的。


无果，继续各种搜索，发现原来jdbc有游标分页读取的功能。

<!--more-->

MySQL JDBC默认客户端数据接收方式为从服务器一次取出所有数据放在客户端内存中，fetch size参数不起作用，当一条SQL返回数据量较大时可能会出现JVM OOM。

解决方法：

设置游标读取

```java
ps = conn.prepareStatement(sql, ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY);
ps.setFetchSize(Integer.MIN_VALUE);
setFetchDirection(ResultSet.FETCH_REVERSE);
```

完整代码：

```java
import java.sql.*;

public class TestCursor {
    public static long importData(String sql) {
        String url = "jdbc:mysql://192.168.61.78:3306/bdsyn?user=pira&password=pira&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;autoReconnect=true&amp;failOverReadOnly=false&amp;maxReconnects=10&amp;rewriteBatchedStatements=true&amp;useCursorFetch=true&amp;defaultFetchSize=1000";

        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e1) {
            e1.printStackTrace();
        }

        long count = 0;

        Connection con = null;
        PreparedStatement ps = null;
        Statement st = null;
        ResultSet rs = null;

        try {
            con = DriverManager.getConnection(url);

            ps = con.prepareStatement(sql, ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY);

            ps.setFetchSize(Integer.MIN_VALUE);

            ps.setFetchDirection(ResultSet.FETCH_REVERSE);

            rs = ps.executeQuery();

            while (rs.next()) {

                count++;
                if (count % 10000 == 0) {
                    System.out.println(" 第  " + count + " 条数据！");
                }
            }
            System.out.println("取回数据量为  " + count + " 行！");
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                if (rs != null) {
                    rs.close();
                }
                if (ps != null) {
                    ps.close();
                }
                if (st != null) {
                    st.close();
                }
                if (ps != null) {
                    ps.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return count;
    }


    public static void main(String[] args){
        String sql= "select * from dt_clear_data";
        importData(sql);
    }
}
```