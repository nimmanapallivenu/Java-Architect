# Mocks, Stubs & Test Doubles

> **Module**: Code Quality  
> **Topic**: Mocks, Stubs & Test Doubles

---

## 📋 Table of Contents



- [Q1: What are mock objects?](#q1)
- [Q2: What is the difference between fake objects, mock objects, and stubs?](#q2)
- [Q3: What is a domain object?](#q3)
- [Q4: What are data transfer objects?](#q4)
- [Q5: What is an anemic model?](#q5)
- [Q6: What is the purpose of Dozer framework?](#q6)

---

## 🔹 Q1: What are mock objects?

**Answer:**

Mock objects are used in unit testing to ensure that your tests don’ t fail due to volatility of the data changes. There are mocking frameworks like
EasyMock, Mockito, and PowerMock .
The key point to remember regarding mock objects is the ability of the mock objects to verify if a particular method was invoked and if yes, how many times
was invoked. This is demonstrated with the last two lines with the verify statement. This is one of the key differences between using a mock object versus a
stub.
mport static org.mockito .Matchers .any;
mport static org.mockito .Mockito .mock ;
mport static org.mockito .Mockito .times ;
mport static org.mockito .Mockito .verify ;
mport static org.mockito .Mockito .when ;
mport org.junit.Before ;
mport org.junit.Test;
mport org.mockito .Mock ;
mport org.mockito .Mockito ;
mport org.mockito .MockitoAnnotations ;
public class MyAppControllerT est
{
 private static final String POR TFOLIO_CODE = "abc123" ;
 private static final java.util.Date VALUA TION_DA TE = new java.util.Date ();
 private MyAppService mockMyAppService ;
 private MyAppController controller ;
@Mock
HttpServletResponse response ;
@Before
public void setup () {
 MockitoAnnotations .initMocks (this);
 controller = new MyAppController ();
 mockMyAppService = mock (MyAppServiceImpl .class );
 controller .setMyAppService (mockMyAppService );
 
@Test

---

## 🔹 Q2: What is the difference between fake objects, mock objects, and stubs?

**Answer:**

Fake objects build a very lightweight implementation of the same functionality as provided by a component that you are faking. Since they take some
shortcut, they are not suitable for production.
Mocks are objects pre-programmed with expectations which form a specification of the calls they are expected to receive. You can use mocking frameworks
like EasyMock, Mockito, PowerMock, etc to achieve this. When an actual service is invoked, a mock object is executed with a known outcome instead of the
actual service. With mock objects, you can verify if expected method calls were made and how many times.
Stubs are like a mock class, except that they don’ t provide the ability to verify that methods have been called or not called. Generally services that are not ready
or currently not stable are stubbed to make the test code more stable or to proceed with your development work to swap to the actual implementation when it is
ready .
When to use what? You use a Mock when it’ s an object that returns values that you set to the tested class. You use a Stub to mimic an Interface or Abstract
class to be tested. In fact, the difference is very subtle and it doesn’ t really matter what you call it, fake, mock, or stub, they are all objects that aren’ t used in
production, and used for managing complexity to write quality tests.

---

## 🔹 Q3: What is a domain object?

**Answer:**

A domain object means a business object. Domain logic or business logic reside in “domain objects” and “business objects” that are protocol
independent. You can access them via any protocol. Domain objects “store data” and “stored data specific business logic” and “domain services” will have
business logic and manipulate the “domain objects”. Domain services often make use of a DAO layer to retrieve and store persistent data.public void testGetPositionFeedCSV () throws Exception {
 String str = "dummyCSV" ;
 //Set up behavior
 when (mockMyAppService .getPositionFeedCSV (any(PositionFeedCriteria .class ))).thenReturn (str);
 when (response .getW riter()).thenReturn (writer );
 
 //Invoke controller
 controller .getPositionFeedCSV (POR TFOLIO_CODE, VALUA TION_DA TE, response );
 
 //Verify behavior
 verify (mockMyAppService, times (1)).getPositionFeedCSV (any(PositionFeedCriteria .class ));
 verify (writer, times (1)).write (any(String .class ));

Domain Object with business logic
@Entity
@Table(name = "account_rebalance" )
public class Rebalance extends GenericDomainObject implements Serializable {
 @Id
 @GeneratedV alue(strategy = GenerationT ype.AUT O)
 @Column (name = "acc_rebal_id" )
 private Long id;
 @Column (name = "available_cash" )
 private Decimal cashA vailable = Decimal .ZERO ;
 @Column (name = "funding_method" )
 @Enumerated (EnumT ype.STRING )
 private FundingMethod fundingMethod = FundingMethod .CASH ;
 @Column (name = "instrument_type" )
 @Enumerated (EnumT ype.STRING )
 private InstrumentT ype instrumentT ype;
 //..... other state variables
 //getters and setters omitted
 //domain or business logic
 public boolean isCash (){
 //logic to determine if cash product based on 'instrumentT ype' and 'funding_method'
 }
 public boolean isInvestment (){
 //logic to determine if cash product based on 'instrumentT ype' and 'funding_method'
 }
 public String getMaxHedgeFundAmount () {
 //business logic to determine max hedge fund
 }

Domain Service interface
Domain Service implementation with business logic public getCashBalanceError () {
 //business logic
 }
public interface RebalanceService {
 public Long saveRebalance (Rebalance rebalance );
 List<Rebalance > findRebalances (RebalanceSearchCriteria criteria );
 Boolean cancelRebalance (Long id);
 public BigDecimal calculateCashBalance (Rebalance rebalance );
}
public class RebalanceServiceImpl implements RebalanceService {
 
 @Resource
 private RebalanceRepository rebalRepository; //dao for JP A calls
 public Long saveRebalance (Rebalance rebalance ) {
 return rebalRepository .saveRebalance (rebalance );
 }
 
 List<Rebalance > findRebalances (RebalanceSearchCriteria criteria ) {
 //businness logic and data access via rebalRepository
 }
 Boolean cancelRebalance (Long id) {

---

## 🔹 Q4: What are data transfer objects?

**Answer:**

A Data T ransfer Object (DT O) is an object that is used to encapsulate data, and send it from one layer of an application to another. DTOs are most
commonly used by the Services layer to transfer data to and from the UI layer. The main benefit is to map domain centric data to view centric data. There are
frameworks like Dozer to map data from a domain object to a DT O. This conversion can be an expensive process and may not be useful if there are no remote
calls involved. The domain objects themselves can be used as DT Os.
DTOs can be used as the models in the MVC pattern .
Another use for DT Os is to encapsulate parameters for r emote calls to minimize the network round trips. DT Os have state variables and getter/setter
methods.

---

## 🔹 Q5: What is an anemic model?

**Answer:**

Anemic domain model is the use of a domain model where the “domain objects” contain little or no business logic. This contradicts the notion of object-
oriented design where you have well encapsulated logic.
An anemic domain model is an anti-pattern because in an anemic model, your domain logic exists somewhere else, probably in a class full of class(static)
method or in multiple places, all with conflicting logic.

---

## 🔹 Q6: What is the purpose of Dozer framework?

**Answer:**

Convert Domain Objects to DT Os and DT Os back to Domain Objects in multi-tiered and multi-layered arechitecture.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » //businness logic and data access via rebalRepository
 }
 public BigDecimal calculateCashBalance (Rebalance rebalance ){
 // calculation business logic
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03


---

## 📚 Related Topics

- [Java Overview](../Module%2001%20-%20Java%20Overview/)
- [Java Data Types](../Module%2002%20-%20Java%20Data%20Types/)
- [OOP Concepts](../Module%2006%20-%20OOP%20and%20FP/)

---

## 💡 Key Takeaways

Review the questions above and ensure you understand:
- Core concepts and their practical applications
- Real-world scenarios and use cases
- Best practices and common pitfalls

---

**[⬆ Back to Top](#)**