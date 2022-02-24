---
sort: 3
---
# ftrace
## ftrace 등장 배경
`printk`, `dump_stack`을 활용해 커널 디버깅을 하던 리눅스 커널 개발자들은 여러 불편함을 느끼기 시작했습니다. \
커널 개발자들은 다음과 같은 요건을 충족하는 커널 디버깅 기능이 있으면 좋겠다고 생각하게 됩니다.
* 함수 호출 흐름을 소스코드를 수정하지 않고도 보고 싶다.
* 커널의 세부 실행 정보를 출력해 줬으면 좋겠다.
* 1초에 수십 번 호출해도 성능에 부담을 주지 않았으면 좋겠다.
* 커널 로그도 함께 보고 싶다.

## ftrace 특징
1. 인터럽트, 스케줄링, 커널 타이머 드으이 커널 동작을 상세히 추적합니다.
2. 함수 필터를 지정하면 지정한 함수를 호출한 함수와 전체 콜 스택까지 출력합니다.
3. 코드를 수정할 필요가 없습니다.
4. 함수를 어느 프로세스가 실행하는지 알 수 있습니다.
5. 함수가 실행된 시각 정보를 알 수 있습니다.
6. ftrace로그를 활성화해도 시스템 동작에 부하를 거의 주지 않습니다.

## ftrace를 사용하기 위한 kernel 설정
ftrace에서 제공하는 `nop`, `function`, `function_graph` 트레이서를 쓰려면 ftrace 관련 코드가 커널 이미지에 포함돼야 합니다. \
즉, ftrace 코드가 추가된 커널 소스를 빌드해야 합니다.
```
CONFIG_FTRACE=y
CONFIG_DYNAMIC_FTRACE=y
CONFIG_FUNCTION_TRACER=y
CONFIG_FUNCTION_GRAPH_TRACER=y
CONFIG_IRQSOFF_TRACER=y
CONFIG_SCHED_TRACER=y
CONFIG_FUNCTION_PROFILER=y
CONFIG_STACK_TRACER=y
CONFIG_TRACER_SNAPSHOT=y
```
위의 커널 설정 컨피그를 활성화해야 합니다.\
ftrace 설정 파일은 `/sys/kernel/debug/tracing`에서 확인할 수 있습니다.

## ftrace 설정
### tracing_on: 트레이서 활성화/비활성화
ftrace를 활성화/비활성화하려면 `tracing_on` 파일을 설정해야 합니다. \
`tracing_on`은 부팅 후 기본적으로 0으로 설정돼 있습니다.
```bash
echo 0 > /sys/kernel/debug/tracing/tracing_on
```
`tracing_on` 파일에 0을 저장합니다. \
ftrace를 비활성화 하는 동작입니다.
```bash
echo 1 > /sys/kernel/debug/tracing/tracing_on
```
`tracing_on` 파일에 1을 저장합니다. \
ftrace를 활성화 하는 동작입니다.

### tracer 설정
tracer 목록은 `/sys/kernel/debug/tracing/available_tracers`에서 확인 가능합니다.
* nop
* function
* function_graph
* blk
* wakeup
* wakeup_dl
* wakeup_rt
* irqsoff
* mmiotrace