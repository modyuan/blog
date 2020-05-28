# Spring中的Test





junit4中：

```java
@RunWith(SpringRunner.class)
```



而在junit5中：

```java
@ExtendWith(SpringExtension.class)
```





使用Mock测试：

```java

@SpringBootTest
public class HttpTest {

    MockMvc mvc;

    @Autowired
    WebApplicationContext webApplicationContext;

    @BeforeEach
    public void init(){
        mvc =  MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
    }

    @Test
    public void test() throws Exception {
        mvc.perform(MockMvcRequestBuilders.get("/"))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.view().name("index"))
                .andExpect(MockMvcResultMatchers.content().string(containsString("Shiro")));
    }
}
```

