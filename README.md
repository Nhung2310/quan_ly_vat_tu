I.Đề tài Quản lý nhập xuất vật tư: Ta tổ chức các danh sách sau:
- Danh sách Vattu : cây nhị phân tìm kiếm ( MAVT (C10), TENVT, DVT, Số
lượng tồn)
- Danh sách Nhanvien : danh sách tuyến tính là 1 mảng con trỏ (MANV, HO,
TEN,
PHAI, con trỏ dshd): con trỏ dshd sẽ trỏ đến danh sách các hóa đơn mà nhân viên
đó đã
lập. Danh sách nhân viên có tối đa 500 nhân viên
- Danh sách HOADON : danh sách liên kết đơn(SoHD (C20), Ngày lập hóa đơn,
Loai,
con trỏ cthd). Lọai chỉ nhận ‘N’ (phiếu nhập) hoặc ‘X’ (phiếu xuất); con trỏ cthd sẻ
trỏ
đến danh sách chi tiết các vật tư của hóa đơn đó.
- Danh sách CT_HOADON : danh sách liên kết đơn ( MAVT, Soluong, Dongia,
%VAT).
Chương trình có các chức năng sau:
a/ Nhập vật tư : cho phép cập nhật (thêm / xóa / hiệu chỉnh ) thông tin của vật tư;
riêng số
lượng tồn chỉ cho phép nhập khi đó là vật tư mới.
b/ In danh sách vật tư tồn kho : liệt kê ds vật tư ra màn hình theo thứ tự tên vật tư
tăng dần.
Kết xuất : Mã VT Tên vật tư Đơn vị tính Số lượng tồn
c/ Nhập nhân viên: Cập nhập các nhân viên dựa vào mã nhân viên, họ, tên không
được rỗng.
d/ In danh sách nhân viên theo thứ tự tên nhân viên tăng dần, nếu trùng tên thì tăng
dần theo họ ra màn hình, 1 nhân viên / dòng

e/ Lập hóa đơn nhập/Lập hóa đơn xuất: nhập vào số hóa đơn, ngày lập, loại (chỉ
nhận ký tự N hoặc X). Sau đó, tiếp tục cho phép nhập các vật tư của hóa đơn đó;
Căn cứ vào loại hóa đơn,chương trình sẽ tự động cập nhật số lượng tồn.
Lưu ý: - Nếu số lượng xuất không đủ hàng thì báo lỗi và in ra số lượng tồn hiện có
trong kho;
- Chỉ được phép xóa vật tư đang lập của hóa đơn hiện tại. Khi hóa đơn đã ghi thì
không được xóa các vật tư trog hóa đơn
f/ In hóa đơn : In hóa đơn dựa vào số hóa đơn do ta nhập vào
g/ Thống kê các hóa đơn trong 1 khỏang thời gian: nhập vào 2 thời điểm từ ngày ,
đến ngày,chương trình sẽ in ra các hóa đơn được lập trong khoảng thời gian như
trên. Kết xuất:
BẢNG LIỆT KÊ CÁC HÓA ĐƠN TRONG KHOẢNG THỜI GIAN
Từ ngày : ##/##/#### Đến ngày : ##/##/####
Số HĐ Ngày lập Loại HĐ Họ tên NV lập Trị giá hóa đơn
h/ In 10 vật tư có doanh thu cao nhất trong 1 khoảng thời gian.
II.Cấu trúc quản lý vật tư

using DO_AN;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Xml.Linq;
using static System.Net.Mime.MediaTypeNames;
namespace DO_AN {

class Vattu {
//sử dụng các từ khóa public để khai báo các thuộc tính và phương thức
//là public để có thể truy cập từ bất kỳ nơi nào trong chương trình.
public string MAVT;
public string TENVT;
public string DVT;
public int SoLuongTon;
public Vattu left;
public Vattu right;
public Vattu(string mAVT, string tENVT, string dVT, int soLuongTon)
{
MAVT = mAVT;
TENVT = tENVT;
DVT = dVT;
SoLuongTon = soLuongTon;
left = null;
right = null;
}
}
class QLVT
{
public Vattu root; // nút gốc của cây nhị phân tìm kiếm danh sách vật tư
// Hàm khởi tạo, tạo một cây rỗng
public QLVT()
{
root = null;
}
}
Cấu trúc này định nghĩa hai lớp: `Vattu` và `QLVT`. Dưới đây là mô tả chi tiết về
cấu trúc này:

1. Lớp `Vattu`:
- Các thuộc tính:
- `MAVT` (kiểu `string`): Mã vật tư.
- `TENVT` (kiểu `string`): Tên vật tư.
- `DVT` (kiểu `string`): Đơn vị tính của vật tư.
- `SoLuongTon` (kiểu `int`): Số lượng tồn của vật tư.
- `left` (kiểu `Vattu`): Tham chiếu đến nút con trái của cây nhị phân.
- `right` (kiểu `Vattu`): Tham chiếu đến nút con phải của cây nhị phân.
- Phương thức:
- `Vattu`: Phương thức khởi tạo của lớp `Vattu`, nhận các tham số để khởi tạo
các thuộc tính của vật tư.
2. Lớp `QLVT`:
- Thuộc tính:
- `root` (kiểu `Vattu`): Tham chiếu đến nút gốc của cây nhị phân tìm kiếm danh
sách vật tư.
- Phương thức:
- `QLVT`: Phương thức khởi tạo của lớp `QLVT`, tạo một cây rỗng (thiết lập
`root` là `null`).
Cấu trúc này định nghĩa một cây nhị phân tìm kiếm, trong đó mỗi nút của cây
(`Vattu`) chứa thông tin về một vật tư, và các vật tư được sắp xếp trong cây theo
thứ tự mã vật tư. Lớp `QLVT` là một giao diện để quản lý cây nhị phân tìm kiếm
danh sách vật tư, và nút gốc (`root`) của cây được sử dụng để tham chiếu đến cây
đó.

class Nhanvien
{
public string MANV;
public string HO;
public string TEN;
public string PHAI;
public Hoadon_List DS_HOADON;
public Nhanvien(string ma, string ho, string ten, string phai)
{
MANV = ma;
HO = ho;
TEN = ten;
PHAI = phai;
DS_HOADON = new Hoadon_List(); // Khởi tạo danh sách hóa đơn
}

}
class DSNhanVien
{
public Nhanvien[] danhSachNV;
public int soLuongNV;
public DSNhanVien()
{
danhSachNV = new Nhanvien[500];
soLuongNV = 0;
}
}

Cấu trúc này định nghĩa hai lớp: `Nhanvien` và `DSNhanVien`. Dưới đây là mô tả
chi tiết về cấu trúc này:
1. Lớp `Nhanvien`:
- Các thuộc tính:
- `MANV` (kiểu `string`): Mã nhân viên.
- `HO` (kiểu `string`): Họ của nhân viên.
- `TEN` (kiểu `string`): Tên của nhân viên.
- `PHAI` (kiểu `string`): Giới tính của nhân viên.
- `DS_HOADON` (kiểu `Hoadon_List`): Danh sách các hóa đơn liên quan đến
nhân viên.
- Phương thức:
- `Nhanvien`: Phương thức khởi tạo của lớp `Nhanvien`, nhận các tham số để
khởi tạo các thuộc tính của nhân viên. Trong phương thức này, danh sách hóa đơn
(`DS_HOADON`) cũng được khởi tạo.
2. Lớp `DSNhanVien`:
- Thuộc tính:
- `danhSachNV` (kiểu `Nhanvien[]`): Mảng chứa danh sách các nhân viên.
- `soLuongNV` (kiểu `int`): Số lượng nhân viên trong danh sách.
- Phương thức:
- `DSNhanVien`: Phương thức khởi tạo của lớp `DSNhanVien`, khởi tạo một
danh sách nhân viên rỗng với kích thước tối đa là 500.
Cấu trúc này cho phép lưu trữ thông tin về một nhân viên và các hóa đơn liên quan
đến nhân viên đó. Lớp `DSNhanVien` là một danh sách các nhân viên, trong đó
mỗi nhân viên được biểu diễn bởi lớp `Nhanvien`. Sử dụng cấu trúc này, bạn có
thể quản lý và thao tác với danh sách các nhân viên và các thông tin liên quan đến
họ.

class Hoadon
{
public string SoHD; // Số hóa đơn
public DateTime NgayLapHD; // Ngày lập hóa đơn
public char Loai; // Loại hóa đơn (N: nhập, X: xuất)
public Ct_Hoadon_List cthd; // Danh sách liên kết đơn các chi tiết hóa
đơn
public Hoadon next; // Con trỏ next trỏ đến phần tử kế tiếp của danh
sách
public Hoadon(string sohd, DateTime ngaylap, char loai)
{
SoHD = sohd;
NgayLapHD = ngaylap;
Loai = loai;
cthd = new Ct_Hoadon_List(); // Khởi tạo danh sách chi tiết hóa đơn
next = null;
}
}

class Hoadon_List
{
public Hoadon head;
public Hoadon tail;
public Hoadon_List()
{
head = null;
tail = null;
}}
Cấu trúc này định nghĩa hai lớp: `Hoadon` và `Hoadon_List`. Dưới đây là mô tả
chi tiết về cấu trúc này:

1. Lớp `Hoadon`:
- Các thuộc tính:
- `SoHD` (kiểu `string`): Số hóa đơn.
- `NgayLapHD` (kiểu `DateTime`): Ngày lập hóa đơn.
- `Loai` (kiểu `char`): Loại hóa đơn (N: nhập, X: xuất).
- `cthd` (kiểu `Ct_Hoadon_List`): Danh sách các chi tiết hóa đơn (liên kết đơn).
- `next` (kiểu `Hoadon`): Con trỏ `next` trỏ đến phần tử kế tiếp trong danh sách
hóa đơn.
- Phương thức:
- `Hoadon`: Phương thức khởi tạo của lớp `Hoadon`, nhận các tham số để khởi
tạo các thuộc tính của hóa đơn. Trong phương thức này, danh sách các chi tiết hóa
đơn (`cthd`) cũng được khởi tạo.
2. Lớp `Hoadon_List`:
- Thuộc tính:
- `head` (kiểu `Hoadon`): Con trỏ đến phần tử đầu tiên trong danh sách hóa đơn.
- `tail` (kiểu `Hoadon`): Con trỏ đến phần tử cuối cùng trong danh sách hóa
đơn.
- Phương thức:
- `Hoadon_List`: Phương thức khởi tạo của lớp `Hoadon_List`, khởi tạo một
danh sách hóa đơn rỗng, trong đó `head` và `tail` được đặt là `null`.
Cấu trúc này cho phép lưu trữ thông tin về một hóa đơn, bao gồm số hóa đơn, ngày
lập hóa đơn, loại hóa đơn (nhập hoặc xuất), và danh sách các chi tiết hóa đơn. Lớp
`Hoadon_List` là một danh sách các hóa đơn, trong đó mỗi hóa đơn được biểu diễn
bởi lớp `Hoadon`. Sử dụng cấu trúc này, bạn có thể quản lý và thao tác với danh
sách các hóa đơn và các thông tin liên quan đến chúng.

class Ct_Hoadon
{
public string MAVT; // Mã vật tư
public int Soluong; // Số lượng
public double Dongia; // Đơn giá

public double VAT; // %VAT
public Ct_Hoadon next; // Con trỏ next trỏ đến phần tử kế tiếp của danh
sách

public Ct_Hoadon(string mavt, int soluong, double dongia, double vat)
{
MAVT = mavt;
Soluong = soluong;
Dongia = dongia;
VAT = vat;
next = null;
} }
class Ct_Hoadon_List
{
public Ct_Hoadon head;
public Ct_Hoadon tail;
public Ct_Hoadon_List()
{
head = null;
tail = null;
}
}

Cấu trúc này định nghĩa hai lớp: `Ct_Hoadon` và `Ct_Hoadon_List`. Dưới đây là
mô tả chi tiết về cấu trúc này:
1. Lớp `Ct_Hoadon`:
- Các thuộc tính:
- `MAVT` (kiểu `string`): Mã vật tư.
- `Soluong` (kiểu `int`): Số lượng vật tư.

- `Dongia` (kiểu `double`): Đơn giá vật tư.
- `VAT` (kiểu `double`): Phần trăm VAT.
- `next` (kiểu `Ct_Hoadon`): Con trỏ `next` trỏ đến phần tử kế tiếp trong danh
sách chi tiết hóa đơn.
- Phương thức:
- `Ct_Hoadon`: Phương thức khởi tạo của lớp `Ct_Hoadon`, nhận các tham số
để khởi tạo các thuộc tính của chi tiết hóa đơn.
2. Lớp `Ct_Hoadon_List`:
- Thuộc tính:
- `head` (kiểu `Ct_Hoadon`): Con trỏ đến phần tử đầu tiên trong danh sách chi
tiết hóa đơn.
- `tail` (kiểu `Ct_Hoadon`): Con trỏ đến phần tử cuối cùng trong danh sách chi
tiết hóa đơn.
- Phương thức:
- `Ct_Hoadon_List`: Phương thức khởi tạo của lớp `Ct_Hoadon_List`, khởi tạo
một danh sách chi tiết hóa đơn rỗng, trong đó `head` và `tail` được đặt là `null`.
Cấu trúc này cho phép lưu trữ thông tin về chi tiết của một hóa đơn, bao gồm mã
vật tư, số lượng, đơn giá và phần trăm VAT. Lớp `Ct_Hoadon_List` là một danh
sách các chi tiết hóa đơn, trong đó mỗi chi tiết được biểu diễn bởi lớp
`Ct_Hoadon`. Sử dụng cấu trúc này, bạn có thể quản lý và thao tác với danh sách
chi tiết hóa đơn và các thông tin liên quan đến chúng.
