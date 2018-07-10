## Junit模拟请求

```java
@RunWith(value=SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration(locations= {"classpath:ApplicationContext.xml","classpath:springmvc-serlvet.xml"})
public class SpringJunit4 {

	@Autowired
	DeptMapper deptMapper;
	@Autowired
	SqlSession sqlSessionBatch;
	
	//SpringMVC模拟请求的对象
	MockMvc mock;
	
	@Autowired
	WebApplicationContext context;
	@Test
	public void test() {
		//批处理操作
		DeptMapper mapper = sqlSessionBatch.getMapper(DeptMapper.class);
		for (int i = 0; i < 1000; i++) {
			mapper.insertSelective(new Dept(null, UUID.randomUUID().toString().substring(0, 5)));
		}
	}
	
	
	@Before
	public void initSpringMVC() {
		//获取模拟请求
		mock = MockMvcBuilders.webAppContextSetup(context).build();
	}
	
	@Test
	public void testccc() throws Exception {
		MvcResult andReturn = mock.perform(/* 设置pn参数值 */ MockMvcRequestBuilders.get("/pages").param("pn","1")).andReturn();
		MockHttpServletRequest request = andReturn.getRequest();

		//从请求域中获取传过来的PageInfo对象
		PageInfo pageInfo = (PageInfo)request.getAttribute("pageInfo");
		int endRow = pageInfo.getEndRow();
		System.out.println("最后一行:" + endRow);
		int firstPage = pageInfo.getFirstPage();
		System.out.println("第一页:" + firstPage);
		List<Dept> list = pageInfo.getList();
		System.out.println(list);
	}
}
```

