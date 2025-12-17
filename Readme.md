# Báo cáo audit ánh xạ kiểm thử đơn vị ↔ thiết kế (Pd)

- Dự án: máy thở Humming Vue R2 – mô-đun Pd (Patient Data)
- Phạm vi audit: ALL
- Thời điểm: 2025-12-17T15:20:05
- Tài liệu đầu vào được audit (nguồn sự thật):
  - UTS: `Pd/Docs/json/HVR2-MAIN-PD-REPT-01-00-Humming-Vue-R2-Patient-Data-Unit-Test-Specification.json`
  - SDD: `Pd/Docs/json/HVR2-MAIN-PD-DESI-01-00-Humming-Vue-R2-Patient-Data-Software-Design.json`
  - SAS: `Pd/Docs/json/HVR2-MAIN-PD-ARCH-01-00-Humming-Vue-R2-Patient-Data-Software-Architecture.json`
  - SRS: `Pd/Docs/json/HVR2-MAIN-PD-SPEC-01-00-Humming-Vue-R2-Patient-Data-Software-Requirement-Specification_CORRECTED.json`
  - Thư mục mã nguồn Pd: `Pd`
  - Thư mục kiểm thử đơn vị Pd: `Test/Pd`

Lưu ý: Các định danh (DesignUnitID/TestCaseID), tên hàm/lớp, đường dẫn file, và trạng thái (OK/MISMATCH/AMBIGUOUS/MISSING/TBD) được giữ nguyên theo chuẩn audit; nội dung diễn giải được chuyển sang tiếng Việt.

## Tóm tắt điều hành

- Số Design Unit trong phạm vi (từ UTS): 13
- Thống kê trạng thái: OK=0, MISMATCH=6, MISSING=0, AMBIGUOUS=7, TBD=0
- Thống kê phát hiện: MAJOR=14, MINOR=13, OBSERVATION=1

## Phương pháp (chỉ dựa trên bằng chứng)

- Phân tích UTS/SDD theo Pandoc JSON AST; trích xuất Design Unit theo mẫu tiêu đề bảng (`Unit ID` trong UTS, `ID:` trong SDD).
- Trích xuất đường dẫn file tham chiếu bằng biểu thức chính quy (regex) cho chuỗi `Pd/...` và `Test/Pd/...` trong các bảng.
- Quét `Test/Pd` để đếm GoogleTest macro (`TEST`, `TEST_F`, `TEST_P`) sau khi loại bỏ chú thích theo cố gắng tối đa (giữ số dòng).
- Tìm kiếm từ khóa hàm (định danh đầy đủ/qualified) từ UTS/SDD trong các file nguồn mà UTS liệt kê; nếu không thấy thì tìm tiếp trong `Pd` để phát hiện lệch baseline tài liệu↔kho mã (cố gắng tối đa khớp chuỗi con; không biên dịch/ctags).
- Không chạy kiểm thử đơn vị; không đưa ra kết luận đậu/rớt hoặc độ bao phủ.

## Thống kê ca kiểm thử (kiểm thử đơn vị Pd)

### A) Tổng quan kho mã (quét `Test/Pd`)

- Tổng số file kiểm thử đơn vị đã quét: 14
- Tổng số ca kiểm thử tìm được (TEST/TEST_F/TEST_P): 400
- Tổng số INSTANTIATE_TEST_*_P phát hiện được: 0

### B) Mức độ ánh xạ (UTS → Design Unit)

- Tổng số Design Unit trong phạm vi: 13
- Số unit có ≥1 ca kiểm thử được ánh xạ (mức file): 13
- Số unit có 0 ca kiểm thử được ánh xạ: (không)
- Tổng số ca kiểm thử được ánh xạ (không đếm trùng; mức file): 399
- Tổng số ca kiểm thử bị mơ hồ do dùng chung file: 399
- Tổng số ca kiểm thử chưa được ánh xạ (có trong kho mã nhưng UTS không tham chiếu): 1
  - Trong các file test mà UTS tham chiếu (mức ca kiểm thử, ánh xạ theo từ khóa, cố gắng tối đa):
    - Map mạnh (chỉ khớp đúng 1 Design Unit): 252
    - Mơ hồ (khớp ≥2 Design Unit): 20
    - Chưa gán (khớp 0 Design Unit): 127
- File test chưa được ánh xạ nhiều nhất theo số ca kiểm thử: Test/Pd/PtDataTest/PtDataTest (copy).cpp(1)

### C) Thống kê theo từng Design Unit

| DesignUnitID | File test theo UTS | Số ca kiểm thử trong kho mã | Số ánh xạ mức file | Số ánh xạ mạnh | Số khớp mơ hồ | Số chưa gán (trong file UTS tham chiếu) | Ghi chú |
|---|---|---:|---:|---:|---:|---:|---|
| DES-MAIN-PD-AL01 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 10 | 0 | 26 | Dùng chung |
| DES-MAIN-PD-AL02 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 22 | 0 | 26 | Dùng chung |
| DES-MAIN-PD-AL03 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 112 | 0 | 26 | Dùng chung |
| DES-MAIN-PD-AL04 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 20 | 0 | 26 | Dùng chung |
| DES-MAIN-PD-PE01 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 3 | 4 | 92 | Dùng chung |
| DES-MAIN-PD-PE02 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 4 | 1 | 92 | Dùng chung |
| DES-MAIN-PD-PE03 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 54 | 0 | 92 | Dùng chung |
| DES-MAIN-PD-PE04 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 21 | 5 | 92 | Dùng chung |
| DES-MAIN-PD-PE05 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 0 | 0 | 92 | Dùng chung |
| DES-MAIN-PD-PT01 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 0 | 15 | 9 | Dùng chung |
| DES-MAIN-PD-PT02 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 6 | 0 | 9 | Dùng chung |
| DES-MAIN-PD-PT03 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 0 | 15 | 9 | Dùng chung |
| DES-MAIN-PD-PT04 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 0 | 2 | 9 | Dùng chung |

### D) 10 file test có nhiều ca kiểm thử nhất (Phụ lục)

| File test | Số ca kiểm thử | Ví dụ tên test |
|---|---:|---|
| Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately |
| Test/Pd/PtDataTest/PtDataTest.cpp | 179 | PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory, PtDataFixture.Sanity_PdSpontaneousMapping, PtDataFixture.DirectIndicatorSpontaneous_Increments, PtDataFixture.PlateauWithSpontaneousBackup_Increments, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly |
| Test/Pd/PdTask/PdTaskTest.cpp | 30 | PdTaskFixture.Smoke_ConstructsAndDestroys, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative |
| Test/Pd/PtDataTest/PtDataTest (copy).cpp | 1 | PtDataFixture.Harness_Boots |
| Test/Pd/PdAlarmsTest/AlarmGlobals.cpp | 0 |  |
| Test/Pd/PdAlarmsTest/main.cpp | 0 |  |
| Test/Pd/PdTask/ptdata_modemgr_stubs.cpp | 0 |  |
| Test/Pd/PdTest.cpp | 0 |  |
| Test/Pd/PtDataTest/globals.cpp | 0 |  |
| Test/Pd/PtDataTest/main.cpp | 0 |  |

## Bảng phát hiện

| FindingID | Mức độ | DesignUnitID | Vấn đề | Bằng chứng | Khắc phục đề xuất |
|---|---|---|---|---|---|
| F-001 | MAJOR | N/A | Kho mã có file kiểm thử đơn vị Pd không được UTS tham chiếu (test mồ côi/không truy vết được). | Kết quả quét kho mã: 1 file mồ côi dưới Test/Pd; top: Test/Pd/PtDataTest/PtDataTest (copy).cpp(1) | Cập nhật UTS để tham chiếu các file test này và map chúng tới Design Unit, hoặc loại bỏ/di chuyển nếu nằm ngoài phạm vi. |
| F-002 | MAJOR | MULTI | Cùng một file kiểm thử đơn vị được nhiều Design Unit tham chiếu; mapping mơ hồ ở mức ca kiểm thử. | File UTS tham chiếu `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` được map tới DesignUnitID: DES-MAIN-PD-AL01, DES-MAIN-PD-AL02, DES-MAIN-PD-AL03, DES-MAIN-PD-AL04. | Thêm trace tag theo từng test trong mã nguồn (ví dụ chú thích chứa UTS ca kiểm thử ID) hoặc tách test theo từng Design Unit và cập nhật UTS tương ứng. |
| F-003 | MAJOR | MULTI | Cùng một file kiểm thử đơn vị được nhiều Design Unit tham chiếu; mapping mơ hồ ở mức ca kiểm thử. | File UTS tham chiếu `Test/Pd/PdTask/PdTaskTest.cpp` được map tới DesignUnitID: DES-MAIN-PD-PT01, DES-MAIN-PD-PT02, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04. | Thêm trace tag theo từng test trong mã nguồn (ví dụ chú thích chứa UTS ca kiểm thử ID) hoặc tách test theo từng Design Unit và cập nhật UTS tương ứng. |
| F-004 | MAJOR | MULTI | Cùng một file kiểm thử đơn vị được nhiều Design Unit tham chiếu; mapping mơ hồ ở mức ca kiểm thử. | File UTS tham chiếu `Test/Pd/PtDataTest/PtDataTest.cpp` được map tới DesignUnitID: DES-MAIN-PD-PE01, DES-MAIN-PD-PE02, DES-MAIN-PD-PE03, DES-MAIN-PD-PE04, DES-MAIN-PD-PE05. | Thêm trace tag theo từng test trong mã nguồn (ví dụ chú thích chứa UTS ca kiểm thử ID) hoặc tách test theo từng Design Unit và cập nhật UTS tương ứng. |
| F-005 | MAJOR | DES-MAIN-PD-PE01 | (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file). | Chứng cứ UTS (file nguồn): UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[99]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::PdSpontaneous:MISMATCH(tên tương tự 'PdSpontaneous' tìm thấy Pd/ptdata.cpp:307, không tìm thấy symbol dạng qualified);Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::CompExhTidalVolumeInAPRV:Pd/ptdata.cpp:385 | Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó. |
| F-006 | MAJOR | DES-MAIN-PD-PE03 | (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file). | Chứng cứ UTS (file nguồn): UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[109]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::MaxVteArray:MISMATCH(tên tương tự 'MaxVteArray' tìm thấy Pd/ptdata.cpp:3068, không tìm thấy symbol dạng qualified);Ptdata::MaxVteArray12:MISMATCH(tên tương tự 'MaxVteArray12' tìm thấy Pd/ptdata.cpp:3113, không tìm thấy symbol dạng qualified);Ptdata::MinVteArray:MISMATCH(tên tương tự 'MinVteArray' tìm thấy Pd/ptdata.cpp:3056, không tìm thấy symbol dạng qualified);Ptdata::MinVteArray12:MISMATCH(tên tương tự 'MinVteArray12' tìm thấy Pd/ptdata.cpp:3112, không tìm thấy symbol dạng qualified);Ptdata::BlankingData:Pd/ptdata.cpp:2624;Ptdata::ClearExhLeakArray:Pd/ptdata.cpp:3753;Ptdata::ClearVteBuffer:Pd/ptdata.cpp:3338 | Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó. |
| F-007 | MAJOR | DES-MAIN-PD-PE05 | (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file). | Chứng cứ UTS (file nguồn): UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[115]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: PdTask::TestLogFileWriting:MISMATCH(tìm thấy Pd/pdtask.cpp:206 không nằm trong danh sách SourceFile_UTS của UTS) | Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó. |
| F-008 | MAJOR | DES-MAIN-PD-PT01 | (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file). | Chứng cứ UTS (file nguồn): UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[86]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::ClearHistory:MISMATCH(tìm thấy Pd/ptdata.cpp:2236 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::TestLogFileWriting:Pd/pdtask.cpp:206;PdTask::run:Pd/pdtask.cpp:42 | Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó. |
| F-009 | MAJOR | DES-MAIN-PD-PT02 | (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file). | Chứng cứ UTS (file nguồn): UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[89]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: PdTask::S_SetEventFlag:MISMATCH(tìm thấy Pd/pdtask.h:115 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::initMsgQueue:Pd/pdtask.cpp:226 | Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó. |
| F-010 | MAJOR | DES-MAIN-PD-PT03 | (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file). | Chứng cứ UTS (file nguồn): UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[93]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::ClearHistory:MISMATCH(tìm thấy Pd/ptdata.cpp:2236 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::run:Pd/pdtask.cpp:42 | Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó. |
| F-011 | MAJOR | DES-MAIN-PD-PE05 | Không có ca kiểm thử kiểm thử đơn vị nào được map theo bằng chứng cho Design Unit này (0 mạnh, 0 mơ hồ) trong (các) file test mà UTS tham chiếu. | UTS TestFile_UTS=Test/Pd/PtDataTest/PtDataTest.cpp; RepoTestCaseCount_ByTestFile=179; StrongMapped=0; AmbiguousMatched=0. | Cập nhật UTS để tham chiếu đúng (các) file test và/hoặc bổ sung kiểm thử đơn vị bao phủ các hàm của Design Unit; thêm trace tag rõ ràng theo từng test để liên kết UTS ca kiểm thử ID ↔ gtest case. |
| F-012 | MAJOR | DES-MAIN-PD-PT01 | Tất cả các ca kiểm thử khớp được đều ở trạng thái mơ hồ; không có kiểm thử đơn vị nào truy vết duy nhất cho Design Unit này. | UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15. | Thêm trace tag theo từng test (UTS ca kiểm thử ID trong mã nguồn) và/hoặc tách file test dùng chung theo từng Design Unit; sau đó cập nhật UTS để gỡ mơ hồ mapping ở mức ca kiểm thử. |
| F-013 | MAJOR | DES-MAIN-PD-PT03 | Tất cả các ca kiểm thử khớp được đều ở trạng thái mơ hồ; không có kiểm thử đơn vị nào truy vết duy nhất cho Design Unit này. | UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15. | Thêm trace tag theo từng test (UTS ca kiểm thử ID trong mã nguồn) và/hoặc tách file test dùng chung theo từng Design Unit; sau đó cập nhật UTS để gỡ mơ hồ mapping ở mức ca kiểm thử. |
| F-014 | MAJOR | DES-MAIN-PD-PT04 | Tất cả các ca kiểm thử khớp được đều ở trạng thái mơ hồ; không có kiểm thử đơn vị nào truy vết duy nhất cho Design Unit này. | UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=2. | Thêm trace tag theo từng test (UTS ca kiểm thử ID trong mã nguồn) và/hoặc tách file test dùng chung theo từng Design Unit; sau đó cập nhật UTS để gỡ mơ hồ mapping ở mức ca kiểm thử. |
| F-015 | MINOR | DES-MAIN-PD-AL01 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdAlarms - ThresholdChecks' so với SDD Unit='ThresholdChecks'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-016 | MINOR | DES-MAIN-PD-AL02 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdAlarms - Debounce' so với SDD Unit='Debounce'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-017 | MINOR | DES-MAIN-PD-AL03 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdAlarms - DwellTimers' so với SDD Unit='DwellTimers'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-018 | MINOR | DES-MAIN-PD-AL04 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdAlarms - AlarmPublisher / O2 Alarms' so với SDD Unit='AlarmPublisher / O2 Alarms'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-019 | MINOR | DES-MAIN-PD-PE01 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='Ptdata - BreathPhaseProcessor' so với SDD Unit='BreathPhaseProcessor'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-020 | MINOR | DES-MAIN-PD-PE02 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='Ptdata - O2Limits' so với SDD Unit='O2Limits'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-021 | MINOR | DES-MAIN-PD-PE03 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='Ptdata - MetricsCalculator' so với SDD Unit='MetricsCalculator'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-022 | MINOR | DES-MAIN-PD-PE04 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='Ptdata - GuiPublisher' so với SDD Unit='GuiPublisher'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-023 | MINOR | DES-MAIN-PD-PE05 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='Ptdata - LogPublisher' so với SDD Unit='LogPublisher'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-024 | MINOR | DES-MAIN-PD-PT01 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdTask - Event Loop Unit' so với SDD Unit='EventLoop'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-025 | MINOR | DES-MAIN-PD-PT02 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdTask - IPC Receive Unit (message gateway)' so với SDD Unit='IPCReceive'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-026 | MINOR | DES-MAIN-PD-PT03 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdTask - Dispatcher Unit' so với SDD Unit='Dispatcher'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-027 | MINOR | DES-MAIN-PD-PT04 | Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID. | UTS Unit='PdTask - Mode Status Unit' so với SDD Unit='ModeStatus'. | Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau). |
| F-028 | OBSERVATION | N/A | UTS tham chiếu tới ma trận truy vết bên ngoài; không thể xác minh truy vết SRS↔UTS chỉ với các đầu vào hiện có. | UTS có chứa chuỗi 'Refer to Traceability matrix'; không phát hiện được ID yêu cầu SRS trực tiếp trong UTS bằng phương pháp tìm chuỗi đơn giản. | Cung cấp tài liệu/bằng chứng ma trận truy vết được tham chiếu và đảm bảo nó được kiểm soát cấu hình cùng baseline với UTS/SDD và phiên bản kho mã. |

## Đề xuất khắc phục chi tiết (theo từng Finding)

### F-001

- Mức độ: MAJOR
- DesignUnitID: N/A
- Vấn đề: Kho mã có file kiểm thử đơn vị Pd không được UTS tham chiếu (test mồ côi/không truy vết được).
- Bằng chứng: Kết quả quét kho mã: 1 file mồ côi dưới Test/Pd; top: Test/Pd/PtDataTest/PtDataTest (copy).cpp(1)
- Hành động khuyến nghị (đủ điều kiện audit):
  - Quyết định xử lý (các) file test mồ côi: đưa vào phạm vi UTS kèm mapping tới Design Unit, hoặc loại bỏ/di chuyển nếu không chủ ý.
  - Đảm bảo kiểm soát cấu hình: baseline UTS phải khớp với phiên bản của kho mã đang chứa các file test.
- Tóm tắt khắc phục: Cập nhật UTS để tham chiếu các file test này và map chúng tới Design Unit, hoặc loại bỏ/di chuyển nếu nằm ngoài phạm vi.

### F-002

- Mức độ: MAJOR
- DesignUnitID: MULTI
- Vấn đề: Cùng một file kiểm thử đơn vị được nhiều Design Unit tham chiếu; mapping mơ hồ ở mức ca kiểm thử.
- Bằng chứng: File UTS tham chiếu `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` được map tới DesignUnitID: DES-MAIN-PD-AL01, DES-MAIN-PD-AL02, DES-MAIN-PD-AL03, DES-MAIN-PD-AL04.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Chọn một: (A) tách file test dùng chung thành các file theo từng Design Unit, hoặc (B) thêm trace tag theo từng test trong mã nguồn tham chiếu UTS ca kiểm thử ID.
  - Cập nhật UTS để map mỗi Design Unit tới (1) file test riêng hoặc (2) tên gtest case cụ thể.
  - Chạy lại audit này để xác nhận số ca kiểm thử được ánh xạ mạnh tăng và mức độ mơ hồ giảm.
- Tóm tắt khắc phục: Thêm trace tag theo từng test trong mã nguồn (ví dụ chú thích chứa UTS ca kiểm thử ID) hoặc tách test theo từng Design Unit và cập nhật UTS tương ứng.

### F-003

- Mức độ: MAJOR
- DesignUnitID: MULTI
- Vấn đề: Cùng một file kiểm thử đơn vị được nhiều Design Unit tham chiếu; mapping mơ hồ ở mức ca kiểm thử.
- Bằng chứng: File UTS tham chiếu `Test/Pd/PdTask/PdTaskTest.cpp` được map tới DesignUnitID: DES-MAIN-PD-PT01, DES-MAIN-PD-PT02, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Chọn một: (A) tách file test dùng chung thành các file theo từng Design Unit, hoặc (B) thêm trace tag theo từng test trong mã nguồn tham chiếu UTS ca kiểm thử ID.
  - Cập nhật UTS để map mỗi Design Unit tới (1) file test riêng hoặc (2) tên gtest case cụ thể.
  - Chạy lại audit này để xác nhận số ca kiểm thử được ánh xạ mạnh tăng và mức độ mơ hồ giảm.
- Tóm tắt khắc phục: Thêm trace tag theo từng test trong mã nguồn (ví dụ chú thích chứa UTS ca kiểm thử ID) hoặc tách test theo từng Design Unit và cập nhật UTS tương ứng.

### F-004

- Mức độ: MAJOR
- DesignUnitID: MULTI
- Vấn đề: Cùng một file kiểm thử đơn vị được nhiều Design Unit tham chiếu; mapping mơ hồ ở mức ca kiểm thử.
- Bằng chứng: File UTS tham chiếu `Test/Pd/PtDataTest/PtDataTest.cpp` được map tới DesignUnitID: DES-MAIN-PD-PE01, DES-MAIN-PD-PE02, DES-MAIN-PD-PE03, DES-MAIN-PD-PE04, DES-MAIN-PD-PE05.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Chọn một: (A) tách file test dùng chung thành các file theo từng Design Unit, hoặc (B) thêm trace tag theo từng test trong mã nguồn tham chiếu UTS ca kiểm thử ID.
  - Cập nhật UTS để map mỗi Design Unit tới (1) file test riêng hoặc (2) tên gtest case cụ thể.
  - Chạy lại audit này để xác nhận số ca kiểm thử được ánh xạ mạnh tăng và mức độ mơ hồ giảm.
- Tóm tắt khắc phục: Thêm trace tag theo từng test trong mã nguồn (ví dụ chú thích chứa UTS ca kiểm thử ID) hoặc tách test theo từng Design Unit và cập nhật UTS tương ứng.

### F-005

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PE01
- Vấn đề: (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file).
- Bằng chứng: Chứng cứ UTS (file nguồn): UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[99]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::PdSpontaneous:MISMATCH(tên tương tự 'PdSpontaneous' tìm thấy Pd/ptdata.cpp:307, không tìm thấy symbol dạng qualified);Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::CompExhTidalVolumeInAPRV:Pd/ptdata.cpp:385
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó.

### F-006

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PE03
- Vấn đề: (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file).
- Bằng chứng: Chứng cứ UTS (file nguồn): UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[109]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::MaxVteArray:MISMATCH(tên tương tự 'MaxVteArray' tìm thấy Pd/ptdata.cpp:3068, không tìm thấy symbol dạng qualified);Ptdata::MaxVteArray12:MISMATCH(tên tương tự 'MaxVteArray12' tìm thấy Pd/ptdata.cpp:3113, không tìm thấy symbol dạng qualified);Ptdata::MinVteArray:MISMATCH(tên tương tự 'MinVteArray' tìm thấy Pd/ptdata.cpp:3056, không tìm thấy symbol dạng qualified);Ptdata::MinVteArray12:MISMATCH(tên tương tự 'MinVteArray12' tìm thấy Pd/ptdata.cpp:3112, không tìm thấy symbol dạng qualified);Ptdata::BlankingData:Pd/ptdata.cpp:2624;Ptdata::ClearExhLeakArray:Pd/ptdata.cpp:3753;Ptdata::ClearVteBuffer:Pd/ptdata.cpp:3338
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó.

### F-007

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PE05
- Vấn đề: (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file).
- Bằng chứng: Chứng cứ UTS (file nguồn): UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[115]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: PdTask::TestLogFileWriting:MISMATCH(tìm thấy Pd/pdtask.cpp:206 không nằm trong danh sách SourceFile_UTS của UTS)
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó.

### F-008

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PT01
- Vấn đề: (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file).
- Bằng chứng: Chứng cứ UTS (file nguồn): UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[86]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::ClearHistory:MISMATCH(tìm thấy Pd/ptdata.cpp:2236 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::TestLogFileWriting:Pd/pdtask.cpp:206;PdTask::run:Pd/pdtask.cpp:42
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó.

### F-009

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PT02
- Vấn đề: (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file).
- Bằng chứng: Chứng cứ UTS (file nguồn): UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[89]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: PdTask::S_SetEventFlag:MISMATCH(tìm thấy Pd/pdtask.h:115 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::initMsgQueue:Pd/pdtask.cpp:226
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó.

### F-010

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PT03
- Vấn đề: (Các) hàm (định danh đầy đủ/qualified) được UTS/SDD tham chiếu không nằm trong (các) file nguồn mà UTS liệt kê, nhưng lại tồn tại ở nơi khác trong Pd (lệch mapping UTS↔kho mã theo file).
- Bằng chứng: Chứng cứ UTS (file nguồn): UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File); Chứng cứ SDD (hàm): SDD:blocks[93]/Table/Body/Row[4]/Col[1] (関数 / Function：); Kết quả tìm trong kho mã: Ptdata::ClearHistory:MISMATCH(tìm thấy Pd/ptdata.cpp:2236 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::run:Pd/pdtask.cpp:42
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Cập nhật danh sách file nguồn trong UTS để bao gồm đúng file triển khai thực tế và/hoặc chỉnh lại danh sách hàm trong SDD; ưu tiên liên kết mỗi UTS ca kiểm thử ID tới gtest cụ thể bao phủ nó.

### F-011

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PE05
- Vấn đề: Không có ca kiểm thử kiểm thử đơn vị nào được map theo bằng chứng cho Design Unit này (0 mạnh, 0 mơ hồ) trong (các) file test mà UTS tham chiếu.
- Bằng chứng: UTS TestFile_UTS=Test/Pd/PtDataTest/PtDataTest.cpp; RepoTestCaseCount_ByTestFile=179; StrongMapped=0; AmbiguousMatched=0.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Cập nhật UTS để tham chiếu đúng (các) file test và/hoặc bổ sung kiểm thử đơn vị bao phủ các hàm của Design Unit; thêm trace tag rõ ràng theo từng test để liên kết UTS ca kiểm thử ID ↔ gtest case.

### F-012

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PT01
- Vấn đề: Tất cả các ca kiểm thử khớp được đều ở trạng thái mơ hồ; không có kiểm thử đơn vị nào truy vết duy nhất cho Design Unit này.
- Bằng chứng: UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Thêm trace tag theo từng test (UTS ca kiểm thử ID trong mã nguồn) và/hoặc tách file test dùng chung theo từng Design Unit; sau đó cập nhật UTS để gỡ mơ hồ mapping ở mức ca kiểm thử.

### F-013

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PT03
- Vấn đề: Tất cả các ca kiểm thử khớp được đều ở trạng thái mơ hồ; không có kiểm thử đơn vị nào truy vết duy nhất cho Design Unit này.
- Bằng chứng: UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Thêm trace tag theo từng test (UTS ca kiểm thử ID trong mã nguồn) và/hoặc tách file test dùng chung theo từng Design Unit; sau đó cập nhật UTS để gỡ mơ hồ mapping ở mức ca kiểm thử.

### F-014

- Mức độ: MAJOR
- DesignUnitID: DES-MAIN-PD-PT04
- Vấn đề: Tất cả các ca kiểm thử khớp được đều ở trạng thái mơ hồ; không có kiểm thử đơn vị nào truy vết duy nhất cho Design Unit này.
- Bằng chứng: UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=2.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cập nhật danh sách SourceFile_UTS trong UTS để bao gồm đúng file triển khai thực tế theo bằng chứng từ kho mã, hoặc cập nhật cách đặt tên hàm trong SDD cho khớp kho mã.
  - Với các symbol bị đổi tên/không khớp, ghi lại lý do (lịch sử đổi tên) và cập nhật SDD/UTS tương ứng.
  - Bổ sung cơ chế truy vết: tag chú thích chứa UTS ca kiểm thử ID đặt gần từng gtest case (hoặc tách test theo unit).
- Tóm tắt khắc phục: Thêm trace tag theo từng test (UTS ca kiểm thử ID trong mã nguồn) và/hoặc tách file test dùng chung theo từng Design Unit; sau đó cập nhật UTS để gỡ mơ hồ mapping ở mức ca kiểm thử.

### F-015

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-AL01
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdAlarms - ThresholdChecks' so với SDD Unit='ThresholdChecks'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-016

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-AL02
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdAlarms - Debounce' so với SDD Unit='Debounce'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-017

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-AL03
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdAlarms - DwellTimers' so với SDD Unit='DwellTimers'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-018

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-AL04
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdAlarms - AlarmPublisher / O2 Alarms' so với SDD Unit='AlarmPublisher / O2 Alarms'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-019

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PE01
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='Ptdata - BreathPhaseProcessor' so với SDD Unit='BreathPhaseProcessor'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-020

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PE02
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='Ptdata - O2Limits' so với SDD Unit='O2Limits'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-021

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PE03
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='Ptdata - MetricsCalculator' so với SDD Unit='MetricsCalculator'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-022

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PE04
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='Ptdata - GuiPublisher' so với SDD Unit='GuiPublisher'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-023

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PE05
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='Ptdata - LogPublisher' so với SDD Unit='LogPublisher'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-024

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PT01
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdTask - Event Loop Unit' so với SDD Unit='EventLoop'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-025

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PT02
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdTask - IPC Receive Unit (message gateway)' so với SDD Unit='IPCReceive'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-026

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PT03
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdTask - Dispatcher Unit' so với SDD Unit='Dispatcher'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-027

- Mức độ: MINOR
- DesignUnitID: DES-MAIN-PD-PT04
- Vấn đề: Tên unit khác nhau giữa UTS và SDD cho cùng một Design Unit ID.
- Bằng chứng: UTS Unit='PdTask - Mode Status Unit' so với SDD Unit='ModeStatus'.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Đồng bộ cách đặt tên (tiêu đề unit trong UTS so với tên unit trong SDD) để giảm mơ hồ khi rà soát; nếu cố ý khác nhau thì ghi rõ lý do.
- Tóm tắt khắc phục: Đồng bộ cách đặt tên unit giữa UTS và SDD (hoặc ghi rõ lý do nếu cố ý khác nhau).

### F-028

- Mức độ: OBSERVATION
- DesignUnitID: N/A
- Vấn đề: UTS tham chiếu tới ma trận truy vết bên ngoài; không thể xác minh truy vết SRS↔UTS chỉ với các đầu vào hiện có.
- Bằng chứng: UTS có chứa chuỗi 'Refer to Traceability matrix'; không phát hiện được ID yêu cầu SRS trực tiếp trong UTS bằng phương pháp tìm chuỗi đơn giản.
- Hành động khuyến nghị (đủ điều kiện audit):
  - Cung cấp ma trận truy vết bên ngoài được tham chiếu và liên kết baseline của nó với UTS/SDD và phiên bản kho mã.
- Tóm tắt khắc phục: Cung cấp tài liệu/bằng chứng ma trận truy vết được tham chiếu và đảm bảo nó được kiểm soát cấu hình cùng baseline với UTS/SDD và phiên bản kho mã.

## Phân tích chi tiết theo từng Design Unit

### DES-MAIN-PD-AL01

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdAlarms - ThresholdChecks`
- Source file:
  - `Pd/pdalarms.cpp` (UTS:blocks[91]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[91]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[91]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ CheckHiPip(); CheckHiPipEndExh()` (UTS:blocks[91]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 10 (ví dụ: DES-MAIN-PD-AL01-TC01, DES-MAIN-PD-AL01-TC02, DES-MAIN-PD-AL01-TC03, DES-MAIN-PD-AL01-TC04, DES-MAIN-PD-AL01-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `ThresholdChecks`
- Các hàm:
  - `void PdAlarms::CheckHiPip(LONG hilimit, LONG value, AlarmStat *alarmtype); void PdAlarms::CheckHiPipEndExh(LONG hilimit, LONG value, AlarmStat *alarmtype);` (SDD:blocks[118]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdalarms.cpp` tồn tại: OK
- `Pd/pdalarms.h` tồn tại: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 190; ví dụ: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_Dùng chungAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231
- Kết quả tìm hàm (từ khóa:file:line): PdAlarms::CheckHiPip:Pd/pdalarms.cpp:631;PdAlarms::CheckHiPipEndExh:Pd/pdalarms.cpp:662

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 10
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1784` `PdAlarmsFixture.CheckHiPip_Clears_When_Value_Is_Below_Limit` (từ khóa khớp: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1796` `PdAlarmsFixture.CheckHiPip_NoOp_When_Value_Equals_Limit` (từ khóa khớp: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1808` `PdAlarmsFixture.CheckHiPip_NoOp_When_Value_Above_Limit` (từ khóa khớp: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1820` `PdAlarmsFixture.CheckHiPip_Idempotent_Clear_If_Already_NotActive` (từ khóa khớp: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1832` `PdAlarmsFixture.CheckHiPip_Never_Activates` (từ khóa khớp: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1851` `PdAlarmsFixture.CheckHiPipEndExh_Clears_When_Below_Limit_And_Active` (từ khóa khớp: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1863` `PdAlarmsFixture.CheckHiPipEndExh_NoChange_When_Below_Limit_And_Already_NotActive` (từ khóa khớp: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1875` `PdAlarmsFixture.CheckHiPipEndExh_NoOp_When_Value_Equals_Limit` (từ khóa khớp: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1887` `PdAlarmsFixture.CheckHiPipEndExh_NoOp_When_Value_Above_Limit` (từ khóa khớp: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1899` `PdAlarmsFixture.CheckHiPipEndExh_Never_Activates` (từ khóa khớp: CheckHiPip, CheckHiPipEndExh)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 0
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 26 (xem mục C)

**Kết luận:** AMBIGUOUS

### DES-MAIN-PD-AL02

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdAlarms - Debounce`
- Source file:
  - `Pd/pdalarms.cpp` (UTS:blocks[93]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[93]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[93]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ CheckHiCondition(); CheckLowCondition(); CheckLowPip()` (UTS:blocks[93]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 22 (ví dụ: DES-MAIN-PD-AL02-TC01, DES-MAIN-PD-AL02-TC02, DES-MAIN-PD-AL02-TC03, DES-MAIN-PD-AL02-TC04, DES-MAIN-PD-AL02-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `Debounce`
- Các hàm:
  - `void PdAlarms::CheckHiCondition(LONG hilimit, LONG value, AlarmStat *alarmtype, int SensitiveValue = 0); void PdAlarms::CheckLowCondition(LONG lowlimit, LONG value, AlarmStat *alarmtype, int SensitiveValue = 0); void PdAlarms::CheckLowPip(LONG lowlimit, LONG value, AlarmStat *alarmtype);` (SDD:blocks[120]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdalarms.cpp` tồn tại: OK
- `Pd/pdalarms.h` tồn tại: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 190; ví dụ: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_Dùng chungAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231
- Kết quả tìm hàm (từ khóa:file:line): PdAlarms::CheckHiCondition:Pd/pdalarms.cpp:105;PdAlarms::CheckLowCondition:Pd/pdalarms.cpp:152;PdAlarms::CheckLowPip:Pd/pdalarms.cpp:696

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 22
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:69` `PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive` (từ khóa khớp: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:105` `PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity` (từ khóa khớp: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:122` `PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal` (từ khóa khớp: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:146` `PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh` (từ khóa khớp: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:161` `PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately` (từ khóa khớp: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:179` `PdAlarmsFixture.CheckHiCondition_StaticCounter_SharedAcross_Alarms` (từ khóa khớp: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:203` `PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter` (từ khóa khớp: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:231` `PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive` (từ khóa khớp: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:266` `PdAlarmsFixture.CheckLowCondition_DoesNotActivate_Before_Sensitivity` (từ khóa khớp: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:282` `PdAlarmsFixture.CheckLowCondition_StaysActive_On_ContinuedLows_And_Clears_On_Normal` (từ khóa khớp: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:306` `PdAlarmsFixture.CheckLowCondition_LimitEquality_IsNormal` (từ khóa khớp: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:320` `PdAlarmsFixture.CheckLowCondition_Sensitivity1_ActivatesImmediately` (từ khóa khớp: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:338` `PdAlarmsFixture.CheckLowCondition_StaticCounter_SharedAcross_Alarms` (từ khóa khớp: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:362` `PdAlarmsFixture.CheckLowCondition_NormalWhileInactive_DoesNotResetCounter` (từ khóa khớp: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1923` `PdAlarmsFixture.CheckLowPip_Activates_On_Second_Consecutive_Low` (từ khóa khớp: CheckLowPip)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 0
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 26 (xem mục C)

**Kết luận:** AMBIGUOUS

### DES-MAIN-PD-AL03

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdAlarms - DwellTimers`
- Source file:
  - `Pd/pdalarms.cpp` (UTS:blocks[96]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[96]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[96]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ CheckLowAmlpitude(); CheckHighAmlpitude(); CheckLowBaseLine(); CheckLowBaseLine2Secs(); CheckHiBaseLine(); CheckHiBaseLine2Secs() ・ CheckLowMap(); CheckHiMap(); CheckLowMinuteVol(); CheckHiMinuteVol(); CheckHiBreathRate() ・ CheckLow_PLow(); CheckHigh_PLow(); CheckLowPLow5Time(); CheckHighPLow5Time() ・ CheckLow_PHigh(); CheckHigh_PHigh(); CheckLowPHigh5Time(); CheckHighPHigh5Time(); ResetAlarmActiveTime()` (UTS:blocks[96]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 138 (ví dụ: DES-MAIN-PD-AL03-TC001, DES-MAIN-PD-AL03-TC002, DES-MAIN-PD-AL03-TC003, DES-MAIN-PD-AL03-TC004, DES-MAIN-PD-AL03-TC005)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `DwellTimers`
- Các hàm:
  - `void PdAlarms::CheckLowAmlpitude(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHighAmlpitude(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowBaseLine(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowBaseLine2Secs(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHiBaseLine(LONG hilimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiBaseLine2Secs(LONG lowlimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckLowMap(LONG lowlimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiMap(LONG hilimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckLowMinuteVol(LONG lowlimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiMinuteVol(LONG hilimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiBreathRate(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLow_PLow(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHigh_PLow(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowPLow5Time(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHighPLow5Time(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLow_PHigh(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHigh_PHigh(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowPHigh5Time(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHighPHigh5Time(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::ResetAlarmActiveTime();` (SDD:blocks[122]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdalarms.cpp` tồn tại: OK
- `Pd/pdalarms.h` tồn tại: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 190; ví dụ: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_Dùng chungAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231
- Kết quả tìm hàm (từ khóa:file:line): PdAlarms::CheckHiBaseLine:Pd/pdalarms.cpp:535;PdAlarms::CheckHiBaseLine2Secs:Pd/pdalarms.cpp:578;PdAlarms::CheckHiBreathRate:Pd/pdalarms.cpp:923;PdAlarms::CheckHiMap:Pd/pdalarms.cpp:788;PdAlarms::CheckHiMinuteVol:Pd/pdalarms.cpp:878;PdAlarms::CheckHighAmlpitude:Pd/pdalarms.cpp:399;PdAlarms::CheckHighPHigh5Time:Pd/pdalarms.cpp:1275;PdAlarms::CheckHighPLow5Time:Pd/pdalarms.cpp:1101;PdAlarms::CheckHigh_PHigh:Pd/pdalarms.cpp:1189;PdAlarms::CheckHigh_PLow:Pd/pdalarms.cpp:1054;PdAlarms::CheckLowAmlpitude:Pd/pdalarms.cpp:353;PdAlarms::CheckLowBaseLine:Pd/pdalarms.cpp:445;PdAlarms::CheckLowBaseLine2Secs:Pd/pdalarms.cpp:487;PdAlarms::CheckLowMap:Pd/pdalarms.cpp:742;PdAlarms::CheckLowMinuteVol:Pd/pdalarms.cpp:832;PdAlarms::CheckLowPHigh5Time:Pd/pdalarms.cpp:1233;PdAlarms::CheckLowPLow5Time:Pd/pdalarms.cpp:1011;PdAlarms::CheckLow_PHigh:Pd/pdalarms.cpp:1144;PdAlarms::CheckLow_PLow:Pd/pdalarms.cpp:966;PdAlarms::ResetAlarmActiveTime:Pd/pdalarms.h:84(member-in-tiêu đề)

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 112
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1192` `PdAlarmsFixture.CheckLowBaseLine_Activates_After_5_Consecutive_Lows` (từ khóa khớp: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1213` `PdAlarmsFixture.CheckLowBaseLine_DoesNotActivate_Before_5_Lows_And_NormalBreaks` (từ khóa khớp: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1237` `PdAlarmsFixture.CheckLowBaseLine_LimitEquality_IsNormal_And_Clears_IfActive` (từ khóa khớp: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1261` `PdAlarmsFixture.CheckLowBaseLine_Alternating_Low_And_Normal_DoesNotActivate` (từ khóa khớp: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1277` `PdAlarmsFixture.CheckLowBaseLine_StaysActive_On_Continued_Lows_Then_Clears_On_Normal` (từ khóa khớp: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1300` `PdAlarmsFixture.CheckLowBaseLine_MemberCounter_SharedAcross_AlarmObjects` (từ khóa khớp: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1345` `PdAlarmsFixture.CheckLowBaseLine2Secs_Activates_After_2s_Of_Consecutive_Lows` (từ khóa khớp: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1362` `PdAlarmsFixture.CheckLowBaseLine2Secs_Clears_After_2s_Of_Consecutive_Highs` (từ khóa khớp: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1383` `PdAlarmsFixture.CheckLowBaseLine2Secs_EqualityIsNoOp_NoActivation_NoClearing` (từ khóa khớp: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1407` `PdAlarmsFixture.CheckLowBaseLine2Secs_Alternating_Low_High_DoesNotActivateOrClear` (từ khóa khớp: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1434` `PdAlarmsFixture.CheckLowBaseLine2Secs_MemberCounters_SharedAcross_AlarmObjects` (từ khóa khớp: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1460` `PdAlarmsFixture.CheckLowBaseLine2Secs_OffByOne_StepSize2_ToHitThreshold` (từ khóa khớp: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1493` `PdAlarmsFixture.CheckHiBaseLine_Activates_After_5_Consecutive_Highs` (từ khóa khớp: CheckHiBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1514` `PdAlarmsFixture.CheckHiBaseLine_DoesNotActivate_Before_5_Highs_And_NormalBreaks` (từ khóa khớp: CheckHiBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1539` `PdAlarmsFixture.CheckHiBaseLine_LimitEquality_IsNormal_And_Clears_IfActive` (từ khóa khớp: CheckHiBaseLine)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 0
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 26 (xem mục C)

**Kết luận:** AMBIGUOUS

### DES-MAIN-PD-AL04

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdAlarms - AlarmPublisher / O2 Alarms`
- Source file:
  - `Pd/pdalarms.cpp` (UTS:blocks[99]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[99]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[99]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ CheckHiO2(); CheckLowO2(); CheckHiInternalO2(); ResetO2Time()` (UTS:blocks[99]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 20 (ví dụ: DES-MAIN-PD-AL04-TC01, DES-MAIN-PD-AL04-TC02, DES-MAIN-PD-AL04-TC03, DES-MAIN-PD-AL04-TC04, DES-MAIN-PD-AL04-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `AlarmPublisher / O2 Alarms`
- Các hàm:
  - `void CheckHiO2(LONG hilimit, LONG value, AlarmStat*); void CheckLowO2(LONG lowlimit, LONG value, AlarmStat*); void ResetO2Time();` (SDD:blocks[124]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdalarms.cpp` tồn tại: OK
- `Pd/pdalarms.h` tồn tại: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 190; ví dụ: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_Dùng chungAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 20
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:403` `PdAlarmsFixture.CheckHiO2_Activates_After_THIRTY_SECONDS_HighSamples` (từ khóa khớp: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:425` `PdAlarmsFixture.CheckHiO2_Resets_After_O2RESETTIMELIMIT_LowSamples` (từ khóa khớp: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:458` `PdAlarmsFixture.CheckHiO2_Alternating_Around_Threshold_DoesNotActivate` (từ khóa khớp: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:475` `PdAlarmsFixture.CheckHiO2_Equality_IsHigh_For_Activation_But_NotFor_Reset` (từ khóa khớp: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:500` `PdAlarmsFixture.CheckHiO2_StaticCounters_SharedAcross_Alarms` (từ khóa khớp: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:544` `PdAlarmsFixture.CheckHiO2_StaysActive_On_HighOrEqual_Only_LowContributesToReset` (từ khóa khớp: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:586` `PdAlarmsFixture.CheckLowO2_Activates_After_THIRTY_SECONDS_LowSamples` (từ khóa khớp: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:606` `PdAlarmsFixture.CheckLowO2_Resets_After_Consecutive_HighSamples_RuntimeDetected` (từ khóa khớp: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:636` `PdAlarmsFixture.CheckLowO2_Alternating_Around_Threshold_DoesNotActivate` (từ khóa khớp: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:654` `PdAlarmsFixture.CheckLowO2_Equality_IsLow_For_Activation_But_NotFor_Reset` (từ khóa khớp: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:684` `PdAlarmsFixture.CheckLowO2_StaticCounters_SharedAcross_Alarms` (từ khóa khớp: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:721` `PdAlarmsFixture.CheckLowO2_StaysActive_On_LowOrEqual_Only_HighContributesToReset` (từ khóa khớp: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:762` `PdAlarmsFixture.CheckHiInternalO2_Activates_After_Consecutive_HighSamples_RuntimeDetected` (từ khóa khớp: CheckHiInternalO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:782` `PdAlarmsFixture.CheckHiInternalO2_Resets_Immediately_On_Single_Low` (từ khóa khớp: CheckHiInternalO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:803` `PdAlarmsFixture.CheckHiInternalO2_OffByOne_EqualityCountsAsHigh_681IsLow` (từ khóa khớp: CheckHiInternalO2)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 0
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 26 (xem mục C)

**Kết luận:** AMBIGUOUS

### DES-MAIN-PD-PE01

**UTS nêu (điểm chứng cứ)**

- Tên unit: `Ptdata - BreathPhaseProcessor`
- Source file:
  - `Pd/ptdata.cpp` (UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `void Ptdata::DoInhalationData(); void Ptdata::DoExhalationData(); void Ptdata::BuildInhMsg(); void Ptdata::BuildExhMsg(); void Ptdata::CompNumBreaths(); void Ptdata::DoEndExhPSInAPRV(); void Ptdata::DoEndInhPSInAPRV(); void Ptdata::DoEndInhExtra(); void Ptdata::DoAPRV_PLow(); void Ptdata::DoAPRV_PHigh(); void Ptdata::CompExhTidalVolumeInAPRV(); bool Ptdata::PdSpontaneous();` (UTS:blocks[82]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 36 (ví dụ: DES-MAIN-PD-PE01-TC01, DES-MAIN-PD-PE01-TC02, DES-MAIN-PD-PE01-TC03, DES-MAIN-PD-PE01-TC04, DES-MAIN-PD-PE01-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `BreathPhaseProcessor`
- Các hàm:
  - `void Ptdata::DoInhalationData(); void Ptdata::DoExhalationData(); void Ptdata::BuildInhMsg(); void Ptdata::BuildExhMsg(); void Ptdata::CompNumBreaths(); void Ptdata::DoEndExhPSInAPRV(); void Ptdata::DoEndInhPSInAPRV(); void Ptdata::DoEndInhExtra(); void Ptdata::DoAPRV_PLow(); void Ptdata::DoAPRV_PHigh(); void Ptdata::CompExhTidalVolumeInAPRV(); bool Ptdata::PdSpontaneous();` (SDD:blocks[99]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/ptdata.cpp` tồn tại: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 179; ví dụ: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Kết quả tìm hàm (từ khóa:file:line): Ptdata::PdSpontaneous:MISMATCH(tên tương tự 'PdSpontaneous' tìm thấy Pd/ptdata.cpp:307, không tìm thấy symbol dạng qualified);Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::CompExhTidalVolumeInAPRV:Pd/ptdata.cpp:385;Ptdata::CompNumBreaths:Pd/ptdata.cpp:286;Ptdata::DoAPRV_PHigh:Pd/ptdata.cpp:2146;Ptdata::DoAPRV_PLow:Pd/ptdata.cpp:1966;Ptdata::DoEndExhPSInAPRV:Pd/ptdata.cpp:3781;Ptdata::DoEndInhExtra:Pd/ptdata.cpp:4119;Ptdata::DoEndInhPSInAPRV:Pd/ptdata.cpp:3898;Ptdata::DoExhalationData:Pd/ptdata.cpp:1689;Ptdata::DoInhalationData:Pd/ptdata.cpp:1432

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 3
  - `Test/Pd/PtDataTest/PtDataTest.cpp:148` `PtDataFixture.Sanity_PdSpontaneousMapping` (từ khóa khớp: PdSpontaneous)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:678` `PtDataExhalationFixture.DoExhalationData_NonAprvAdvancesIndexAndComputesInhMinuteVolume` (từ khóa khớp: DoExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:712` `PtDataExhalationFixture.DoExhalationData_AprvSkipsInhMinuteVolume` (từ khóa khớp: DoExhalationData)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 4
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3675` `PtDataFixture.BuildInhMsg_ProxyReadyUsesActualValues` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3709` `PtDataFixture.BuildInhMsg_ProxyErrorBlanksVolumes` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3772` `PtDataFixture.BuildExhMsg_ProxyReadyCopiesValues` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3815` `PtDataFixture.BuildExhMsg_ProxyErrorBlanksVolumes` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 92 (xem mục C)

**Kết luận:** MISMATCH

### DES-MAIN-PD-PE02

**UTS nêu (điểm chứng cứ)**

- Tên unit: `Ptdata - O2Limits`
- Source file:
  - `Pd/ptdata.cpp` (UTS:blocks[84]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[84]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ DoO2()` (UTS:blocks[84]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 4 (ví dụ: DES-MAIN-PD-PE02-TC01, DES-MAIN-PD-PE02-TC02, DES-MAIN-PD-PE02-TC03, DES-MAIN-PD-PE02-TC04)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `O2Limits`
- Các hàm:
  - `void Ptdata::DoO2(); void Ptdata::BuildO2Msg();` (SDD:blocks[103]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/ptdata.cpp` tồn tại: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 179; ví dụ: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Kết quả tìm hàm (từ khóa:file:line): Ptdata::BuildO2Msg:Pd/ptdata.cpp:2563;Ptdata::DoO2:Pd/ptdata.cpp:1790

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 4
  - `Test/Pd/PtDataTest/PtDataTest.cpp:773` `PtDataO2Fixture.DoO2_SensorConnectedComputesAndChecksAlarms` (từ khóa khớp: DoO2)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:806` `PtDataO2Fixture.DoO2_SensorDisconnectedBlanksAndResetsAlarms` (từ khóa khớp: DoO2)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:828` `PtDataO2Fixture.DoO2_HighReadingClampedToTenThousand` (từ khóa khớp: DoO2)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:848` `PtDataO2Fixture.DoO2_StandbyModeReportsNotPresent` (từ khóa khớp: DoO2)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 1
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3840` `PtDataFixture.BuildO2Msg_PostsConcentrationAndVoltage` (ứng viên: DES-MAIN-PD-PE02, DES-MAIN-PD-PE04)
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 92 (xem mục C)

**Kết luận:** AMBIGUOUS

### DES-MAIN-PD-PE03

**UTS nêu (điểm chứng cứ)**

- Tên unit: `Ptdata - MetricsCalculator`
- Source file:
  - `Pd/pdhist.h` (UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/ptdata.cpp` (UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ CompCircuitComplianceVolume() ・ CompCompliance() ・ CompExhTidalVolume() ・ CompExhMinuteVolume(); CompInhMinuteVolume() ・ GetAverageVte12(); GetAverageCompliance12() ・ GetMin/MaxVteArray(); GetMin/MaxComplianceArray12() ・ ClearVteBuffer(); ClearExhLeakArray() ・ UpdateMandExhTidalVolume(); CompRapidShallowBreathix() ・ CompPressureSum(); GetLeakVolume(); GetPessureSum() ・ CompCompliance_InAPRV(); CompAverageCompliance(); GetAverageVte() ・ GetMinCompliance(); GetMaxCompliance(); MinVteArray(); MaxVteArray() ・ MinVteArray12(); MaxVteArray12(); GetMin/MaxVteArray12() ・ ResetLeakAndPress(); CompIeRatio(); CompMeanAirwayPressure(); CompRespRate() ・ GetCircuitComplianceFactor(); BlankingData(); ModeInducedAlarmClearing() ・ CompLeakVolume(); CompLeakCompensate()` (UTS:blocks[86]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 88 (ví dụ: DES-MAIN-PD-PE03-TC01, DES-MAIN-PD-PE03-TC02, DES-MAIN-PD-PE03-TC03, DES-MAIN-PD-PE03-TC04, DES-MAIN-PD-PE03-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `MetricsCalculator`
- Các hàm:
  - `LONG Ptdata::CompCircuitComplianceVolume(); void Ptdata::CompCompliance(); void Ptdata::CompExhTidalVolume(); void Ptdata::CompExhMinuteVolume(); void Ptdata::CompInhMinuteVolume(); LONG Ptdata::GetAverageVte12(); DOUBLE Ptdata::GetAverageCompliance12(); LONG Ptdata::GetMinVteArray(); LONG Ptdata::GetMaxVteArray(); DOUBLE Ptdata::GetMinComplianceArray12(); DOUBLE Ptdata::GetMaxComplianceArray12(); void Ptdata::ClearVteBuffer(); void Ptdata::ClearExhLeakArray(); void Ptdata::UpdateMandExhTidalVolume(); LONG Ptdata::CompRapidShallowBreathix(); void Ptdata::CompPressureSum(); LONG Ptdata::GetLeakVolume(); LONG Ptdata::GetPessureSum(); DOUBLE Ptdata::CompCompliance_InAPRV(); DOUBLE Ptdata::CompAverageCompliance(); LONG Ptdata::GetAverageVte(); LONG Ptdata::GetMinCompliance(); LONG Ptdata::GetMaxCompliance(); LONG Ptdata::MinVteArray(); LONG Ptdata::MaxVteArray(); LONG Ptdata::MinVteArray12(); LONG Ptdata::MaxVteArray12(); LONG Ptdata::GetMinVteArray12(); LONG Ptdata::GetMaxVteArray12(); void Ptdata::ResetLeakAndPress(); LONG Ptdata::CompIeRatio(); LONG Ptdata::CompMeanAirwayPressure(); LONG Ptdata::CompRespRate(); LONG Ptdata::GetCircuitComplianceFactor(); void Ptdata::BlankingData(); void Ptdata::ModeInducedAlarmClearing(); void Ptdata::CompLeakVolume(); void Ptdata::CompLeakCompensate();` (SDD:blocks[109]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdhist.h` tồn tại: OK
- `Pd/ptdata.cpp` tồn tại: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 179; ví dụ: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Kết quả tìm hàm (từ khóa:file:line): Ptdata::MaxVteArray:MISMATCH(tên tương tự 'MaxVteArray' tìm thấy Pd/ptdata.cpp:3068, không tìm thấy symbol dạng qualified);Ptdata::MaxVteArray12:MISMATCH(tên tương tự 'MaxVteArray12' tìm thấy Pd/ptdata.cpp:3113, không tìm thấy symbol dạng qualified);Ptdata::MinVteArray:MISMATCH(tên tương tự 'MinVteArray' tìm thấy Pd/ptdata.cpp:3056, không tìm thấy symbol dạng qualified);Ptdata::MinVteArray12:MISMATCH(tên tương tự 'MinVteArray12' tìm thấy Pd/ptdata.cpp:3112, không tìm thấy symbol dạng qualified);Ptdata::BlankingData:Pd/ptdata.cpp:2624;Ptdata::ClearExhLeakArray:Pd/ptdata.cpp:3753;Ptdata::ClearVteBuffer:Pd/ptdata.cpp:3338;Ptdata::CompAverageCompliance:Pd/ptdata.cpp:1538;Ptdata::CompCircuitComplianceVolume:Pd/ptdata.cpp:358;Ptdata::CompCompliance:Pd/ptdata.cpp:1461;Ptdata::CompCompliance_InAPRV:Pd/ptdata.cpp:2025;Ptdata::CompExhMinuteVolume:Pd/ptdata.cpp:588;Ptdata::CompExhTidalVolume:Pd/ptdata.cpp:385;Ptdata::CompIeRatio:Pd/ptdata.cpp:816;Ptdata::CompInhMinuteVolume:Pd/ptdata.cpp:670;Ptdata::CompLeakCompensate:Pd/ptdata.cpp:2929;Ptdata::CompLeakVolume:Pd/ptdata.cpp:3965;Ptdata::CompMeanAirwayPressure:Pd/ptdata.cpp:874;Ptdata::CompPressureSum:Pd/ptdata.cpp:4021;Ptdata::CompRapidShallowBreathix:Pd/ptdata.cpp:776;Ptdata::CompRespRate:Pd/ptdata.cpp:943;Ptdata::GetAverageCompliance12:Pd/ptdata.cpp:3220;Ptdata::GetAverageVte:Pd/ptdata.cpp:3046;Ptdata::GetAverageVte12:Pd/ptdata.cpp:3102;Ptdata::GetCircuitComplianceFactor:Pd/ptdata.cpp:2905;Ptdata::GetLeakVolume:Pd/ptdata.cpp:4049;Ptdata::GetMaxCompliance:Pd/ptdata.cpp:1626;Ptdata::GetMaxComplianceArray12:Pd/ptdata.cpp:3686;Ptdata::GetMaxVteArray:Pd/ptdata.cpp:3425;Ptdata::GetMaxVteArray12:Pd/ptdata.cpp:3539

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 54
  - `Test/Pd/PtDataTest/PtDataTest.cpp:390` `PtDataFixture.CompCompliance_PositiveDeltaUpdatesSamples` (từ khóa khớp: CompCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:425` `PtDataFixture.CompCompliance_ZeroDeltaInitializesWarmupBuffer` (từ khóa khớp: CompCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:458` `PtDataFixture.CompCompliance_Sample12RemainsZeroUntilFillFlagSet` (từ khóa khớp: CompCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:501` `PtDataFixture.CompAverageCompliance_SingleSampleUsesAloneValue` (từ khóa khớp: CompAverageCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:512` `PtDataFixture.CompAverageCompliance_SubsetDropsOnlyMinimum` (từ khóa khớp: CompAverageCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:527` `PtDataFixture.CompAverageCompliance_FullBufferDropsMinAndMax` (từ khóa khớp: CompAverageCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:543` `PtDataFixture.GetMinCompliance_ReturnsMinimumAmongPrefix` (từ khóa khớp: GetMinCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:552` `PtDataFixture.GetMinCompliance_IgnoresValuesBeyondIndex` (từ khóa khớp: GetMinCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:562` `PtDataFixture.GetMinCompliance_ZeroIndexReturnsMaxLong` (từ khóa khớp: GetMinCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:572` `PtDataFixture.GetMaxCompliance_ReturnsMaximumAmongPrefix` (từ khóa khớp: GetMaxCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:581` `PtDataFixture.GetMaxCompliance_IgnoresValuesBeyondIndex` (từ khóa khớp: GetMaxCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:591` `PtDataFixture.GetMaxCompliance_ZeroIndexReturnsZero` (từ khóa khớp: GetMaxCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:1406` `PtDataFixture.MinVteArray_ReturnsLowestSample` (từ khóa khớp: MinVteArray)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:1416` `PtDataFixture.MinVteArray_HandlesNegativeValues` (từ khóa khớp: MinVteArray)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:1426` `PtDataFixture.MinVteArray_NoSamplesReturnsSentinel` (từ khóa khớp: MinVteArray)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 0
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 92 (xem mục C)

**Kết luận:** MISMATCH

### DES-MAIN-PD-PE04

**UTS nêu (điểm chứng cứ)**

- Tên unit: `Ptdata - GuiPublisher`
- Source file:
  - `Pd/pdinterface.h` (UTS:blocks[88]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/ptdata.cpp` (UTS:blocks[88]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[88]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ DoPEEP(); DoNCPAP(); DoHFOData() ・ BuildExhMsg(); BuildInhMsg(); BuildO2Msg() ・ BuildHFOMsg(); BuildHFOSIdata(); BuildAPRVPLow() ・ DoAPRVBR() ・ GetExhalationData(); GetInhalationData() ・ CompProxymalLeak() ・ CalculateHFOExh(); GetEndExhFlow(); GetEndExhFlow_Average() ・ ClearHistory(); GetHFOData()` (UTS:blocks[88]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 41 (ví dụ: DES-MAIN-PD-PE04-TC01, DES-MAIN-PD-PE04-TC02, DES-MAIN-PD-PE04-TC03, DES-MAIN-PD-PE04-TC04, DES-MAIN-PD-PE04-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `GuiPublisher`
- Các hàm:
  - `void Ptdata::DoPEEP(void); void Ptdata::DoNCPAP(void); void Ptdata::DoHFOData(void); void Ptdata::BuildExhMsg(void); void Ptdata::BuildInhMsg(void); void Ptdata::BuildO2Msg(void); void Ptdata::BuildHFOMsg(void); void Ptdata::BuilHFOSIdata(int); void Ptdata::BuildAPRVPLow(void); void Ptdata::DoAPRVBR(void); void Ptdata::GetExhalationData(void); void Ptdata::GetInhalationData(void); void Ptdata::CompProxymalLeak(void); void Ptdata::CalculateHFOExh(void); void Ptdata::GetEndExhFlow(void); LONG Ptdata::GetEndExhFlow_Average(void); void Ptdata::ClearHistory(void); void Ptdata::GetHFOData(void);` (SDD:blocks[112]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdinterface.h` tồn tại: OK
- `Pd/ptdata.cpp` tồn tại: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 179; ví dụ: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Kết quả tìm hàm (từ khóa:file:line): Ptdata::BuilHFOSIdata:Pd/ptdata.cpp:2819;Ptdata::BuildAPRVPLow:Pd/ptdata.cpp:2059;Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildHFOMsg:Pd/ptdata.cpp:2470;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::BuildO2Msg:Pd/ptdata.cpp:2563;Ptdata::CalculateHFOExh:Pd/ptdata.cpp:2784;Ptdata::ClearHistory:Pd/ptdata.cpp:2236;Ptdata::CompProxymalLeak:Pd/ptdata.cpp:1246;Ptdata::DoAPRVBR:Pd/ptdata.cpp:2113;Ptdata::DoHFOData:Pd/ptdata.cpp:1659;Ptdata::DoNCPAP:Pd/ptdata.cpp:1917;Ptdata::DoPEEP:Pd/ptdata.cpp:1880;Ptdata::GetEndExhFlow:Pd/ptdata.cpp:2849;Ptdata::GetEndExhFlow_Average:Pd/ptdata.cpp:2874;Ptdata::GetExhalationData:Pd/ptdata.cpp:1069;Ptdata::GetHFOData:Pd/ptdata.cpp:1393;Ptdata::GetInhalationData:Pd/ptdata.cpp:1315

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 21
  - `Test/Pd/PtDataTest/PtDataTest.cpp:619` `PtDataHfoDisabledFixture.DoHFOData_NoOpWhenHfoSystemDisabled` (từ khóa khớp: DoHFOData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:644` `PtDataHfoDisabledFixture.DoHFOData_RepeatedCallsMaintainState` (từ khóa khớp: DoHFOData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:877` `PtDataPeepFixture.DoPEEP_PostsCurrPressureAndChecksAlarms` (từ khóa khớp: DoPEEP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:898` `PtDataPeepFixture.DoPEEP_ClampsNegativePressureToZero` (từ khóa khớp: DoPEEP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:938` `PtDataNcpapFixture.DoNCPAP_PostsValueAndTriggersAlarms` (từ khóa khớp: DoNCPAP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:960` `PtDataNcpapFixture.DoNCPAP_BlanekdLimitsResetExistingAlarms` (từ khóa khớp: DoNCPAP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2636` `PtDataFixture.GetExhalationData_SetsMode_AndHistory_APRV` (từ khóa khớp: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2654` `PtDataFixture.GetExhalationData_AprvAddsExtraCompensatedVolume` (từ khóa khớp: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2675` `PtDataFixture.GetExhalationData_SetsMode_AndHistory_NonAPRV` (từ khóa khớp: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2691` `PtDataFixture.GetExhalationData_NonAprv_TrimAppliesLeakLimit` (từ khóa khớp: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2720` `PtDataFixture.GetExhalationData_NonAprv_ClampNegativeLeakToZero` (từ khóa khớp: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2903` `PtDataFixture.GetInhalationData_NonAprv_PopulatesFieldsAndLimits` (từ khóa khớp: GetInhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2920` `PtDataFixture.GetInhalationData_Aprv_NonPsBranchWritesHistory` (từ khóa khớp: GetInhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2936` `PtDataFixture.GetInhalationData_Aprv_PsBranchUsesAPRVValues` (từ khóa khớp: GetInhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3994` `PtDataFixture.GetEndExhFlow_WritesValueAndAdvancesIndex` (từ khóa khớp: GetEndExhFlow)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 5
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3675` `PtDataFixture.BuildInhMsg_ProxyReadyUsesActualValues` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3709` `PtDataFixture.BuildInhMsg_ProxyErrorBlanksVolumes` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3772` `PtDataFixture.BuildExhMsg_ProxyReadyCopiesValues` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3815` `PtDataFixture.BuildExhMsg_ProxyErrorBlanksVolumes` (ứng viên: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3840` `PtDataFixture.BuildO2Msg_PostsConcentrationAndVoltage` (ứng viên: DES-MAIN-PD-PE02, DES-MAIN-PD-PE04)
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 92 (xem mục C)

**Kết luận:** AMBIGUOUS

### DES-MAIN-PD-PE05

**UTS nêu (điểm chứng cứ)**

- Tên unit: `Ptdata - LogPublisher`
- Source file:
  - `Pd/pdinterface.h` (UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/ptdata.cpp` (UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `(No direct logging API in Ptdata - logging handled in PdTask)` (UTS:blocks[90]/Table/Body/Row[3]/Col[1] (対象関数/Function))

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `LogPublisher`
- Các hàm:
  - `void PdTask::TestLogFileWriting();` (SDD:blocks[115]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdinterface.h` tồn tại: OK
- `Pd/ptdata.cpp` tồn tại: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 179; ví dụ: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Kết quả tìm hàm (từ khóa:file:line): PdTask::TestLogFileWriting:MISMATCH(tìm thấy Pd/pdtask.cpp:206 không nằm trong danh sách SourceFile_UTS của UTS)

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 0
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 0
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 92 (xem mục C)

**Kết luận:** MISMATCH

### DES-MAIN-PD-PT01

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdTask - Event Loop Unit`
- Source file:
  - `Pd/pdtask.cpp` (UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ run() ・ TestLogFileWriting()` (UTS:blocks[74]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 4 (ví dụ: DES-MAIN-PD-PT01-TC01, DES-MAIN-PD-PT01-TC02, DES-MAIN-PD-PT01-TC03, DES-MAIN-PD-PT01-TC04)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `EventLoop`
- Các hàm:
  - `void PdTask::run(); void PdTask::TestLogFileWriting();` (SDD:blocks[86]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdtask.cpp` tồn tại: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 30; ví dụ: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Kết quả tìm hàm (từ khóa:file:line): Ptdata::ClearHistory:MISMATCH(tìm thấy Pd/ptdata.cpp:2236 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::TestLogFileWriting:Pd/pdtask.cpp:206;PdTask::run:Pd/pdtask.cpp:42

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 0
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 15
  - `Test/Pd/PdTask/PdTaskTest.cpp:354` `PdTaskFixture.Run_NonBreathingMode_CallsClearHistory_AndSkipsElseBranch` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
  - `Test/Pd/PdTask/PdTaskTest.cpp:369` `PdTaskFixture.Run_StartInh_CallsDoExhalationData` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:383` `PdTaskFixture.Run_StartExh_CallsDoInhalationData` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:397` `PdTaskFixture.Run_HFODataReady_CallsDoHFOData` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:410` `PdTaskFixture.Run_UpdateAprvBr_CallsDoAPRVBR` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:423` `PdTaskFixture.Run_UpdateNcpap_CallsDoNCPAP` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:436` `PdTaskFixture.Run_AprvPLowStart_CallsDoAPRV_PHigh` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:449` `PdTaskFixture.Run_AprvPHighStart_CallsDoAPRV_PLow` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:462` `PdTaskFixture.Run_O2SettingThenTimedEvent_CallsDoO2` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:477` `PdTaskFixture.Run_TimedEvent_WithNonBreathing_SkipsDoO2` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 9 (xem mục C)

**Kết luận:** MISMATCH

### DES-MAIN-PD-PT02

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdTask - IPC Receive Unit (message gateway)`
- Source file:
  - `Pd/pdtask.cpp` (UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ S_SetEventFlag()` (UTS:blocks[76]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 6 (ví dụ: DES-MAIN-PD-PT02-TC01, DES-MAIN-PD-PT02-TC02, DES-MAIN-PD-PT02-TC03, DES-MAIN-PD-PT02-TC04, DES-MAIN-PD-PT02-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `IPCReceive`
- Các hàm:
  - `void PdTask::initMsgQueue(); int32_t PdTask::S_SetEventFlag(UNSIGNED flags);` (SDD:blocks[89]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdtask.cpp` tồn tại: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 30; ví dụ: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Kết quả tìm hàm (từ khóa:file:line): PdTask::S_SetEventFlag:MISMATCH(tìm thấy Pd/pdtask.h:115 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::initMsgQueue:Pd/pdtask.cpp:226

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 6
  - `Test/Pd/PdTask/PdTaskTest.cpp:228` `PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs` (từ khóa khớp: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:251` `PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags` (từ khóa khớp: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:269` `PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent` (từ khóa khớp: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:284` `PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative` (từ khóa khớp: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:299` `PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags` (từ khóa khớp: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:315` `PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount` (từ khóa khớp: S_SetEventFlag)
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 0
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 9 (xem mục C)

**Kết luận:** MISMATCH

### DES-MAIN-PD-PT03

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdTask - Dispatcher Unit`
- Source file:
  - `Pd/pdtask.cpp` (UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ run() - dispatch mapping` (UTS:blocks[78]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 14 (ví dụ: DES-MAIN-PD-PT03-TC01, DES-MAIN-PD-PT03-TC02, DES-MAIN-PD-PT03-TC03, DES-MAIN-PD-PT03-TC04, DES-MAIN-PD-PT03-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `Dispatcher`
- Các hàm:
  - `(nằm trong PdTask::run())` (SDD:blocks[93]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdtask.cpp` tồn tại: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 30; ví dụ: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Kết quả tìm hàm (từ khóa:file:line): Ptdata::ClearHistory:MISMATCH(tìm thấy Pd/ptdata.cpp:2236 không nằm trong danh sách SourceFile_UTS của UTS);PdTask::run:Pd/pdtask.cpp:42

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 0
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 15
  - `Test/Pd/PdTask/PdTaskTest.cpp:354` `PdTaskFixture.Run_NonBreathingMode_CallsClearHistory_AndSkipsElseBranch` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
  - `Test/Pd/PdTask/PdTaskTest.cpp:369` `PdTaskFixture.Run_StartInh_CallsDoExhalationData` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:383` `PdTaskFixture.Run_StartExh_CallsDoInhalationData` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:397` `PdTaskFixture.Run_HFODataReady_CallsDoHFOData` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:410` `PdTaskFixture.Run_UpdateAprvBr_CallsDoAPRVBR` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:423` `PdTaskFixture.Run_UpdateNcpap_CallsDoNCPAP` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:436` `PdTaskFixture.Run_AprvPLowStart_CallsDoAPRV_PHigh` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:449` `PdTaskFixture.Run_AprvPHighStart_CallsDoAPRV_PLow` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:462` `PdTaskFixture.Run_O2SettingThenTimedEvent_CallsDoO2` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:477` `PdTaskFixture.Run_TimedEvent_WithNonBreathing_SkipsDoO2` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 9 (xem mục C)

**Kết luận:** MISMATCH

### DES-MAIN-PD-PT04

**UTS nêu (điểm chứng cứ)**

- Tên unit: `PdTask - Mode Status Unit`
- Source file:
  - `Pd/pdtask.cpp` (UTS:blocks[80]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- File kiểm thử đơn vị triển khai:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[80]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Hàm mục tiêu:
  - `・ run() - mode gating ・ run() - exit standby ・ TestLogFileWriting() - standby gating` (UTS:blocks[80]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Số ca kiểm thử ID được liệt kê trong UTS: 5 (ví dụ: DES-MAIN-PD-PT04-TC01, DES-MAIN-PD-PT04-TC02, DES-MAIN-PD-PT04-TC03, DES-MAIN-PD-PT04-TC04, DES-MAIN-PD-PT04-TC05)

**SDD mô tả (điểm chứng cứ)**

- Tên unit: `ModeStatus`
- Các hàm:
  - `(nằm trong PdTask::run())` (SDD:blocks[96]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Bằng chứng từ kho mã (cố gắng tối đa)**

- `Pd/pdtask.cpp` tồn tại: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` tồn tại: OK
  - Số TEST macro tìm thấy: 30; ví dụ: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Kết quả tìm hàm (từ khóa:file:line): PdTask::run:Pd/pdtask.cpp:42

**Mapping mức ca kiểm thử (cố gắng tối đa, trong các file test UTS tham chiếu)**

- ca kiểm thử map mạnh (khớp duy nhất): 0
- ca kiểm thử khớp mơ hồ (khớp nhiều unit): 2
  - `Test/Pd/PdTask/PdTaskTest.cpp:354` `PdTaskFixture.Run_NonBreathingMode_CallsClearHistory_AndSkipsElseBranch` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
  - `Test/Pd/PdTask/PdTaskTest.cpp:523` `PdTaskFixture.Run_ExitStandby_CallsGetCircuitComplianceFactor` (ứng viên: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
- ca kiểm thử chưa gán trong (các) file test mà unit này tham chiếu: 9 (xem mục C)

**Kết luận:** AMBIGUOUS

## Checklist sẵn sàng IEC 62304 (khách quan)

- Độ đầy đủ truy vết: xử lý các phát hiện MAJOR (bài kiểm thử mồ côi, file test dùng chung gây mơ hồ, và các ánh xạ còn thiếu).
- Tiêu chí khách quan: đảm bảo mỗi ca kiểm thử trong UTS có tiêu chí kỳ vọng khách quan và truy vết trực tiếp tới gtest case cụ thể.
- Kiểm soát baseline: đảm bảo UTS/SDD/SRS/SAS và phiên bản kho mã được liên kết baseline; cập nhật tên/đường dẫn bị lỗi thời.
- Độ vững của bằng chứng: ưu tiên trace tag theo từng test (ví dụ: chú thích chứa UTS Test Case ID) để gỡ mơ hồ do file test dùng chung.

## Các tài liệu/bằng chứng đã tạo

- `Pd_UnitTest_Design_Mapping_Audit_Report.md`
- `Pd_UnitTest_Design_Mapping_Matrix.csv`
- `Pd_PdUnitTest_TestCase_Statistics.csv`
- `Pd_PdUnitTest_TestCase_Mapping.csv`
