# 5. Redis 的 Java 客户端 Jedis

1. Jedis 所需要的 jar 包，可通过 Maven 的依赖引入

1. 1. Commons-pool-1.6.jar
    2. Jedis-2.1.0.jar

1. 使用 windows 环境下连接虚拟机的 redis 注意事项

1. 1. 禁用Linux的防火墙：Linux(CentOS7)里执行命令 ： systemctl stop firewalld.service  
    2. redis.conf中注释掉bind 127.0.0.1 ,然后 protect-mode no。

1. Jedis 测试连通性



```
public static void main(String[] args) {
    //连接本地的 Redis 服务
    Jedis jedis = new Jedis("127.0.0.1",6379);
    //查看服务是否运行，打出pong表示OK
    System.out.println("connection is OK=======>:"+jedis.ping());
}
```



**完成一个手机验证码功能**

要求：

1. 输入手机号，点击发送后随机生成6位数字码，2分钟有效
2. 输入验证码，点击验证，返回成功或失败
3. 每个手机号每天只能输入3次



**发送验证码**



```
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    /**
         * 发送验证码，将手机号和验证码存储到redis中
         */
    //获取手机号
    String phone_no = request.getParameter("phone_no");
    //获取验证码
    String code = getCode(6);

    //拼接key
    String codeKey = "Verify_code:" + phone_no + ":code"; // Verify_code:12345:code
    //记录点击发送验证码的次数
    String countKey = "Verify_code:" + phone_no + ":count";

    Jedis jedis = new Jedis("192.168.1.128",6379);
    //判断发送验证码的次数
    String count = jedis.get(countKey);
    if (count == null) { //代表第一次
        jedis.setex(countKey,24*60*60,"1");
    }else if (Integer.parseInt(count) <= 2) {
        jedis.incr(countKey); //将countKey这个键对应的值+1
    }else if (Integer.parseInt(count) > 2) {
        response.getWriter().print("limit");
        jedis.close();
        return;
    }


    //向redis中进行存储，以手机号为键，以验证码为值
    jedis.set(codeKey,code);
    jedis.setex(codeKey,120,code); //设置过期时间
    jedis.close();
    response.getWriter().print(true);

}


//获得验证码
private String getCode(int length) {
    String code = "";
    Random random = new Random();
    for(int i = 0; i < length; i++) {
        int rand = random.nextInt(10);
        code += rand;
    }
    return code;
}
```



**校验验证码**



```
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //获取验证码和手机号
    String phone_no = request.getParameter("phone_no");
    String verify_code = request.getParameter("verify_code");

    //拼接key
    String codeKey = "Verify_code:" + phone_no + ":code";
    //从redis中获取手机号所对应的验证码
    Jedis jedis = new Jedis("192.168.1.128",6379);
    String code = jedis.get(codeKey);
    if (code.equals(verify_code)) {
        response.getWriter().print(true);
    }
    jedis.close();
}
```



![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1576639178126-77e2d7b2-18f0-47a4-875e-a2100378ac4d.png)

![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1576639417481-bcb30c31-04cf-4bf5-85fc-863804770092.png)![image.png](https://cdn.nlark.com/yuque/0/2019/png/446852/1576639847560-c6408a4a-6f8b-4917-8dd5-2e4a02a1b5d5.png)