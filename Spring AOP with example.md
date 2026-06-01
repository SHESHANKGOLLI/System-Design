
@Service
public class OrderService {

    public String processOrder(OrderRequest request) {
        // Validation check to simulate an error path
        if (request.getPrice() <= 0) {
            throw new IllegalArgumentException("Order aborted: Price must be greater than zero.");
        }
        
        // Success path execution
        return "SUCCESS_CONFIRMED_ID_9982";
    }
}


@Aspect
@Component
public class AopClass {

private static final Logger log = LoggerFactory.getLogger(AopClass.class);


@Before("execution=(* com.example.MyService.executeBussinessLogic(..))")
public void executeMyServiceMethodBeforeThis()
{
    log.info("This method executed before my service executed)
}


@After("execution=(* com.example.MyService.executeBussinessLogic(..))")
public void executeMyServiceMethodAfterhis()
{
    log.info("This method executed after my service executed)
}

@Around("execution=(* com.example.MyService.executeBussinessLogic(..))") //advice
public Object executeMyServiceMethodAroundThis(ProceedingJoinPoint jointPoint) 
//PointCut to tell where to execute the method
{
    Object result = jointPoint.proceed();
    log.info("This method executed before my service executed)
}

@PointCut("execution=(* com.example.OrderService.processOrder(..))") //advice
public void repeatThisPointCutMethod(){

}

// 2. AFTER RETURNING: Runs ONLY if processOrder succeeds
// 'returning = "returnedValue"' binds the string output to our parameter
@AfterReturning(pointCut="repeatThisPointCutMethod()", returning="returedValue")
public void repeatThisPointCutMethodAfterReturning(Object returnedValue){

}
// 3. AFTER THROWING: Runs ONLY if processOrder fails
// 'throwing = "returnedValue"' binds the thrown exception to our parameter
@AfterThrowing(pointCut="repeatThisPointCutMethod()", throwing="returedValue")
public void repeatThisPointCutMethodAfterThrowing(Throwable returnedValue){

}
}