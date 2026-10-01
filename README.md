
@Component
@RequiredArgsConstructor
public class ChargeBackValidator extends BaseValidator {

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
     *
     * @param chargebackDashboardRequest chargebackDashboardrequest
     */
    public void validateChargebackSearchRequest(ChargebackDashboardRequest chargebackDashboardRequest) {
        logger.info("Inside validateChargebackSearchRequest for mId: {}", chargebackDashboardRequest.getMerchantId());
        errorDtoList = new ArrayList<>();
        validateMandatoryFields(chargebackDashboardRequest);
        validateLeadingTrailingSpaces(chargebackDashboardRequest);
        validateFieldsValue(chargebackDashboardRequest);
        validateChargebackDates(chargebackDashboardRequest.getFromDate(), chargebackDashboardRequest.getToDate());
        throwIfErrors();
        logger.info("Chargeback search request validation completed");
    }

    /**
     * Validate mandatory fields for chargeback search request.
     * <p>
     * fromDate and toDate are mandatory.
     *
     * @param request ChargebackSearchRequest
     */
    protected void validateMandatoryFields(ChargebackDashboardRequest request) {
        logger.info("Validating mandatory fields for chargeback search request");
        checkMandatoryDateField(request.getFromDate(), FROM_DATE);
        checkMandatoryDateField(request.getToDate(), TO_DATE);
        throwIfErrors();
    }

    /**
     * Validate field values for chargeback search request.
     * <p>
     * MID and chargebackStatus are optional.
     * Validation will happen only when values are provided.
     *
     * @param request ChargebackSearchRequest
     */
    protected void validateFieldsValue(ChargebackDashboardRequest request) {
        logger.info("Inside validateFieldsValue for chargeback search request, mId: {}", request.getMerchantId());

        // MID is optional
        if (StringUtils.isNotEmpty(request.getMerchantId())) {
            validateFixedFieldLength(request.getMerchantId(), TransactionConstant.MID_MAX_LENGTH, MID);
            throwIfErrors();
            validateFieldWithRegex(request.getMerchantId(), NUMBER_ONLY, MID, INCORRECT_FORMAT);
            throwIfErrors();
        }

        // Chargeback status is optional
//        if (StringUtils.isNotEmpty(request.getChargebackStatus())) {
//            validateChargebackStatus(request.getChargebackStatus());
//            throwIfErrors();
//        }
    }

    /**
     * Validate chargeback status.
     *
     * @param chargebackStatus String
     */
//    private void validateChargebackStatus(String chargebackStatus) {
//
//        logger.info("Validating chargeback status: {}", chargebackStatus);
//
//        /*
//         * Use your actual ChargebackStatus enum here.
//         *
//         * Example:
//         *
//         * ChargebackStatus.getChargebackStatus(chargebackStatus);
//         */
//        validateFieldValue(chargebackStatus, Arrays.stream(ChargebackStatus.values()).map(Enum::name).toList(), CHARGEBACK_STATUS);
//        throwIfErrors();
//    }

    /**
     * Validate leading and trailing spaces.
     *
     * @param request ChargebackSearchRequest
     */
    private void validateLeadingTrailingSpaces(ChargebackDashboardRequest request) {

        logger.info("Checking leading/trailing spaces for chargeback request");

        checkForLeadingTrailingAndSingleSpace(request.getMerchantId(), MID);
        checkForLeadingTrailingAndSingleSpace(request.getChargebackStatus(), CHARGEBACK_STATUS);
        throwIfErrors();
    }

    /**
     * Validate chargeback report dates.
     * <p>
     * Rules:
     * <p>
     * 1. fromDate is mandatory
     * 2. toDate is mandatory
     * 3. fromDate cannot be future date
     * 4. toDate cannot be future date
     * 5. fromDate cannot be greater than toDate
     * 6. Maximum date range is 6 months
     *
     * @param fromDate Long
     * @param toDate   Long
     */
    private void validateChargebackDates(Long fromDate, Long toDate) {

        logger.info("Validating chargeback dates fromDate: {}, toDate: {}", fromDate, toDate);
        // Step-1: Mandatory date validation
        checkMandatoryDateField(fromDate, FROM_DATE);
        checkMandatoryDateField(toDate, TO_DATE);
        throwIfErrors();
        // Step-2: Future from date
        if (fromDate > DateTimeUtils.endOfDayMillis()) {
            addError(FROM_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, FROM_DATE, FUTURE_DATE_ERROR));
            throwIfErrors();
        }
        // Step-3: Future to date
        if (toDate > DateTimeUtils.endOfDayMillis()) {
            addError(TO_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, TO_DATE, FUTURE_DATE_ERROR));
            throwIfErrors();
        }
        // Step-4: From date cannot be greater than to date
        if (fromDate > toDate) {
            addError(FROM_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, FROM_DATE, FROM_TO_DATE_ERROR));
            throwIfErrors();
        }
        // Step-5: Maximum six months
        validateSixMonthRange(fromDate, toDate);
        logger.info("Chargeback date validation completed");
    }

    /**
     * Validate that date range does not exceed six months.
     *
     * @param fromDate Long
     * @param toDate   Long
     */
    private void validateSixMonthRange(Long fromDate, Long toDate) {
        LocalDate fromLocalDate = Instant.ofEpochMilli(fromDate).atZone(ZoneId.systemDefault()).toLocalDate();
        LocalDate toLocalDate = Instant.ofEpochMilli(toDate).atZone(ZoneId.systemDefault()).toLocalDate();
        LocalDate maximumDate = fromLocalDate.plusMonths(6);

        if (toLocalDate.isAfter(maximumDate)) {
            logger.info("Chargeback date range exceeds six months. fromDate: {}, toDate: {}", fromDate, toDate);
            addError(FROM_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, FROM_DATE, CHARGEBACK_DATE_RANGE_6_MONTH_ERROR));
            throwIfErrors();
        }

    }
}
-------------------------
@Service
@RequiredArgsConstructor
public class ChargeBackDashBoardService {
    private final ChargebackDao chargebackDao;
    private final ChargeBackValidator chargeBackValidator;
    private final ChargebackBookingRepository chargebackBookingRepository;

    private final LoggerUtility logger = LoggerFactoryUtility.getLogger(this.getClass());

    public List<ChargebackBookingDto> getChargebackDetails(ChargebackDashboardRequest request) {
        Long fromDate = request.getFromDate();
        Long toDate = request.getToDate();

        return chargebackDao.getChargebackDetailsBetweenDate(fromDate, toDate);
    }

}
// this service layer how to implement here chargebackvalidation give me whole code  



