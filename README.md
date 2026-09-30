Fixed 


public class BaseValidator {
    private final LoggerUtility logger = LoggerFactoryUtility.getLogger(this.getClass());
    protected List<ErrorDto> errorDtoList;
    protected int mandatoryCount = 0;
    protected List<String> mandatoryFields;

    /**
     * Check mandatory field
     *
     * @param value     String
     * @param fieldName String
     */
    protected void checkMandatoryField(String value, String fieldName) {
        if (StringUtils.isEmpty(value) || StringUtils.equals(value, "null") || StringUtils.isAllBlank(value)) {
            addError(fieldName, TransactionErrorConstants.MANDATORY_ERROR_CODE, TransactionErrorConstants.MANDATORY_ERROR_MESSAGE);
        }
    }

    /**
     * countMandatoryField
     *
     * @param value     object
     *
     */
    protected void countAndAddMandatoryField(Object value, String fieldName) {

        if (ObjectUtils.isNotEmpty(value)) {
            this.mandatoryCount++;
        }
        mandatoryFields.add(fieldName);
    }

    /**
     * Check checkMandatoryFieldCount
     */
    protected void checkMandatoryFieldCount() {
        if (this.mandatoryCount < 1) {
            addError( mandatoryFields.toString(), TransactionErrorConstants.MANDATORY_ERROR_CODE, TransactionErrorConstants.ONE_MANDATORY_ERROR);
        }
    }

    /**
     * Check mandatory field
     *
     * @param value     UUID
     * @param fieldName String
     */
    protected void checkMandatoryField(Object value, String fieldName) {
        if (ObjectUtils.isEmpty(value)) {
            addError(fieldName, TransactionErrorConstants.MANDATORY_ERROR_CODE, TransactionErrorConstants.MANDATORY_ERROR_MESSAGE);
        }
    }

    /**
     * Check mandatory collection
     *
     * @param collection Collection
     * @param fieldName  String
     */
    protected void checkMandatoryCollection(Collection<?> collection, String fieldName) {
        if (CollectionUtils.isEmpty(collection)) {
            addError(fieldName, TransactionErrorConstants.MANDATORY_ERROR_CODE, TransactionErrorConstants.MANDATORY_ERROR_MESSAGE);
        }
    }

    /**
     * Check mandatary fields
     *
     * @param fieldName String
     * @param values    String...
     */
    protected void checkMandatoryFields(String fieldName, String... values) {
        boolean allEmpty = Arrays.stream(values).allMatch(StringUtils::isEmpty);
        if (allEmpty) {
            addError(fieldName, TransactionErrorConstants.MANDATORY_ERROR_CODE, TransactionErrorConstants.MANDATORY_ERROR_MESSAGE);
        }
    }

    /**
     * Check mandatory date field
     *
     * @param date      Long
     * @param fieldName String
     */
    protected void checkMandatoryDateField(Long date, String fieldName) {
        if (ObjectUtils.isEmpty(date) || date < 0) {
            addError(fieldName, MANDATORY_ERROR_CODE, MANDATORY_ERROR_MESSAGE);
        }
    }

    protected void validateFieldLength(String value, int maxLength, String fieldName) {
        if (StringUtils.isNotEmpty(value) && value.length() > maxLength) {
            logger.info("field {} exceeds maxLength {}", fieldName, maxLength);
            addError(EXCEED_LENGTH_ERROR_CODE, EXCEED_LENGTH_ERROR_MESSAGE, fieldName, maxLength);
        }
    }

    protected void validateFixedFieldLength(String value, int maxLength, String fieldName) {
        if (StringUtils.isNotEmpty(value) && value.length() != maxLength) {
            logger.info("field {} exceeds maxLength {}", fieldName, maxLength);
            addError(FIXED_LENGTH_ERROR_CODE, FIXED_LENGTH_ERROR_MESSAGE, fieldName, maxLength);
        }
    }

    protected void validateFieldWithRegex(String value, int maxLength, String regex, String fieldName, String message) {
        if (StringUtils.isNotEmpty(value) && (value.length() > maxLength || validate(value, regex))) {
            addError(fieldName, INVALID_ERROR_CODE, message + " " + maxLength);
        }
    }

    protected void validateFieldWithRegex(String value, String regex, String fieldName, String reason) {
        if (StringUtils.isNotEmpty(value) && validate(value, regex)) {
            addError(fieldName, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, fieldName, reason));
        }
    }

    /**
     * Validate if given
     *
     * @param enumName  String
     * @param fieldName String
     * @param enumClass Class
     * @param <E>       Enum Class Name
     */
    protected <E extends Enum<E>> void validateFieldValue(String enumName, String fieldName, Class<E> enumClass) {
        if (StringUtils.isNotEmpty(enumName) && !EnumUtils.isValidEnum(enumClass, enumName)) {
            addError(INVALID_ERROR_CODE, INVALID_ERROR_MESSAGE, fieldName, VALID_VALUES_ARE + EnumUtils.getEnumList(enumClass).stream().map(Enum::name).toList());
        }
    }

    /**
     * validate given field value from given list.
     *
     * @param value       String
     * @param validValues List of values
     * @param fieldName   String
     */
    protected void validateFieldValue(String value, List<String> validValues, String fieldName, String reason) {
        boolean isValid = validValues.stream().anyMatch(validValue -> validValue.equalsIgnoreCase(value));
        if (!isValid) {
            addError(fieldName, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, fieldName, reason));
        }
    }

    protected void addError(String fieldName, String errorCode, String errorMessage) {
        errorDtoList.add(ErrorDto.builder().errorCode(errorCode).errorMessage(MessageFormat.format(errorMessage, fieldName)).build());
    }

    protected void addError(String errorCode, String errorMessage, Object... fieldNames) {
        errorDtoList.add(ErrorDto.builder().errorCode(errorCode).errorMessage(MessageFormat.format(errorMessage, fieldNames)).build());
    }

    protected void throwIfErrors() {
        if (CollectionUtils.isNotEmpty(errorDtoList)) {
            throw new ValidationException(errorDtoList);
        }
    }

    protected boolean validate(String value, String regex) {
        return !Pattern.matches(regex, value);
    }

    protected void checkForLeadingTrailingAndSingleSpace(String value, String fieldName) {
        logger.debug("checking for Leading and Trailing space for field {} and value {}", fieldName, value);
        if (StringUtils.isNotEmpty(value) && !value.equals(value.trim())) {
            addError(fieldName, WHITESPACE_ERROR_CODE, TransactionErrorConstants.WHITESPACE_ERROR_MESSAGE);
        }
    }

    /**
     * Validate amount(BigDecimal) length before and after decimal as per configuration
     * @param amount    BigDecimal
     * @param fieldName String
     */
    protected void validateAmount(BigDecimal amount, String fieldName) {
        if (amount.signum() <= 0) {
            addError(fieldName, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, fieldName, AMOUNT_GREATER_THAN_ZERO));
        } else if (amount.scale() > AMOUNT_LENGTH_AFTER_DECIMAL) {
            logger.info("field {} amount {} exceeds {} decimal point.", fieldName, amount, AMOUNT_LENGTH_AFTER_DECIMAL);
            addError(EXCEED_LENGTH_ERROR_CODE, MessageFormat.format(EXCEED_LENGTH_ERROR_MESSAGE, fieldName + AMOUNT_DECIMAL_MESSAGE, AMOUNT_LENGTH_AFTER_DECIMAL));
        } else if (StringUtils.isNotEmpty(amount.toString()) && (amount.precision() - amount.scale()) > AMOUNT_LENGTH) {
            logger.info("field {} exceeds maxLength {}", fieldName, AMOUNT_LENGTH);
            addError(EXCEED_LENGTH_ERROR_CODE, EXCEED_LENGTH_ERROR_MESSAGE, fieldName + AMOUNT_BEFORE_DECIMAL_MESSAGE, AMOUNT_LENGTH);
        } else {
            validateFieldWithRegex(amount.toString(), TransactionConstant.AMOUNT_LENGTH_REGEX, fieldName, INCORRECT_FORMAT);
        }
        throwIfErrors();
    }

    protected void checkAllCaps(String value, String field) {
        if (!value.matches(CAPS_REGEX)) {
            addError(field, TransactionErrorConstants.INVALID_ERROR_CODE, MessageFormat.format(TransactionErrorConstants.INVALID_ERROR_MESSAGE, field, SMALL_LETTERS_NOT_ALLOWED));
        }
    }

    /**
     * Check mandatory field
     *
     * @param file     MultipartFile
     * @param fieldName String
     */
    protected void checkFilePresentOrNot(MultipartFile file, String fieldName) {
        if (ObjectUtils.isEmpty(file) || file.isEmpty()) {
            addError(fieldName, TransactionErrorConstants.MANDATORY_ERROR_CODE, TransactionErrorConstants.MANDATORY_ERROR_MESSAGE);
        }
    }
    /**
     * Check mandatory field
     *
     * @param file     MultipartFile
     * @param fileType String []
     */
    protected void checkFileFormat(MultipartFile file, String[] fileType) {
        if (!StringUtils.containsAnyIgnoreCase(file.getContentType(), fileType)) {
            addError(TransactionErrorConstants.MANDATORY_ERROR_CODE, TransactionErrorConstants.UNSUPPORTED_FILE_TYPE);
        }
    }

    /**
     * Check mandatory field
     *
     * @param fileSize  MultipartFile
     * @param confgSize Long
     */
    protected void checkFileMaxSize(MultipartFile fileSize, Long confgSize) {
        if (confgSize < fileSize.getSize()) {
            logger.debug("field {} exceeds maxLength {}", fileSize.getOriginalFilename(), confgSize);
            addError(EXCEED_LENGTH_ERROR_CODE, EXCEED_LENGTH_ERROR_MESSAGE, BULK_REFUND_FILE, confgSize);
        }
    }

    protected void validateFieldValue(String value, List<String> validValues, String fieldName) {
        boolean isValid = validValues.stream().anyMatch(validValue -> validValue.equals(value));
        if (!isValid) {
            addError(fieldName,INVALID_ENUM_ERROR_CODE,INVALID_ENUM_ERROR_MESSAGE);
        }
    }

    protected void validateReportDates(Long fromDate, Long toDate) {

        logger.info("Validating from and to date starts");

        //Step-1: If both from and to date not present, no need to validate
        if (ObjectUtils.isEmpty(fromDate) && ObjectUtils.isEmpty(toDate)) {
            return;
        }

        //Step-2: If any one date present then both fields should be mandatory
        checkMandatoryDateField(fromDate, FROM_DATE);
        checkMandatoryDateField(toDate, TO_DATE);
        throwIfErrors();

        //Step-3: Future from date
        if (fromDate > DateTimeUtils.endOfDayMillis()) {
            addError(FROM_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, FROM_DATE, FUTURE_DATE_ERROR));
            throwIfErrors();
        }

        //Step-4: From date within 1 year
        if (DateTimeUtils.calculateDaysBetween(DateTimeUtils.endOfDayMillis(), fromDate) > ALLOWED_FROM_DATE_DIFF) {
            addError(FROM_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, FROM_DATE, PAST_DATE_12M_ERROR));
            throwIfErrors();
        }

        //Step-5: Future to date
        if (toDate > DateTimeUtils.endOfDayMillis()) {
            addError(TO_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, TO_DATE, FUTURE_DATE_ERROR));
            throwIfErrors();
        }

        //Step-6: From date can not be more than to date
        if (fromDate > toDate) {
            addError(FROM_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, FROM_DATE, FROM_TO_DATE_ERROR));
            throwIfErrors();
        }

        //Step-6: From-To date difference more than 1 month
        if (DateTimeUtils.calculateDaysBetween(fromDate, toDate) > ALLOWED_FROM_TO_DATE_DIFF) {
            addError(FROM_DATE, INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, FROM_DATE, DATE_DIFF_1M_ERROR));
            throwIfErrors();
        }

        logger.info("Validating from and to date ends");
    }

    /**
     * This method is used to validate field length.
     * @param value value.
     * @param minLength min length.
     * @param maxLength max length.
     * @param fieldName file name.
     */
    void validateFieldMinMaxLength(String value, int minLength, int maxLength, String fieldName) {
        if ((value.length() < minLength) || (value.length() > maxLength)) {
            addError(FIXED_LENGTH_ERROR_CODE, FIXED_VALUE_MIN_MAX_ERROR_MESSAGE, fieldName, minLength, maxLength);
        }
    }

}

-----
@Component
@RequiredArgsConstructor
public class RefundValidator extends BaseValidator {

    private final LoggerUtility logger = LoggerFactoryUtility.getLogger(this.getClass());
    private final TransactionConfig transactionConfig;
    private final AdminDao adminDao;
    private final RefundDao refundDao;
    private final ErrorLogDao errorLogDao;
    private final MerchantOrderPaymentDao merchantOrderPaymentDao;

    /**
     * Validate business rules for refund booking
     *
     * @param refundBookRequest       RefundBookRequest
     * @param merchantPaymentOrderDto refundBookRequest
     */
    public void validateBusinessRules(RefundBookRequest refundBookRequest, MerchantPaymentOrderDto merchantPaymentOrderDto) {
        logger.info("Inside validateBusinessRules for atrn: {}", refundBookRequest.getAtrnNumber());

        //Step-1: Check merchant is enabled for refund booking
        MerchantInfoResponse merchantInfoResponse = adminDao.getMerchantByMId(merchantPaymentOrderDto.getMId());
        if(!TransactionConstant.FLAG_Y.equalsIgnoreCase(merchantInfoResponse.getIsRefundApplicable())){
            logger.info("Invalid refund applicable flag : {}", merchantInfoResponse.getIsRefundApplicable());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, REFUND_BOOK_REQUEST, MERCHANT_REFUND_FLAG_NOT_ENABLED));
        }

        //Step-2: Check if refund booking window is expired
        if(merchantInfoResponse.getRefundWindowDays() > 0 && merchantInfoResponse.getRefundWindowDays() < DateTimeUtils.calculateDaysBetween(DateTimeUtils.getCurrentTimeInMills(), merchantPaymentOrderDto.getCreatedDate())){
            logger.info("Refund window expired for allowed days : {}", merchantInfoResponse.getRefundWindowDays());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, REFUND_BOOK_REQUEST, MERCHANT_REFUND_WINDOW_EXPIRED));
        }

        //Step-3: Check transaction status should be only SUCCESS/SETTLED
        if (!TransactionStatus.SUCCESS.equals(merchantPaymentOrderDto.getTransactionStatus()) ) {
            logger.info("Invalid transaction status: {}", merchantPaymentOrderDto.getTransactionStatus());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, MERCHANT_ORDER_PAYMENT_STATUS, MERCHANT_ORDR_PAYMENT_ST_NOT_VALID_AGAINST_ATRN));
        }

        //Step-4: check if it is first refund request, then update available refund amount
        if (ObjectUtils.isEmpty(merchantPaymentOrderDto.getRefundStatus())) {
            logger.info("First refund request for atrn: {}", refundBookRequest.getAtrnNumber());
            merchantPaymentOrderDto.setAvailableRefundAmount(merchantPaymentOrderDto.getOrderAmount());
        }

        //Step-5: Check amount should not be more than available refund amount
        if (ObjectUtils.isEmpty(merchantPaymentOrderDto.getAvailableRefundAmount()) || merchantPaymentOrderDto.getAvailableRefundAmount().compareTo(refundBookRequest.getRefundAmount()) < 0) {
            logger.info("Available refund amount is less than requested amount");
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, REFUND_AMOUNT, REFUND_AMNT_MORE_THAN_AVAILABLE_AMNT_ERR_MSG));
        }

        //Step-6: Validate and get refund type throws exception if invalid refund type is passed
        RefundType refundType = RefundType.getRefundType(refundBookRequest.getRefundType());

        //Step-7: If refund type is full then match the amount with full available refund amount
        if (RefundType.FULL.equals(refundType) && merchantPaymentOrderDto.getAvailableRefundAmount().compareTo(refundBookRequest.getRefundAmount()) != 0) {
            logger.info("Available refund amount does not match with requested amount for full refund request type");
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, REFUND_AMOUNT, FULL_REFUND_AMOUNT_MIS_MATCH));
        }

        //Step-8: If refund type is partial then match the amount with full available refund amount
        if (RefundType.PARTIAL.equals(refundType) && merchantPaymentOrderDto.getAvailableRefundAmount().compareTo(refundBookRequest.getRefundAmount()) == 0) {
            logger.info("Request amount should be less than available refund amount for partial refund request type");
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, REFUND_AMOUNT, "Refund amount should be less than available refund amount for partial refund request type."));
        }
        //Step-9: Validating refund amount with available amount.
        if(TransactionConstant.FLAG_Y.equalsIgnoreCase(merchantInfoResponse.getRefundAdjustment())){
            logger.info("Validating Refund Adjustment flag : {}", merchantInfoResponse.getRefundAdjustment());
            validateAvailableRefundAmount(refundBookRequest, merchantInfoResponse);
        }
        //Step-10: Refund is not allowed for CASH and NEFT/RTGS...
        if((TransactionConstant.CASH.equalsIgnoreCase(merchantPaymentOrderDto.getPayMode().getValue())) || (TransactionConstant.NEFT_RTGS.equalsIgnoreCase(merchantPaymentOrderDto.getPayMode().getValue()))){
            logger.info("Refund is not allowed for this PayMode : {}", merchantPaymentOrderDto.getPayMode().getValue());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, PAYMODE_CONST, CASH_REFUND_NOT_ALLOWED));
        }

        //Step-11: Partial cancellation not allowed for multi-account merchant
        if (TransactionConstant.FLAG_Y.equalsIgnoreCase(merchantInfoResponse.getMerchantMultiAccountFlag()) && RefundType.PARTIAL.equals(refundType) && ObjectUtils.isEmpty(merchantPaymentOrderDto.getSettlementStatus())) {
            logger.info("Partial cancellation is not allowed for multi-account merchant");
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, REFUND_TYPE, "Partial cancellation is not allowed."));
        }
    }

    private void validateAvailableRefundAmount(RefundBookRequest refundBookRequest, MerchantInfoResponse merchantInfoResponse) {
        logger.info("Entering validateAvailableRefundAmount for merchant {} with requested refund amount {}",merchantInfoResponse.getMId(), refundBookRequest.getRefundAmount());

        // Calculate the merchants available balance
        BigDecimal totalUnsettledOrders = merchantOrderPaymentDao.sumSuccessfulUnsettledOrders(merchantInfoResponse.getMId());
        BigDecimal totalPendingRefunds = refundDao.sumPendingRefunds(merchantInfoResponse.getMId());
        logger.debug("Total pending refunds for merchant {}: {}", merchantInfoResponse.getMId(), totalPendingRefunds);

        BigDecimal currentAvailableBalance = totalUnsettledOrders.subtract(totalPendingRefunds);
        logger.info("Calculated current available balance for merchant {}: {}", merchantInfoResponse.getMId(), currentAvailableBalance);

        // Check if the requested refund exceeds the available balance
        if (refundBookRequest.getRefundAmount().compareTo(currentAvailableBalance) > 0) {
            logger.error("Refund validation failed. Requested amount {} exceeds available balance {} for merchant {}",refundBookRequest.getRefundAmount(), currentAvailableBalance, merchantInfoResponse.getMId());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(EXCEEDS_MERCHANT_AVAILABLE_BALANCE_MESSAGE,refundBookRequest.getRefundAmount(),currentAvailableBalance));
        }
    }

    /**
     * Validate refund book request for basic validations
     *
     * @param refundBookRequest RefundBookRequest
     */
    public void validateRefundRequest(RefundBookRequest refundBookRequest) {
        errorDtoList = new ArrayList<>();
        logger.info("validation started for refundBookRequest : {}", refundBookRequest);
        validateMandatoryFields(refundBookRequest);
        logger.info("mandatory fields validation completed for refundBookRequest");
        validateLeadingTrailingSpaces(refundBookRequest);
        logger.info("leading and trailing space fields validation completed for refundBookRequest");
        validateFieldsLength(refundBookRequest);
        logger.info("fields length validation completed for refundBookRequest");
        validateFieldsValue(refundBookRequest);
        logger.info("fields value validation completed for refundBookRequest");
    }

    /**
     * Validate fields length for refund book request
     *
     * @param refundBookRequest RefundBookRequest
     */
    private void validateFieldsLength(RefundBookRequest refundBookRequest) {
        validateFixedFieldLength(refundBookRequest.getMId(), MID_LENGTH, MID);
        throwIfErrors();
    }

    /**
     * Validate mandatory fields for refund book request
     *
     * @param refundBookRequest RefundBookRequest
     */
    protected void validateMandatoryFields(RefundBookRequest refundBookRequest) {
        logger.info("Validating mandatory fields for refund search request: {}", refundBookRequest);
        checkMandatoryField(refundBookRequest.getRefundType(), REFUND_TYPE);
        checkMandatoryField(refundBookRequest.getRefundAmount(), REFUND_AMOUNT);
        checkMandatoryField(refundBookRequest.getAtrnNumber(), ATRN);
        checkMandatoryField(refundBookRequest.getMId(), MID);
        throwIfErrors();
    }

    /**
     * Validate fields value for refund book request
     *
     * @param refundBookRequest RefundBookRequest
     */
    protected void validateFieldsValue(RefundBookRequest refundBookRequest) {
        logger.info("Inside validateFieldsValue for atrn: {}", refundBookRequest.getAtrnNumber());
        validateAtrn(refundBookRequest.getAtrnNumber());

        validateRemark(refundBookRequest.getRemark());
        validateAmount(refundBookRequest.getRefundAmount(), REFUND_AMOUNT);
        validateFieldLength(refundBookRequest.getRefundType(), TransactionConstant.REFUND_TYPE_LENGTH, REFUND_TYPE);
        throwIfErrors();
        RefundType.getRefundType(refundBookRequest.getRefundType());
        throwIfErrors();
        validateFieldWithRegex(refundBookRequest.getRefundType(), CAPS_REGEX, REFUND_TYPE, INVALID_REFUND_TYPE_REASON);
        throwIfErrors();
        validateFieldWithRegex(refundBookRequest.getMId(), ALLOWED_DIGIT_REGEX, MID,  INCORRECT_FORMAT);
        throwIfErrors();

    }

    /**
     * Method name : validateLeadingAndTraling
     * Description : Validates leading and Traling spaces from refund book request
     * @param refundBookRequest : Object of RefundBookRequest
     */
    private void validateLeadingTrailingSpaces(RefundBookRequest refundBookRequest) {
        checkForLeadingTrailingAndSingleSpace(refundBookRequest.getAtrnNumber(),ATRN);
        checkForLeadingTrailingAndSingleSpace(refundBookRequest.getRefundType(),REFUND_TYPE );
        checkForLeadingTrailingAndSingleSpace(refundBookRequest.getMId(), MID);
        throwIfErrors();
    }

    /**
     * Validate comment for refund book request
     *
     * @param comment String
     */
    protected void validateRemark(String comment) {
        validateFieldLength(comment, COMMENT_LENGTH, REMARK);
        throwIfErrors();
        validateFieldWithRegex(comment, COMMENT_REGEX, REMARK, INVALID_REMARK_REASON);
        throwIfErrors();
    }

    /**
     * Validate atrn for refund book request
     *
     * @param atrn String
     */
    protected void validateAtrn(String atrn) {
        validateFixedFieldLength(atrn, TransactionConstant.ATRN_ARRN_LENGTH, ATRN);
        throwIfErrors();
        validateFieldWithRegex(atrn, ATRN_ARRN_REGEX, ATRN, INVALID_ATRN_REASON);
        throwIfErrors();
    }

    /**
     * Validate refund search request
     *
     * @param refundSearchRequest RefundSearchRequest
     */
    public void validateRefundSearchRequest(RefundSearchRequest refundSearchRequest) {
        logger.info("Inside validateRefundDetailRequest for mid: {}", refundSearchRequest.getMId());
        errorDtoList = new ArrayList<>();
        validateMandatoryFields(refundSearchRequest);
        checkMandatoryFieldCount(refundSearchRequest);
        validateFieldsValue(refundSearchRequest);
        validateReportDates(refundSearchRequest.getFrom(), refundSearchRequest.getTo());
    }

    /**
     * check mandatory field count for refund search request
     *
     * @param refundSearchRequest RefundSearchRequest
     */
    private void checkMandatoryFieldCount(RefundSearchRequest refundSearchRequest) {
        logger.info("checking mandatory field count starts");
        mandatoryFields = new ArrayList<>();
        this.mandatoryCount = 0;
        countAndAddMandatoryField(refundSearchRequest.getFrom(), FROM_DATE);
        countAndAddMandatoryField(refundSearchRequest.getTo(), TO_DATE);
        countAndAddMandatoryField(refundSearchRequest.getAtrnNumber(), ATRN);
        countAndAddMandatoryField(refundSearchRequest.getArrnNumber(), ARRN);
        countAndAddMandatoryField(refundSearchRequest.getSbiOrderRefNumber(), SBIORDER_REFERENCE_NUMBER_STATUS );
        checkMandatoryFieldCount();
        throwIfErrors();
        logger.info("checking mandatory field count ends");
    }

    /**
     * Validate mandatory fields for refund search request
     *
     * @param refundSearchRequest RefundSearchRequest
     */
    protected void validateMandatoryFields(RefundSearchRequest refundSearchRequest) {
        logger.info("Inside refundDetailRequest for refundSearchRequest: {}", refundSearchRequest);
        checkMandatoryField(refundSearchRequest.getMId(), MID);
        throwIfErrors();
    }

    /**
     * Validate  field values for refund search request
     *
     * @param refundSearchRequest RefundSearchRequest
     */
    protected void validateFieldsValue(RefundSearchRequest refundSearchRequest) {

        logger.info("Inside validateFieldsValue for mid: {}", refundSearchRequest.getMId());
        checkForLeadingTrailingAndSingleSpace(refundSearchRequest);
        validateFieldLengthAndFormat(refundSearchRequest);
        validateEnums(refundSearchRequest);
        validateActiveMid(refundSearchRequest.getMId());
    }

    private void validateFieldLengthAndFormat(RefundSearchRequest refundSearchRequest) {

        logger.info("Inside validateFieldLengthAndFormat for mid: {}", refundSearchRequest.getMId());

        validateFixedFieldLength(refundSearchRequest.getAtrnNumber(), TransactionConstant.ATRN_ARRN_LENGTH, ATRN);
        throwIfErrors();
        validateFieldWithRegex(refundSearchRequest.getAtrnNumber(), ATRN_ARRN_REGEX, ATRN, INVALID_ATRN_REASON);
        throwIfErrors();
        validateFixedFieldLength(refundSearchRequest.getArrnNumber(), TransactionConstant.ATRN_ARRN_LENGTH, ARRN);
        throwIfErrors();
        validateFieldWithRegex(refundSearchRequest.getArrnNumber(), ATRN_ARRN_REGEX, ARRN, INVALID_ARRN_REASON);
        throwIfErrors();
        validateFixedFieldLength(refundSearchRequest.getMId(), TransactionConstant.MID_MAX_LENGTH, MID);
        throwIfErrors();
        validateFieldWithRegex(refundSearchRequest.getMId(), NUMBER_ONLY, MID, INCORRECT_FORMAT);
        throwIfErrors();
        validateFieldLength(refundSearchRequest.getSbiOrderRefNumber(), ORDER_REF_LENGTH, SBI_ORDER_REF_NUMBER);
        throwIfErrors();
        validateFieldWithRegex(refundSearchRequest.getSbiOrderRefNumber(), ORDER_REFERENCE_NUMBER_REGEX, SBI_ORDER_REF_NUMBER, INVALID_ORDER_REF_REASON);
        throwIfErrors();
    }

    private void validateEnums(RefundSearchRequest refundSearchRequest) {

        logger.info("Inside validateEnums for mid: {}", refundSearchRequest.getMId());

        //Step-1: validate refund type
        if(StringUtils.isNotEmpty(refundSearchRequest.getRefundType())){
            RefundType.getRefundType(refundSearchRequest.getRefundType());
        }

        //Step-2:  validate refund status
        if(StringUtils.isNotEmpty(refundSearchRequest.getRefundStatus())){
            RefundStatus.getRefundStatus(refundSearchRequest.getRefundStatus());
        }
    }

    /**
     * Validate  field values for leading trailing space for refund search request
     *
     * @param refundSearchRequest RefundSearchRequest
     */
    private void checkForLeadingTrailingAndSingleSpace(RefundSearchRequest refundSearchRequest) {

        logger.info("Inside checkForLeadingAndSingleSpace for mid: {}", refundSearchRequest.getMId());

        checkForLeadingTrailingAndSingleSpace(refundSearchRequest.getAtrnNumber(),"atrnNumber");
        checkForLeadingTrailingAndSingleSpace(refundSearchRequest.getArrnNumber(),"arrnNumber");
        checkForLeadingTrailingAndSingleSpace( refundSearchRequest.getSbiOrderRefNumber(),"sbiOrderRefNumber");
        checkForLeadingTrailingAndSingleSpace(refundSearchRequest.getRefundStatus(),"refundStatus");
        checkForLeadingTrailingAndSingleSpace(refundSearchRequest.getRefundType(),"refundType");
        checkForLeadingTrailingAndSingleSpace(refundSearchRequest.getMId(),"mId");

        throwIfErrors();
    }

    /**
     * Validate bulk refund upload request
     *
     * @param mId            Merchant Id
     * @param file Multipart File
     */
    public void validateBulkRefundUploadRequest(String mId, MultipartFile file) {
        validateMid(mId);
        validateBulkRefundFile(file);
    }

    private void validateBulkRefundFile(MultipartFile file) {
        errorDtoList = new ArrayList<>();
        checkFilePresentOrNot(file, BULK_REFUND_FILE);
        throwIfErrors();
        validateFieldLength(file.getOriginalFilename(), BULK_REFUND_FILE_NAME_MAX_LENGTH, BULK_REFUND_FILE);
        throwIfErrors();
        validateFieldWithRegex(file.getOriginalFilename(), BULK_REFUND_FILE_NAME_REGEX, BULK_REFUND_FILE,  INCORRECT_FORMAT);
        throwIfErrors();
        checkFileFormat(file,BULK_REFUND_FILE_TYPES);
        throwIfErrors();
        checkFileMaxSize(file,transactionConfig.getBulkRefundFileMaxSize());
        throwIfErrors();
    }

    /**
     * Validate BulkRefund headers.
     *
     * @param csvFile List<String[]>
     * Return String of error
     */
    public String validateBulkRefundHeader(List<String[]> csvFile, String mId) {

        logger.info("Validate bulk refund header for mId: {}", mId);

        if (ObjectUtils.isEmpty(csvFile) || ObjectUtils.isEmpty(csvFile.getFirst())) {
            logger.debug("Valid file for mId: {}", mId);
            return "Invalid file, headers not available";
        }

        String[] requiredHeaders = BULK_REFUND_HEADERS.split(",");
        // Check for extra headers
        if (csvFile.getFirst().length > requiredHeaders.length) {
            logger.debug("Invalid file: extra headers provided. Expected {} headers, but found {}.", requiredHeaders.length, csvFile.getFirst().length);
            return "Invalid file, extra headers provided";
        }
        for (int i = 0; i < requiredHeaders.length; i++) {

            if (i >= csvFile.getFirst().length || !csvFile.getFirst()[i].equalsIgnoreCase(requiredHeaders[i])) {

                logger.debug("Invalid file for mId: {}, error: {} ", mId, requiredHeaders[i] + " not found in " + (i + 1) + " column.");
                return requiredHeaders[i] + " not found in " + (i + 1) + " column.";
            }

        }

        return null;

    }

    public void validateRefundRequest(BulkRefundBookingDetails bulkRefundBookingDetails) {
        errorDtoList = new ArrayList<>();
        logger.info("validation started for bulkRefundBookingDetails : {}", bulkRefundBookingDetails);
        validateMandatoryFields(bulkRefundBookingDetails);
        validateLeadingTrailingSpaces(bulkRefundBookingDetails);
        validateFieldsValue(bulkRefundBookingDetails);
    }
    protected void validateMandatoryFields(BulkRefundBookingDetails bulkRefundBookingDetails) {
        logger.info("Validating mandatory fields for refund search request: {}", bulkRefundBookingDetails);
        checkMandatoryField(bulkRefundBookingDetails.getRefundType(), REFUND_TYPE);
        checkMandatoryField(bulkRefundBookingDetails.getRefundAmount(), REFUND_AMOUNT);
        checkMandatoryField(bulkRefundBookingDetails.getAtrnNum(), ATRN);
        checkMandatoryField(bulkRefundBookingDetails.getMerchantOrderId(), MID);
        checkMandatoryField(bulkRefundBookingDetails.getRefundCurrency(), CURRENCY_CODE);
        throwIfErrors();
    }
    private void validateLeadingTrailingSpaces(BulkRefundBookingDetails bulkRefundBookingDetails) {
        checkForLeadingTrailingAndSingleSpace(bulkRefundBookingDetails.getAtrnNum(),ATRN);
        checkForLeadingTrailingAndSingleSpace(bulkRefundBookingDetails.getRemark(),REMARK );
        checkForLeadingTrailingAndSingleSpace(bulkRefundBookingDetails.getRefundType(),REFUND_TYPE );
        checkForLeadingTrailingAndSingleSpace(bulkRefundBookingDetails.getRefundCurrency(), CURRENCY_CODE);
        throwIfErrors();
    }
    protected void validateFieldsValue(BulkRefundBookingDetails bulkRefundBookingDetails) {
        logger.info("Inside validateFieldsValue for atrn: {}", bulkRefundBookingDetails.getAtrnNum());
        validateMerchantOderIdAndRefundCurrency(bulkRefundBookingDetails);
        validateAtrn(bulkRefundBookingDetails.getAtrnNum());
        validateRemark(bulkRefundBookingDetails.getRemark());
        validateAmount(new BigDecimal(bulkRefundBookingDetails.getRefundAmount()), REFUND_AMOUNT);
        RefundType.getRefundType(bulkRefundBookingDetails.getRefundType());
    }

    private void validateMerchantOderIdAndRefundCurrency(BulkRefundBookingDetails bulkRefundBookingDetails) {
        BulkRefundBooking bulkRefundBooking = refundDao.findByBulkId(bulkRefundBookingDetails.getBulkId());
        MerchantPaymentOrderDto merchantPaymentOrderDto = refundDao.findByAtrnNumber(bulkRefundBooking.getMerchantId() ,bulkRefundBookingDetails.getAtrnNum());
        if(!StringUtils.equalsIgnoreCase(bulkRefundBookingDetails.getMerchantOrderId(), merchantPaymentOrderDto.getOrderRefNumber())){
            logger.debug("Invalid MerchantOrderId {}", bulkRefundBookingDetails.getMerchantOrderId());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, MERCHANT_ORDER_ID, INCORRECT_FORMAT));
        }
        if(!StringUtils.equalsIgnoreCase(bulkRefundBookingDetails.getRefundCurrency(), merchantPaymentOrderDto.getCurrencyCode())){
            logger.debug("Invalid currency {}", bulkRefundBookingDetails.getRefundCurrency());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, REFUND_CURRENCY, INCORRECT_FORMAT));
        }
        if((StringUtils.equalsIgnoreCase(TransactionConstant.CASH,merchantPaymentOrderDto.getPayMode().getValue())) || (StringUtils.equalsIgnoreCase(TransactionConstant.NEFT_RTGS,merchantPaymentOrderDto.getPayMode().getValue()))){
            logger.debug("Refund is not allowed for this PayMode : {}", merchantPaymentOrderDto.getPayMode().toString());
            throw new TransactionException(INVALID_ERROR_CODE, MessageFormat.format(INVALID_ERROR_MESSAGE, PAYMODE_CONST, CASH_REFUND_NOT_ALLOWED));
        }
    }

    /**
     * validates the MId.
     *
     * @param mId The request containing mId.
     * @throws ValidationException if any validation fails.
     */
    public void validateMid(String mId) {
        logger.debug("Request Validation start for {}", mId);
        errorDtoList = new ArrayList<>();
        checkMandatoryField(mId, MID);
        throwIfErrors();
        checkForLeadingTrailingAndSingleSpace(mId, MID);
        throwIfErrors();
        validateFixedFieldLength(mId, MID_LENGTH, MID);
        throwIfErrors();
        validateFieldWithRegex(mId, ALLOWED_DIGIT_REGEX, MID, INVALID_FORMAT);
        throwIfErrors();
        validateActiveMid(mId);
        logger.debug("Request Validation end for {}", mId);
    }
    /**
     * validate active merchant id
     *
     * @param mId String
     *
     */
    private void validateActiveMid(String mId) {
        logger.info("Inside getActiveMerchantById for mId: {}", mId);
        MerchantInfoResponse response = adminDao.getMerchantByMId(mId);
        if (!StringUtils.equalsIgnoreCase(FLAG_Y, response.getIsActive())) {
            logger.info("The mId is InActive for mId: {}", mId);
            errorLogDao.logCustomerError(mId, EntityType.REFUND,null,null,null,null, NOT_FOUND_ERROR_CODE, MessageFormat.format(NOT_FOUND_ERROR_MESSAGE, VALID_MERCHANT));
            throw new TransactionException(NOT_FOUND_ERROR_CODE, MessageFormat.format(NOT_FOUND_ERROR_MESSAGE, VALID_MERCHANT));
        }
    }
    /**
     * Validates field values
     * @param bulkId String.
     * @param status String.
     */
    public void validateDownloadBulkRefund(String bulkId, String status) {
        errorDtoList = new ArrayList<>();
        logger.info("Mandatory fields validating started for bulkId: {} and status: {}", bulkId,status);
        checkMandatoryField(bulkId,BULK_ID);
        checkMandatoryField(status,REFUND_STATUS);
        logger.info("Mandatory fields validating end for bulkId: {} and status: {}", bulkId,status);
        throwIfErrors();
        logger.info("Leading and trailing fields validating started for bulkId: {} and status: {}", bulkId,status);
        checkForLeadingTrailingAndSingleSpace(bulkId,BULK_ID);
        checkForLeadingTrailingAndSingleSpace(status,REFUND_STATUS);
        logger.info("Leading and trailing fields validating end for bulkId: {} and status: {}", bulkId,status);
        throwIfErrors();
        logger.info("Validating field value started for bulkId: {} and status: {}", bulkId,status);
        validateFieldValue(bulkId,status);
        logger.info("Validating field value end for bulkId: {} and status: {}", bulkId,status);
        throwIfErrors();
        logger.info("Validating RefundStatus ENUM started for status: {}",status);
        validateFieldValue(status, Arrays.stream(BulkRefundRowStatus.values()).map(Enum::name).toList(), REFUND_STATUS);
        logger.info("Validating RefundStatus ENUM end for status: {}",status);
        throwIfErrors();
    }
    /**
     * Validates field values
     * @param bulkId String.
     * @param status String.
     */
    void validateFieldValue(String bulkId, String status) {
        validateFieldLength(bulkId, BULK_ID_MAX_LENGTH, BULK_ID);
        validateFieldWithRegex(bulkId, ALLOWED_DIGIT_REGEX,BULK_ID, INVALID_FORMAT);
        validateFieldLength(status, REFUND_STATUS_MAX_LENGTH, REFUND_STATUS);
        validateFieldWithRegex(status, REFUND_STATUS_REGEX,REFUND_STATUS, INVALID_FORMAT);
    }
}                            // Here We have BaseValidator class Extends refund validator i want same this format but just showning 6 months records and fromDate and toDate is mandatory field when mid and chargeback status is optional but we filter just MID wise data
