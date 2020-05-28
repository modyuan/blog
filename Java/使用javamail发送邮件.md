# 使用JavaMail发送邮件



以outlook邮箱为例：

它的SMTP服务器是：

> server: smtp.office365.com
>
> port: 587
>
> method: STARTTLS



首先引入依赖，基本上这个版本到顶了，新版换了名字而且需要额外的依赖。

```xml
<!-- 邮件 -->
<dependency>
    <groupId>javax.mail</groupId>
    <artifactId>mail</artifactId>
    <version>1.4.7</version>
</dependency>
```



```java
import javax.mail.*;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Properties;

public class Test {

    public static void main(String[] args) throws MessagingException {
        
        String email = "xxxxxxx@outlook.com";
        String password = "pppppppppp";
        String sendTo = "666666666@qq.com";
        String subject = "邮件发送测试";
        String content = "你好呀👋";
        
        Properties props = new Properties();
        props.setProperty("mail.transport.protocol", "SMTP");
        props.setProperty("mail.smtp.host", "smtp.office365.com");
        props.setProperty("mail.smtp.port", "587");
        // 指定验证为true
        props.setProperty("mail.smtp.auth", "true");
        props.setProperty("mail.smtp.timeout", "5000");
        //启用START TLS
        props.setProperty("mail.smtp.starttls.enable", "true");
        // 如果是SSL安全连接，则这样写：
		// props.setProperty("mail.smtp.ssl.enable", "true");
        
        // 验证账号及密码，密码需要是第三方授权码
        Authenticator auth = new Authenticator() {
            public PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(email, password);
            }
        };
        Session session = Session.getInstance(props, auth);

        // 2.创建一个Message，它相当于是邮件内容
        MimeMessage message = new MimeMessage(session);
        // 设置发送者
        message.setFrom(new InternetAddress(email));
        // 设置发送方式与接收者
        message.setRecipient(MimeMessage.RecipientType.TO, new InternetAddress(sendTo));
        // 设置主题
        message.setSubject(subject);
        // 设置内容
        message.setContent(content, "text/html;charset=utf-8");

        // 3.创建 Transport用于将邮件发送
        Transport.send(message);
		System.out.println("发送成功");
    }
}

```





第一次发送outlook邮箱会报错：SMTPSendFailedException:STOREDRV.Submission.Exception:OutboundSpamException

这时需要进入邮箱验证。