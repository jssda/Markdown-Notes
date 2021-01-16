# Mockito 测试

Mockito 可以模拟一些获得困难的对象, 方便我们的测试.

## 接口测试样式

```java
@SpringBootTest
@ExtendWith(SpringExtension.class)
@AutoConfigureMockMvc
@Slf4j
public class UserControllerTest {

    @Resource
    private MockMvc mockMvc;

    @Resource
    private ObjectMapper objectMapper;

    @Test
    void TestSaveUser() throws Exception {

        User user = new User();
        user.setUserId(UUID.randomUUID().toString());
        user.setUserName("wangjingjing");
        user.setAge(22);
        Address address = new Address();
        address.setAddrId(UUID.randomUUID().toString());
        address.setAddr("aaa");
        address.setNumber(205);
        user.setAddresses(Collections.singletonList(address));

        MvcResult success = mockMvc.perform(MockMvcRequestBuilders.request(HttpMethod.POST, "/user")
                                                    .contentType(MediaType.APPLICATION_JSON)
                                                    .content(objectMapper.writeValueAsString(user)))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.jsonPath("$.message").value("success"))
                .andDo(result -> System.out.println("result = " + result))
                .andReturn();

        success.getResponse().setCharacterEncoding("UTF-8");
        log.info(success.getResponse().getContentAsString());
    }
}
```

## 打桩

打桩的意思就是, 模拟一个方法的返回。用在此方法没有实现， 或者不需要测试此方法的时候， 直接给一个预期返回值即可。



```java
/**
 * @author wangjingjing@bonc.com.cn
 * @date 2021/1/15 15:54
 */
@SpringBootTest
@ExtendWith(SpringExtension.class)
@AutoConfigureMockMvc
@Slf4j
public class UserControllerTest {

    @Resource
    private MockMvc mockMvc;

    @Resource
    private ObjectMapper objectMapper;

    @MockBean
    private UserService userService;

    @Test
    void TestSaveUser() throws Exception {

        User user = new User();
        user.setUserId(UUID.randomUUID().toString());
        user.setUserName("wangjingjing");
        user.setAge(22);
        Address address = new Address();
        address.setAddrId(UUID.randomUUID().toString());
        address.setAddr("aaa");
        address.setNumber(205);
        user.setAddresses(Collections.singletonList(address));

        //打桩
        Mockito.when(userService.saveUser(Mockito.any())).thenReturn(false);

        MvcResult success = mockMvc.perform(MockMvcRequestBuilders.request(HttpMethod.POST, "/user")
                                                    .contentType(MediaType.APPLICATION_JSON)
                                                    .content(objectMapper.writeValueAsString(user)))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.jsonPath("$.message").value("success"))
                .andDo(result -> System.out.println("result = " + result))
                .andReturn();

        success.getResponse().setCharacterEncoding("UTF-8");
        log.info(success.getResponse().getContentAsString());
    }


}

```

