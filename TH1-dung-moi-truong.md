# Thực hành 1: dựng môi trường, có URL chạy được

**Tuần 2 · 5 tiết · Làm theo nhóm, mỗi người trên máy mình.**

## Cuối buổi phải có

Trước khi rời phòng, nhóm phải có đủ bốn thứ:

- [ ] Repo trên GitHub, tạo từ repo mẫu bằng **Use this template**, **cả 4 người đã commit** ít nhất một lần bằng tài khoản riêng
- [ ] Một sơ đồ Mermaid trong `diagrams/`. Vẽ xấu không sao, miễn GitHub hiện ra
- [ ] Một **URL công khai** (Cloudflare Pages hoặc Netlify) mở được trên **điện thoại**, hiện tên nhóm
- [ ] Workflow *Kiểm mốc* đã chạy ít nhất một lần, đỏ cũng tính

Thầy đi từng bàn kiểm bốn ô này. Nhóm nào xong sớm thì giúp nhóm bên cạnh.

## Dùng agent trong buổi này

Cứ dùng thoải mái, hôm nay cốt đi cho nhanh. Chỉ một điều: lệnh nào agent bảo chạy thì đọc qua một lần đã. Thấy `rm -rf`, `sudo`, hoặc lệnh dán khoá API vào tệp thì dừng, hỏi thầy.

---

## A. Tài khoản (30 phút, làm trước ở nhà thì tốt)

| # | Việc | Kiểm |
|---|---|---|
| A1 | Tài khoản GitHub. Có email trường `@gm.uit.edu.vn` thì dùng | Đăng nhập được |
| A2 | Cài Git. Đặt tên và email: `git config --global user.name "Tên"` và `user.email` **trùng với email GitHub** | `git config --global user.email` in ra đúng email. Sai thì commit của em không được đếm ở M0 |
| A3 | Cài VS Code (hoặc Zed). Cài extension "Markdown Preview Mermaid Support" | Mở tệp `.md` có ```mermaid, preview hiện sơ đồ |
| A4 | Tài khoản Cloudflare (đăng nhập bằng GitHub) **hoặc** Netlify | Vào được dashboard, **không** bị hỏi thẻ |
| A5 | Một agent: Antigravity, hoặc Cline + Z.ai. Xem `cong-cu-mien-phi.md` | Hỏi được nó một câu |

A2 hay hỏng kiểu này: commit xong rồi mới sửa email, commit cũ vẫn mang email cũ. Chữa commit gần nhất bằng `git commit --amend --reset-author --no-edit`.

## B. Repo nhóm (45 phút)

**B1.** Một người, là người chủ trì mốc M0, vào repo mẫu của môn, bấm **Use this template**, tạo repo tên `se100-nhom-XX` trong tổ chức của môn. Chọn **Public**, vì Actions miễn phí không giới hạn cho repo public.

**B2.** Settings → Collaborators, thêm 3 người còn lại, quyền Write.

**B3.** Mỗi người clone về máy:
```
git clone https://github.com/<to-chuc>/se100-nhom-XX.git
cd se100-nhom-XX
```

**B4.** Mỗi người mở `README.md`, thêm **một dòng** tên mình vào bảng Thành viên, rồi:
```
git add README.md
git commit -m "Thêm <tên> vào danh sách thành viên"
git pull --rebase
git push
```
Ai push sau phải `pull --rebase`. Đây thường là lần đầu các em gặp xung đột: mở tệp ra, giữ cả bốn dòng, `git add`, rồi `git rebase --continue`.

**B5.** Trên GitHub vào tab **Insights → Contributors**, phải thấy đủ 4 người. Ai không hiện thì email trong git config chưa khớp, xem lại A2.

## C. Sơ đồ đầu tiên (30 phút)

**C1.** Sáng nay nhóm đã vẽ nháp sơ đồ lớp trên lớp. Đưa nó vào `diagrams/class.mmd`. Đừng bọc ```mermaid, vì CI đọc tệp `.mmd` thuần. Chưa có gì thì viết tạm:
```
classDiagram
  class TenLopGiDo {
    +thuocTinh
    +phuongThuc()
  }
```

**C2.** Tạo thêm `diagrams/xem.md`, bọc cùng nội dung đó trong ```mermaid để GitHub render:
````
```mermaid
classDiagram
  ...
```
````

**C3.** Commit, push, mở tệp `.md` trên GitHub. Phải thấy sơ đồ vẽ ra. Không thấy là sai cú pháp, dán vào `mermaid.live` xem nó báo hỏng dòng nào.

**C4.** Máy có node thì kiểm luôn ở local, không bắt buộc:
```
npx -y @mermaid-js/mermaid-cli -i diagrams/class.mmd -o /tmp/class.svg
```

## D. Deploy (45 phút)

Chọn một trong hai. Cloudflare Pages nhanh hơn với repo public, Netlify dễ hơn nếu chưa quen.

**D1.** Tạo `src/index.html`:
```html
<!doctype html>
<meta charset="utf-8">
<title>SE100 — Nhóm XX</title>
<h1>Nhóm XX — [tên hệ thống]</h1>
<p>Chạy từ commit: <code id="c">…</code></p>
```

**D2 với Cloudflare Pages.** Dashboard → Workers & Pages → Create → Pages → Connect to Git → chọn repo → Build output directory là `src` → Save and Deploy. Chừng một phút sau có URL dạng `se100-nhom-xx.pages.dev`.

**D2 với Netlify.** Add new site → Import from Git → chọn repo → Publish directory là `src` → Deploy.

**D3.** Mở URL trên **điện thoại**, không phải laptop. Chụp màn hình, dán vào `docs/deploy.md` kèm URL.

**D4.** Điền URL vào dòng `Bản chạy:` trong README, commit, push. Từ nay mỗi lần push là tự deploy lại.

## E. Chạy workflow (15 phút)

**E1.** Tạo nhánh và đẩy lên:
```
git checkout -b moc/M0
git push -u origin moc/M0
```

**E2.** Trên GitHub mở Pull Request từ `moc/M0` vào `main`. Tab **Checks** sẽ hiện *Kiểm mốc* đang chạy.

**E3.** Đợi kết quả. Nó sẽ đỏ, vì chưa có `docs/cau-hoi-khach-hang.md`, chưa có `phan-tu/M0.md`. Đọc log, mỗi dòng ❌ nói rõ thiếu gì. Nộp mốc từ nay là làm đúng vòng đó: đọc ❌, sửa, push, không phải chờ ai chấm.

**E4.** Hôm nay đừng merge PR. M0 nộp cuối tuần này, khi nào đủ hãy merge.

---

## Về nhà, trước hạn M0 cuối tuần

1. Điền `docs/cau-hoi-khach-hang.md`: 3 câu nhóm muốn hỏi khách hàng về đề tài. Hỏi thật vào, đừng hỏi kiểu "hệ thống cần những gì".
2. Viết `phan-tu/M0.md` theo mẫu. M0 chỉ cần từ 40 từ trở lên, các mốc sau là 120, nhưng phải có ít nhất một hash commit.
3. Lưu hội thoại với agent hôm nay vào `ai-log/M0-*.md`.
4. Push, đợi workflow xanh, merge PR.

## Nếu gặp

| Tình huống | Làm gì |
|---|---|
| Máy Intel Mac cũ, cài không được gì | Dùng GitHub Codespaces. Miễn phí 60 giờ mỗi tháng với tài khoản thường, 180 giờ với Student. Mọi thứ chạy trong trình duyệt |
| Không có laptop | Báo thầy đầu buổi, hôm nay ghép máy với bạn cùng nhóm, rồi xin Codespaces cho về sau |
| Cloudflare hay Netlify hỏi thẻ | Em đang ở trang nâng cấp. Quay lại dashboard tìm gói Free |
| `git push` bị từ chối 403 | Chưa được thêm collaborator ở B2, hoặc dùng HTTPS mà chưa đăng nhập. Cài `gh` rồi `gh auth login` |
| Sơ đồ không render trên GitHub | Thiếu dòng ```mermaid mở hoặc đóng, hoặc tên lớp có ký tự lạ. Chỉ dùng chữ, số, gạch dưới |
| Agent đề xuất cài 15 thư viện | Hỏi lại nó: "Cần tối thiểu gì để có một trang HTML tĩnh deploy được?" Câu trả lời đúng là không cần gì cả |
