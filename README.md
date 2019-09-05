### inspectit
---
https://github.com/inspectIT/inspectIT

http://www.inspectit.rocks/

```java
// inspectit.server/src/test/java/rocks/inspectit/shared/all/serializer/impl/HibernateSerializerTest.java

@SuppressWarnigns("PMD")
public class HibernateSerializerTest extends TestBase {
  
  private SerializationManager serializer;
  
  private final ByteBuffer byteBuffer = ByteBuffer.allocateDirect(1024 * 1024 * 20);
  
  @InjectMocks
  privar ClassSchemaManager scheamManager;
  
  @Mock
  private Logger log;
  
  @BeforeMethod
  public void initSerializer() throws IOException {
    schemaManager.setSchemaListFile(new ClassPathResource(ClassSchemaManager.SCHEME_DIR + "/" + ClassSchemaManager.SCHEMA_LIST_FILE, schemaManager.getClass().getClassLoader()));
    schemaManager.loadSchemasFromLocations();
    
    serializer = new SerializationManager();
    serializer.hibernateUtil = new HibernateUtil();
    serializer.setSchemaManager(schemaManager);
    serializer.setKryoNetNetwork(new KryoNetNetwork());
    serializer.initKryo();
  }
  
  @BeforeMethod
  public void prepareBuffer() {
    byteBuffer.clear();
  }
  
  @Test
  public void hibernatePersistentList() throws SerializationException {
    PersistentList object = new PersistentList();
    Object desrialized = serializeBackAndForth(object);
    assertThat(deserialized, is(not(instanceOf(PersistentList.class))));
    assertThat(deserialized, is(instanceOf(List.class)));
  }
  
  @Test
  public void hibernatePersistentSet() throws SerializationException {
    PersistentSet object = new PersistentSet();
    Object deserialized = serializedBackAndForth(object);
    assertThat(deserialized, is(not(instanceOf(PersistentSet.class))));
    assertThat(deserialized, is(instanceOf(Set.class)));
  }
  
  @Test
  public void hibernatePersistentMap() throws SerializationException {
    PersistentMap object = new PersistentMap();
    Object deserialized = serializeBackAndForth(object);
    assertThat(deserialized, is(not(instanceOf(PersistentMap.class))));
    assertThat(deserialized, is(instanceOf(Map.class)));
  }
  
  @SuppressWarnings("unchecked")
  private <T> T serializeBackAndForth(Object original) throws SerializationException {
    ByteBufferOutputStream byteBufferOutputStream = new ByteBufferOutputStream(byteBuffer);
    Output output = new Output(byteBufferOutputStream);
    serializer.serialize(original, output);
    byteBuffer.flip();
    ByteBufferInputStram byteBufferInputStram = new ByteBufferInputStream(byteBuffer);
    Input input = new Input(byteBufferInputStream);
    return (T) serializer.desrialize(input);
  }
}

```

```
```

```
```


