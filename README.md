# advanced-scheduler
## Mô tả
Dự án này tập trung vào việc mở rộng và sửa đổi bộ lập lịch trong hệ điều hành pintos để xử lý các chức năng như:
- Quản lý ưu tiên luồng (thread priority)
- Hỗ trợ luồng nhường CPU khi cần thiết (priority donation)
- Hỗ trợ hệ thống lập lịch với hàng đợi đa mức phản hồi (MLFQS - Multi-Level Feedback Queue Scheduler)

## Những thay đổi so với thiết kế ban đầu
### Cấu trúc dữ liệu
* Thêm trường mới vào 'struct thread' : 
    - 'int priority'
    - 'int original_priority'
    - 'struct líst donations'
    - 'struct lock *waiting_lock'
    - 'int nice'
    - 'int recent_cpu'
* Thêm file 'fixed-point.h' và 'fixed-point.c' để hỗ trợ phép toán số thực
* Dùng trong MLFQS để tính toán: 'loa_avg', 'recent_cpu', 'priority'
### Thay đổi trong hành vi luồng
- 'thread_create()' xét đến mức ưu tiên để sắp xếp lại lịch
- 'thread_yield()' và 'schedule()' chọn luồng có ưu tiên cao nhất
- Thêm logic **donation** khi 'lock_acquire()' và **revert** trong 'lock_release()'
