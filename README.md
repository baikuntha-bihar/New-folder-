spring.application.name=epay_payment_service
    server.port=9093   
    server.servlet.context-path=/api/payments/v1/

    # Db connectivity
    spring.jpa.show-sql=true
    spring.jpa.properties.hibernate.show_sql=true
    spring.jpa.properties.hibernate.format_sql=true
    spring.web.resources.static-locations=classpath:/,file:/non-existent-folder
    spring.datasource.url=jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=epaydbscanuat.sbiepay.sbi)(PORT=1524))(CONNECT_DATA= (SERVER=DEDICATED) (SERVICE_NAME=newuat)))
    spring.datasource.username=${DB_USERNAME}
    spring.datasource.password=${DB_PASSWORD}
    spring.jpa.show-sql=true
    spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
    
    #Application Proxy Details
    https_protocols = TLSv1.2
    https_proxySet = true
    https_proxyHost = serverswg.sbi.co.in
    https_proxyPort = 9090

    # Optional settings
    spring.datasource.hikari.maximum-pool-size=10
    spring.datasource.hikari.minimum-idle=5
    spring.datasource.hikari.idle-timeout=30000
    spring.datasource.hikari.max-lifetime=2000000

    #In Minutes
    transaction.token.expiry.time=30

    # Liquibase Properties
    spring.liquibase.change-log=classpath:db/changelog/db.changelog-master.xml
    spring.liquibase.enabled=false
    spring.liquibase.drop-first=false
    logging.level.liquibase=DEBUG

    spring.jpa.hibernate.ddl-auto=none
    #WIBMO PG Constants
    epay.payment.card.callback_url = https://uat.epay.sbi/api/payments/v1/cards/sbi/visamaster/callback
    epay.payment.card.callbackg_url_rupay = https://uat.epay.sbi/api/payments/v1/cards/sbi/rupay/callback
    epay.payment.card.callback_url_intl = https://uat.epay.sbi/api/payments/v1/cards/sbi/intl/visamaster/callback
    epay.payment.card.callbackg_url_rupay_intl = https://uat.epay.sbi/api/payments/v1/cards/sbi/intl/rupay/callback
    
    epay.payment.card.pvReqURL = https://secure-3dssapi-az.wibmo.com/3dsserverapi/v5/pVrq/8642/
    epay.payment.card.saleAuthURL = https://areionsbi.wibmo.com/saleservice/api/v1/sale
    epay.payment.card.checkbin_Url = https://areionsbi.wibmo.com/authentication/api/v1/checkbin
    epay.payment.card.initiate_url = https://areionsbi.wibmo.com/authentication/api/v1/initiate
    epay.payment.card.generateOtp_url = https://areionsbi.wibmo.com/authentication/api/v1/generateOtp
    epay.payment.card.resendOtp_url = https://areionsbi.wibmo.com/authentication/api/v1/resendOtp
    epay.payment.card.verifyOtp_url = https://areionsbi.wibmo.com/authentication/api/v1/verifyOtp
    epay.payment.card.authorize_url = https://areionsbi.wibmo.com/saleservice/api/v1/authorize
    epay.payment.card.reverse_url = https://areionsbi.wibmo.com/authentication/web/v1/parseRupayResponse
    epay.payment.card.preAuth_url = https://areionsbi.wibmo.com/saleservice/api/v1/preauth
    epay.payment.card.token_url = https://tokenvault-sbi.wibmo.com/tokenVault/v3/tokenize
    
    epay.payment.card.api_key = 7e377272-d4b8-11ee-a8c7-005056b5bdb3
    epay.payment.card.api_key = 41b52a74-92f0-4c05-9487-8d0e01238170
    epay.payment.card.clientId = 7823a827-6cd3-4fc9-8f8f-c76db9ef407c
    epay.payment.card.clientApiUser = 100001-SBIe-2sL5qU9wR3
    epay.payment.card.clientApiKey = SBIe8cG8fI0hB7
    epay.payment.card.tokenSecretKey = c19215e2-c2a4-4630-937b-a3bfd764c96b
    epay.payment.card.tokenSecretKey_1 = c19215e2-c2a4-4630-937b-a3bfd764c96b
    epay.payment.card.merchantId_Token = SBIEPAYMUINALTID001
    epay.payment.card.tokenRequesterId_MC = 035f91e4-8b3a-4127-a037-574fb82387c0
    epay.payment.card.tokenRequesterId_VS = DFFFAC4C-B2D8-4CEA-BEBB-E7EB1716A348
    epay.payment.card.tokenRequesterId_R = 77700011145
    epay.payment.card.acquiringBankId = 35761063
    epay.payment.card.acquireInstanceId = 30b8b010-5987-4dc2-96cd-7a1d20a520d6
    epay.payment.card.acquirerMerchantId = 243754249380023
        
    #SBIEPAY Client jks properties
    wibmo.client_jks_filename=sbiepay_newpg.jks
    wibmo.client_jks_file_pwd=Sbi@1234
    wibmo.client_alias_name=1
    wibmo.client_alias_pwd=Sbi@1234
    wibmo.server_pk_alias_name=client_keystore

    epay.payment.card.pgInstanceId_1 = 8642
    epay.payment.card.pgInstanceId = 72702415
    epay.payment.card.pgInstanceId_2 = 72702415
    epay.payment.card.acquirerBIN_MASTER = 597291
    epay.payment.card.acquirerBIN_VISA = 401934
    epay.payment.card.acquirerBIN_RUPAY = 300090
    epay.payment.card.acquirerID = 8642
    epay.payment.card.MCC = 4900
    epay.payment.card.transactionTypeCode = 9003
    epay.payment.card.deviceCategory = 0
    epay.payment.card.deviceChannel = 02
    epay.payment.card.p_messageVersion = 2.1.0
    epay.payment.card.p_messageVersion_two = 2.2.0
    epay.payment.card.messageCategory = 01
    epay.payment.card.vaultId = 100001
    epay.payment.card.INR = 356
    epay.payment.card.USD = 840
    epay.payment.card.proxySet = true
    epay.payment.card.browserAgent=TestMerchant/26.5.0.100 (Android/13/SM-S908E)
  

    ##SBIEPAY Client jks properties - Dom
    wibmo.client_jks_filename=sbiepay_newpg.jks
    wibmo.client_jks_file_pwd=Sbi@1234
    wibmo.client_alias_name=1
    wibmo.client_alias_pwd=Sbi@1234
    wibmo.server_pk_alias_name=client_keystore


    epay.payment.card.paymentsRedirectUrl=https://uat.epay.sbi/ui/channel/card/sbi/
    epay.payment.card.cardOnboardURL=/card
    wibmo.proxy.required=Y

    epay.payment.card.api_key_new=de95d081-ce9c-4af7-aff6-121ce2fe58e4
    epay.payment.card.pgInstanceId_3=9999
    epay.payment.card.mle_ver=1
    #epay.payment.card.checkbin_New_Url=https://areionpg.pc.enstage-sas.com/binservice/v1/bins
    epay.payment.card.checkbin_New_Url=https://areionvas.wibmo.com/binservice/v1/bins
    wibmo.client_jks_filename_rupay=sbiepay_newpg_rupay.jks
    wibmo.client_jks_file_pwd_rupay=Wibmo@123
    wibmo.client_alias_name_rupay=Wibmo3dss
    wibmo.client_alias_pwd_rupay=Wibmo@123
    wibmo.server_pk_alias_name_rupay=de95d081-ce9c-4af7-aff6-121ce2fe58e4

    ##### End Wibmo

   
    #################### sbiinb constants start #####################
    
    epay.payment.sbiinb.gtwmapid=86
    epay.payment.sbiinb.sbiinbKey=keys/SBI_EPAY.key
    epay.payment.sbiinb.meCode=SBIEPAY
    epay.payment.sbiinb.dvURL=SBIEPAY
    #epay.payment.sbiinb.bankURL=https://uatmerchant.onlinesbi.sbi
    epay.payment.sbiinb.bankURL=https://merchant.sbiuat.bank.in

    #epay.payment.sbiinb.bankbrowserurl=https://uatmerchant.onlinesbi.sbi/merchant/merchantprelogin.htm
    epay.payment.sbiinb.bankbrowserurl=https://merchant.sbiuat.bank.in/merchant/merchantprelogin.htm
    epay.payment.sbiinb.bankdvurl=https://merchantuat.onlinesbi.sbi/thirdparties/doubleverification.htm
    epay.payment.sbiinb.keyvalue=encdata
    epay.payment.sbiinb.merchantcode=SBIEPAY2
    epay.payment.sbiinb.paymentsRedirectUrl=/api/payments/v1/sbi/inb/responseRedirect?status=
    epay.payment.sbiinb.devredirecturl=https://uat.epay.sbi/ui/channel/inb/sbi/

    #SBI INB Redirection URL
    epay.payment.sbiinb.cancelurl=https://uat.epay.sbi/api/payments/v1/inb/sbi/callback
    epay.payment.sbiinb.sbiredirecturl=https://uat.epay.sbi/api/payments/v1/inb/sbi/callback
    
    ################## Other INB #######################
    epay.payment.other_inb_mid=1000524

    epay.payment.otherinb.success.base.path=https://uat.epay.sbi/api/payments/v1/other-inb/sbi/callback
    epay.payment.otherinb.fail.base.path=https://uat.epay.sbi/api/payments/v1/other-inb/sbi/callback
    epay.payment.other_inb_key_browser=cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    epay.payment.other_inb_key=cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    epay.payment.merchant_post_url=https://epay.sbiuat.bank.in/secure/MerchantHostedListener
    epay.payment.sbiinb.otherinbstatusquery=https://epay.sbiuat.bank.in/payagg/otherINBstatusQuery/getTxnStatusQuery

    epay.payment.otherinb.proxyip=10.176.172.160
    epay.payment.otherinb.proxyport=3066
    epay.payment.otherinb.tlsversion=TLSv1.2
    #################### sbiinb constants end #####################

    #Wallet Config
    epay.payment.wallet.cancelurl=https://uat.epay.sbi/api/payments/v1/wallet/sbi/mobikwikcallback
    epay.payment.wallet_sbiredirectdvurl=https://uat.epay.sbi/api/payments/v1/wallet/sbi/mobikwikcallback
    epay.payment.wallet.bankbrowserurl=https://test.mobikwik.com/encwallet
    epay.payment.wallet.bankdvurl=https://test.mobikwik.com/enccheckstatus
    epay.payment.wallet.mid=MBK1034202
    epay.payment.wallet.cell=7039262141
    epay.payment.wallet.email=punam.rajput.cedge@sbi.co.in
    epay.payment.wallet.merchantname=SBIEPAY2
    epay.payment.wallet.secretkey=ju6tygh7u7tdg554k098ujd5468o
    epay.payment.wallet.encryptionkey = 1234567890123456
    epay.payment.wallet.Statuscode=0
    epay.payment.wallet.statusmessage=The payment has been successfully collected
    epay.payment.wallet.refid=838731552

    
    #START OF UPI DETAILS---#SBIEpay2.0

    UPI.UPI_CLIENT_ID = SBIEPAY_MID_2.0
    UPI.SECRETKEY=f65e8f0a484d45babd33c10b4535ef66
    UPI.OAUTH_USERNAME =oauth2-api-merweb-SBI0000000032588
    UPI.OAUTH_PASSWORD = 05B6948CCFCA37A74B3DDDD15F9482040A998019ED67D2C77B4F5554DCB48B13

    #<!--Merchant me code--> USED IN PG MERCHANTID
    PG.UPI_MERCHANT_ID=SBI0000000034214

    #<!--PGP Encryption> -Password use(Epay side)----Created for Encryption-Decryption in Utility jar
    UPI_PVT_KEY_PWD = Uat@Sbiepay


    #UPI public key-------------
    #UPI.UPI_SBI_PUBLIC_KEY_PATH= /certs/SBI0000000032588_UAT_1224_PublicKey.asc
    UPI.UPI_SBI_PUBLIC_KEY_PATH= resources/keys/SBI0000000032588_UAT_1224_PublicKey.asc

    #Epay private -------------------
    #UPI.EPAY_PVT_KEY_PATH= /certs/0x1D883787-sec.asc
    UPI.EPAY_PVT_KEY_PATH== resources/keys/0x1D883787-sec.asc
    UPI.PACKET_ENCRYPTION_KEY= 25dea54b392d9b24803d95e2df4d11d8
    UPI.OAUTH_TOKEN_URL= https://uatupionline.sbi/upi2/oauth/token
    #UPI.VALIDATE_VPA_CHECK_URL= https://uatupionline.sbi/upi2/upi/web/v2.0/validatevpaweb
    UPI.VALIDATE_VPA_CHECK_URL= https://uatupionline.sbi/upi2/upi/web/v2.0/validateVPAWeb
    UPI.VPA_COLLECT_URL= https://uatupionline.sbi/upi2/upi/web/v2.0/meCollectInitiateWeb
    UPI.TXN_STATUS_ENQUIRY_URL=https://uatupionline.sbi/upi2/upi/web/v2.0/meTranStatusQueryWeb
    UPI.UPI_CALLBACK_URL=https://epay.sbiuat.bank.in/secure/upiProcessingServlet
    UPI.UPICONFIG.URL=http://localhost:9098/admin/v1/merchant/upi/getUpiConfigDeatils
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
    UPI_upiQrVpa=sbiepay2.1000003@sbi
    UPI_businessName =TestMerchant
    UPI_mccCode =9399
    UPI.TXN_STATUS_ENQUIRY_API_URL=
    UPI.VPA_CHECK_API_URL=

    #upiqr.private.key.path= /certs/0x1D883787-sec.asc
    upiqr.private.key.path= resources/keys/0x1D883787-sec.asc
    upiqr.private.key.password= Uat@Sbiepay
    #upiqr.sbi.public.key.path= /certs/SBI0000000032588_UAT_1224_PublicKey.asc
    upiqr.sbi.public.key.path= resources/keys/SBI0000000032588_UAT_1224_PublicKey.asc

    #URLs
    UPI.INTENT_API_URL=https://uatupionline.sbi/upi2/upi/mandate/signVerifyIntent
    UPI.UPI_CONFIG_DEATILS_URL=http://localhost:9097/admin/v1/merchant/upi/getUpiConfigDeatils
    UPI.HANDSHAKE_API_URL=https://uatupionline.sbi/upi2/upi/oauth2-web-handshake
    upi.server.private.key.path=0x1D883787-sec.asc
    upi.server.public.key.path=SBI0000000032588_UAT_1224_PublicKey.asc
    
    #END OF UPI DETAILS
  
    cors.origin=https://uat.epay.sbi
    security.cors.origin=https://uat.epay.sbi
    cors.allowedOrigins=https://uat.epay.sbi
    
    security.whitelist.url=/webjars/, /actuator/, /swagger-resources/, /v3/api-docs, /swagger-ui/, /swagger-ui.html, /token,/downtime/api,/s1/fetch-data,/inb/sbi/**,/other-inb/sbi/**,/cards/sbi/**,/upi/sbi/** 
    whitelisted.endpoints=/webjars/, /actuator/, /swagger-resources/, /v3/api-docs, /swagger-ui/, /swagger-ui.html, /token,/downtime/api,/s1/fetch-data,/inb/sbi/**,/other-inb/sbi/**,/cards/sbi/**,/upi/sbi/**
    jwt.secret.key=gdjfgskjfhsdjkhkflkdlksdlfkskfwperip3ke3le3lmldrnkfnhiewjfejfokepfkldkfoikfokork3dklwedlsvflvkfkvlkdfvodkvcdokro3
    security.whitelist.urls=/webjars/, /actuator/, /swagger-resources/, /v3/api-docs, /swagger-ui/, /swagger-ui.html, /token,/downtime/api,/s1/fetch-data,/payments/v1/sbi,/sbi/inb,/cards/sbi/**,/inb/sbi/**,/other-inb/sbi/**,/upi/sbi/**, /actuator/health/**

    security.jwt.secret.key=gdjfgskjfhsdjkhkflkdlksdlfkskfwperip3ke3le3lmldrnkfnhiewjfejfokepfkldkfoikfokork3dklwedlsvflvkfkvlkdfvodkvcdokro3
    security.jwt.secret.issuer=sbi.epay
  

    spring.profiles.active=uat
    #Kafka boot strap server -uat
    spring.kafka.bootstrapServers=uat-kafka-cluster-kafka-bootstrap.uat-kafka.svc.cluster.local:9094
    #Kafka consumer setting
    spring.kafka.consumer.groupId=payment-consumers
    spring.kafka.consumer.enableAutoCommit=true
    spring.kafka.consumer.autoCommitInterval=100
    spring.kafka.consumer.sessionTimeoutMS=300000
    spring.kafka.consumer.requestTimeoutMS=420000
    spring.kafka.consumer.fetchMaxWaitMS=200
    spring.kafka.consumer.maxPollRecords=5
    spring.kafka.consumer.autoOffsetReset=latest
    spring.kafka.consumer.keyDeserializer=org.apache.kafka.common.serialization.StringDeserializer
    spring.kafka.consumer.valueDeserializer=org.apache.kafka.common.serialization.StringDeserializer
    spring.kafka.consumer.retryMaxAttempts=3
    spring.kafka.consumer.retryBackOffInitialIntervalMS=10000
    spring.kafka.consumer.retryBackOffMaxIntervalMS=30000
    spring.kafka.consumer.numberOfConsumers=1
    #Kafka producer setting
    spring.kafka.producer.acks=all
    spring.kafka.producer.retries=3
    spring.kafka.producer.batchSize=1000
    spring.kafka.producer.lingerMs=1
    spring.kafka.producer.bufferMemory=33554432
    spring.kafka.producer.keyDeserializer=org.apache.kafka.common.serialization.StringSerializer
    spring.kafka.producer.valueDeserializer=org.apache.kafka.common.serialization.StringSerializer
    #Topics
    spring.kafka.topic.payment.notification.sms=payment_sms_notification_topic
    spring.kafka.topic.payment.notification.email=payment_email_notification_topic
    spring.kafka.topic.payment.notification.cashstatusquery=payment_cashstatusquery_notification_topic
    spring.kafka.topic.partitions=4
    spring.kafka.topic.replicationFactor=1
    #SMS and Email config
    external.api.sms.gateway.base.path=https://smsapipprod.sbi.co.in:9443
    external.api.sms.gateway.url=/bmg/sms/epaypgotpdom
    external.api.sms.gateway.user=epaypgotpdom
    external.api.sms.gateway.password=Ep@y1Dpt
    external.api.sms.body.content.type=text
    external.api.sms.body.sender.id=SBIBNK
    external.api.sms.body.int.flag=0
    external.api.sms.body.charging=0
    spring.mail.host=10.177.210.181
    spring.mail.port=587
    spring.mail.username=sbitestclient
    spring.mail.password=sbitestclient_7f827c4b3aa6cd1f08d6b9cce2c0c80e
    email.recipient=ebms_uat_receiver@ebmsgits.sbi.co.in
    email.from=ebms_uat_sender@ebmsgits.sbi.co.in

    external.api.kms.services.base.path=https://uat.epay.sbi/api/kms/v1
    spring.kafka.topic.payment.push.verification=payment_push_verification_topic
    logging.level.org.apache.kafka= ERROR


    #External Base Path

    external.api.admin.services.base.path=http://admin-adminservice.uat-admin.svc.cluster.local:9094/api/admin/v1
    external.api.transaction.services.base.path=http://txn-transactionservice.uat-transaction.svc.cluster.local:9092/api/transaction/v1
    epay.payment.card.finalResponse_Url=/callback
    kms.service.base-url=
    kms.service.cors-origin=
    key.aek=BiIZ5feKr16Td3XSpVywqXlwRNfSy9Gtis04WqEbD/0=
    kms.service.base-url=http://kms-kmsservice.uat-kms.svc.cluster.local:9093/api/kms/v1
    kms.service.cors-origin=https://uat.epay.sbi
    epay.payment.card.finalResponse_Url=/callback
    external.api.ui.service.redirectView=https://uat.epay.sbi/ui/channel/sbi
    external.api.paymentCallBackUrl=https://uat.epay.sbi/demo/
    security.cors.allowed.origins=https://uat.epay.sbi
    security.cors.allowed.methods=GET,POST
    security.cors.allowed.headers="Authorization, Origin, X-Correlation-Id, Content-Type, Accept, Content-Disposition"
    security.cors.max.age=3600
      ############################ Cash Challan Start #############################

    cashchallan.api.eis.services.publickey.path=/certs/EIS_ENC_UAT.cer
    cashchallan.api.eis.services.privatekey.path=/certs/SBI_private.key

    aws.s3.key=IDHJO1513FFMMNLPR9BC
    aws.s3.secret=avXqEPv5B_QqqPK0D1VJzP3pSyAA4x31Zu_KucQ9
    aws.s3.region=ap-south-1
    aws.s3.url=https://s3store.bank.sbi/
    aws.s3.bucket.report=epay-nonprod-s3bucket
    aws.s3.bucket.ops=${aws.s3.bucket.report}
    aws.s3.bucket=epay-nonprod-s3bucket
    spring.datasource.hikari.connection-timeout=30000

    spring.kafka.topic.payment.cashstatusquery.cron_time= 0/60 * * * * *
    cashchallan.api.eis.services.gateway.enquiry= https://eissiuat.sbi.co.in/gen6/gateway/payments/Dr1Cr2MR/enquiry
    cashchallan.api.eis.services.gateway.branchcode=05049
    cashchallan.api.eis.services.gateway.sourceid=NY
    cashchallan.api.eis.services.gateway.sbisourceid=SBINY
    cashchallan.api.maxlength.otherdetails=250
    epay.payment.cash.begin=-----BEGIN PRIVATE KEY-----
    epay.payment.cash.end=-----END PRIVATE KEY-----
    ############################ Cash Challan End #############################

    ############################ NEFT Challan Start ##########################################

    neft.pdf.challan.accountnumber=AGG5344299114547
    neft.pdf.challan.beneficiaryname=SBIePAY NEFT
    neft.pdf.challan.branchname=FINANCIAL INSTITUTIONS BRANCH
    neft.pdf.challan.ifsccode=SBIN0011777

    ############################ NEFT Challan End #########################################
