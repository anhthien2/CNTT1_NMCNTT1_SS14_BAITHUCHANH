# Chức năng 1
- Tạo thư mục gốc
```powershell
mkdir smartfarm-system/docs,smartfarm-system/scripts,smartfarm-system/data,smartfarm-system/backup -Force
```

- Tạo cấu hình file .env
```
cd smartfarm-system/data
(Get-Item ..\.env).Attributes += 'Hidden'
```
- Đặt thuộc tínhHidden
# Robot lọc log tự động
- Tạo file
```
# AUDIT: xác nhận đã thẩm định script
$logFile = "..\data\log-he-thong.log"
$reportFile = "..\backup\bao-cao-su-co.txt"

# Tao tieu de bao cao - ghi de file cu
Write-Output "BAO CAO SU CO HE THONG" | Set-Content -Path $reportFile
Write-Output "Ngay gio bao cao: $(Get-Date)" >> $reportFile
Write-Output "------------------------------" >> $reportFile
Write-Output "Danh sach su co CRITICAL va ERROR:" >> $reportFile

# Pipeline loc log va chen noi vao bao cao
Get-Content -Path $logFile | Select-String -Pattern "CRITICAL|ERROR" >> $reportFile

# Pipeline dem so dong loi
$count = (Get-Content -Path $logFile | Select-String -Pattern "CRITICAL|ERROR" | Measure-Object).Count
Write-Output "------------------------------" >> $reportFile
Write-Output "TONG SO SU CO: $count" >> $reportFile
```
- chạy thư mục scripts
# Chức năng 3: excel
**Cột E - Đặt tên là** **`ma_linh_kien_sach`** **(ở E1):**  
Tại E2 gõ:

```
=TRIM(B2)
```
**Cột F - Đặt tên là** **`ten_thiet_bi`** **(ở F1):**
```
=VLOOKUP(E2,'danh-muc-linh-kien'!$A$2:$D$100,2,FALSE)
```

# Chức năng 4 : tài liệu .md
```
# SmartFarm System

He thong quan tri trang trai thong minh.

## Huong dan chay script

```powershell
cd smartfarm-system/scripts
powershell -ExecutionPolicy Bypass -File .\auto_report.ps1

