Dự báo giá cổ phiếu GOOGLE bằng phương pháp Time Series Analysis
1. Tổng quan dự án
Dự án này tập trung vào bài toán dự báo giá đóng cửa (Close price) của cổ phiếu Alphabet Inc. (Google) mã GOOGL trong ngắn hạn. Trong bối cảnh thị trường chứng khoán biến động phức tạp bởi các yếu tố vĩ mô và tâm lý, dự án nhằm cung cấp các tín hiệu khách quan giúp nhà đầu tư xác định điểm mua/bán, thiết lập ngưỡng cắt lỗ và chốt lời để bảo toàn nguồn vốn
Mục tiêu chính:
Xây dựng mô hình dự báo đạt mức sai số MAPE < 10%
Đạt chỉ số RMSE thấp hơn mức biến động tự nhiên của thị trường
So sánh hiệu quả giữa các phương pháp thống kê truyền thống và học sâu
2. Phương pháp tiếp cận
Dự án được định nghĩa là một bài toán dự báo chuỗi thời gian (Time series forecasting) dưới dạng hồi quy (Regression)
. Nhóm đã thử nghiệm và so sánh ba phương pháp chủ đạo:
ARIMA (Autoregressive Integrated Moving Average): Sử dụng các thành phần tự hồi quy (AR), sai phân (I) để làm dừng chuỗi và trung bình trượt (MA) để xử lý sai số
. Đây là mô hình nền tảng để xử lý các xu hướng tuyến tính
. Prophet: Một mô hình do Meta phát triển, có khả năng xử lý tốt tính chu kỳ (tuần, năm) và các ngày nghỉ lễ của thị trường tài chính
. Mô hình này được tối ưu hóa thông qua kỹ thuật Grid Search để điều chỉnh độ nhạy của xu hướng
. LSTM (Long Short-Term Memory): Một dạng mạng nơ-ron hồi tiếp (RNN) chuyên sâu, có khả năng học các quy luật phi tuyến tính phức tạp và các mối quan hệ phụ thuộc dài hạn trong dữ liệu chứng khoán
. Mô hình sử dụng cửa sổ trượt (sliding window) với độ dài 60 ngày để dự báo giá trị ngày tiếp theo
3. Hướng dẫn cài đặt
Để chuẩn bị môi trường chạy mã nguồn, cài đặt các thư viện Python cần thiết được liệt kê trong tệp cấu hình
Sử dụng lệnh sau để cài đặt tự động:
pip install -r requirements.txt
Các thư viện chính bao gồm: pandas, numpy, scikit-learn, keras, statsmodels, và prophet.
4. Hướng dẫn chạy code
Quy trình thực hiện dự án đi từ xử lý dữ liệu thô đến việc đưa ra dự báo cuối cùng:
Bước 1: Tiền xử lý dữ liệu
Dữ liệu được thu thập từ Yahoo Finance với 1255 bản ghi
Thực hiện làm sạch dữ liệu bằng cách loại bỏ các dòng thiếu giá trị (NaN) và kiểm tra dữ liệu trùng lặp
Giữ lại các giá trị ngoại lai (outliers) vì chúng phản ánh biến động thực tế của thị trường
Đối với mô hình LSTM, cần thực hiện chuẩn hóa dữ liệu về khoảng
 bằng MinMaxScaler để tăng tốc độ hội tụ
Bước 2: Huấn luyện mô hình (Training)
Dữ liệu được chia theo tỷ lệ 80% huấn luyện và 20% kiểm tra theo trình tự thời gian
Chạy script huấn luyện: Sử dụng file train.py hoặc các notebook tương ứng để huấn luyện mô hình đã chọn
Với LSTM, kiến trúc bao gồm 2 lớp LSTM xếp chồng với 50 đơn vị mỗi lớp và tích hợp lớp Dropout (0.2) để ngăn chặn hiện tượng học vẹt (overfitting)
Bước 3: Dự báo và Đánh giá (Predict & Evaluate)
Chạy script dự báo: Sử dụng file predict.py để lấy kết quả từ các mô hình đã lưu
Đối với LSTM, kết quả sau khi dự báo cần được đưa qua hàm inverse_transform để chuyển đổi ngược về đơn vị tiền tệ thực tế (USD)
5. Kết luận chung
5.1. Kết luận về kỹ thuật
Sự vượt trội của mô hình Học sâu (Deep Learning): Qua thực nghiệm so sánh, mô hình LSTM là lựa chọn tối ưu nhất với chỉ số MAPE đạt 2,81% (thấp hơn nhiều so với mục tiêu đề ra là < 10%)
. So với các phương pháp thống kê truyền thống như ARIMA (MAPE 24,33%) hay Prophet (MAPE 21,22%), LSTM thể hiện khả năng thích nghi vượt trội với dữ liệu có tính phi tuyến tính và các biến động mạnh
. Hạn chế của mô hình truyền thống: Các mô hình như ARIMA và Prophet gặp hiện tượng underfitting nghiêm trọng; chúng xu hướng "làm mượt" dữ liệu quá mức hoặc dự báo theo kiểu "ngẫu nhiên" (Random Walk), dẫn đến việc bỏ lỡ hoàn toàn các đà tăng trưởng bùng nổ của cổ phiếu GOOGL trong giai đoạn 2025-2026
5.2. Insight chiến lược cho nhà đầu tư (Strategic Insights)
Tín hiệu thị trường: Sự xuất hiện của "Giao cắt vàng" (Golden Cross) và việc duy trì cấu trúc giá nằm trên các đường trung bình MA5, MA50, MA100 xác nhận một thị trường giá lên (bullish) rất bền vững
. Tuy nhiên, khoảng cách hiện tại giữa giá và đường MA100 đang ở mức kỷ lục, cảnh báo trạng thái "quá mua" (overbought) và khả năng sẽ có các nhịp điều chỉnh kỹ thuật về vùng hỗ trợ
Đặc điểm thanh khoản: Biến động giá của Google diễn ra độc lập với khối lượng giao dịch (hệ số tương quan chỉ từ 0,027 đến 0,047)
. Điều này có nghĩa là giá có thể biến động mạnh mà không cần sự bùng nổ về thanh khoản, đòi hỏi nhà đầu tư phải theo dõi sát sao các chỉ báo kỹ thuật thay vì chỉ nhìn vào khối lượng
5.3. Giá trị ứng dụng thực tiễn
Quản trị rủi ro: Mô hình cung cấp căn cứ khách quan để thiết lập các ngưỡng Cắt lỗ (Stop Loss) và Chốt lời (Take Profit), giúp nhà đầu tư loại bỏ yếu tố cảm tính và bảo toàn nguồn vốn hiệu quả
Hỗ trợ ra quyết định: Với độ chính xác cao trong ngắn hạn, LSTM là công cụ hỗ trợ đắc lực trong việc xác định điểm Mua/Bán tối ưu trên từng phiên giao dịch
