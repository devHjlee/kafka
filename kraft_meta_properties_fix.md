
# Kafka KRaft 모드 실행 중 meta.properties 오류 해결

Kafka 브로커를 KRaft 모드로 실행할 때 발생한 오류와 해결 과정을 정리합니다.

## 오류 상황

Kafka 브로커 실행 명령어:

```bash
bin/kafka-server-start.sh config/kraft/server.properties
```

실행 후 발생한 오류 메시지:

```
ERROR Exiting Kafka due to fatal exception (kafka.Kafka$)
org.apache.kafka.common.KafkaException: No `meta.properties` found in /tmp/kafka-log (have you run `kafka-storage.sh` to format the directory?)
```

## 원인

Kafka 3.x 이상 KRaft 모드에서는 서버 실행 전 데이터 저장소를 미리 포맷해야 합니다. 이 과정에서 `meta.properties` 파일이 생성됩니다.

## 해결 방법

1. **UUID 생성**

```bash
UUID=$(bin/kafka-storage.sh random-uuid)
echo $UUID
```

2. **서버 설정 파일(server.properties) 확인**

`config/kraft/server.properties`에서 로그 디렉터리 설정을 확인합니다.

```properties
log.dirs=/tmp/kraft-combined-logs
```


3. **스토리지 포맷 실행**

```bash
bin/kafka-storage.sh format \
  --cluster-id <위에서 생성한 UUID> \
  --config config/kraft/server.properties
```

이 명령이 성공하면 지정된 로그 디렉터리에 `meta.properties` 파일이 생성됩니다.

4. **Kafka 서버 다시 실행**

```bash
bin/kafka-server-start.sh config/kraft/server.properties
```

정상적으로 실행이 시작됩니다.

## 추가 확인

- 프로세스 실행 여부 확인:

```bash
ps -ef | grep kafka.Kafka
```

- Kafka 포트(9092) 확인:

```bash
netstat -plnt | grep :9092
```

