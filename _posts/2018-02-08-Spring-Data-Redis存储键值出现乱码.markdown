---
layout: post
title: "Spring-Data-Redis存储键值出现乱码"
---  

   最近在利用Spring Data操作redis出现了乱码，在key前面出现了\xAC\xED\x00\x05t\x00，value也是如此，只是内容不同，如下图所示：
   
    ![乱码](/img/post/20180208_1.png)
	
经过航哥的指导才发现，主要是序列化的原因，最开始的代码使用如下:
	
	```java
	
	@Autowired
	RedisTemplate<Object, Object> redisTemplate;
	
	```
	
	由于该方法使用的是默认的序列化工具，查看源码可以发现使用了标准的Java serialization；
	
	```java
	
	private boolean enableDefaultSerializer = true;
	private RedisSerializer<?> defaultSerializer;
	private ClassLoader classLoader;

	private RedisSerializer keySerializer = null;
	private RedisSerializer valueSerializer = null;
	private RedisSerializer hashKeySerializer = null;
	private RedisSerializer hashValueSerializer = null;
	private RedisSerializer<String> stringSerializer = new StringRedisSerializer();
	
	......
	其中：
	defaultSerializer = new JdkSerializationRedisSerializer(classLoader != null ? classLoader : this.getClass().getClassLoader());
	```
	
	但是由于标准的序列化添加了类信息，所以在key或者value前面出现了类似下面的结果：
	
	1."\xac\xed\x00\x05t\x00\x05atInt"
    2."\xac\xed\x00\x05t\x00\nmySuperKey"
    3."\xac\xed\x00\x05t\x00\bsuperKey"
	
	所以可以通过配置redisTemple的序列化来改变：
	
	```java
	
	@Bean("redisWithSerial")
	public RedisTemplate<Object, Object> getRedisTemplate(RedisTemplate<Object, Object> redisTemplate) {
	    RedisSerializer<String> stringSerializer = new StringRedisSerializer();
	    redisTemplate.setKeySerializer(stringSerializer);
	    redisTemplate.setValueSerializer(stringSerializer);
	    redisTemplate.setHashKeySerializer(stringSerializer);
	    redisTemplate.setHashValueSerializer(stringSerializer);
	    return redisTemplate;
	}
	
	```
	或者直接使用StringRedisTemplate代替RedisTemplate即可，因为StringRedisTemplate的序列化配置如下：
	
	```java
	
	public StringRedisTemplate() {
		RedisSerializer<String> stringSerializer = new StringRedisSerializer();
		setKeySerializer(stringSerializer);
		setValueSerializer(stringSerializer);
		setHashKeySerializer(stringSerializer);
		setHashValueSerializer(stringSerializer);
	}
	```
	
	最后测试解决了乱码：
	![乱码](/img/post/20180208_2.png)
	
	更多请参考[Spring Data with Redis](https://dzone.com/articles/spring-data-redis)和[Weird redis key with spring data Jedis](https://stackoverflow.com/questions/13215024/weird-redis-key-with-spring-data-jedis)。
	
	
	
