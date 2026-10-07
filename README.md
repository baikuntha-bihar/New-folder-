epay.payment.card.callback_url = https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/visamaster/callback
    epay.payment.card.callbackg_url_rupay = https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/rupay/callback
    epay.payment.card.pvReqURL = https://secure-3dssapi-az.wibmo.com/3dsserverapi/v5/pVrq/8642/
    epay.payment.card.saleAuthURL = https://areionsbi.wibmo.com/saleservice/api/v1/sale
    epay.payment.card.doubleVerification = https://areionsbi.wibmo.com/merchantOrderPayment/api/v1/txn-status
    epay.payment.card.REFUNDURL = https://areionsbi.wibmo.com/voidrefundservice/api/v1/voidorrefund
    epay.payment.card.REFUNDENQUIRY = https://areionsbi.wibmo.com/merchantOrderPayment/api/v2/txn-status 
    epay.payment.card.checkbin_Url = https://areionsbi.wibmo.com/authentication/api/v1/checkbin
    epay.payment.card.initiate_url = https://areionsbi.wibmo.com/authentication/api/v1/initiate
    epay.payment.card.generateOtp_url = https://areionsbi.wibmo.com/authentication/api/v1/generateOtp 
    epay.payment.card.resendOtp_url = https://areionsbi.wibmo.com/authentication/api/v1/resendOtp
    epay.payment.card.verifyOtp_url = https://areionsbi.wibmo.com/authentication/api/v1/verifyOtp
    epay.payment.card.authorize_url = https://areionsbi.wibmo.com/saleservice/api/v1/authorize
    epay.payment.card.reverse_url = https://areionsbi.wibmo.com/authentication/web/v1/parseRupayResponse
    epay.payment.card.token_url = https://tokenvault-sbi.wibmo.com/tokenVault/v3/tokenize
    epay.payment.card.api_key = 41b52a74-92f0-4c05-9487-8d0e01238170
    epay.payment.card.clientId = 7823a827-6cd3-4fc9-8f8f-c76db9ef407c
    epay.payment.card.clientApiUser = 100001-SBIe-2sL5qU9wR3
    epay.payment.card.clientApiKey = SBIe8cG8fI0hB7
    epay.payment.card.tokenSecretKey = c19215e2-c2a4-4630-937b-a3bfd764c96b
    epay.payment.card.tokenSecretKey_1 = c19215e2-c2a4-4630-937b-a3bfd764c96b
    epay.payment.card.merchantId_Token = SBIEPAYMUINALTID001
    epay.payment.card.SALT = 8d3e5e6436d3b0ef2bd61424c82ab7e3
    epay.payment.card.tokenRequesterId_MC = 035f91e4-8b3a-4127-a037-574fb82387c0
    epay.payment.card.tokenRequesterId_VS = DFFFAC4C-B2D8-4CEA-BEBB-E7EB1716A348
    epay.payment.card.tokenRequesterId_R = 77700011145
    epay.payment.card.pgInstanceId_1 = 8642
    epay.payment.card.pgInstanceId = 72702415
    epay.payment.card.pgInstanceId_2 = 72702415
    epay.payment.card.acquirerBIN_MASTER = 597291
    epay.payment.card.acquirerBIN_VISA = 401934
    epay.payment.card.acquiringBankId = 35761063
    epay.payment.card.acquirerID = 8642
    epay.payment.card.acquireInstanceId = 30b8b010-5987-4dc2-96cd-7a1d20a520d6
    epay.payment.card.acquirerMerchantId = 243754249380023
    epay.payment.card.MCC = 4900
    epay.payment.card.merchantId = 35967615
    epay.payment.card.transactionTypeCode = 9003
    epay.payment.card.deviceCategory = 0
    epay.payment.card.deviceChannel = 02
    epay.payment.card.p_messageVersion = 2.1.0
    epay.payment.card.messageCategory = 01
    epay.payment.card.vaultId = 100001
    epay.payment.card.INR = 356
    epay.payment.card.USD = 840
    epay.payment.card.trantype = 1
    epay.payment.card.verficationtype = 8
    epay.payment.card.refundtype = 2
    epay.payment.card.proxySet = true
    epay.payment.card.tokenSecretKey_Onboard = 6993cf96-7c56-47ab-9ceb-b7a8a5bfddb8
    epay.payment.card.Acquirer_ICA = 32996
    epay.payment.card.dpaIdApi_Url = https://tokenvault-sbi.wibmo.com/tokenVault/nts/v1/dpa/onboard
    epay.payment.card.browserAgent=TestMerchant/26.5.0.100 (Android/13/SM-S908E)
    epay.payment.card.seamlessURL = https://sbiepay.sbi.bank.in/RupayCardOTPGeneration.jsp
    #SBIEPAY Client jks properties
    #CLIENT_JKS_FILE_NAME=resources/keys/sbiepay_newpg.jks
    wibmo.client_jks_filename=keys/sbiepay_newpg.jks
    #CLIENT_JKS_FILE_NAME=/jboss/app-config/keys/newpg/sbiepay_newpg.jks
    wibmo.client_jks_file_pwd=Sbi@1234
    wibmo.client_alias_name=1
    wibmo.client_alias_pwd=Sbi@1234
    #WIBMO PG Public cert AliasName
    wibmo.server_pk_alias_name=client_keystore
    epay.payment.card.paymentsRedirectUrl=https://sbiepay.sbi.bank.in/ui/channel/card/sbi/
    epay.payment.card.cardOnboardURL=/card
    external.api.ui.service.redirectView=https://sbiepay.sbi.bank.in/ui/channel/sbi
    external.api.paymentCallBackUrl=https://sbiepay.sbi.bank.in/demo/
    wibmo.proxy.required=Y
    security.cors.allowed.origins=https://sbiepay.sbi.bank.in
    security.cors.allowed.methods=GET,POST
    security.cors.allowed.headers="Authorization, Origin, X-Correlation-Id, Content-Type, Accept, Content-Disposition"
    security.cors.max.age=3600
    
    #SBIEPAY Client jks properties
    wibmo.client_jks_filename_intl=keys/sbiepay_newpg_intl.jks
    wibmo.client_jks_file_pwd_intl=keystore
    wibmo.client_alias_name_intl=client_keypair
    wibmo.client_alias_pwd_intl=password
    wibmo.server_pk_alias_name_intl=wibmo_pk
    ##### End Wibmo

   
    #################### sbiinb constants start #####################
    epay.payment.sbiinb.gtwmapid=4
    epay.payment.sbiinb.sbiinbKey=resources/keys/SBI_EPAY.key
    epay.payment.sbiinb.meCode=SBIEPAY
    epay.payment.sbiinb.dvURL=SBIEPAY
    epay.payment.sbiinb.bankURL=https://uatmerchant.onlinesbi.sbi

    epay.payment.sbiinb.bankbrowserurl=https://merchant.onlinesbi.sbi/merchant/merchantprelogin.htm
    epay.payment.sbiinb.bankdvurl=https://merchant.onlinesbi.sbi/thirdparties/doubleverification.htm
    epay.payment.sbiinb.keyvalue=encdata
    epay.payment.sbiinb.merchantcode=SBIEPAY2_SBTEMER
    epay.payment.sbiinb.paymentsRedirectUrl=/api/payments/v1/sbi/inb/responseRedirect?status=
    epay.payment.sbiinb.devredirecturl=https://sbiepay.sbi.bank.in/ui/channel/inb/sbi/

    #SBI INB Redirection URL
    epay.payment.sbiinb.cancelurl=https://sbiepay.sbi.bank.in/api/payments/v1/inb/sbi/callback
    epay.payment.sbiinb.sbiredirecturl=https://sbiepay.sbi.bank.in/api/payments/v1/inb/sbi/callback
    
    ####### Other INB #######
    #epay.payment.otherinb.success.base.path=https://epay.sbi.bank.in
    #epay.payment.otherinb.fail.base.path=https://epay.sbi.bank.in
    epay.payment.otherinb.success.base.path=https://sbiepay.sbi.bank.in/api/payments/v1/other-inb/sbi/callback
    epay.payment.otherinb.fail.base.path=https://sbiepay.sbi.bank.in/api/payments/v1/other-inb/sbi/callback

    epay.payment.other_inb_mid=1000524
    #epay.payment.other_inb_key=A3FZ8NvgJN4IjD2OYtYQQfEN83Ej13rOVNYm38Prtpo=
    #epay.payment.other_inb_key_browser=RmmaxYFU4HkPvshgl5ngkg==
    #epay.payment.other_inb_key=dHngr3HvVv5Iq3J/ZLJUkPl5o3hJp/fQ9G3YQbKDZ0k=

    epay.payment.other_inb_key_browser=cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    epay.payment.other_inb_key=cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    epay.payment.merchant_post_url=https://epay.sbi.bank.in/secure/MerchantHostedListener
    #merchant_post_url=https://epay.sbi.bank.in/secure/AggregatorHostedListener
    #sbiinb.otherInbStatusQuery=https://sbiepay.sbi.bank.in/payagg/orderStatusQuery/getOrderStatusQuery
    #epay.payment.sbiinb.otherinbstatusquery=https://www.epay.sbi.bank.in/payagg/otherINBstatusQuery/getTxnStatusQuery
    epay.payment.sbiinb.otherinbstatusquery=https://epay.sbi.bank.in/payagg/otherINBstatusQuery/getTxnStatusQuery

    epay.payment.otherinb.proxyip=10.188.32.80
    epay.payment.otherinb.proxyport=3066
    epay.payment.otherinb.tlsversion=TLSv1.2

    #################### sbiinb constants end #####################
    
    #START OF UPI DETAILS---#SBIEpay2.0

    UPI.UPI_CLIENT_ID = SBIEPAY2
    UPI.SECRETKEY=f65e8f0a484d45babd33c10b4535ef66
    UPI.OAUTH_USERNAME =oauth2-api-merweb-SBI0000007165940
    UPI.OAUTH_PASSWORD =44A243905F8B96F9280EB056CECC608A0A998019ED67D2C77B4F5554DCB48B13

      #<!--Merchant me code--> USED IN PG MERCHANTID
    PG.UPI_MERCHANT_ID=SBI0000007165940

    #<!--PGP Encryption> -Password use(Epay side)----Created for Encryption-Decryption in Utility jar
    UPI_PVT_KEY_PWD=Sbiepay@2027


    #UPI public key-------------
    UPI.UPI_SBI_PUBLIC_KEY_PATH= /certs/SBI0000000032588_UAT_1224_PublicKey.asc

    #Epay private -------------------
    UPI.EPAY_PVT_KEY_PATH= /certs/0x1D883787-sec.asc
    UPI.PACKET_ENCRYPTION_KEY= 25dea54b392d9b24803d95e2df4d11d8
    UPI.OAUTH_TOKEN_URL= https://upionline.sbi/oauth/token

    #UPI.VALIDATE_VPA_CHECK_URL= https://uatupionline.sbi/upi2/upi/web/v2.0/validatevpaweb
    UPI.VALIDATE_VPA_CHECK_URL= https://upionline.sbi/upi/web/v2.0/validateVPAWeb
    UPI.VPA_COLLECT_URL= https://upionline.sbi/upi/web/v2.0/meCollectInitiateWeb
    UPI.TXN_STATUS_ENQUIRY_URL=https://upionline.sbi/upi/web/v2.0/meTranStatusQueryWeb
    UPI.UPI_CALLBACK_URL=https://sbiepay.sbi.bank.in/api/payments/v1/upi/sbi/callback
    UPI.UPICONFIG.URL=http://admin-adminservice.prod-dc-admin.svc.cluster.local:9094/admin/v1/merchant/upi/getUpiConfigDeatils
    upi.upiGatewayConfigDetailsUrl= /merchant/gateway

    #For QR Sign -----------------
    UPI_category=01
    UPI_intentMode=05
    UPI_ivtoken=2F70CBBDFAE8DBA95F46CEB68A970673
    UPI_keyid=10
    UPI_mode=05
    UPI_purpose=00
    UPI_qrMedium=06
    UPI_ver=01
    UPI_cu=INR
    UPI_tier=TIER1
    #---TEMPORARY FIELD ---
    UPI_upiQrVpa=sbiepay2@sbi
    UPI_businessName =TestMerchant
    UPI_mccCode =6012
    UPI.TXN_STATUS_ENQUIRY_API_URL=https://upionline.sbi/upi/web/v2.0/meTranStatusQueryWeb
    UPI.VPA_CHECK_API_URL=https://upionline.sbi/upi/web/v2.0/validateVPAWeb
