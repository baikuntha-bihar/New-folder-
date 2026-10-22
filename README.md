================================================================
ICICI NB - DB CONFIG ROW - CORRECTED (NOTEPAD VERSION)
================================================================
Cross-checked against:
  - ICICI Net Banking Technical Integration doc
  - ICICIRetailPaymentService.java
  - ICICIResponseDto / ICICIDvResponseDto
  - Your submitted config row
================================================================


================================================================
SECTION 1: ISSUES FOUND IN YOUR SUBMITTED ROW
================================================================

[ISSUE 1] BANK_URL missing query string
   BEFORE : https://cibnext.icicibank.com/corp/BANKAWAY
   AFTER  : https://cibnext.icicibank.com/corp/BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI
   WHY    : Doc requires IWQRYTASKOBJNAME + BAY_BANKID on the URL itself.
            Without them, ICICI returns "Invalid request" on every call.

[ISSUE 2] DV_URL missing query string
   BEFORE : https://shopping.icicibank.com/corp/BANKAWAY
   AFTER  : https://shopping.icicibank.com/corp/BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI
   WHY    : Same reason as BANK_URL.

[ISSUE 3] WEB_RESPONSE_FORMAT - PAID hardcoded to "Y"
   BEFORE : PRN={0}&ITC={1}&AMT={2}&CRN=INR&PAID=Y&BID={3}
   AFTER  : PRN={0}&ITC={1}&AMT={2}&CRN=INR&PAID={4}&BID={3}&ATRN={5}
   WHY    : ICICI sends PAID=N on cancelled payments.
            Hardcoding Y means cancel responses fail to parse.

[ISSUE 4] WEB_RESPONSE_FORMAT - ATRN placeholder missing
   WHY    : Newer API version includes ATRN in web response.
            Without a placeholder, parser silently drops it.

[ISSUE 5] VERI_RESPONSE_FORMAT - field name typo
   BEFORE : CRU_CONSTRN="INR"
   AFTER  : CRN=INR
   WHY    : Wrong field name, will never match response.

[ISSUE 6] VERI_RESPONSE_FORMAT - hardcoded ITC
   BEFORE : ITC="ICICI BANK"
   AFTER  : (use dynamic placeholder - see new format)
   WHY    : ITC must match the original transaction's ITC.
            Hardcoded value breaks reconciliation.

[ISSUE 7] VERI_RESPONSE_FORMAT - format mismatch with code [CRITICAL]
   BEFORE : IWQRYTASKOBJNAME={0}&...&BID={6}  (query-string format)
   AFTER  : {"atrn":"{5}","prn":"{4}","bId":"{7}","paId":"{0}","amount":{6}}
   WHY    : Your code uses:
                new ObjectMapper().readValue(iciciDvResponse, ICICIDvResponseDto.class)
            This is STRICT JSON parsing.
            A query-string body will throw JsonProcessingException on EVERY DV call.

[ISSUE 8] BANK_URL - corporate vs retail endpoint
   YOUR ROW : cibnext.icicibank.com/corp/BANKAWAY  (corporate)
   DOC      : payments.icici.bank.in/sm/BANKAWAY    (retail)
   WHY      : Class is ICICIRetailPaymentService but URL is /corp/.
              Confirm with ICICI which endpoint your PID is provisioned on.
              Kept your UAT URL because it may be correct for your setup.

[ISSUE 9] FORM_POSTING_TEMPLATE - SBMTTYPE field missing
   WHY      : ICICI doc example includes <INPUT TYPE=HIDDEN NAME=SBMTTYPE='POST'>
              Some ICICI environments reject forms without it.

NO ISSUE (correctly empty):
   CHECKSUM_METHOD_NAME  = NULL  (ICICI has no checksum)
   CHECKSUM_TYPE         = NULL
   CHECKSUM_VALUE        = NULL
   WEB_ENC_KEY           = NULL  (uses COMMON_ENC_KEY)
   WEB_ENC_KEYIV         = NULL
   VERI_EN_CKEY          = NULL
   VERI_ENC_KEY_IV       = NULL
   TOKEN_URL             = NULL  (ICICI has no token flow)
   DV_CHECKSUM_VALUE     = NULL
   WEB_RESP_CHECKSUM     = NULL


================================================================
SECTION 2: CORRECTED ROW - ALL COLUMNS
================================================================

COLUMN                    | VALUE
--------------------------|------------------------------------------------------------------
ID                        | 5794ED7A8462CAE7E0637D86B10AD17A
GTW_MAP_ID                | (keep your existing value)
PAY_MODE                  | 131
UNIT_NAME                 | NB
UNIT_TYPE                 | PAYMENT
WEB_REQUEST_FORMAT        | PRN={0}&ITC={1}&AMT={2}&CRN=INR&RU={3}&CG=Y
WEB_RESPONSE_FORMAT       | PRN={0}&ITC={1}&AMT={2}&CRN=INR&PAID={4}&BID={3}&ATRN={5}
VERI_REQUEST_FORMAT       | IWQRYTASKOBJNAME={0}&BAY_BANKID={1}&MD={2}&PID={3}&PRN={4}&ITC={5}&AMT={6}&CRN=INR&BID={7}
VERI_RESPONSE_FORMAT      | {"atrn":"{5}","prn":"{4}","bId":"{7}","paId":"{0}","amount":{6}}
FROM_POSTING_FORMAT       | YES
FROM_POSTING_METHOD       | POST
ENC_METHOD_NAME           | iciciEncrypt
DEC_METHOD_NAME           | iciciDecrypt
CHANNEL_RETURN_URL        | https://uat.sbiepay.sbi/payagg/AggICICINetBankingGatewayServlet
CHECKSUM_METHOD_NAME      | (NULL)
CHECKSUM_TYPE             | (NULL)
CHECKSUM_VALUE            | (NULL)
COMMON_ENC_KEY            | uxowYdKK3so2w6Ad
COMMON_ENC_KEY_IV         | uxowYdKK3so2w6Ad
WEB_ENC_KEY               | (NULL)
WEB_ENC_KEYIV             | (NULL)
VERI_EN_CKEY              | (NULL)
VERI_ENC_KEY_IV           | (NULL)
CREATED_BY                | system
UPDATED_BY                | system
CREATED_DATE              | 1729600000000
UPDATED_DATE              | 1729600000000
STATUS                    | A
REMARKS                   | ICICI NB retail - no checksum, DV encrypted JSON
BANK_URL                  | https://cibnext.icicibank.com/corp/BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI
DV_URL                    | https://shopping.icicibank.com/corp/BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI
TOKEN_URL                 | (NULL)
FORM_POSTING_TEMPLATE     | (base64 - see Section 4 for updated value)
BANK_ROUTING_METHOD       | processInitiate
BANK_ROUTING_CLASS        | ICICIRetailEncryptionDecryptionUtil
PAYMENT_REDIRECT_URL      | https://your-merchant-domain.com/payment/status
DV_CHECKSUM_VALUE         | (NULL)
WEB_RESP_CHECKSUM         | (NULL)


================================================================
SECTION 3: COMPLETE SQL INSERT (Oracle syntax)
================================================================
Adjust table name ICICI_NB_CONFIG to your actual table name.
================================================================

INSERT INTO ICICI_NB_CONFIG (
    ID, GTW_MAP_ID, PAY_MODE,
    WEB_REQUEST_FORMAT, WEB_RESPONSE_FORMAT,
    VERI_REQUEST_FORMAT, VERI_RESPONSE_FORMAT,
    FROM_POSTING_FORMAT, UNIT_TYPE, UNIT_NAME,
    ENC_METHOD_NAME, DEC_METHOD_NAME,
    CHANNEL_RETURN_URL,
    CHECKSUM_METHOD_NAME, CHECKSUM_TYPE, CHECKSUM_VALUE,
    COMMON_ENC_KEY, COMMON_ENC_KEY_IV,
    WEB_ENC_KEY, WEB_ENC_KEYIV,
    VERI_EN_CKEY, VERI_ENC_KEY_IV,
    CREATED_BY, UPDATED_BY, CREATED_DATE, UPDATED_DATE,
    STATUS, REMARKS,
    BANK_URL, DV_URL, TOKEN_URL,
    FORM_POSTING_TEMPLATE, FROM_POSTING_METHOD,
    BANK_ROUTING_METHOD, BANK_ROUTING_CLASS,
    PAYMENT_REDIRECT_URL,
    DV_CHECKSUM_VALUE, WEB_RESP_CHECKSUM
) VALUES (
    '5794ED7A8462CAE7E0637D86B10AD17A',
    '<YOUR_GTW_MAP_ID>',
    131,
    'PRN={0}&ITC={1}&AMT={2}&CRN=INR&RU={3}&CG=Y',
    'PRN={0}&ITC={1}&AMT={2}&CRN=INR&PAID={4}&BID={3}&ATRN={5}',
    'IWQRYTASKOBJNAME={0}&BAY_BANKID={1}&MD={2}&PID={3}&PRN={4}&ITC={5}&AMT={6}&CRN=INR&BID={7}',
    '{"atrn":"{5}","prn":"{4}","bId":"{7}","paId":"{0}","amount":{6}}',
    'YES',
    'PAYMENT',
    'NB',
    'iciciEncrypt',
    'iciciDecrypt',
    'https://uat.sbiepay.sbi/payagg/AggICICINetBankingGatewayServlet',
    NULL, NULL, NULL,
    'uxowYdKK3so2w6Ad',
    'uxowYdKK3so2w6Ad',
    NULL, NULL,
    NULL, NULL,
    'system', 'system',
    1729600000000, 1729600000000,
    'A',
    'ICICI NB retail - no checksum, DV encrypted JSON',
    'https://cibnext.icicibank.com/corp/BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI',
    'https://shopping.icicibank.com/corp/BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI',
    NULL,
    'PGh0bWw+Cjxib2R5IG9ubG9hZD1kb2N1bWVudC5mb3Jtc1swXS5zdWJtaXQoKT4KPGZvcm0KbWV0aG9kPSIke01FVEhPRH0iCmFjdGlvbj0iJHtBQ1RJT05fVVJMfSI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IlNCTVRUWVBFIiB2YWx1ZT0iUE9TVCI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IkJBWV9CQU5LSUQiIHZhbHVlPSIke0JBWV9CQU5LSUR9Ij4KPGlucHV0IHR5cGU9ImhpZGRlbiIgbmFtZT0iTUQiIHZhbHVlPSIke01EfSI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IlBJRCIgdmFsdWU9IiR7UElEfSI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IlNQSUQiIHZhbHVlPSIke1NQSUR9Ij4KPGlucHV0IHR5cGU9ImhpZGRlbiIgbmFtZT0iRVMiIHZhbHVlPSIke0VOQ19EQVRBfSI+CjwvZm9ybT4KPC9ib2R5Pgo8L2h0bWw+',
    'POST',
    'processInitiate',
    'ICICIRetailEncryptionDecryptionUtil',
    'https://your-merchant-domain.com/payment/status',
    NULL, NULL
);
COMMIT;


================================================================
SECTION 4: FORM_POSTING_TEMPLATE - UPDATED (with SBMTTYPE)
================================================================

PLAIN HTML (decoded - for readability only, do NOT store this):

<html>
<body onload=document.forms[0].submit()>
<form method="${METHOD}" action="${ACTION_URL}">
<input type="hidden" name="SBMTTYPE" value="POST">
<input type="hidden" name="BAY_BANKID" value="${BAY_BANKID}">
<input type="hidden" name="MD" value="${MD}">
<input type="hidden" name="PID" value="${PID}">
<input type="hidden" name="SPID" value="${SPID}">
<input type="hidden" name="ES" value="${ENC_DATA}">
</form>
</body>
</html>

BASE64 (for DB column FORM_POSTING_TEMPLATE):

PGh0bWw+Cjxib2R5IG9ubG9hZD1kb2N1bWVudC5mb3Jtc1swXS5zdWJtaXQoKT4KPGZvcm0KbWV0aG9kPSIke01FVEhPRH0iCmFjdGlvbj0iJHtBQ1RJT05fVVJMfSI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IlNCTVRUWVBFIiB2YWx1ZT0iUE9TVCI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IkJBWV9CQU5LSUQiIHZhbHVlPSIke0JBWV9CQU5LSUR9Ij4KPGlucHV0IHR5cGU9ImhpZGRlbiIgbmFtZT0iTUQiIHZhbHVlPSIke01EfSI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IlBJRCIgdmFsdWU9IiR7UElEfSI+CjxpbnB1dCB0eXBlPSJoaWRkZW4iIG5hbWU9IlNQSUQiIHZhbHVlPSIke1NQSUR9Ij4KPGlucHV0IHR5cGU9ImhpZGRlbiIgbmFtZT0iRVMiIHZhbHVlPSIke0VOQ19EQVRBfSI+CjwvZm9ybT4KPC9ib2R5Pgo8L2h0bWw+


================================================================
SECTION 5: BEFORE vs AFTER - SIDE-BY-SIDE SUMMARY
================================================================

COLUMN                   | BEFORE                                  | AFTER
-------------------------|-----------------------------------------|--------------------------------------------------
BANK_URL                 | .../BANKAWAY                            | .../BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI
DV_URL                   | .../BANKAWAY                            | .../BANKAWAY?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI
WEB_RESPONSE_FORMAT      | ...&PAID=Y&BID={3}                      | ...&PAID={4}&BID={3}&ATRN={5}
VERI_RESPONSE_FORMAT     | ...&ITC="ICICI BANK"&CRU_CONSTRN="INR"&BID={6} | {"atrn":"{5}","prn":"{4}","bId":"{7}","paId":"{0}","amount":{6}}
FORM_POSTING_TEMPLATE    | 6 hidden fields                         | 7 hidden fields (+SBMTTYPE)
CHECKSUM_*               | NULL                                    | NULL (correct - ICICI has none)
WEB_REQUEST_FORMAT       | PRN={0}&ITC={1}&AMT={2}&CRN=INR&RU={3}&CG=Y | unchanged (verify RU wrapping - see preflight)
VERI_REQUEST_FORMAT      | IWQRY...&BID={7}                        | unchanged (valid)


================================================================
SECTION 6: PREFLIGHT CHECKLIST - CONFIRM BEFORE GO-LIVE
================================================================

[ ] 1. BANK_URL and DV_URL endpoint
        - Confirm with ICICI: your PID provisioned on
          cibnext.icicibank.com/corp/BANKAWAY (corporate)
          or payments.icici.bank.in/sm/BANKAWAY (retail)?
        - Both URLs MUST include ?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI

[ ] 2. VERI_RESPONSE_FORMAT JSON vs query-string
        - Make one LIVE UAT DV call
        - Decrypt the response body and inspect structure:
            * JSON  -> keep the new {"atrn":...} format
            * Query -> change to:
                      PRN={4}&ITC={5}&AMT={6}&CRN=INR&PAID={0}&BID={7}&ATRN={5}
        - This drives whether ObjectMapper.readValue() works

[ ] 3. RU quoting in WEB_REQUEST_FORMAT
        - Current format : RU={3}
        - If {3} is ALREADY wrapped with %22 by your code, keep as-is
        - If not, change to : RU=%22{3}%22
        - Check where {3} is populated in the initiation code

[ ] 4. OtherInbConstants - split success constants
        - ICICI_WEB_STATUS_SUCCESS_CONST = "Y"   (for web callback PAID)
        - ICICI_DV_STATUS_SUCCESS_CONST  = "0"   (for DV response paId)
        - If your code has only ONE constant for both, SPLIT it now
        - This is the #1 bug source (web uses Y, DV uses 0)

[ ] 5. ATRN placeholder in WEB_RESPONSE_FORMAT
        - If ICICI does NOT return ATRN for your PID, remove &ATRN={5}
        - If it does, keep it (field exists in ICICIResponseDto)

[ ] 6. FORM_POSTING_TEMPLATE with SBMTTYPE
        - Test in UAT that SBMTTYPE does not cause validation error
        - If it does, revert to original template without SBMTTYPE

[ ] 7. Encryption keys
        - COMMON_ENC_KEY  = uxowYdKK3so2w6Ad
        - COMMON_ENC_KEY_IV = uxowYdKK3so2w6Ad
        - 16 chars each - AES-128 compatible
        - Confirm PROD key with ICICI (UAT key above should NOT be in PROD)

[ ] 8. CHANNEL_RETURN_URL whitelist
        - https://uat.sbiepay.sbi/payagg/AggICICINetBankingGatewayServlet
        - Confirm this URL is whitelisted with ICICI
        - PROD value must be your PROD merchant domain

[ ] 9. PAYMENT_REDIRECT_URL
        - Replace https://your-merchant-domain.com/payment/status
          with your actual final landing page URL

[ ] 10. Test end-to-end in UAT
         - Initiate a payment
         - Confirm BANK_URL with query string loads ICICI login page
         - Complete payment
         - Verify WEB_RESPONSE parse (PAID=Y case)
         - Cancel a payment
         - Verify WEB_RESPONSE parse (PAID=N case)
         - Verify DV call succeeds (JSON parse succeeds)
         - Verify DV_SUCCESS (paId=0) maps to SUCCESS status


================================================================
SECTION 7: 3 MUST-FIX ITEMS (BLOCKING)
================================================================

Item 1 (BLOCKING): BANK_URL + DV_URL
   These currently have no query string.
   Every payment initiation will fail with "Invalid request".
   Add: ?IWQRYTASKOBJNAME=bay_mc_login&BAY_BANKID=ICI

Item 2 (BLOCKING): VERI_RESPONSE_FORMAT
   Currently query-string. Code uses ObjectMapper (JSON).
   Every DV call will throw JsonProcessingException.
   Replace with: {"atrn":"{5}","prn":"{4}","bId":"{7}","paId":"{0}","amount":{6}}

Item 3 (BLOCKING): WEB_RESPONSE_FORMAT
   PAID hardcoded to "Y". Cancellations (PAID=N) will fail parsing.
   Replace with: ...&PAID={4}&BID={3}&ATRN={5}


================================================================
END OF CONFIG REFERENCE
================================================================
