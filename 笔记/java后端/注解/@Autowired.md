三种使用场景

1. 属性注入

   ```
   public class Test{
   	@Autowired
   	private User user;
   }
   ```

   


2. 构造器注入(推荐)

```
public class Test{
	private final User user;
	@Autowired
	public Test(User user){
	this.user = user;
	}
}
```

