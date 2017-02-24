---
title: Java实现AES加密
tags:
  - Java
  - AES
  - 加密
categories: Java
date: 2017-02-24 10:22:51
updated:
comments:
permalink:
---

AES是一个对称分组密码算法，由美国国家标准技术研究所在2001年发布，旨在取代DES成为广泛使用的标准。

本文记录了Java实现AES算法进行加密解密，实际测试时还用到了Base64进行变换，以便于存储。

<!--more-->

````java
package com.wentuotuo.aes;

import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

import javax.crypto.*;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.Security;

/**
 * Created by Ekimin on 2017/2/23.
 * UniTest
 */
public class AESMethod {
    //KeyGenerator 提供对称密钥生成器的功能，支持各种算法
    private KeyGenerator keygen;
    //SecretKey 负责保存对称密钥
    private SecretKey deskey;
    //Cipher负责完成加密或解密工作
    private Cipher c;
    //该字节数组负责保存加密的结果
    private byte[] cipherByte;

    public AESMethod() throws NoSuchAlgorithmException, NoSuchPaddingException{
        Security.addProvider(new com.sun.crypto.provider.SunJCE());
        //实例化支持DES算法的密钥生成器(算法名称命名需按规定，否则抛出异常)
        keygen = KeyGenerator.getInstance("AES");
        //生成密钥
        deskey = keygen.generateKey();
        //生成Cipher对象,指定其支持的DES算法
        c = Cipher.getInstance("AES");
    }

    /**
     * 对字符串加密
     *
     * @param str
     * @return
     * @throws InvalidKeyException
     * @throws IllegalBlockSizeException
     * @throws BadPaddingException
     */
    public byte[] Encrytor(String str) throws InvalidKeyException,
            IllegalBlockSizeException, BadPaddingException {
        // 根据密钥，对Cipher对象进行初始化，ENCRYPT_MODE表示加密模式
        c.init(Cipher.ENCRYPT_MODE, deskey);
        byte[] src = str.getBytes();
        // 加密，结果保存进cipherByte
        cipherByte = c.doFinal(src);
        return cipherByte;
    }

    /**
     * 对字符串解密
     *
     * @param buff
     * @return
     * @throws InvalidKeyException
     * @throws IllegalBlockSizeException
     * @throws BadPaddingException
     */
    public byte[] Decryptor(byte[] buff) throws InvalidKeyException,
            IllegalBlockSizeException, BadPaddingException {
        // 根据密钥，对Cipher对象进行初始化，DECRYPT_MODE表示加密模式
        c.init(Cipher.DECRYPT_MODE, deskey);
        cipherByte = c.doFinal(buff);
        return cipherByte;
    }

    /**
     * @param args
     * @throws NoSuchPaddingException
     * @throws NoSuchAlgorithmException
     * @throws BadPaddingException
     * @throws IllegalBlockSizeException
     * @throws InvalidKeyException
     */
    public static void main(String[] args) throws Exception {
        AESMethod de1 = new AESMethod();
        String msg ="怕是要翻水水";
        byte[] encontent = de1.Encrytor(msg);

        System.out.println("明文是:" + msg);
        System.out.println("加密后:" + encontent);
        BASE64Encoder base64Encoder = new BASE64Encoder();
        BASE64Decoder base64Decoder = new BASE64Decoder();
        String base64 = base64Encoder.encode(encontent);
        System.out.println("转为base64后:" + base64);
        byte[] rawCode = base64Decoder.decodeBuffer(base64);
        System.out.println("base64还原:" + rawCode);

        byte[] decontent = de1.Decryptor(rawCode);
        System.out.println("解密后:" + new String(decontent));
    }
}

````

**测试结果**

```
明文是:怕是要翻水水
加密后:[B@a3a7a74
转为base64后:ldQQ3iZzKBfUJ8o2ogQHxNcWfGvB1ddZja/+brSAx+A=
base64还原:[B@7482384a
解密后:怕是要翻水水
```

[C语言实现的AES加密](https://github.com/dhuertas/AES/blob/master/aes.c)