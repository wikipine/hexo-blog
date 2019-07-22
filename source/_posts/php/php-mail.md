---
title: PHP Mail的使用
date: 2019-07-19 10:50
categories: ['php']
tags: ['php']
comments: true
---

# 题记

TP5.1下的PHP Mail的使用，基于网易邮箱的发送。只是最基本的使用，会附上源码，如果是想看详细的使用文档，还是去看官网吧。[>>>快速飞跃<<<](https://github.com/PHPMailer/PHPMailer)

# 正文

### composer大法
```
composer require phpmailer/phpmailer
```
composer 一下，眯一会儿眼，这边安装了6.0的版本

然后直接上代码

```PHP
namespace app\common\libs\Mail;

use PHPMailer\PHPMailer\PHPMailer;

class Mail
{
    const SMTP_HOST = 'smtp.163.com';
    const MAILER_ACCOUNT = '网易的账号';
    const MAILER_PASSWORD = '授权码';
    const MAILER_USERNAME = '网易的账号';
    const MAILER_PORT = 465;

    // 收件人
    private $email_list = [];
    // 邮件内容
    private $email_content = [];

    /**
     * 设置邮箱
     * @param $email
     * @param $name
     * @return $this
     */
    public function setEmail($email, $name){
        $this->email_list[$email] = $name;
        return $this;
    }

    /**
     * 设置邮件内容
     * @param $title
     * @param $content
     * @return $this
     * @throws \Exception
     */
    public function setBody($title, $content){
        if(empty($this->email_list)){
            throw new \Exception('收件人未设置或设置失败');
        }
        // 邮件内容
        $this->email_content['title'] = $title ? $title : '(无主题)';
        $body = $content ? $content : '内容设置失败,请联系管理员';
        $body .= "<br/><br/><br/>系统邮件，无需回复";
        $this->email_content['HtmlBody'] = $body;
        $this->email_content['AltBody'] = htmlspecialchars($body);

        return $this;
    }

    /**
     * 发送邮件
     * @return bool
     * @throws \Exception
     */
    public function send(){
        if(empty($this->email_list)){
            throw new \Exception('Mail Users No Set');
        }
        if(empty($this->email_content)){
            throw new \Exception('Mail Content No Set');
        }
        // Passing `true` enables exceptions
        $mail = new PHPMailer(true);
        try {
            // 服务器配置
            $mail->CharSet ="UTF-8";                       // 设定邮件编码
            $mail->SMTPDebug = 0;                           // 调试模式输出
            $mail->isSMTP();                                 // 使用SMTP
            $mail->Host = self::SMTP_HOST;                // SMTP服务器
            $mail->SMTPAuth = true;                        // 允许 SMTP 认证
            $mail->Username = self::MAILER_USERNAME;    // SMTP 用户名  即邮箱的用户名
            $mail->Password = self::MAILER_PASSWORD;    // SMTP 密码  部分邮箱是授权码(例如163邮箱)
            $mail->SMTPSecure = 'ssl';                    // 允许 TLS 或者ssl协议
            $mail->Port = self::MAILER_PORT;             // 服务器端口 25 或者465 具体要看邮箱服务器支持

            // 发件人
            $mail->setFrom(self::MAILER_ACCOUNT, self::MAILER_USERNAME);
            // 收件人设置
            foreach ($this->email_list as $k=>$v){
                $mail->addAddress($k, $v);
            }
            // 回复的时候回复给哪个邮箱 建议和发件人一致
            $mail->addReplyTo(self::MAILER_ACCOUNT, self::MAILER_USERNAME);

            // Content
            // 是否以HTML文档格式发送  发送后客户端可直接显示对应HTML内容
            $mail->isHTML(true);
            $mail->Subject = $this->email_content['title'];
            $mail->Body = $this->email_content['HtmlBody'];
            $mail->AltBody = $this->email_content['AltBody'];

            $mail->send();
            return true;
        } catch (\Exception $e) {
            echo '邮件发送失败: ', $mail->ErrorInfo;
        }
    }
}
```

具体的使用规则

```PHP
$Mail = new Mail();
$Mail->setEmail($email, $name)->setBody($title, $content)->send();
// or
// $Mail->setEmail($email, $name)
// $Mail->setBody($title, $content)
// $Mail->send()
```

网易那边的配置，登录网易邮箱

**POP3/SMTP/IMAP** 开通即可，拿到授权码

其他的邮箱如QQ也类似，不过QQ好像要付一块钱的，很早之前弄过。

# 结束语

本文只是做简单的使用介绍，并分享了代码，欢迎指正
