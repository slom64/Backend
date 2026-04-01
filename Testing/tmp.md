```java
@JsonTest
class CashCardJsonTest {

    @Autowired
    private JacksonTester<CashCard> json;

    @Test
    void cashCardSerializationTest() throws IOException {
        CashCard cashCard = new CashCard(99L, 123.45);
        assertThat(json.write(cashCard)).isStrictlyEqualToJson("expected.json");
        assertThat(json.write(cashCard)).hasJsonPathNumberValue("@.id");
        assertThat(json.write(cashCard)).extractingJsonPathNumberValue("@.id").isEqualTo(99);
        assertThat(json.write(cashCard)).hasJsonPathNumberValue("@.amount");
        assertThat(json.write(cashCard)).extractingJsonPathNumberValue("@.amount").isEqualTo(123.45);
    }
    
	@Test
	void cashCardDeserializationTest() throws IOException {
		String expected = """
		{
			"id":99,
			"amount":123.45
		}
		""";
		assertThat(json.parse(expected)).isEqualTo(new CashCard(1000L, 67.89));
		assertThat(json.parseObject(expected).id()).isEqualTo(1000);
		assertThat(json.parseObject(expected).amount()).isEqualTo(67.89);
	}

}
```


---

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class CashCardApplicationTests {
    @Autowired
    TestRestTemplate restTemplate;
	
    @Test
    void shouldReturnACashCardWhenDataIsSaved() {
        ResponseEntity<String> response = restTemplate.getForEntity("/cashcards/99", String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    }
    
    @Test  
	void shouldReturnACashCardWhenDataIsSaved(){  
	    ResponseEntity<CashCard> response = restTemplate.getForEntity("/cashcard/20", CashCard.class);  
	    assertThat(new CashCard(20L,20.5)).isEqualTo(response.getBody());
		
		DocumentContext documentContext = JsonPath.parse(response.getBody());  
		Number id = documentContext.read("$.id");  
		assertThat(id).isNotNull();
	}
}
```