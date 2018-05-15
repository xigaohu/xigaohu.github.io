---
title: spring boot实战1.5的集成测试
date: 2018-05-14 09:26:06
tags: spring
---
书中用的版本较早 有些变化 下面是readingList 基于Spring Boot 1.5的主页测试
```java
package readinglist;

import org.hamcrest.Matchers;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.hamcrest.Matchers.is;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@RunWith(SpringRunner.class) //不再使用SpringJUnit4ClassRunner.class
@SpringBootTest //不再使用SpringApplicationConfiguration
@AutoConfigureMockMvc //不需要下面的before了
public class MockMvcWebTests {

    @Autowired
    private MockMvc mockMvc;

//    @Before
//    public void setupMockMvc(){
//        mockMvc = MockMvcBuilders
//                .webAppContextSetup(webApplicationContext)
//                .build();
//    }

    @Test
    public void homePage() throws Exception {
        mockMvc.perform(get("/readingList/gao"))
                .andExpect(status().isOk())
                .andExpect(view().name("readingList"))
                .andExpect(model().attributeExists("books"))
                .andExpect(model().attribute("books",
                        is(Matchers.empty())));
    }
}
```
代码清单4-3 测试提交一本新书 postBook
[参考链接]("https://segmentfault.com/a/1190000007397071")