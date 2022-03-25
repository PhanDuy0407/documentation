<h1 align="center"> Vim overview </h1>

### I. Mode
- 3 modes: **Normal**, **Insert** và **Visual Mode**
- Vào từng mode:
  + Lần đầu tiên truy cập file: -> **Normal**
  + `i` hoặc `Insert`: **Normal** -> **Insert**
  + `v`: **Normal** -> **Visual**
  + `ESC`: -> Back to **Normal** 
### II. Basic Command
- Di chuyển: 
   *Note: *number* + *motion*
   + `h` `j` `k` `l` hoặc các phím di chuyển trái - xuống - lên - phải
   + `w`: Di chuyển theo từ
   + `gg`: Di chuyển lên đầu file
   + `G`: Di chuyển xuống cuối file
   + *number* + `G`: Di chuyển tới dòng thứ *number*
   + `CTRL_O`: Quay lại vị trí trước khi nhảy
   + `CTRL_I`: Undo của `CTRL_O`
- Thoát vim: `q` for ***quit***, `w` for ***write***
  + Quay trở về **Normal mode**
  + `:q!`	-> Thoát mà không lưu lại thay đổi
  + `:q`	-> Thoát nếu chưa có thay đổi
  + `:wq`	-> Lưu lại thay đổi và thoát (write n quit)
- Câu lệnh sửa: `c` biểu thị cho ***change operator***
  *Note: `c` + *number* + *motion* hoặc *number* + `c` + *motion* -> Đổi sang **Insert mode**
  + `ce`: Xóa các ký tự bắt đầu từ con trỏ cho đến hết từ trừ ký tự cuối (dấu space)
  + `cc`: Xóa hàng
  + `c$`: Xóa các ký tự từ con trỏ để hết hàng
  + `i` để ***insert*** - chèn vào trước con trỏ ; `a` để ***append*** - nối vào sau con trỏ
  + `I` - chèn vào trước hàng; `A` - nối vào sau hàng  
  + `r` + `char`: Thay đổi ký tự đang được trỏ tới bằng *char*
  + `p`: Put đoạn text vừa bị xóa vào sau con trỏ
  + `o`: Mở một dòng bên dưới con trỏ và chuyển sang **Insert mode**
  + `O` viết hoa: Mở một dòng bên trên con trỏ và chuyển sang **Insert mode**
- Câu lệnh xóa: `d`  biểu thị cho ***delete operator***
  *Note: `d` + *number* + *motion* hoặc *number* + `d` + *motion*
  + `x`: Xóa ký tự con trỏ đang chỉ vào
  + `dw`: Xóa các ký tự bắt đầu từ con trỏ cho đến hết từ
  + `de`: Xóa các ký tự bắt đầu từ con trỏ cho đến hết từ trừ ký tự cuối (dấu space)
  + `dd`: Xóa hàng
  + `d$`: Xóa từ con trỏ đến hết hàng
- Câu lệnh tìm kiếm:
  + `\` + *chuỗi cần tìm*: Tìm kiếm từ trên xuống
  + `?` + *chuỗi cần tìm*: Tìm kiếm từ dưới lên 
  + `%`: Tìm kiếm ngoặc đóng (hoặc ngoặc mở) của dấu ngoặc đang được trỏ vào
- Handling File:
  + `:!` + *command*: Thực thi một external command
  + `:w` + *filename*: Save file
  + `:r` + *filename*: Thêm nội dung của một file khác vào dưới con trỏ 
  + `y`: ***copy operator*** -> Paste ? -> `p`
  + `:set`: Set option (example: `ic` - *ignore case*;` hls` - *hightlight search*)
- Help tool:
  + `:help`: Mở text hướng dẫn (example: `:help w` - *word motion helper text*) -> `CTRL_W` để nhảy qua lại các cửa sổ khi bật *cửa sổ help*
  + `CTRL_D`: Hiển thị gợi ý cho command (example: `:e` + `CTRL_D`: gợi ý tất cả command bắt đầu bằng `:e`)
### III. Summary
- Operator:
  + `d`: Delete
  + `y`: Yank (copy)
  + `c`: Change
  + `<`, `>`, `=`: Indent (right, left, autoindent)
  + `g~`: Swap case -> `gU` và `gu` - Uppercase và Lowercase

- Form: `number + operator + motion` or `number + operator + motion`

### IV. Reference

1. [Vim cheatsheet](https://devhints.io/vim)
2. [vimtutor](http://www2.geog.ucl.ac.uk/~plewis/teaching/unix/vimtutor)