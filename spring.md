### 验证码

数据类

```java
@Autowired
private Producer producer;
//因为此时用户还没有登陆，为了确认身份，需要从客户端传一个uuid过来
//生成验证码
@GetMapping("captcha.jpg")
public void captcha(HttpServletResponse response, String uuid)throws IOException {
    response.setHeader("Cache-Control", "no-store, no-cache");
    response.setContentType("image/jpeg");
    SysCaptchaEntity captchaEntity = new SysCaptchaEntity();
    captchaEntity.setUuid(uuid);
    captchaEntity.setCode(code);
    //生成文字验证码
    String code = producer.createText();
    //获取图片验证码
    BufferedImage image=producer.createImage(code);

    ServletOutputStream out = response.getOutputStream();
    ImageIO.write(image, "jpg", out);
    IOUtils.closeQuietly(out);
}





```