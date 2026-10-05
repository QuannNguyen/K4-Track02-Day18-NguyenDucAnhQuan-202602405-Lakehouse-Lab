# Reflection — Small-file anti-pattern

Anti-pattern tôi quan tâm nhất là tạo quá nhiều small files. Pipeline observability
LLM dễ gặp vấn đề này khi ghi theo micro-batch nhỏ, retry cùng request hoặc partition
quá chi tiết theo thời gian/model. Mỗi file làm tăng chi phí listing, mở file và lập
kế hoạch query; metadata cũng phình lên dù tổng dữ liệu chưa lớn. Trong lab, compaction
NB6 giảm 200 file còn 11.

Để phòng tránh, tôi sẽ chọn kích thước micro-batch phù hợp, khử trùng theo request ID,
tránh partition cardinality cao, theo dõi số/kích thước file theo partition và chạy
compaction có ngưỡng thay vì gom mù quáng thành một file. Với point lookup, clustering
hoặc Z-order cần được xác nhận bằng file statistics và query workload thực tế. Các
benchmark phải đo trên môi trường đại diện, vì cache, I/O và tải máy làm thời gian
truy vấn dao động. Tôi dùng Copilot SDK trong VS Code hỗ trợ chạy notebook và soạn
giải thích; số liệu lấy từ output thực tế. Chi tiết: [AI_USAGE.md](AI_USAGE.md).
