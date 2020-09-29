@Autowired的注入是单例注入，单例类似于静态，具有全局唯一性。
一个类使用@Autowired,那么这个类必须由spring容器托管，就比如这个类要打上@Component或@Service的注解。一个由Spring容器代理的类不能有不带@Autowired自己的私有成员变量。
我遇到的业务场景是，我要给A服务不同的Controller访问的路径的model中，放入B服务生成的Map令牌集合。

我要通过RestTemplate来请求B服务并待会结果信息。

此时的，错误代码：

```
public class CreateJwtUtil{
	@Autowired
	private RestTemplate restTemplate;
    public static Map<String,String> getJwts(){
		return restTemplate.postForObject(url,requestObject,Map.class);
	}
}
// 此时的错误 我在非Spring管理的类中使用了@Autowired
/*
 我这个RestTemplate是不是应该单例呢，明显不是
因为会有多个Controller来调用这个工具类
*/
```
调用这个方法的Controller
```
public Controlelr{
	
	@GetMapping("/test")
	public String test(){
		model.addAttribute("jwt",CreateJwtUtil.getJwts());
		return "jsp";
	}
}
```

解决方式一：
不依赖Spring容器
```
public class CreateJwtUtil{
	private RestTemplate restTemplate;
	public CreateJwtUtil(RestTemplate restTemplate){
		this.restTemplate = restTemplate;
	}
    public static Map<String,String> getJwts(){
		return restTemplate.postForObject(url,requestObject,Map.class);
	}
}
```
调用的Controller
```
public Controlelr{
	@Autowired
	private RestTemplate restTemplate;
	
	@GetMapping("/test")
	public String test(){
		CreateJwtUtil util = new CreateJwtUtil(restTemplate);
		model.addAttribute("jwt",util.getJwts());
		return "jsp";
	}
}
```
解决方式二：
依赖Spring容器，多例注入这个RestTemplate我不知道怎么实现。
多例注入的例子
方案一：
```
@Component
// @Scope默认是单例模式，即scope="singleton"
// prototype原型模式，每次获取Bean的时候会有一个新的实例
@Scope(value="prototype")
public class Test {
    private String name = "7";
}
```
注入方式
```
	@Autowired
	private Test test;
	@Autowired
	private Test test1;
```
方案二（对象工厂）：
```
@Component
// @Scope默认是单例模式，即scope="singleton"
// prototype原型模式，每次获取Bean的时候会有一个新的实例
@Scope(value="prototype")
public class Test {
    private String name = "7";
}
```
注入方式
```
public class Controller{
	@Autowired
	private ObjectFactory<Test> test;
	@GetMapping("/test")
	public void test(){
		this.test.getObject();
	}
}
```
方案三（动态代理）：
```
@Component
// @Scope默认是单例模式，即scope="singleton"
// prototype原型模式，每次获取Bean的时候会有一个新的实例
@Scope(value="prototype",proxyMode = ScopedProxyMode.TARGET_CLASS)
public class Test {
    private String name = "7";
}
```