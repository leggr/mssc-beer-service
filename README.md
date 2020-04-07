# MSSC Beer Service Kata

Spring Boot Microservice IntelliJ kata

## Introduction
Create an Intellij project based on MSSC Beer Service from git hub
Checkout the 'Fix Readme' Project and create a new branch at this point

```
## Create the BeerDto pojo in the web.model package
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class BeerDto {
    private UUID id;
    private Integer version;

    private OffsetDateTime createdDate;
    private OffsetDateTime lastModifiedDate;

    private BeerStyleEnum beerStyleEnum;

    private Long upc;

    private BigDecimal price;

    private Integer quantityOnHand;
}
```
- [Fn+Alt+Ins] Click on package guru.springframework.msscbeerservice and create a new package called web.model
- [Fn+Alt+Ins] to create the BeerDto
- [Alt+Enter] for quick fixes on the UUID
- [Ctrl+D] to duplicate the createdDate
- [Shift+F6] to refactor the name to create the lastModifiedDate
- [Alt+Enter] for quick fixes on the BeerStyleEnum to create it as an Enum type
- [Ctrl+Shift+N]DEVIATION now open the pom.xml file
- [Ctrl+Shift+Backspace] DEVIATION to navigate back to the BeerStyleEnum i.e. the last edit location
- [Alt+->/<-] DEVIATION to switch back to the pom.xml editor tab
- [Ctrl+F4] DEVIATION to close the current pom.xml tab should land you back at the BeerStyleEnum
- [Ctrl+E] DEVIATION show recent editor list and select back to the BeerDto to continue

## Create the BeerPagedList pojo in the web.model package
```
public class BeerPagedList extends PageImpl<BeerDto> {
    public BeerPagedList(List<BeerDto> content, Pageable pageable, long total) {
        super(content, pageable, total);
    }

    public BeerPagedList(List<BeerDto> content) {
        super(content);
    }
}
```
- [Fn+Alt+Ins] to create the BeerPagedList that extends PageImpl
- [Shift+Ctrl+N] to open the pom.xml file again
- [Ctrl+W] click into spring-boot-starter-web and expand selection beyond the artifact definition
- [Shift+Ctrl+W] shrink the selection back down to the artifact so that we can duplicate it
- [Ctrl+D] to duplicate the artifact and enter the id as spring-boot-starter-data-jpa
- [Alt+->/<-] to return to the BeerPagedList class
- [Alt+Enter] to quick fix the PageImpl import class which leaves an error due to constructors
- [Alt+Enter] or [Shift+Alt+Insert] to quick fix or generate code. Both give us the list of constructors from the super class to implement
- 

## Create the BeerController initial implementation
```
@RequestMapping("/api/v1/beer")
@RestController
public class BeerController {

    @GetMapping("/{beerId}")
    public ResponseEntity<BeerDto> getBeerById(@PathVariable("beerId") UUID beerId) {
        //todo impl later
        return new ResponseEntity<>(BeerDto.builder().build(), HttpStatus.OK);
    }

    @PostMapping
    public ResponseEntity saveNewBeer(@RequestBody BeerDto beerDto) {
        //todo impl later
        return new ResponseEntity<>(HttpStatus.CREATED);
    }

    @PutMapping
    public ResponseEntity updateBeerById(@PathVariable("beerId") UUID beerId, @RequestBody BeerDto beerDto) {
        //todo impl later
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);
    }
}
```
- [Fn+Alt+Ins] Click on package guru.springframework.msscbeerservice and create a new package called web.controller
- [Fn+Alt+Ins] Create the Beer Controller and add the getBeerById method
- [Ctrl+W] in the getBeerById method to expand selection fully.
- [Ctrl+D] to duplicate the selection to implement the createBeer POST method
- implement this create method now
- [Ctrl+D] on the createBeer method to implement the updateBeer method
- [Ctrl+W] click in the paramater list of the getBeerById method and expand selection, copy it, do the same in the updateBeer method and replace it with the clipboard data

## Update Junit4 to Junit5 for the existing Tests
```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>junit</groupId>
                    <artifactId>junit</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```
and
```
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-junit-jupiter</artifactId>
            <scope>test</scope>
```
and
```
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
```
- [Ctrl+Shift+N] goto the pom.xml and edit for excluding junit4 from spring, add in junit5 deps, mockito and h2database
- [Ctrl+Shift+F12] to toggle editor expansion to give more space
- [Fn+Alt+Ins] to generate dependency template code, insert the junit jupiter api artifact ids
- [Ctrl+W] to expand dependency block selection
- [Ctrl+D] to duplicate the dependency and update the new one to be junit jupiter engine
- [Ctrl+N] open the MsscBeerServiceApplicationTests and update to get it running. Replace @Runner... with @SpringBootTest and delete other imports
- [Alt+Enter] to quick fix the @Test import. this will bring in jupiter @Test annotation
- [Ctrl+Shift+F10] to run the test via cursor context


## Add new Junit5 tests for the BeerController
```
@WebMvcTest(BeerController.class)
class BeerControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Autowired
    ObjectMapper objectMapper;

    @Test
    void getBeerById() throws Exception {
        mockMvc.perform(get("/api/v1/beer/" + UUID.randomUUID()).accept(MediaType.APPLICATION_JSON))
        .andExpect(status().isOk());
    }

    @Test
    void saveNewBeer() throws Exception {
        BeerDto beerDto = BeerDto.builder().build();
        String beerDtoJson = objectMapper.writeValueAsString(beerDto);

        mockMvc.perform(post("/api/v1/beer/")
                .contentType(MediaType.APPLICATION_JSON_VALUE)
                .content(beerDtoJson))
                .andExpect(status().isCreated());
    }

    @Test
    void updateBeerById() throws Exception {
        BeerDto beerDto = BeerDto.builder().build();
        String beerDtoJson = objectMapper.writeValueAsString(beerDto);

        mockMvc.perform(put("/api/v1/beer/" + UUID.randomUUID())
                .contentType(MediaType.APPLICATION_JSON_VALUE)
                .content(beerDtoJson))
                .andExpect(status().isNoContent());
    }
}
```
- [Alt+left/right arrow] switch back to the BeerController so we can create the test for it
- [Shift+Ctrl+T] click to the class definition line to create a new test class for the BeerController. Select all methods to test
- [Ctrl+Shift+F12] to maximise the editor to make the view clearer and implement the tests
- [Ctrl+Alt+O] where necessary to optimise the imports
- [Alt+Enter] start implementing the mockmvc.perform method and import static reference for the get() implemention method
- same for the put and the post methods
- [Ctrl+Shift+F10] to run the test(s) from the cursor context

## Refactoring
Refactor the BeerDto to extract a BaseDto for the common members
- [Ctrl+E] switch to the BeerDto class
- [Shift+Ctrl+Alt+T] refactoring context menu
- [above+6] to Extract baseDto (don't select methods at this point)
- [above+7] to pull members up into the base clase. Select id, version,created and updated plus getters/setters.
- [Shift+F6] to refactor/rename baseDto into BaseDto

## Debugging
- Add a debug breakpoint in the BeerControllertest#saveNewBeer method
- [F9] open debug contexts and select your method to debut
- [F7] step into the method
- [Shift+F8] step out of the method
- [F8] to step over
- [Alt+F8] to evaluate the MediaType.APPLICATION_JSON constant.
- [F9] resume the program to the end or next breakpoint

## Other Standalone Shortcuts
- [Ctrl+Alt+S] open general settings, build, plugin, jvm etc etc

