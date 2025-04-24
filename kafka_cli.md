bin/kafka-topics.sh --bootstrap-server localhost:9092 --list                 # 모든 토픽 목록 조회
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic my-topic --partitions 3 --replication-factor 1   # 토픽 생성
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic my-topic  # 특정 토픽 상세 정보 확인
bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic my-topic     # 토픽 삭제

bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic my-topic    # 프로듀서 실행 (표준 입력으로 메시지 전송)
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning   # 처음부터 메시지 소비
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --group test-group  # 그룹 ID로 메시지 소비

bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list              # 모든 컨슈머 그룹 목록 확인
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group test-group  # 특정 컨슈머 그룹 상태 확인

bin/kafka-storage.sh random-uuid                            # KRaft 모드용 클러스터 UUID 생성
bin/kafka-storage.sh format --cluster-id <UUID> --config config/kraft/server.properties  # KRaft 로그 디렉터리 포맷

bin/kafka-server-start.sh config/kraft/server.properties    # Kafka 브로커 시작 (KRaft 모드)
bin/kafka-server-stop.sh                                    # Kafka 브로커 종료 (스크립트가 있을 경우)

