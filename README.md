spring.application.name=epay_payment_service
    server.port=9093   
    server.servlet.context-path=/api/payments/v1/

    # Db connectivity
    spring.jpa.show-sql=true
    spring.jpa.properties.hibernate.show_sql=true
    spring.jpa.properties.hibernate.format_sql=true
    spring.web.resources.static-locations=classpath:/,file:/non-existent-folder

    #logging.level.org.springframework.web=debug
    spring.datasource.url=jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=dbscanproddc.epay.sbi)(PORT=1524))(CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=epaydb)))
    spring.datasource.username=EPAYTRANSCTION
    spring.datasource.password=April_2025
    spring.jpa.show-sql=true
    spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
    
    #Application Proxy Details
    https_protocols = TLSv1.2
    https_proxySet = true
    # https_proxyHost = serverswg.sbi.co.in
    https_proxyHost = 10.176.187.203
    https_proxyPort = 9090

    #Optional settings
    spring.datasource.hikari.maximum-pool-size=300
    spring.datasource.hikari.minimum-idle=20
    spring.datasource.hikari.idle-timeout=30000
    spring.datasource.hikari.max-lifetime=1800000

    #In Minutes
    transaction.token.expiry.time=30

    # Liquibase Properties
    spring.liquibase.change-log=classpath:db/changelog/db.changelog-master.xml
    spring.liquibase.enabled=false
    spring.liquibase.drop-first=false
    logging.level.liquibase=DEBUG
    spring.jpa.hibernate.ddl-auto=none
    #WIBMO PG Constants
    epay.payment.card.callback_url_intl = https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/intl/visamaster/callback
    epay.payment.card.callbackg_url_rupay_intl = https://sbiepay.sbi.bank.in/api/payments/v1/cards/sbi/intl/rupay/callback
    epay.payment.card.pvReqURL_intl = https://3ds2-api-3dsserver-intg.pc.enstage-sas.com/3dsserverapi/v5/pVrq/8642/
    epay.payment.card.saleAuthURL_intl = https://areionsbi.pc.enstage-sas.com/saleservice/api/v1/sale
    epay.payment.card.checkbin_Url_intl = https://areionsbi.pc.enstage-sas.com/authentication/api/v1/checkbin
    epay.payment.card.initiate_url_intl = https://areionsbi.pc.enstage-sas.com/authentication/api/v1/initiate
    epay.payment.card.generateOtp_url_intl = https://areionsbi.pc.enstage-sas.com/authentication/api/v1/generateOtp 
    epay.payment.card.resendOtp_url_intl = https://areionsbi.pc.enstage-sas.com/authentication/api/v1/resendOtp
    epay.payment.card.verifyOtp_url_intl = https://areionsbi.pc.enstage-sas.com/authentication/api/v1/verifyOtp
    epay.payment.card.authorize_url_intl = https://areionsbi.pc.enstage-sas.com/saleservice/api/v1/authorize
    epay.payment.card.reverse_url_intl = https://areionsbi.pc.enstage-sas.com/authentication/web/v1/parseRupayResponse
    epay.payment.card.token_url_intl = https://cardvault-azure.pc.enstage-sas.com/tokenVault/v3/tokenize
    epay.payment.card.api_key_intl = 41b52a74-92f0-4c05-9487-8d0e01238170
    epay.payment.card.clientId_intl = 175f0289-0272-4cd2-bffa-51fc9b6c4101
    epay.payment.card.clientApiUser_intl = 100001-HDFC-2lP6tK9sB7
    epay.payment.card.clientApiKey_intl = HDFC5nN2pO3aR5
    epay.payment.card.tokenSecretKey_intl = f3a81ff0-e139-4e6a-8888-9709a0407713
    epay.payment.card.tokenSecretKey_1_intl = c19215e2-c2a4-4630-937b-a3bfd764c96b
    epay.payment.card.merchantId_Token_intl = hdfctestmid1
    epay.payment.card.tokenRequesterId_MC_intl = 7D6D61B2-0BF9-4763-885F-6E9D7516C00E
    epay.payment.card.tokenRequesterId_VS_intl = DFFFAC4C-B2D8-4CEA-BEBB-E7EB1716A348
    epay.payment.card.tokenRequesterId_R_intl = 77799966611
    epay.payment.card.acquiringBankId_intl = 93734895
    epay.payment.card.acquireInstanceId_intl = 93954c92-58ed-4912-8648-0948d5becc69
    epay.payment.card.acquirerMerchantId_intl = PAYUGCMID1
