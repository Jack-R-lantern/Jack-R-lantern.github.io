---
sort: 1
---
# printk() 함수
printk() 함수를 호출하면 커널 로그를 볼 수 있습니다.
## printk 자료형에 따른 서식 지정

|변수 타입|서식 지정자|
|-------|---------|
|int|%d, %x|
|unsigned int|%u, %x|
|long|%ld, %lx|
|unsigned long|%lu, %lx|
|long long|%lld, %llx|
|unsigned long long|%llu, %llx|
|size_t|%zu, %zx|
|ssize+t|%zd, %zx|
|s32|%d, %x|
|u32|%u, %x|
|s64|%lld, %llx|
|u64|%llu, %llx|

## printk()를 이용한 로그레벨 적용 방법
[log level](/README.md#%EB%A6%AC%EB%88%85%EC%8A%A4%EC%97%90%EC%84%9C%EC%9D%98-%EB%A1%9C%EA%B7%B8-%EB%A0%88%EB%B2%A8linuxkernelh)
```c
printk(KERN_INFO "info message...\n");
printk("<4>" "warning message...\n");
printk("<3>" error message...\n);
printk("default loglevel...\n");
```
## printk로 함수 심벌 정보를 보는 방법
https://github.com/raspberrypi/linux/blob/rpi-4.19.y/kernel/workqueue.c
```c
static void insert_wq_barrier(struct pool_workqueue *pwq,
                              struct wq_barrier *barr,
                              struct work_struct *target, struct worker *worker)
{
        struct list_head *head;
        unsigned int linked = 0;

        printk("[+] process: %s \n", current->comm);
        printk("[+][debug] message [F: %s, L: %d]: caller:(%pS)\n",
                        __func__, __LINE__, (void*)__builtin_return_address(0));
...
```
* \_\_func\_\_: 현재 실행 중인 함수의 이름
* \_\_LINE\_\_:현재 실행 중인 코드 라인
* \_\_builtin_return_address(0): 현재 실행중인 함수를 호출한 함수의 주소

`%pS`는 아규먼트로 지정한 주소를 심벌로 변환해 출력합니다.

## printk를 쓸 때 주의해야 할 점
printk를 사용할 때는 printk의 호출 빈도를 반드시 체크해야 합니다. \
리눅스 커널에서 1초에 수백번 이상 호출되는 함수에 printk를 사용하면 시스템이 락업(Lockup)되거나 커널 패닉으로 오동작할 수 있습니다.