# [MODULE-2] Audit Module 2 - Database

##Nội dung câu hỏi và phần trả lời

## [ Câu hỏi ] 1.  What is outer join, left join, inner join?
```
- Full outer join: Return all records from two tables. 
- Right outer join: Preserves the unmatched rows from first table (right), joining with Null rows in the shape of the second table (left). 
- Left outer join: Preserves the unmatched rows from first table (left), joining with Null rows in the shape of the second table (right).
```
## [ Câu hỏi ] 2. What is index? When you should use index?
```
- Data structure created in DB for finding data more quickly and efficiently. 
```
## [ Câu hỏi ] 3. What is transaction?  Why do you use transaction?
```
- A unit of work, performed against DB. 
- Two main purposes: 
o Allow correct recovery from failures and keep DB consistent even in cases of system failure. 
o Provide isolation btw programs accessing DB concurrently. 
```
## [ Câu hỏi ] 4. What is ACID? 
```
- Four main features of transaction: Atomicity, Consistency, Isolation, Durability. 
- Atomicity: Ensures that the entire sequence of operations is successfully or not. 
- Consistency: Ensures that the database properly changes states upon a successfully committed transaction.
- Isolation: Enables transactions to operate independently of and transparent to each other.
- Durability: Ensures that the result or effect of a committed transaction persists in case of a system failure.
```
## [ Câu hỏi ] 5. Can you explain database partitioning?
```
- A division of a logical DB or its constituting elements into distinct independent parts. 
- Purposes: 
o For manageability
o Performance
- Three common criteria to split DB: 
o Range partitioning (use certain range)
o List partitioning (assign a list of values)
o Hash partitioning (use value of a hash function)
```
## [ Câu hỏi ] 6. What is the difference between DELETE and TRUNCATE?
```
- DELETE: 
o Deletion of each row or the whole table gets logged and physically deleted.
o Could have condition, triggers
o Need to commit the changes
- TRUNCATE: 
o Log the de-allocation of the data pages in which the data exists. 
o No need the commit statement. 
o Don’t have condition, triggers. 
```
## [ Câu hỏi ] 7. When to use group by clause?
```
- Group similar data```
## [ Câu hỏi ] 8. What is the different between Having and Where clause?
```
- Having: conditions used with group by.
- Where: conditions used with SELECT.```
## [ Câu hỏi ] 9. What is a Sub Query?
```
- A query nested inside a SELECT statements
- Alternative to complex join statements. 
```
## [ Câu hỏi ] 10. What is a View?
```
- A virtual table created on basis of the result set returned by the select statement. 
- Increase performance and security
```
## [ Câu hỏi ] 11. How to find how many duplicated record in a table?
```
- Step1: count all distinct records in table. 
- Step2: count all records in that table
- Step3: The result: the result in step2 subtracts to the result in step 1. 
```
## [ Câu hỏi ] 12. How to count a number of records in a table. 
```
- Use count(*) from Table. ```
## [ Câu hỏi ] 13. What is composite key? 
```
- One primary key consists of two tables. ```
## [ Câu hỏi ] 14. Select cơ bản
```
-- xem toàn bộ thông tin từ bảng nhan_vien
select * from nhan_vien 
```
## [ Câu hỏi ] 15. Delete cơ bản
```
-- xóa toàn bộ thông tin của bảng nhan_vien
delete from nhan_vien 
```
## [ Câu hỏi ] 16.Update có whereSelect có điều kiện AND, ORSelect có where và LIKE %
```
-- update lại thông tin địa chỉ Quảng Nam thành Đà Nẵng của nhân viên có tên Hoàng, Nam (bảng nhan_vien)
update nhan_vien set dia_chi = 'Đà Nẵng' where (ten_nhan_vien = '%Hoang' or ten_nhan_vien = '%Nam') and dia_chi = 'Quảng Nam'
```
## [ Câu hỏi ] 17. Select có ORDER BY
```
-- xem toàn bộ thông tin từ bảng nhan_vien sắp xếp theo thứ thự tên nhân viên theo bảng chữ cái alphabet
select * from nhan_vien order by ten_nhan_vien ASC
-- xem toàn bộ thông tin từ bảng nhan_vien sắp xếp theo thứ thự tuổi giảm dần
select * from nhan_vien order by ngay_sinh DESC
```
## [ Câu hỏi ] 18. Select có GROUP BY và HAVING
```
-- hiển thị thông tin thống kê theo dân tộc mà có số lượng lớn hơn 50
select dan_toc, count(ma_nhan_vien) as so_luong from nhan_vien group by dan_toc having so_luong  > 50
```
## [ Câu hỏi ] 19. Select có DISTINCT
```
-- Xem danh sách các tỉnh thành (địa chỉ) không trùng nhau của nhân viên
select distinct dia_chi from nhan_vien
```
## [ Câu hỏi ] 20. Select có INNER JOIN, Select có UNION
```
-- Hiển thị toàn bộ thông tin loại dịch vụ của các dịch vụ (inner join loai_dich_vu,dich_vu)
select * from loai_dich_vu inner join dich_vu on dich_vu .id_dich_vu = loai_dich_vu .id_dich_vu 
-- Hiển thị thông tin của tất cả các Nhân viên và Khách hàng có trong hệ thống, thông tin hiển thị bao gồm ID (IDNhanVien, IDKhachHang), HoTen, Email, SoDienThoai, NgaySinh, DiaChi
select nhan_vien.id_nhan_vien, nhan_vien.ho_ten , nhan_vien.email, nhan_vien.so_dt, nhan_vien.ngay_sinh, nhan_vien.dia_chi, nhan_vien
from nhan_vien
union all
select khach_hang.id_khach_hang, khach_hang.ho_ten, khach_hang.email, khach_hang.so_dt, khach_hang.ngay_sinh, khach_hang.dia_chi, khach_hang
from khach_hang;
```
## [ Câu hỏi ] 21. Select có NOT IN, NOT EXISTS
```
-- hiển thị thông tin nhân viên có địa chỉ là Đà Nẵng , Quảng Nam, Huế
select * from nhan_vien where dia_chi in ('Đà Nẵng ', 'Quảng Nam', 'Huế')
-- hiển thị danh sách nhân viên chưa có hợp đồng
select * from nhan_vien where not exists (select id_nhan_vien from hop_dong where hop_dong.id_nhan_vien = nhan_vien.id_nhan_vien);
```
## [ Câu hỏi ] 22.Select hỗn hợp phức tạp
```
-- Hiển thi thông tin của tất cả nhân viên bao gồm IDNhanVien, HoTen, TrinhDo, TenBoPhan, SoDienThoai, DiaChi mới chỉ lập được tối đa 3 hợp đồng từ năm 2018 đến 2019
select nhan_vien.id_nhan_vien, nhan_vien.ho_ten, nhan_vien.trinh_do, nhan_vien.so_dt, count(hop_dong.id_nhan_vien) as so_luong_hop_dong
from nhan_vien
inner join hop_dong on nhan_vien.id_nhan_vien = hop_dong.id_nhan_vien
where hop_dong.ngay_bat_dau between 2018-01-01 and 2019-12-31
group by nhan_vien.id_nhan_vien
having so_luong_hop_dong <=3;
```



## Mục tiêu

* Audit module 2 Database

## Youtube Link

* [[CaseStudy] Query database](https://www.youtube.com/watch?v=obM5UMPGTqc&list=PL9yu4ScFhKGwH2rZGi2CZGrt2DSRccUXe)
* [[Database] Design ERD]()
* [[Database] Normalization](https://www.youtube.com/watch?v=rMJu2gTawz0&list=PL9yu4ScFhKGyc-L5a6n0cbfe8X4J3cmS_&index=2&t=0s)
