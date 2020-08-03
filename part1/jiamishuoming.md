## RSA加密标准

将POST的所有参数对转换为JSON字符串，对JSON字符串进行RSA 256位分段加密，RSA的公钥由微油提供。


##### RSA加密示例，以Java代码为例

```
package cn.edaijia.nuggets.api.third.web.common.util;
import cn.edaijia.userdev.common.tool.net.MD5Utils;
import org.apache.commons.lang.StringUtils;
import org.slf4j.Logger; 
import org.slf4j.LoggerFactory;
import org.springframework.web.client.RestTemplate;
import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

import javax.crypto.Cipher;
import java.security.*;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.TreeMap;

/**
 * RSAUtil
 * rsa加密辅助类
 *
 * @author zhaoyue
 * @date 2015/8/11
 */
public class RSAUtil {

    public static Logger logger = LoggerFactory.getLogger(RSAUtil.class);

    /**
     * 加密算法RSA
     */
    public static final String KEY_ALGORITHM = "RSA";

    /**
     * RSA最大加密明文大小
     */
    private static final int MAX_ENCRYPT_BLOCK = 117;

    /**
     * RSA最大解密密文大小
     */
    private static final int MAX_DECRYPT_BLOCK = 128;

    /**
     * 加密
     *
     * @param param  参数
     * @param pubKey 公钥
     * @throws Exception
     */
    public static String encrypt(String param, String pubKey) {
        String result = "";
        try {
            logger.info("要加密的参数是：{}", param);
            if (StringUtils.isEmpty(pubKey) || StringUtils.isEmpty(param)) {
                return result;
            }
            BASE64Decoder b64d = new BASE64Decoder();
            byte[] keyByte = b64d.decodeBuffer(pubKey);
            X509EncodedKeySpec x509ek = new X509EncodedKeySpec(keyByte);
            KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM);
            PublicKey publicKey = keyFactory.generatePublic(x509ek);

            Cipher cipher = Cipher.getInstance(KEY_ALGORITHM);
            cipher.init(Cipher.ENCRYPT_MODE, publicKey);
            byte[] sbt = param.getBytes();
//            byte[] epByte = cipher.doFinal(sbt);

            int inputLen = sbt.length;
            byte[] cache;
            byte[] lastResult = new byte[0];
            int offSet = 0;
            int i = 0;
            // 对数据分段加密
            while (inputLen - offSet > 0) {
                if (inputLen - offSet > MAX_ENCRYPT_BLOCK) {
                    cache = cipher.doFinal(sbt, offSet, MAX_ENCRYPT_BLOCK);
                } else {
                    cache = cipher.doFinal(sbt, offSet, inputLen - offSet);
                }
                lastResult = arraycat(lastResult, cache);
                i++;
                offSet = i * MAX_ENCRYPT_BLOCK;
            }

            BASE64Encoder encoder = new BASE64Encoder();
            result = encoder.encode(lastResult);
            logger.info("要加密的参数是：{},加密结果是：{}", param, result);
        } catch (Exception e) {
            logger.info("要加密的参数是：{},加密失败,错误信息：{}", param, e.getMessage(), e);
        }
        return result;
    }

    /**
     * 解密
     *
     * @param param  参数
     * @param priKey 私钥
     * @return
     * @throws Exception
     */
    public static String decrypt(String param, String priKey) {
        String result = "";
        try {
            logger.info("要解密的参数是：{}", param);
            if (StringUtils.isEmpty(priKey) || StringUtils.isEmpty(param)) {
                return result;
            }

            BASE64Decoder b64d = new BASE64Decoder();
            byte[] keyByte = b64d.decodeBuffer(priKey);
            PKCS8EncodedKeySpec s8ek = new PKCS8EncodedKeySpec(keyByte);
            KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM);
            PrivateKey privateKey = keyFactory.generatePrivate(s8ek);

            /** 得到Cipher对象对已用公钥加密的数据进行RSA解密 */
            Cipher cipher = Cipher.getInstance(KEY_ALGORITHM);
            cipher.init(Cipher.DECRYPT_MODE, privateKey);
            BASE64Decoder decoder = new BASE64Decoder();
            byte[] b1 = decoder.decodeBuffer(param);
            /** 执行解密操作 */
//            byte[] b = cipher.doFinal(b1);

            int inputLen = b1.length;
            byte[] lastResult = new byte[0];
            int offSet = 0;
            byte[] cache;
            int i = 0;
            // 对数据分段解密
            while (inputLen - offSet > 0) {
                if (inputLen - offSet > MAX_DECRYPT_BLOCK) {
                    cache = cipher.doFinal(b1, offSet, MAX_DECRYPT_BLOCK);
                } else {
                    cache = cipher.doFinal(b1, offSet, inputLen - offSet);
                }
                lastResult = arraycat(lastResult, cache);
                i++;
                offSet = i * MAX_DECRYPT_BLOCK;
            }

            result = new String(lastResult);
            logger.info("要解密的参数是：{},解密结果是：{}", param, result);
        } catch (Exception e) {
            logger.info("要解密的参数是：{},解密失败,错误信息：{}", param, e.getMessage(), e);
        }
        return result;
    }

    /**
     * 连接byte数组
     *
     * @param buf1
     * @param buf2
     * @return
     */
    private static byte[] arraycat(byte[] buf1, byte[] buf2) {
        byte[] bufret = null;
        int len1 = 0;
        int len2 = 0;
        if (buf1 != null) {
            len1 = buf1.length;
        }
        if (buf2 != null) {
            len2 = buf2.length;
        }
        if (len1 + len2 > 0)
            bufret = new byte[len1 + len2];
        if (len1 > 0) {
            System.arraycopy(buf1, 0, bufret, 0, len1);
        }
        if (len2 > 0) {
            System.arraycopy(buf2, 0, bufret, len1, len2);
        }
        return bufret;
    }
```
    

<!-- *****
[^Copyright © 微油科技(北京)有限公司 2020 all right reserved，powered by Gitbook] -->