## Spring单元测试笔记

Spring单元测试需要`Spring test`相关的Jar包组件。

```
@RunWith(value=SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations= {"classpath:ApplicationContext.xml"})
public class SpringJunit4 {
	@Autowired
	DeptMapper deptMapper;
	@Autowired
	SqlSession sqlSessionBatch;
	
	@Test
	public void test() {
		//批处理操作
		DeptMapper mapper = sqlSessionBatch.getMapper(DeptMapper.class);
		for (int i = 0; i < 1000; i++) {
			mapper.insertSelective(new Dept(null, UUID.randomUUID().toString().substring(0, 5)));
		}
	}
}
```