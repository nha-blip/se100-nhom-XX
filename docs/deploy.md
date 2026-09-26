# Triển khai — Nhóm xxx

- URL công khai: Chưa triển khai.
- Thư mục xuất bản: `src`
- Trang chính: `src/index.html`
- Kiểm tra trên điện thoại: Chưa thực hiện.

## Triển khai bằng Netlify

1. Commit và push trang web lên GitHub.
2. Trong Netlify, nhập dự án từ Git và chọn repo của nhóm.
3. Chọn nhánh `main`, để trống lệnh build và đặt Publish directory là `src`.
4. Deploy, rồi sao chép URL HTTPS thực tế vào mục URL công khai ở trên và dòng **Bản chạy:** trong README.

Có thể triển khai bằng Cloudflare Pages với cùng thư mục đầu ra `src`, không cần lệnh build.

## Bằng chứng trên điện thoại

1. Mở URL công khai trên điện thoại và kiểm tra trang hiển thị **Nhóm xxx**.
2. Chụp màn hình có thanh địa chỉ, lưu ảnh vào `docs/images/deploy-mobile.png`.
3. Thêm dòng sau vào tài liệu này sau khi đã lưu ảnh:

```markdown
![Trang Nhóm xxx mở trên điện thoại](images/deploy-mobile.png)
```

4. Cập nhật trạng thái kiểm tra trên điện thoại, commit và push tài liệu cùng ảnh.
