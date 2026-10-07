upiqr.private.key.path= /certs/0x1D883787-sec.asc
    upiqr.private.key.password=Sbiepay@2027
    upiqr.sbi.public.key.path= /certs/SBI0000000032588_UAT_1224_PublicKey.asc

    #URLs
    UPI.INTENT_API_URL=https://upionline.sbi/upi/mandate/signVerifyIntent
    UPI.UPI_CONFIG_DEATILS_URL=http://admin-adminservice.prod-dc-admin.svc.cluster.local:9094/admin/v1/merchant/upi/getUpiConfigDeatils
    UPI.HANDSHAKE_API_URL=https://upionline.sbi/upi/oauth2-web-handshake

    upi.server.private.key.path=0x1D883787-sec.asc
    upi.server.public.key.path=SBI0000000032588_UAT_1224_PublicKey.asc

    #END OF UPI DETAILS

    cors.origin=https://sbiepay.sbi.bank.in
    security.cors.origin=https://sbiepay.sbi.bank.in
    cors.allowedOrigins=*
    security.whitelist.url=/webjars/, /actuator/,/v3/api-docs,/token,/downtime/api,/s1/fetch-data,/inb/sbi/**,/other-inb/sbi/**,/cards/sbi/**,/upi/sbi/** 
    whitelisted.endpoints=/webjars/, /actuator/,/v3/api-docs,/token,/downtime/api,/s1/fetch-data,/inb/sbi/**,/other-inb/sbi/**,/cards/sbi/**,/upi/sbi/**
    jwt.secret.key=gdjfgskjfhsdjkhkflkdlksdlfkskfwperip3ke3le3lmldrnkfnhiewjfejfokepfkldkfoikfokork3dklwedlsvflvkfkvlkdfvodkvcdokro3
    

    spring.profiles.active=prod-dc
    #Kafka boot strap server -preprod
    spring.kafka.bootstrapServers=prod-dc-cluster-kafka-bootstrap.prod-dc-kafka.svc.cluster.local:9092
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
    spring.mail.host=10.176.245.236
    spring.mail.port=587
    spring.mail.username=sbitestclient
    spring.mail.password=sbitestclient_7f827c4b3aa6cd1f08d6b9cce2c0c80e
    email.recipient=ebms_uat_receiver@ebmsgits.sbi.co.in
    email.from=ebms_uat_sender@ebmsgits.sbi.co.in
    management.endpoints.web.expo-sure.include=*
    management.endpoint.health.show-details=always
    management.endpoints.web.exposure.include=health,info
    management.endpoint.health.probes.enabled=true
    management.health.livenessState.enabled=true
    management.health.readinessState.enabled=true

    external.api.kms.services.base.path=https://sbiepay.sbi.bank.in/api/kms/v1
    spring.kafka.topic.payment.push.verification=payment_push_verification_topic
    logging.level.org.apache.kafka= ERROR

    ############################ Wallet Mobikwik start #############################
    epay.payment.wallet.cancelurl=https://sbiepay.sbi.bank.in/api/payments/v1/wallet/sbi/mobikwikcallback
    epay.payment.wallet_sbiredirectdvurl=https://sbiepay.sbi.bank.in/api/payments/v1/wallet/sbi/mobikwikcallback
    epay.payment.wallet.bankbrowserurl=https://walletapi.mobikwik.com/encwallet
    epay.payment.wallet.bankdvurl=https://walletapi.mobikwik.com/enccheckstatus
    epay.payment.wallet.mid=MBK6982415
    epay.payment.wallet.cell=7039262141
    epay.payment.wallet.email=punam.rajput.cedge@sbi.co.in
    epay.payment.wallet.merchantname=SBIEPAY2
    epay.payment.wallet.secretkey=svBeeXQOlbUK3ZuF3Vxj3G5yoEYq
    epay.payment.wallet.encryptionkey=7EBE585BB1F9916FAA40A5BD0E9CA885
    epay.payment.wallet.Statuscode=0
    epay.payment.wallet.statusmessage=The payment has been successfully collected
    epay.payment.wallet.refid=838731552
    ############################ Wallet Mobikwik end #############################

    #External Base Path
    external.api.admin.services.base.path=http://admin-adminservice.prod-dc-admin.svc.cluster.local:9094/api/admin/v1
    external.api.transaction.services.base.path=http://txn-transactionservice.prod-dc-transaction.svc.cluster.local:9092/api/transaction/v1
    epay.payment.card.finalResponse_Url=/callback
    kms.service.base-url=
    kms.service.cors-origin=
    key.aek=BiIZ5feKr98Ud3XSpVywqXlwRNfSy9Gtis04WqEbD/0=
    kms.service.base-url=http://kms-kmsservice.prod-dc-kms.svc.cluster.local:9093/api/kms/v1
    kms.service.cors-origin=https://sbiepay.sbi.bank.in

{{- end }}

