spring:
  application:
    name: epay_payment_service
  jpa:
    show-sql: true
    properties:
      hibernate:
        show_sql: true
        format_sql: true
    hibernate:
      ddl-auto: none
  web:
    resources:
      static-locations: "classpath:/,file:/non-existent-folder"

# Db connectivity
# logging.level.org.springframework.web=debug

  datasource:
    url: "jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=dbscanproddc.epay.sbi)(PORT=1524))(CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=epaydb)))"
    username: EPAYTRANSCTION
    password: April_2025
    driver-class-name: oracle.jdbc.OracleDriver
    hikari:
      maximum-pool-size: 300
      minimum-idle: 20
      idle-timeout: 30000
      max-lifetime: 1800000
  liquibase:
    change-log: "classpath:db/changelog/db.changelog-master.xml"
    enabled: false
    drop-first: false
  profiles:
    active: prod-dc
  kafka:
    #Kafka boot strap server -preprod
    bootstrapServers: prod-dc-cluster-kafka-bootstrap.prod-dc-kafka.svc.cluster.local:9092
    #Kafka consumer setting
    consumer:
      groupId: payment-consumers
      enableAutoCommit: true
      autoCommitInterval: 100
      sessionTimeoutMS: 300000
      requestTimeoutMS: 420000
      fetchMaxWaitMS: 200
      maxPollRecords: 5
      autoOffsetReset: latest
      keyDeserializer: org.apache.kafka.common.serialization.StringDeserializer
      valueDeserializer: org.apache.kafka.common.serialization.StringDeserializer
      retryMaxAttempts: 3
      retryBackOffInitialIntervalMS: 10000
      retryBackOffMaxIntervalMS: 30000
      numberOfConsumers: 1
    #Kafka producer setting
    producer:
      acks: all
      retries: 3
      batchSize: 1000
      lingerMs: 1
      bufferMemory: 33554432
      keyDeserializer: org.apache.kafka.common.serialization.StringSerializer
      valueDeserializer: org.apache.kafka.common.serialization.StringSerializer
    #Topics
    topic:
      payment:
        notification:
          sms: payment_sms_notification_topic
          email: payment_email_notification_topic
        push:
          verification: payment_push_verification_topic
      partitions: 4
      replicationFactor: 1
  mail:
    host: 10.176.245.236
    port: 587
    username: sbitestclient
    password: sbitestclient_7f827c4b3aa6cd1f08d6b9cce2c0c80e

server:
  port: 9093
  servlet:
    context-path: /api/payments/v1/

#Application Proxy Details
https_protocols: TLSv1.2
https_proxySet: true
# https_proxyHost = serverswg.sbi.co.in
https_proxyHost: 10.176.187.203
https_proxyPort: 9090

#Optional settings

#In Minutes
transaction:
  token:
    expiry:
      time: 30

# Liquibase Properties
logging:
  level:
    liquibase: DEBUG
    org:
      apache:
        kafka: ERROR

#WIBMO PG Constants
epay:
  payment:
    card:
      # ---- International ----
      callback_url_intl: https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/intl/visamaster/callback
      callbackg_url_rupay_intl: https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/intl/rupay/callback
      pvReqURL_intl: https://3ds2-api-3dsserver-intg.pc.enstage-sas.com/3dsserverapi/v5/pVrq/8642/
      saleAuthURL_intl: https://areionsbi.pc.enstage-sas.com/saleservice/api/v1/sale
      checkbin_Url_intl: https://areionsbi.pc.enstage-sas.com/authentication/api/v1/checkbin
      initiate_url_intl: https://areionsbi.pc.enstage-sas.com/authentication/api/v1/initiate
      generateOtp_url_intl: https://areionsbi.pc.enstage-sas.com/authentication/api/v1/generateOtp
      resendOtp_url_intl: https://areionsbi.pc.enstage-sas.com/authentication/api/v1/resendOtp
      verifyOtp_url_intl: https://areionsbi.pc.enstage-sas.com/authentication/api/v1/verifyOtp
      authorize_url_intl: https://areionsbi.pc.enstage-sas.com/saleservice/api/v1/authorize
      reverse_url_intl: https://areionsbi.pc.enstage-sas.com/authentication/web/v1/parseRupayResponse
      token_url_intl: https://cardvault-azure.pc.enstage-sas.com/tokenVault/v3/tokenize
      api_key_intl: 41b52a74-92f0-4c05-9487-8d0e01238170
      clientId_intl: 175f0289-0272-4cd2-bffa-51fc9b6c4101
      clientApiUser_intl: 100001-HDFC-2lP6tK9sB7
      clientApiKey_intl: HDFC5nN2pO3aR5
      tokenSecretKey_intl: f3a81ff0-e139-4e6a-8888-9709a0407713
      tokenSecretKey_1_intl: c19215e2-c2a4-4630-937b-a3bfd764c96b
      merchantId_Token_intl: hdfctestmid1
      tokenRequesterId_MC_intl: 7D6D61B2-0BF9-4763-885F-6E9D7516C00E
      tokenRequesterId_VS_intl: DFFFAC4C-B2D8-4CEA-BEBB-E7EB1716A348
      tokenRequesterId_R_intl: 77799966611
      acquiringBankId_intl: 93734895
      acquireInstanceId_intl: 93954c92-58ed-4912-8648-0948d5becc69
      acquirerMerchantId_intl: PAYUGCMID1

      # ---- Domestic ----
      callback_url: https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/visamaster/callback
      callbackg_url_rupay: https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/rupay/callback
      pvReqURL: https://secure-3dssapi-az.wibmo.com/3dsserverapi/v5/pVrq/8642/
      saleAuthURL: https://areionsbi.wibmo.com/saleservice/api/v1/sale
      doubleVerification: https://areionsbi.wibmo.com/merchantOrderPayment/api/v1/txn-status
      REFUNDURL: https://areionsbi.wibmo.com/voidrefundservice/api/v1/voidorrefund
      REFUNDENQUIRY: https://areionsbi.wibmo.com/merchantOrderPayment/api/v2/txn-status
      checkbin_Url: https://areionsbi.wibmo.com/authentication/api/v1/checkbin
      initiate_url: https://areionsbi.wibmo.com/authentication/api/v1/initiate
      generateOtp_url: https://areionsbi.wibmo.com/authentication/api/v1/generateOtp
      resendOtp_url: https://areionsbi.wibmo.com/authentication/api/v1/resendOtp
      verifyOtp_url: https://areionsbi.wibmo.com/authentication/api/v1/verifyOtp
      authorize_url: https://areionsbi.wibmo.com/saleservice/api/v1/authorize
      reverse_url: https://areionsbi.wibmo.com/authentication/web/v1/parseRupayResponse
      token_url: https://tokenvault-sbi.wibmo.com/tokenVault/v3/tokenize
      api_key: 41b52a74-92f0-4c05-9487-8d0e01238170
      clientId: 7823a827-6cd3-4fc9-8f8f-c76db9ef407c
      clientApiUser: 100001-SBIe-2sL5qU9wR3
      clientApiKey: SBIe8cG8fI0hB7
      tokenSecretKey: c19215e2-c2a4-4630-937b-a3bfd764c96b
      tokenSecretKey_1: c19215e2-c2a4-4630-937b-a3bfd764c96b
      merchantId_Token: SBIEPAYMUINALTID001
      SALT: 8d3e5e6436d3b0ef2bd61424c82ab7e3
      tokenRequesterId_MC: 035f91e4-8b3a-4127-a037-574fb82387c0
      tokenRequesterId_VS: DFFFAC4C-B2D8-4CEA-BEBB-E7EB1716A348
      tokenRequesterId_R: 77700011145
      pgInstanceId_1: 8642
      pgInstanceId: 72702415
      pgInstanceId_2: 72702415
      acquirerBIN_MASTER: 597291
      acquirerBIN_VISA: 401934
      acquiringBankId: 35761063
      acquirerID: 8642
      acquireInstanceId: 30b8b010-5987-4dc2-96cd-7a1d20a520d6
      acquirerMerchantId: 243754249380023
      MCC: 4900
      merchantId: 35967615
      transactionTypeCode: 9003
      deviceCategory: 0
      deviceChannel: '02'
      p_messageVersion: 2.1.0
      messageCategory: '01'
      vaultId: 100001
      INR: 356
      USD: 840
      trantype: 1
      verficationtype: 8
      refundtype: 2
      proxySet: true
      tokenSecretKey_Onboard: 6993cf96-7c56-47ab-9ceb-b7a8a5bfddb8
      Acquirer_ICA: 32996
      dpaIdApi_Url: https://tokenvault-sbi.wibmo.com/tokenVault/nts/v1/dpa/onboard
      browserAgent: "TestMerchant/26.5.0.100 (Android/13/SM-S908E)"
      seamlessURL: https://sbiepay.sbi.bank.in/RupayCardOTPGeneration.jsp
      paymentsRedirectUrl: https://sbiepay.sbi.bank.in/ui/channel/card/sbi/
      cardOnboardURL: /card
      finalResponse_Url: /callback

    #################### sbiinb constants start #####################
    sbiinb:
      gtwmapid: 4
      sbiinbKey: resources/keys/SBI_EPAY.key
      meCode: SBIEPAY
      dvURL: SBIEPAY
      bankURL: https://uatmerchant.onlinesbi.sbi
      bankbrowserurl: https://merchant.onlinesbi.sbi/merchant/merchantprelogin.htm
      bankdvurl: https://merchant.onlinesbi.sbi/thirdparties/doubleverification.htm
      keyvalue: encdata
      merchantcode: SBIEPAY2_SBTEMER
      paymentsRedirectUrl: /api/payments/v1/sbi/inb/responseRedirect?status=
      devredirecturl: https://sbiepay.sbi.bank.in/ui/channel/inb/sbi/
      #SBI INB Redirection URL
      cancelurl: https://sbiepay.sbi.bank.in/api/payments/v1/inb/sbi/callback
      sbiredirecturl: https://sbiepay.sbi.bank.in/api/payments/v1/inb/sbi/callback
      otherinbstatusquery: https://epay.sbi.bank.in/payagg/otherINBstatusQuery/getTxnStatusQuery

    ####### Other INB #######
    #epay.payment.otherinb.success.base.path=https://epay.sbi.bank.in
    #epay.payment.otherinb.fail.base.path=https://epay.sbi.bank.in
    otherinb:
      success:
        base:
          path: https://sbiepay.sbi.bank.in/api/payments/v1/other-inb/sbi/callback
      fail:
        base:
          path: https://sbiepay.sbi.bank.in/api/payments/v1/other-inb/sbi/callback
      proxyip: 10.188.32.80
      proxyport: 3066
      tlsversion: TLSv1.2

    other_inb_mid: 1000524
    #epay.payment.other_inb_key=A3FZ8NvgJN4IjD2OYtYQQfEN83Ej13rOVNYm38Prtpo=
    #epay.payment.other_inb_key_browser=RmmaxYFU4HkPvshgl5ngkg==
    #epay.payment.other_inb_key=dHngr3HvVv5Iq3J/ZLJUkPl5o3hJp/fQ9G3YQbKDZ0k=
    other_inb_key_browser: cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    other_inb_key: cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    merchant_post_url: https://epay.sbi.bank.in/secure/MerchantHostedListener
    #merchant_post_url=https://epay.sbi.bank.in/secure/AggregatorHostedListener
    #sbiinb.otherInbStatusQuery=https://sbiepay.sbi.bank.in/payagg/orderStatusQuery/getOrderStatusQuery
    #epay.payment.sbiinb.otherinbstatusquery=https://www.epay.sbi.bank.in/payagg/otherINBstatusQuery/getTxnStatusQuery

    #################### sbiinb constants end #####################

    ############################ Wallet Mobikwik start #############################
    wallet:
      cancelurl: https://sbiepay.sbi.bank.in/api/payments/v1/wallet/sbi/mobikwikcallback
      bankbrowserurl: https://walletapi.mobikwik.com/encwallet
      bankdvurl: https://walletapi.mobikwik.com/enccheckstatus
      mid: MBK6982415
      cell: 7039262141
      email: punam.rajput.cedge@sbi.co.in
      merchantname: SBIEPAY2
      secretkey: svBeeXQOlbUK3ZuF3Vxj3G5yoEYq
      encryptionkey: 7EBE585BB1F9916FAA40A5BD0E9CA885
      Statuscode: 0
      statusmessage: The payment has been successfully collected
      refid: 838731552
    wallet_sbiredirectdvurl: https://sbiepay.sbi.bank.in/api/payments/v1/wallet/sbi/mobikwikcallback
    ############################ Wallet Mobikwik end #############################

    sbiinb:
      gtwmapid: 4
      sbiinbKey: resources/keys/SBI_EPAY.key
      meCode: SBIEPAY
      dvURL: SBIEPAY
      bankURL: https://uatmerchant.onlinesbi.sbi
      bankbrowserurl: https://merchant.onlinesbi.sbi/merchant/merchantprelogin.htm
      bankdvurl: https://merchant.onlinesbi.sbi/thirdparties/doubleverification.htm
      keyvalue: encdata
      merchantcode: SBIEPAY2_SBTEMER
      paymentsRedirectUrl: /api/payments/v1/sbi/inb/responseRedirect?status=
      devredirecturl: https://sbiepay.sbi.bank.in/ui/channel/inb/sbi/
      cancelurl: https://sbiepay.sbi.bank.in/api/payments/v1/inb/sbi/callback
      sbiredirecturl: https://sbiepay.sbi.bank.in/api/payments/v1/inb/sbi/callback
      otherinbstatusquery: https://epay.sbi.bank.in/payagg/otherINBstatusQuery/getTxnStatusQuery

    otherinb:
      success:
        base:
          path: https://sbiepay.sbi.bank.in/api/payments/v1/other-inb/sbi/callback
      fail:
        base:
          path: https://sbiepay.sbi.bank.in/api/payments/v1/other-inb/sbi/callback
      proxyip: 10.188.32.80
      proxyport: 3066
      tlsversion: TLSv1.2

    other_inb_mid: 1000524
    #epay.payment.other_inb_key=A3FZ8NvgJN4IjD2OYtYQQfEN83Ej13rOVNYm38Prtpo=
    #epay.payment.other_inb_key_browser=RmmaxYFU4HkPvshgl5ngkg==
    #epay.payment.other_inb_key=dHngr3HvVv5Iq3J/ZLJUkPl5o3hJp/fQ9G3YQbKDZ0k=
    other_inb_key_browser: cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    other_inb_key: cs7rDrDAB1e2ZCNYn58xO/wqT4RuTd5hmhhftFO8bd0=
    merchant_post_url: https://epay.sbi.bank.in/secure/MerchantHostedListener
    #merchant_post_url=https://epay.sbi.bank.in/secure/AggregatorHostedListener
    #sbiinb.otherInbStatusQuery=https://sbiepay.sbi.bank.in/payagg/orderStatusQuery/getOrderStatusQuery
    #epay.payment.sbiinb.otherinbstatusquery=https://www.epay.sbi.bank.in/payagg/otherINBstatusQuery/getTxnStatusQuery

    wallet:
      cancelurl: https://sbiepay.sbi.bank.in/api/payments/v1/wallet/sbi/mobikwikcallback
      bankbrowserurl: https://walletapi.mobikwik.com/encwallet
      bankdvurl: https://walletapi.mobikwik.com/enccheckstatus
      mid: MBK6982415
      cell: 7039262141
      email: punam.rajput.cedge@sbi.co.in
      merchantname: SBIEPAY2
      secretkey: svBeeXQOlbUK3ZuF3Vxj3G5yoEYq
      encryptionkey: 7EBE585BB1F9916FAA40A5BD0E9CA885
      Statuscode: 0
      statusmessage: The payment has been successfully collected
      refid: 838731552
    wallet_sbiredirectdvurl: https://sbiepay.sbi.bank.in/api/payments/v1/wallet/sbi/mobikwikcallback

#SBIEPAY Client jks properties
#CLIENT_JKS_FILE_NAME=resources/keys/sbiepay_newpg.jks
#CLIENT_JKS_FILE_NAME=/jboss/app-config/keys/newpg/sbiepay_newpg.jks
wibmo:
  client_jks_filename: keys/sbiepay_newpg.jks
  client_jks_file_pwd: Sbi@1234
  client_alias_name: '1'
  client_alias_pwd: Sbi@1234
  #WIBMO PG Public cert AliasName
  server_pk_alias_name: client_keystore
  proxy:
    required: Y
  client_jks_filename_intl: keys/sbiepay_newpg_intl.jks
  client_jks_file_pwd_intl: keystore
  client_alias_name_intl: client_keypair
  client_alias_pwd_intl: password
  server_pk_alias_name_intl: wibmo_pk
##### End Wibmo

external:
  api:
    ui:
      service:
        redirectView: https://sbiepay.sbi.bank.in/ui/channel/sbi
    paymentCallBackUrl: https://sbiepay.sbi.bank.in/demo/
    sms:
      gateway:
        base:
          path: https://smsapipprod.sbi.co.in:9443
        url: /bmg/sms/epaypgotpdom
        user: epaypgotpdom
        password: Ep@y1Dpt
      body:
        content:
          type: text
        sender:
          id: SBIBNK
        int:
          flag: 0
        charging: 0
    kms:
      services:
        base:
          path: https://sbiepay.sbi.bank.in/api/kms/v1
    admin:
      services:
        base:
          path: http://admin-adminservice.prod-dc-admin.svc.cluster.local:9094/api/admin/v1
    transaction:
      services:
        base:
          path: http://txn-transactionservice.prod-dc-transaction.svc.cluster.local:9092/api/transaction/v1

security:
  cors:
    allowed:
      origins: https://sbiepay.sbi.bank.in
      methods: GET,POST
      headers: '"Authorization, Origin, X-Correlation-Id, Content-Type, Accept, Content-Disposition"'
    max:
      age: 3600
    origin: https://sbiepay.sbi.bank.in
  whitelist:
    url: /webjars/, /actuator/,/v3/api-docs,/token,/downtime/api,/s1/fetch-data,/inb/sbi/**,/other-inb/sbi/**,/cards/sbi/**,/upi/sbi/**

cors:
  origin: https://sbiepay.sbi.bank.in
  allowedOrigins: '*'

whitelisted:
  endpoints: /webjars/, /actuator/,/v3/api-docs,/token,/downtime/api,/s1/fetch-data,/inb/sbi/**,/other-inb/sbi/**,/cards/sbi/**,/upi/sbi/**

jwt:
  secret:
    key: gdjfgskjfhsdjkhkflkdlksdlfkskfwperip3ke3le3lmldrnkfnhiewjfejfokepfkldkfoikfokork3dklwedlsvflvkfkvlkdfvodkvcdokro3

email:
  recipient: ebms_uat_receiver@ebmsgits.sbi.co.in
  from: ebms_uat_sender@ebmsgits.sbi.co.in

management:
  endpoints:
    web:
      expo-sure:
        include: '*'
      exposure:
        include: health,info
  endpoint:
    health:
      show-details: always
      probes:
        enabled: true
  health:
    livenessState:
      enabled: true
    readinessState:
      enabled: true

external:
  api:
    kms:
      services:
        base:
          path: https://sbiepay.sbi.bank.in/api/kms/v1

#START OF UPI DETAILS---#SBIEpay2.0
UPI:
  UPI_CLIENT_ID: SBIEPAY2
  SECRETKEY: f65e8f0a484d45babd33c10b4535ef66
  OAUTH_USERNAME: oauth2-api-merweb-SBI0000007165940
  OAUTH_PASSWORD: 44A243905F8B96F9280EB056CECC608A0A998019ED67D2C77B4F5554DCB48B13
  #UPI public key-------------
  UPI_SBI_PUBLIC_KEY_PATH: /certs/SBI0000000032588_UAT_1224_PublicKey.asc
  #Epay private -------------------
  EPAY_PVT_KEY_PATH: /certs/0x1D883787-sec.asc
  PACKET_ENCRYPTION_KEY: 25dea54b392d9b24803d95e2df4d11d8
  OAUTH_TOKEN_URL: https://upionline.sbi/oauth/token
  #UPI.VALIDATE_VPA_CHECK_URL= https://uatupionline.sbi/upi2/upi/web/v2.0/validatevpaweb
  VALIDATE_VPA_CHECK_URL: https://upionline.sbi/upi/web/v2.0/validateVPAWeb
  VPA_COLLECT_URL: https://upionline.sbi/upi/web/v2.0/meCollectInitiateWeb
  TXN_STATUS_ENQUIRY_URL: https://upionline.sbi/upi/web/v2.0/meTranStatusQueryWeb
  UPI_CALLBACK_URL: https://sbiepay.sbi.bank.in/api/payments/v1/upi/sbi/callback
  UPICONFIG:
    URL: http://admin-adminservice.prod-dc-admin.svc.cluster.local:9094/admin/v1/merchant/upi/getUpiConfigDeatils
  TXN_STATUS_ENQUIRY_API_URL: https://upionline.sbi/upi/web/v2.0/meTranStatusQueryWeb
  VPA_CHECK_API_URL: https://upionline.sbi/upi/web/v2.0/validateVPAWeb
  INTENT_API_URL: https://upionline.sbi/upi/mandate/signVerifyIntent
  UPI_CONFIG_DEATILS_URL: http://admin-adminservice.prod-dc-admin.svc.cluster.local:9094/admin/v1/merchant/upi/getUpiConfigDeatils
  HANDSHAKE_API_URL: https://upionline.sbi/upi/oauth2-web-handshake

#<!--Merchant me code--> USED IN PG MERCHANTID
PG:
  UPI_MERCHANT_ID: SBI0000007165940

#<!--PGP Encryption> -Password use(Epay side)----Created for Encryption-Decryption in Utility jar
UPI_PVT_KEY_PWD: Sbiepay@2027

upi:
  upiGatewayConfigDetailsUrl: /merchant/gateway
  server:
    private:
      key:
        path: 0x1D883787-sec.asc
    public:
      key:
        path: SBI0000000032588_UAT_1224_PublicKey.asc

#For QR Sign -----------------
UPI_category: '01'
UPI_intentMode: '05'
UPI_ivtoken: 2F70CBBDFAE8DBA95F46CEB68A970673
UPI_keyid: 10
UPI_mode: '05'
UPI_purpose: '00'
UPI_qrMedium: '06'
UPI_ver: '01'
UPI_cu: INR
UPI_tier: TIER1
#---TEMPORARY FIELD ---
UPI_upiQrVpa: sbiepay2@sbi
UPI_businessName: TestMerchant
UPI_mccCode: 6012

upiqr:
  private:
    key:
      path: /certs/0x1D883787-sec.asc
      password: Sbiepay@2027
  sbi:
    public:
      key:
        path: /certs/SBI0000000032588_UAT_1224_PublicKey.asc

#URLs
#END OF UPI DETAILS

#External Base Path
kms:
  service:
    #kms.service.base-url=
    #kms.service.cors-origin=
    base-url: http://kms-kmsservice.prod-dc-kms.svc.cluster.local:9093/api/kms/v1
    cors-origin: https://sbiepay.sbi.bank.in

key:
  aek: BiIZ5feKr98Ud3XSpVywqXlwRNfSy9Gtis04WqEbD/0=

# {{- end }}
