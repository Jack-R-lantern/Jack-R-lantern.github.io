---
sort: 1
---

# trace
## 리눅스에서의 로그 레벨(linux/kernel.h)

|level|define|
|-----|------|
|0|#define KERN_EMERG "<0>" / * system is unusable */|
|1|#define KERN_ALERT "<1>" / * action must be taken immediately */|
|2|#define KERN_CRIT "<2>" / * critical conditions */|
|3|#define KERN_ERR "<3>" / * error conditions */|
|4|#define KERN_WARNING "<4>" / * WARNING conditions */|
|5|#define KERN_NOTICE "<5>" / * normal but significant condition */|
|6|#define KERN_INFO "<6>" / * informational */|
|7|#define KERN_DEBUG "<7>" / * debug-level messages */|

## printk
## dump_stack
## ftrace