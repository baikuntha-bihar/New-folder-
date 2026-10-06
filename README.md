@Component @RequiredArgsConstructor public class ChargeBackValidator extends BaseValidator {

private static final String TO_DATE = "yyyy-mm-dd";
private static final String CHARGEBACK_STATUS = "Chargeback";
private final LoggerUtility logger = LoggerFactoryUtility.getLogger(this.getClass());

/**
 * Validate chargebackDashboardrequest
 * <p>
 * Mandatory fields:
 * 1. fromDate
 * 2. toDate
 * <p>
 * Optional fields:
 * 1. mId
 * 2. chargebackStatus
