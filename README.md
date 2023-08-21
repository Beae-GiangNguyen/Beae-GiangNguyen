# Quy trình làm việc với git

## Làm việc với branch trong git
**Note:** Branch sẽ theo cấu trúc của task hay bug bên jira
- Tạo một branch mới:\
`` git branch <tên branch> ``
- Tạo branch mới và chuyển sang luôn branch ấy để làm việc:\
`` git branch -b <tên branch> ``
- Chuyển đổi qua lại giữa các branch:\
`` git checkout <tên branch> ``
 - Liệt kê tất cả các branch:\
`` git branch -a ``
## Commit
Khi mọi người đã code xong chức năng hoặc bug thì cần commit code lên
**Note:** Commit phải theo cấu trúc bên jira(tiền tố, mục đích của commit)
- Add tất cả các file đã sửa vào stage:\
`` git add ``
- Add một số file nhất định vào stage:\
`` git add <tên file> ``
- Commit code: (sẽ theo format trên jira)\
`` git commit -m"tên commit" ``
- Commit tất cả các file (lười mà không muốn thêm bước add):\
`` git commit -a -m"tên commit" ``
## Push code
- Push code lên repository:\
`` git push origin <tên branch> ``
- Sau đó mọi người sẽ tạo pull request vào nhánh chính để người quản lý review code và merge

## Rebase
Chức năng này có tác dụng chính là giúp cho các Developer có thể lưu trữ được lịch sử làm việc của dự án. Lịch sử này sẽ được lưu giữ theo tuyến tính.

Ví dụ như, bạn đang làm việc ở một branch feature, cùng lúc đó sẽ có nhiều branch master có sự phát triển, thay đổi. Lúc này, bạn muốn cập nhật branch đang làm việc theo branch master, tuy nhiên không lưu lại các thông tin cập nhật đó không được lưu trữ trong branch history.

Đây chính là lúc mà bạn sẽ cần sử dụng đến chức năng Git Rebase. Lịch sử Git tuyến tính, ít rẽ nhánh sẽ được thực hiện. Điều này sẽ giúp cho các lập trình viên có thể dễ dàng thực hiện truy vết những sự thay đổi hơn.
**Note** Trường hợp conflig sau khi rebase thì cần xử lý conflig trước khi push.\
`` git add . `` add các file sau khi đã xử lý conflig\
`` git rebase --continue `` đẻ hoàn tất rebase\
`` git rebase --abort `` nếu muốn huỷ bỏ quá trình rebase

## Trường hợp branch nhiều người làm
**Note:** Nếu branch mà có nhiều người làm chung, chưa kịp push code lên đã có người push code lên trước thì trước khi push code cần pull rebase\
Rebase sẽ giúp cho 
`` git pull --rebase ``\
Commit sẽ được đẩy lên trên commit của người khác trong log\
Sau đó push code lên 
## Trường hợp mà branch chính (dev) đã có người khác push thêm code vào
`` git pull --rebase origin dev ``\
Lệnh trên sẽ giúp bạn lấy những code mới nhất từ branch dev về, sau đó "viết lại" branch feature của bạn để đẩy commit của bạn lên trên cùng. Cuối cùng là push force lên feature branch. Push force sẽ apply toàn bộ log ở local của bạn lên branch ở repo, bất chấp log 2 nơi khác nhau:\
`` git push -f origin <tên branch> ``
## Gộp commit
**Note:** Trong quá trình làm có thể sẽ tạo nhiều commit trên một branch, khi muốn gộp các commit thành 1 commit duy nhất:
`` git rebase -i HEAD~<số commit cần gộp> ``\
Sau đó terminal sẽ hiện ra màn hình với nhiều option như edit, reword, squash... sử dụng các thao tác của vim Thực hiện thay pick ở đầu dòng bằng option mình muốn(ở trường hợp này thì là squash, hoặc có thể dùng s thay thế). Lưu thay đổi (:wq trong vim)\
![Screenshot 2023-08-21 at 15 21 10](https://github.com/Beae-GiangNguyen/Beae-GiangNguyen/assets/139731665/af1e8baf-eba6-4de1-ab1b-0bce93f6f731)\
xoá bỏ commit message không cần thiết (:wq).\
![Screenshot 2023-08-21 at 15 22 49](https://github.com/Beae-GiangNguyen/Beae-GiangNguyen/assets/139731665/bc4a2905-6e72-4560-af33-d52fc308963b)\
Force push code sau khi đã gộp commit
## Các option khác của git

### Reset commit 
- Reset commit nhưng code vẫn ở trong stage, sẵn sàng cho commit lại:\
`` git reset --soft commit_id ``
- Reset commit và đẩy code ra khỏi stage. Cần dùng git add trước khi có thể commit lại:\
`` git reset --mixed commit_id ``
- Reset commit và xóa toàn bộ code đã làm:\
`` git reset --hard commit_id ``
### Stash
**Note:** Có thể dùng cái này như một cứu cánh để lưu tạm code trước khi thực hiện các lệnh rebase hay checkout sang branch khác mà bị conflict
- Lưu code vào stash: \
`` git stash `` hoặc `` git stash save "tên" ``
- Apply stash cuối cùng vừa lưu:\
`` git stash apply ``
- Apply một stash cụ thể: \
``git stash apply stash@{1} ``
- Liệt kê các stash:\
`` git stash list ``\
![Screenshot 2023-08-21 at 15 18 37](https://github.com/Beae-GiangNguyen/Beae-GiangNguyen/assets/139731665/0cf5557c-45a2-46a7-a940-ac1260084a82)

