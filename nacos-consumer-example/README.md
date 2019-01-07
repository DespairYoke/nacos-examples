springcloud远程调用提供了二种方式：一种restTemplete 一种fegin方式

### restTemplete方式
* 注入restTemplate
```java
    @Autowired
    private RestTemplate restTemplate;
```
* 向服务发送远程请求
```java
    @RequestMapping(value = "/echo-rest/{str}", method = RequestMethod.GET)
    public String rest(@PathVariable String str) {
        return restTemplate.getForObject("http://service-provider/echo/" + str,
                String.class);
    }
```
### fegin方式
大家都知道使用fegin一般会引入熔断机制。而spring cloud alibaba提供了sentinel组件给我使用。
```java
    @FeignClient(name = "service-provider", fallback = EchoServiceFallback.class, configuration = FeignConfiguration.class)
    public interface EchoService {
        @RequestMapping(value = "/echo/{str}", method = RequestMethod.GET)
        String echo(@PathVariable("str") String str);

        @RequestMapping(value = "/divide", method = RequestMethod.GET)
        String divide(@RequestParam("a") Integer a, @RequestParam("b") Integer b);

        @RequestMapping(value = "/notFound", method = RequestMethod.GET)
        String notFound();
    }
```
当我们访问服务出现错误或访问不存在时，会回调自己的实现
```java
class EchoServiceFallback implements EchoService {
    @Override
    public String echo(@PathVariable("str") String str) {
        System.out.println("echo==================");
        return "echo fallback";
    }

    @Override
    public String divide(@RequestParam Integer a, @RequestParam Integer b) {
        System.out.println("divide================");
        return "divide fallback";
    }

    @Override
    public String notFound() {
        System.out.println("notFound=================");
        return "notFound fallback";
    }
}
```