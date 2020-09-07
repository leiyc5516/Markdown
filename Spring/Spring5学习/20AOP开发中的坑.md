### 第二十章，AOP中的不常见问题

#### 1.业务相互调用

为了原始类方法相互调用，我们的原始类必须实现以下的方法，才能够做到调用代理两不误

主要实现ApplicationContextAware接口保存好ctx.

~~~java
public class UserServiceImpl implements UserService, ApplicationContextAware {
    private ApplicationContext ctx;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.ctx = applicationContext;
    }

    @Override
    public void login(String name, String password) {

        System.out.println("---login---");
        /*调用的是原始功能对象方法
         * 调用代理对象方法
         * */
        UserService userService = (UserService) ctx.getBean("userService");
        userService.register(name, password);
    }

    @Override
    public void register(String name, String password) {
        System.out.println("---register---");
    }
}

~~~

