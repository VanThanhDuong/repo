# Pd Unit Test ↔ Design Mapping Audit Report

- Project: Humming Vue R2 ventilator – Pd (Patient Data) module
- Audit scope: ALL
- Timestamp: 2025-12-17T15:20:05
- Inputs audited (source-of-truth):
  - UTS: `Pd/Docs/json/HVR2-MAIN-PD-REPT-01-00-Humming-Vue-R2-Patient-Data-Unit-Test-Specification.json`
  - SDD: `Pd/Docs/json/HVR2-MAIN-PD-DESI-01-00-Humming-Vue-R2-Patient-Data-Software-Design.json`
  - SAS: `Pd/Docs/json/HVR2-MAIN-PD-ARCH-01-00-Humming-Vue-R2-Patient-Data-Software-Architecture.json`
  - SRS: `Pd/Docs/json/HVR2-MAIN-PD-SPEC-01-00-Humming-Vue-R2-Patient-Data-Software-Requirement-Specification_CORRECTED.json`
  - Repo code root: `Pd`
  - Repo unit tests root: `Test/Pd`

## Executive Summary

- Design Units in scope (from UTS): 13
- Status counts: OK=0, MISMATCH=6, MISSING=0, AMBIGUOUS=7, TBD=0
- Findings: MAJOR=14, MINOR=13, OBSERVATION=1

## Method (Evidence-Only)

- Parsed UTS/SDD as Pandoc JSON AST; extracted Design Units by table-header patterns (`Unit ID` in UTS, `ID:` in SDD).
- Extracted referenced file paths by regex matching `Pd/...` and `Test/Pd/...` strings in those tables.
- Scanned `Test/Pd` for GoogleTest macros (`TEST`, `TEST_F`, `TEST_P`) after best-effort comment stripping that preserves line numbers.
- Searched for qualified function tokens from UTS/SDD in UTS-listed source files; if not found, searched under `Pd` to detect spec↔repo mismatches (best-effort substring match; no build/ctags).
- Did not execute unit tests; no pass/fail or coverage claims are made.

## Test Case Statistics (Pd Unit Tests)

### A) Repository Totals (from scanning `Test/Pd`)

- Total unit test files scanned: 14
- Total unit test cases found (TEST/TEST_F/TEST_P): 400
- Total parameterized instantiations detected (INSTANTIATE_TEST_*_P): 0

### B) Mapping Coverage (UTS → Design Units)

- Total Design Units in scope: 13
- Units with ≥1 mapped test case (file-level): 13
- Units with 0 mapped test case: (none)
- Total mapped test cases (no double-count inflation; file-level): 399
- Total mapped test cases that are ambiguous due to shared files: 399
- Total unmapped test cases (repo but not referenced by UTS): 1
  - Within UTS-referenced test files (test-case-level, best-effort token mapping):
    - Strongly mapped (exactly 1 Design Unit matched): 252
    - Ambiguous (matched ≥2 Design Units): 20
    - Unassigned (matched 0 Design Units): 127
- Top unmapped test files by test-case count: Test/Pd/PtDataTest/PtDataTest (copy).cpp(1)

### C) Per-Design-Unit Breakdown

| DesignUnitID | UTS Test File(s) | TestCasesInRepo(Count) | FileLevelMappedCount | StrongMappedCount | AmbiguousMatchedCount | UnassignedInReferencedFiles | Notes |
|---|---|---:|---:|---:|---:|---:|---|
| DES-MAIN-PD-AL01 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 10 | 0 | 26 | Shared |
| DES-MAIN-PD-AL02 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 22 | 0 | 26 | Shared |
| DES-MAIN-PD-AL03 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 112 | 0 | 26 | Shared |
| DES-MAIN-PD-AL04 | Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp | 190 | 190 | 20 | 0 | 26 | Shared |
| DES-MAIN-PD-PE01 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 3 | 4 | 92 | Shared |
| DES-MAIN-PD-PE02 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 4 | 1 | 92 | Shared |
| DES-MAIN-PD-PE03 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 54 | 0 | 92 | Shared |
| DES-MAIN-PD-PE04 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 21 | 5 | 92 | Shared |
| DES-MAIN-PD-PE05 | Test/Pd/PtDataTest/PtDataTest.cpp | 179 | 179 | 0 | 0 | 92 | Shared |
| DES-MAIN-PD-PT01 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 0 | 15 | 9 | Shared |
| DES-MAIN-PD-PT02 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 6 | 0 | 9 | Shared |
| DES-MAIN-PD-PT03 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 0 | 15 | 9 | Shared |
| DES-MAIN-PD-PT04 | Test/Pd/PdTask/PdTaskTest.cpp | 30 | 30 | 0 | 2 | 9 | Shared |

### D) Top 10 Test Files by Test-Case Count (Appendix)

| TestFile | TestCaseCount | Sample test names |
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

## Findings Table

| FindingID | Severity | DesignUnitID | Issue | Evidence | Proposed Fix |
|---|---|---|---|---|---|
| F-001 | MAJOR | N/A | Repository contains Pd unit test files not referenced by UTS (orphan/untraceable tests). | Repo scan: 1 orphan files under Test/Pd; top: Test/Pd/PtDataTest/PtDataTest (copy).cpp(1) | Update UTS to reference these test files and map them to Design Units, or remove/relocate tests that are out-of-scope. |
| F-002 | MAJOR | MULTI | Same unit test file is referenced by multiple Design Units; mapping is ambiguous at test-case level. | UTS-referenced file `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` mapped to DesignUnitIDs: DES-MAIN-PD-AL01, DES-MAIN-PD-AL02, DES-MAIN-PD-AL03, DES-MAIN-PD-AL04. | Add per-test trace tags in code (e.g., comments with UTS Test Case IDs) or split tests per Design Unit and update UTS accordingly. |
| F-003 | MAJOR | MULTI | Same unit test file is referenced by multiple Design Units; mapping is ambiguous at test-case level. | UTS-referenced file `Test/Pd/PdTask/PdTaskTest.cpp` mapped to DesignUnitIDs: DES-MAIN-PD-PT01, DES-MAIN-PD-PT02, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04. | Add per-test trace tags in code (e.g., comments with UTS Test Case IDs) or split tests per Design Unit and update UTS accordingly. |
| F-004 | MAJOR | MULTI | Same unit test file is referenced by multiple Design Units; mapping is ambiguous at test-case level. | UTS-referenced file `Test/Pd/PtDataTest/PtDataTest.cpp` mapped to DesignUnitIDs: DES-MAIN-PD-PE01, DES-MAIN-PD-PE02, DES-MAIN-PD-PE03, DES-MAIN-PD-PE04, DES-MAIN-PD-PE05. | Add per-test trace tags in code (e.g., comments with UTS Test Case IDs) or split tests per Design Unit and update UTS accordingly. |
| F-005 | MAJOR | DES-MAIN-PD-PE01 | Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch). | UTS source evidence: UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[99]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::PdSpontaneous:MISMATCH(similar-name 'PdSpontaneous' found Pd/ptdata.cpp:307, qualified symbol not found);Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::CompExhTidalVolumeInAPRV:Pd/ptdata.cpp:385 | Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it. |
| F-006 | MAJOR | DES-MAIN-PD-PE03 | Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch). | UTS source evidence: UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[109]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::MaxVteArray:MISMATCH(similar-name 'MaxVteArray' found Pd/ptdata.cpp:3068, qualified symbol not found);Ptdata::MaxVteArray12:MISMATCH(similar-name 'MaxVteArray12' found Pd/ptdata.cpp:3113, qualified symbol not found);Ptdata::MinVteArray:MISMATCH(similar-name 'MinVteArray' found Pd/ptdata.cpp:3056, qualified symbol not found);Ptdata::MinVteArray12:MISMATCH(similar-name 'MinVteArray12' found Pd/ptdata.cpp:3112, qualified symbol not found);Ptdata::BlankingData:Pd/ptdata.cpp:2624;Ptdata::ClearExhLeakArray:Pd/ptdata.cpp:3753;Ptdata::ClearVteBuffer:Pd/ptdata.cpp:3338 | Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it. |
| F-007 | MAJOR | DES-MAIN-PD-PE05 | Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch). | UTS source evidence: UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[115]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: PdTask::TestLogFileWriting:MISMATCH(found Pd/pdtask.cpp:206 not in UTS SourceFile_UTS list) | Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it. |
| F-008 | MAJOR | DES-MAIN-PD-PT01 | Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch). | UTS source evidence: UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[86]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::ClearHistory:MISMATCH(found Pd/ptdata.cpp:2236 not in UTS SourceFile_UTS list);PdTask::TestLogFileWriting:Pd/pdtask.cpp:206;PdTask::run:Pd/pdtask.cpp:42 | Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it. |
| F-009 | MAJOR | DES-MAIN-PD-PT02 | Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch). | UTS source evidence: UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[89]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: PdTask::S_SetEventFlag:MISMATCH(found Pd/pdtask.h:115 not in UTS SourceFile_UTS list);PdTask::initMsgQueue:Pd/pdtask.cpp:226 | Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it. |
| F-010 | MAJOR | DES-MAIN-PD-PT03 | Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch). | UTS source evidence: UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[93]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::ClearHistory:MISMATCH(found Pd/ptdata.cpp:2236 not in UTS SourceFile_UTS list);PdTask::run:Pd/pdtask.cpp:42 | Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it. |
| F-011 | MAJOR | DES-MAIN-PD-PE05 | No evidence-based unit test cases mapped to this Design Unit (0 strong, 0 ambiguous) within UTS-referenced test file(s). | UTS TestFile_UTS=Test/Pd/PtDataTest/PtDataTest.cpp; RepoTestCaseCount_ByTestFile=179; StrongMapped=0; AmbiguousMatched=0. | Update UTS to reference the correct test file(s) and/or add unit tests that exercise the Design Unit’s functions; add explicit per-test trace tags to link UTS Test Case IDs ↔ gtest cases. |
| F-012 | MAJOR | DES-MAIN-PD-PT01 | All detected test-case matches are ambiguous; no uniquely traceable unit tests for this Design Unit. | UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15. | Add per-test trace tags (UTS Test Case IDs in code) and/or split shared test files per Design Unit; then update UTS to disambiguate mapping at test-case level. |
| F-013 | MAJOR | DES-MAIN-PD-PT03 | All detected test-case matches are ambiguous; no uniquely traceable unit tests for this Design Unit. | UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15. | Add per-test trace tags (UTS Test Case IDs in code) and/or split shared test files per Design Unit; then update UTS to disambiguate mapping at test-case level. |
| F-014 | MAJOR | DES-MAIN-PD-PT04 | All detected test-case matches are ambiguous; no uniquely traceable unit tests for this Design Unit. | UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=2. | Add per-test trace tags (UTS Test Case IDs in code) and/or split shared test files per Design Unit; then update UTS to disambiguate mapping at test-case level. |
| F-015 | MINOR | DES-MAIN-PD-AL01 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdAlarms - ThresholdChecks' vs SDD Unit='ThresholdChecks'. | Align unit naming between UTS and SDD (or document rationale). |
| F-016 | MINOR | DES-MAIN-PD-AL02 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdAlarms - Debounce' vs SDD Unit='Debounce'. | Align unit naming between UTS and SDD (or document rationale). |
| F-017 | MINOR | DES-MAIN-PD-AL03 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdAlarms - DwellTimers' vs SDD Unit='DwellTimers'. | Align unit naming between UTS and SDD (or document rationale). |
| F-018 | MINOR | DES-MAIN-PD-AL04 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdAlarms - AlarmPublisher / O2 Alarms' vs SDD Unit='AlarmPublisher / O2 Alarms'. | Align unit naming between UTS and SDD (or document rationale). |
| F-019 | MINOR | DES-MAIN-PD-PE01 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='Ptdata - BreathPhaseProcessor' vs SDD Unit='BreathPhaseProcessor'. | Align unit naming between UTS and SDD (or document rationale). |
| F-020 | MINOR | DES-MAIN-PD-PE02 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='Ptdata - O2Limits' vs SDD Unit='O2Limits'. | Align unit naming between UTS and SDD (or document rationale). |
| F-021 | MINOR | DES-MAIN-PD-PE03 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='Ptdata - MetricsCalculator' vs SDD Unit='MetricsCalculator'. | Align unit naming between UTS and SDD (or document rationale). |
| F-022 | MINOR | DES-MAIN-PD-PE04 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='Ptdata - GuiPublisher' vs SDD Unit='GuiPublisher'. | Align unit naming between UTS and SDD (or document rationale). |
| F-023 | MINOR | DES-MAIN-PD-PE05 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='Ptdata - LogPublisher' vs SDD Unit='LogPublisher'. | Align unit naming between UTS and SDD (or document rationale). |
| F-024 | MINOR | DES-MAIN-PD-PT01 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdTask - Event Loop Unit' vs SDD Unit='EventLoop'. | Align unit naming between UTS and SDD (or document rationale). |
| F-025 | MINOR | DES-MAIN-PD-PT02 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdTask - IPC Receive Unit (message gateway)' vs SDD Unit='IPCReceive'. | Align unit naming between UTS and SDD (or document rationale). |
| F-026 | MINOR | DES-MAIN-PD-PT03 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdTask - Dispatcher Unit' vs SDD Unit='Dispatcher'. | Align unit naming between UTS and SDD (or document rationale). |
| F-027 | MINOR | DES-MAIN-PD-PT04 | Unit name differs between UTS and SDD for the same Design Unit ID. | UTS Unit='PdTask - Mode Status Unit' vs SDD Unit='ModeStatus'. | Align unit naming between UTS and SDD (or document rationale). |
| F-028 | OBSERVATION | N/A | UTS refers to an external traceability matrix; SRS↔UTS traceability not verifiable from provided inputs alone. | UTS contains 'Refer to Traceability matrix' text; no direct SRS requirement IDs were detected in UTS by simple string search. | Provide the referenced traceability matrix artifact and ensure it is configuration-controlled with the same baseline as UTS/SDD and repo revision. |

## Proposed Fix Details (Per Finding)

### F-001

- Severity: MAJOR
- DesignUnitID: N/A
- Issue: Repository contains Pd unit test files not referenced by UTS (orphan/untraceable tests).
- Evidence: Repo scan: 1 orphan files under Test/Pd; top: Test/Pd/PtDataTest/PtDataTest (copy).cpp(1)
- Recommended actions (audit-ready):
  - Decide disposition of the orphan test file(s): add to UTS scope with Design Unit mapping, or remove/relocate if unintended.
  - Ensure configuration control: the UTS baseline must match the repository revision containing the test files.
- Proposed fix (summary): Update UTS to reference these test files and map them to Design Units, or remove/relocate tests that are out-of-scope.

### F-002

- Severity: MAJOR
- DesignUnitID: MULTI
- Issue: Same unit test file is referenced by multiple Design Units; mapping is ambiguous at test-case level.
- Evidence: UTS-referenced file `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` mapped to DesignUnitIDs: DES-MAIN-PD-AL01, DES-MAIN-PD-AL02, DES-MAIN-PD-AL03, DES-MAIN-PD-AL04.
- Recommended actions (audit-ready):
  - Choose one: (A) split the shared test file into per-Design-Unit test files, or (B) add per-test trace tags in code that reference UTS Test Case IDs.
  - Update UTS to map each Design Unit to either a dedicated test file or explicit gtest case names.
  - Re-run this audit to confirm StrongMappedCount increases and ambiguity decreases.
- Proposed fix (summary): Add per-test trace tags in code (e.g., comments with UTS Test Case IDs) or split tests per Design Unit and update UTS accordingly.

### F-003

- Severity: MAJOR
- DesignUnitID: MULTI
- Issue: Same unit test file is referenced by multiple Design Units; mapping is ambiguous at test-case level.
- Evidence: UTS-referenced file `Test/Pd/PdTask/PdTaskTest.cpp` mapped to DesignUnitIDs: DES-MAIN-PD-PT01, DES-MAIN-PD-PT02, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04.
- Recommended actions (audit-ready):
  - Choose one: (A) split the shared test file into per-Design-Unit test files, or (B) add per-test trace tags in code that reference UTS Test Case IDs.
  - Update UTS to map each Design Unit to either a dedicated test file or explicit gtest case names.
  - Re-run this audit to confirm StrongMappedCount increases and ambiguity decreases.
- Proposed fix (summary): Add per-test trace tags in code (e.g., comments with UTS Test Case IDs) or split tests per Design Unit and update UTS accordingly.

### F-004

- Severity: MAJOR
- DesignUnitID: MULTI
- Issue: Same unit test file is referenced by multiple Design Units; mapping is ambiguous at test-case level.
- Evidence: UTS-referenced file `Test/Pd/PtDataTest/PtDataTest.cpp` mapped to DesignUnitIDs: DES-MAIN-PD-PE01, DES-MAIN-PD-PE02, DES-MAIN-PD-PE03, DES-MAIN-PD-PE04, DES-MAIN-PD-PE05.
- Recommended actions (audit-ready):
  - Choose one: (A) split the shared test file into per-Design-Unit test files, or (B) add per-test trace tags in code that reference UTS Test Case IDs.
  - Update UTS to map each Design Unit to either a dedicated test file or explicit gtest case names.
  - Re-run this audit to confirm StrongMappedCount increases and ambiguity decreases.
- Proposed fix (summary): Add per-test trace tags in code (e.g., comments with UTS Test Case IDs) or split tests per Design Unit and update UTS accordingly.

### F-005

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PE01
- Issue: Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch).
- Evidence: UTS source evidence: UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[99]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::PdSpontaneous:MISMATCH(similar-name 'PdSpontaneous' found Pd/ptdata.cpp:307, qualified symbol not found);Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::CompExhTidalVolumeInAPRV:Pd/ptdata.cpp:385
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it.

### F-006

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PE03
- Issue: Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch).
- Evidence: UTS source evidence: UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[109]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::MaxVteArray:MISMATCH(similar-name 'MaxVteArray' found Pd/ptdata.cpp:3068, qualified symbol not found);Ptdata::MaxVteArray12:MISMATCH(similar-name 'MaxVteArray12' found Pd/ptdata.cpp:3113, qualified symbol not found);Ptdata::MinVteArray:MISMATCH(similar-name 'MinVteArray' found Pd/ptdata.cpp:3056, qualified symbol not found);Ptdata::MinVteArray12:MISMATCH(similar-name 'MinVteArray12' found Pd/ptdata.cpp:3112, qualified symbol not found);Ptdata::BlankingData:Pd/ptdata.cpp:2624;Ptdata::ClearExhLeakArray:Pd/ptdata.cpp:3753;Ptdata::ClearVteBuffer:Pd/ptdata.cpp:3338
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it.

### F-007

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PE05
- Issue: Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch).
- Evidence: UTS source evidence: UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[115]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: PdTask::TestLogFileWriting:MISMATCH(found Pd/pdtask.cpp:206 not in UTS SourceFile_UTS list)
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it.

### F-008

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PT01
- Issue: Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch).
- Evidence: UTS source evidence: UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[86]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::ClearHistory:MISMATCH(found Pd/ptdata.cpp:2236 not in UTS SourceFile_UTS list);PdTask::TestLogFileWriting:Pd/pdtask.cpp:206;PdTask::run:Pd/pdtask.cpp:42
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it.

### F-009

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PT02
- Issue: Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch).
- Evidence: UTS source evidence: UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[89]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: PdTask::S_SetEventFlag:MISMATCH(found Pd/pdtask.h:115 not in UTS SourceFile_UTS list);PdTask::initMsgQueue:Pd/pdtask.cpp:226
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it.

### F-010

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PT03
- Issue: Qualified function(s) referenced by UTS/SDD are not found in UTS-listed source file(s) but exist elsewhere in Pd (UTS↔repo file mapping mismatch).
- Evidence: UTS source evidence: UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File); SDD function evidence: SDD:blocks[93]/Table/Body/Row[4]/Col[1] (関数 / Function：); Repo search: Ptdata::ClearHistory:MISMATCH(found Pd/ptdata.cpp:2236 not in UTS SourceFile_UTS list);PdTask::run:Pd/pdtask.cpp:42
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Update UTS source-file list to include the actual implementing file(s) and/or adjust SDD function listing; prefer linking each UTS Test Case ID to the concrete gtest that covers it.

### F-011

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PE05
- Issue: No evidence-based unit test cases mapped to this Design Unit (0 strong, 0 ambiguous) within UTS-referenced test file(s).
- Evidence: UTS TestFile_UTS=Test/Pd/PtDataTest/PtDataTest.cpp; RepoTestCaseCount_ByTestFile=179; StrongMapped=0; AmbiguousMatched=0.
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Update UTS to reference the correct test file(s) and/or add unit tests that exercise the Design Unit’s functions; add explicit per-test trace tags to link UTS Test Case IDs ↔ gtest cases.

### F-012

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PT01
- Issue: All detected test-case matches are ambiguous; no uniquely traceable unit tests for this Design Unit.
- Evidence: UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15.
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Add per-test trace tags (UTS Test Case IDs in code) and/or split shared test files per Design Unit; then update UTS to disambiguate mapping at test-case level.

### F-013

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PT03
- Issue: All detected test-case matches are ambiguous; no uniquely traceable unit tests for this Design Unit.
- Evidence: UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=15.
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Add per-test trace tags (UTS Test Case IDs in code) and/or split shared test files per Design Unit; then update UTS to disambiguate mapping at test-case level.

### F-014

- Severity: MAJOR
- DesignUnitID: DES-MAIN-PD-PT04
- Issue: All detected test-case matches are ambiguous; no uniquely traceable unit tests for this Design Unit.
- Evidence: UTS TestFile_UTS=Test/Pd/PdTask/PdTaskTest.cpp; RepoTestCaseCount_ByTestFile=30; StrongMapped=0; AmbiguousMatched=2.
- Recommended actions (audit-ready):
  - Update UTS SourceFile_UTS list to include the actual implementing file(s) indicated by repo evidence, or update SDD function naming to match the repository.
  - For renamed/mismatched symbols, record the rationale (rename history) and update SDD/UTS accordingly.
  - Add a traceability mechanism: comment tags containing UTS Test Case IDs near each gtest case (or split tests per unit).
- Proposed fix (summary): Add per-test trace tags (UTS Test Case IDs in code) and/or split shared test files per Design Unit; then update UTS to disambiguate mapping at test-case level.

### F-015

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-AL01
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdAlarms - ThresholdChecks' vs SDD Unit='ThresholdChecks'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-016

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-AL02
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdAlarms - Debounce' vs SDD Unit='Debounce'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-017

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-AL03
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdAlarms - DwellTimers' vs SDD Unit='DwellTimers'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-018

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-AL04
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdAlarms - AlarmPublisher / O2 Alarms' vs SDD Unit='AlarmPublisher / O2 Alarms'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-019

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PE01
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='Ptdata - BreathPhaseProcessor' vs SDD Unit='BreathPhaseProcessor'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-020

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PE02
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='Ptdata - O2Limits' vs SDD Unit='O2Limits'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-021

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PE03
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='Ptdata - MetricsCalculator' vs SDD Unit='MetricsCalculator'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-022

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PE04
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='Ptdata - GuiPublisher' vs SDD Unit='GuiPublisher'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-023

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PE05
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='Ptdata - LogPublisher' vs SDD Unit='LogPublisher'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-024

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PT01
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdTask - Event Loop Unit' vs SDD Unit='EventLoop'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-025

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PT02
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdTask - IPC Receive Unit (message gateway)' vs SDD Unit='IPCReceive'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-026

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PT03
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdTask - Dispatcher Unit' vs SDD Unit='Dispatcher'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-027

- Severity: MINOR
- DesignUnitID: DES-MAIN-PD-PT04
- Issue: Unit name differs between UTS and SDD for the same Design Unit ID.
- Evidence: UTS Unit='PdTask - Mode Status Unit' vs SDD Unit='ModeStatus'.
- Recommended actions (audit-ready):
  - Align naming (UTS unit titles vs SDD unit names) to reduce review ambiguity; if intentional, document rationale.
- Proposed fix (summary): Align unit naming between UTS and SDD (or document rationale).

### F-028

- Severity: OBSERVATION
- DesignUnitID: N/A
- Issue: UTS refers to an external traceability matrix; SRS↔UTS traceability not verifiable from provided inputs alone.
- Evidence: UTS contains 'Refer to Traceability matrix' text; no direct SRS requirement IDs were detected in UTS by simple string search.
- Recommended actions (audit-ready):
  - Provide the referenced external traceability matrix and baseline-link it to UTS/SDD and repo revision.
- Proposed fix (summary): Provide the referenced traceability matrix artifact and ensure it is configuration-controlled with the same baseline as UTS/SDD and repo revision.

## Per-Design-Unit Deep Dive

### DES-MAIN-PD-AL01

**UTS claims (evidence pointers)**

- Unit name: `PdAlarms - ThresholdChecks`
- Source files:
  - `Pd/pdalarms.cpp` (UTS:blocks[91]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[91]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[91]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ CheckHiPip(); CheckHiPipEndExh()` (UTS:blocks[91]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 10 (sample: DES-MAIN-PD-AL01-TC01, DES-MAIN-PD-AL01-TC02, DES-MAIN-PD-AL01-TC03, DES-MAIN-PD-AL01-TC04, DES-MAIN-PD-AL01-TC05)

**SDD says (evidence pointers)**

- Unit name: `ThresholdChecks`
- Functions:
  - `void PdAlarms::CheckHiPip(LONG hilimit, LONG value, AlarmStat *alarmtype); void PdAlarms::CheckHiPipEndExh(LONG hilimit, LONG value, AlarmStat *alarmtype);` (SDD:blocks[118]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdalarms.cpp` exists: OK
- `Pd/pdalarms.h` exists: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` exists: OK
  - TEST macros found: 190; sample: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_SharedAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231
- Function search hits (token:file:line): PdAlarms::CheckHiPip:Pd/pdalarms.cpp:631;PdAlarms::CheckHiPipEndExh:Pd/pdalarms.cpp:662

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 10
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1784` `PdAlarmsFixture.CheckHiPip_Clears_When_Value_Is_Below_Limit` (match tokens: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1796` `PdAlarmsFixture.CheckHiPip_NoOp_When_Value_Equals_Limit` (match tokens: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1808` `PdAlarmsFixture.CheckHiPip_NoOp_When_Value_Above_Limit` (match tokens: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1820` `PdAlarmsFixture.CheckHiPip_Idempotent_Clear_If_Already_NotActive` (match tokens: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1832` `PdAlarmsFixture.CheckHiPip_Never_Activates` (match tokens: CheckHiPip)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1851` `PdAlarmsFixture.CheckHiPipEndExh_Clears_When_Below_Limit_And_Active` (match tokens: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1863` `PdAlarmsFixture.CheckHiPipEndExh_NoChange_When_Below_Limit_And_Already_NotActive` (match tokens: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1875` `PdAlarmsFixture.CheckHiPipEndExh_NoOp_When_Value_Equals_Limit` (match tokens: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1887` `PdAlarmsFixture.CheckHiPipEndExh_NoOp_When_Value_Above_Limit` (match tokens: CheckHiPip, CheckHiPipEndExh)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1899` `PdAlarmsFixture.CheckHiPipEndExh_Never_Activates` (match tokens: CheckHiPip, CheckHiPipEndExh)
- Ambiguous matched test cases (multiple units matched): 0
- Unassigned test cases in this unit’s referenced test file(s): 26 (see report section C totals)

**Conclusion:** AMBIGUOUS

### DES-MAIN-PD-AL02

**UTS claims (evidence pointers)**

- Unit name: `PdAlarms - Debounce`
- Source files:
  - `Pd/pdalarms.cpp` (UTS:blocks[93]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[93]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[93]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ CheckHiCondition(); CheckLowCondition(); CheckLowPip()` (UTS:blocks[93]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 22 (sample: DES-MAIN-PD-AL02-TC01, DES-MAIN-PD-AL02-TC02, DES-MAIN-PD-AL02-TC03, DES-MAIN-PD-AL02-TC04, DES-MAIN-PD-AL02-TC05)

**SDD says (evidence pointers)**

- Unit name: `Debounce`
- Functions:
  - `void PdAlarms::CheckHiCondition(LONG hilimit, LONG value, AlarmStat *alarmtype, int SensitiveValue = 0); void PdAlarms::CheckLowCondition(LONG lowlimit, LONG value, AlarmStat *alarmtype, int SensitiveValue = 0); void PdAlarms::CheckLowPip(LONG lowlimit, LONG value, AlarmStat *alarmtype);` (SDD:blocks[120]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdalarms.cpp` exists: OK
- `Pd/pdalarms.h` exists: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` exists: OK
  - TEST macros found: 190; sample: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_SharedAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231
- Function search hits (token:file:line): PdAlarms::CheckHiCondition:Pd/pdalarms.cpp:105;PdAlarms::CheckLowCondition:Pd/pdalarms.cpp:152;PdAlarms::CheckLowPip:Pd/pdalarms.cpp:696

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 22
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:69` `PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive` (match tokens: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:105` `PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity` (match tokens: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:122` `PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal` (match tokens: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:146` `PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh` (match tokens: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:161` `PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately` (match tokens: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:179` `PdAlarmsFixture.CheckHiCondition_StaticCounter_SharedAcross_Alarms` (match tokens: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:203` `PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter` (match tokens: CheckHiCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:231` `PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive` (match tokens: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:266` `PdAlarmsFixture.CheckLowCondition_DoesNotActivate_Before_Sensitivity` (match tokens: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:282` `PdAlarmsFixture.CheckLowCondition_StaysActive_On_ContinuedLows_And_Clears_On_Normal` (match tokens: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:306` `PdAlarmsFixture.CheckLowCondition_LimitEquality_IsNormal` (match tokens: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:320` `PdAlarmsFixture.CheckLowCondition_Sensitivity1_ActivatesImmediately` (match tokens: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:338` `PdAlarmsFixture.CheckLowCondition_StaticCounter_SharedAcross_Alarms` (match tokens: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:362` `PdAlarmsFixture.CheckLowCondition_NormalWhileInactive_DoesNotResetCounter` (match tokens: CheckLowCondition)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1923` `PdAlarmsFixture.CheckLowPip_Activates_On_Second_Consecutive_Low` (match tokens: CheckLowPip)
- Ambiguous matched test cases (multiple units matched): 0
- Unassigned test cases in this unit’s referenced test file(s): 26 (see report section C totals)

**Conclusion:** AMBIGUOUS

### DES-MAIN-PD-AL03

**UTS claims (evidence pointers)**

- Unit name: `PdAlarms - DwellTimers`
- Source files:
  - `Pd/pdalarms.cpp` (UTS:blocks[96]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[96]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[96]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ CheckLowAmlpitude(); CheckHighAmlpitude(); CheckLowBaseLine(); CheckLowBaseLine2Secs(); CheckHiBaseLine(); CheckHiBaseLine2Secs() ・ CheckLowMap(); CheckHiMap(); CheckLowMinuteVol(); CheckHiMinuteVol(); CheckHiBreathRate() ・ CheckLow_PLow(); CheckHigh_PLow(); CheckLowPLow5Time(); CheckHighPLow5Time() ・ CheckLow_PHigh(); CheckHigh_PHigh(); CheckLowPHigh5Time(); CheckHighPHigh5Time(); ResetAlarmActiveTime()` (UTS:blocks[96]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 138 (sample: DES-MAIN-PD-AL03-TC001, DES-MAIN-PD-AL03-TC002, DES-MAIN-PD-AL03-TC003, DES-MAIN-PD-AL03-TC004, DES-MAIN-PD-AL03-TC005)

**SDD says (evidence pointers)**

- Unit name: `DwellTimers`
- Functions:
  - `void PdAlarms::CheckLowAmlpitude(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHighAmlpitude(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowBaseLine(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowBaseLine2Secs(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHiBaseLine(LONG hilimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiBaseLine2Secs(LONG lowlimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckLowMap(LONG lowlimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiMap(LONG hilimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckLowMinuteVol(LONG lowlimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiMinuteVol(LONG hilimit, LONG value, AlarmStat alarmtype); void PdAlarms::CheckHiBreathRate(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLow_PLow(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHigh_PLow(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowPLow5Time(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHighPLow5Time(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLow_PHigh(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHigh_PHigh(LONG hilimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckLowPHigh5Time(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::CheckHighPHigh5Time(LONG lowlimit, LONG value, AlarmStat* alarmtype); void PdAlarms::ResetAlarmActiveTime();` (SDD:blocks[122]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdalarms.cpp` exists: OK
- `Pd/pdalarms.h` exists: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` exists: OK
  - TEST macros found: 190; sample: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_SharedAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231
- Function search hits (token:file:line): PdAlarms::CheckHiBaseLine:Pd/pdalarms.cpp:535;PdAlarms::CheckHiBaseLine2Secs:Pd/pdalarms.cpp:578;PdAlarms::CheckHiBreathRate:Pd/pdalarms.cpp:923;PdAlarms::CheckHiMap:Pd/pdalarms.cpp:788;PdAlarms::CheckHiMinuteVol:Pd/pdalarms.cpp:878;PdAlarms::CheckHighAmlpitude:Pd/pdalarms.cpp:399;PdAlarms::CheckHighPHigh5Time:Pd/pdalarms.cpp:1275;PdAlarms::CheckHighPLow5Time:Pd/pdalarms.cpp:1101;PdAlarms::CheckHigh_PHigh:Pd/pdalarms.cpp:1189;PdAlarms::CheckHigh_PLow:Pd/pdalarms.cpp:1054;PdAlarms::CheckLowAmlpitude:Pd/pdalarms.cpp:353;PdAlarms::CheckLowBaseLine:Pd/pdalarms.cpp:445;PdAlarms::CheckLowBaseLine2Secs:Pd/pdalarms.cpp:487;PdAlarms::CheckLowMap:Pd/pdalarms.cpp:742;PdAlarms::CheckLowMinuteVol:Pd/pdalarms.cpp:832;PdAlarms::CheckLowPHigh5Time:Pd/pdalarms.cpp:1233;PdAlarms::CheckLowPLow5Time:Pd/pdalarms.cpp:1011;PdAlarms::CheckLow_PHigh:Pd/pdalarms.cpp:1144;PdAlarms::CheckLow_PLow:Pd/pdalarms.cpp:966;PdAlarms::ResetAlarmActiveTime:Pd/pdalarms.h:84(member-in-header)

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 112
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1192` `PdAlarmsFixture.CheckLowBaseLine_Activates_After_5_Consecutive_Lows` (match tokens: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1213` `PdAlarmsFixture.CheckLowBaseLine_DoesNotActivate_Before_5_Lows_And_NormalBreaks` (match tokens: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1237` `PdAlarmsFixture.CheckLowBaseLine_LimitEquality_IsNormal_And_Clears_IfActive` (match tokens: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1261` `PdAlarmsFixture.CheckLowBaseLine_Alternating_Low_And_Normal_DoesNotActivate` (match tokens: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1277` `PdAlarmsFixture.CheckLowBaseLine_StaysActive_On_Continued_Lows_Then_Clears_On_Normal` (match tokens: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1300` `PdAlarmsFixture.CheckLowBaseLine_MemberCounter_SharedAcross_AlarmObjects` (match tokens: CheckLowBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1345` `PdAlarmsFixture.CheckLowBaseLine2Secs_Activates_After_2s_Of_Consecutive_Lows` (match tokens: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1362` `PdAlarmsFixture.CheckLowBaseLine2Secs_Clears_After_2s_Of_Consecutive_Highs` (match tokens: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1383` `PdAlarmsFixture.CheckLowBaseLine2Secs_EqualityIsNoOp_NoActivation_NoClearing` (match tokens: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1407` `PdAlarmsFixture.CheckLowBaseLine2Secs_Alternating_Low_High_DoesNotActivateOrClear` (match tokens: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1434` `PdAlarmsFixture.CheckLowBaseLine2Secs_MemberCounters_SharedAcross_AlarmObjects` (match tokens: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1460` `PdAlarmsFixture.CheckLowBaseLine2Secs_OffByOne_StepSize2_ToHitThreshold` (match tokens: CheckLowBaseLine, CheckLowBaseLine2Secs)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1493` `PdAlarmsFixture.CheckHiBaseLine_Activates_After_5_Consecutive_Highs` (match tokens: CheckHiBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1514` `PdAlarmsFixture.CheckHiBaseLine_DoesNotActivate_Before_5_Highs_And_NormalBreaks` (match tokens: CheckHiBaseLine)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:1539` `PdAlarmsFixture.CheckHiBaseLine_LimitEquality_IsNormal_And_Clears_IfActive` (match tokens: CheckHiBaseLine)
- Ambiguous matched test cases (multiple units matched): 0
- Unassigned test cases in this unit’s referenced test file(s): 26 (see report section C totals)

**Conclusion:** AMBIGUOUS

### DES-MAIN-PD-AL04

**UTS claims (evidence pointers)**

- Unit name: `PdAlarms - AlarmPublisher / O2 Alarms`
- Source files:
  - `Pd/pdalarms.cpp` (UTS:blocks[99]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/pdalarms.h` (UTS:blocks[99]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` (UTS:blocks[99]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ CheckHiO2(); CheckLowO2(); CheckHiInternalO2(); ResetO2Time()` (UTS:blocks[99]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 20 (sample: DES-MAIN-PD-AL04-TC01, DES-MAIN-PD-AL04-TC02, DES-MAIN-PD-AL04-TC03, DES-MAIN-PD-AL04-TC04, DES-MAIN-PD-AL04-TC05)

**SDD says (evidence pointers)**

- Unit name: `AlarmPublisher / O2 Alarms`
- Functions:
  - `void CheckHiO2(LONG hilimit, LONG value, AlarmStat*); void CheckLowO2(LONG lowlimit, LONG value, AlarmStat*); void ResetO2Time();` (SDD:blocks[124]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdalarms.cpp` exists: OK
- `Pd/pdalarms.h` exists: OK
- `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp` exists: OK
  - TEST macros found: 190; sample: PdAlarmsFixture.CheckHiCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@69, PdAlarmsFixture.CheckHiCondition_DoesNotActivate_Before_Sensitivity@105, PdAlarmsFixture.CheckHiCondition_StaysActive_On_ContinuedHighs_And_Clears_On_Normal@122, PdAlarmsFixture.CheckHiCondition_LimitEquality_IsNotHigh@146, PdAlarmsFixture.CheckHiCondition_Sensitivity1_ActivatesImmediately@161, PdAlarmsFixture.CheckHiCondition_StaticCounter_SharedAcross_Alarms@179, PdAlarmsFixture.CheckHiCondition_NormalWhileInactive_DoesNotResetCounter@203, PdAlarmsFixture.CheckLowCondition_Activates_After_Reaching_Sensitivity_NonConsecutive@231

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 20
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:403` `PdAlarmsFixture.CheckHiO2_Activates_After_THIRTY_SECONDS_HighSamples` (match tokens: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:425` `PdAlarmsFixture.CheckHiO2_Resets_After_O2RESETTIMELIMIT_LowSamples` (match tokens: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:458` `PdAlarmsFixture.CheckHiO2_Alternating_Around_Threshold_DoesNotActivate` (match tokens: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:475` `PdAlarmsFixture.CheckHiO2_Equality_IsHigh_For_Activation_But_NotFor_Reset` (match tokens: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:500` `PdAlarmsFixture.CheckHiO2_StaticCounters_SharedAcross_Alarms` (match tokens: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:544` `PdAlarmsFixture.CheckHiO2_StaysActive_On_HighOrEqual_Only_LowContributesToReset` (match tokens: CheckHiO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:586` `PdAlarmsFixture.CheckLowO2_Activates_After_THIRTY_SECONDS_LowSamples` (match tokens: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:606` `PdAlarmsFixture.CheckLowO2_Resets_After_Consecutive_HighSamples_RuntimeDetected` (match tokens: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:636` `PdAlarmsFixture.CheckLowO2_Alternating_Around_Threshold_DoesNotActivate` (match tokens: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:654` `PdAlarmsFixture.CheckLowO2_Equality_IsLow_For_Activation_But_NotFor_Reset` (match tokens: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:684` `PdAlarmsFixture.CheckLowO2_StaticCounters_SharedAcross_Alarms` (match tokens: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:721` `PdAlarmsFixture.CheckLowO2_StaysActive_On_LowOrEqual_Only_HighContributesToReset` (match tokens: CheckLowO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:762` `PdAlarmsFixture.CheckHiInternalO2_Activates_After_Consecutive_HighSamples_RuntimeDetected` (match tokens: CheckHiInternalO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:782` `PdAlarmsFixture.CheckHiInternalO2_Resets_Immediately_On_Single_Low` (match tokens: CheckHiInternalO2)
  - `Test/Pd/PdAlarmsTest/PdAlarmsTest.cpp:803` `PdAlarmsFixture.CheckHiInternalO2_OffByOne_EqualityCountsAsHigh_681IsLow` (match tokens: CheckHiInternalO2)
- Ambiguous matched test cases (multiple units matched): 0
- Unassigned test cases in this unit’s referenced test file(s): 26 (see report section C totals)

**Conclusion:** AMBIGUOUS

### DES-MAIN-PD-PE01

**UTS claims (evidence pointers)**

- Unit name: `Ptdata - BreathPhaseProcessor`
- Source files:
  - `Pd/ptdata.cpp` (UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[82]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `void Ptdata::DoInhalationData(); void Ptdata::DoExhalationData(); void Ptdata::BuildInhMsg(); void Ptdata::BuildExhMsg(); void Ptdata::CompNumBreaths(); void Ptdata::DoEndExhPSInAPRV(); void Ptdata::DoEndInhPSInAPRV(); void Ptdata::DoEndInhExtra(); void Ptdata::DoAPRV_PLow(); void Ptdata::DoAPRV_PHigh(); void Ptdata::CompExhTidalVolumeInAPRV(); bool Ptdata::PdSpontaneous();` (UTS:blocks[82]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 36 (sample: DES-MAIN-PD-PE01-TC01, DES-MAIN-PD-PE01-TC02, DES-MAIN-PD-PE01-TC03, DES-MAIN-PD-PE01-TC04, DES-MAIN-PD-PE01-TC05)

**SDD says (evidence pointers)**

- Unit name: `BreathPhaseProcessor`
- Functions:
  - `void Ptdata::DoInhalationData(); void Ptdata::DoExhalationData(); void Ptdata::BuildInhMsg(); void Ptdata::BuildExhMsg(); void Ptdata::CompNumBreaths(); void Ptdata::DoEndExhPSInAPRV(); void Ptdata::DoEndInhPSInAPRV(); void Ptdata::DoEndInhExtra(); void Ptdata::DoAPRV_PLow(); void Ptdata::DoAPRV_PHigh(); void Ptdata::CompExhTidalVolumeInAPRV(); bool Ptdata::PdSpontaneous();` (SDD:blocks[99]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/ptdata.cpp` exists: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` exists: OK
  - TEST macros found: 179; sample: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Function search hits (token:file:line): Ptdata::PdSpontaneous:MISMATCH(similar-name 'PdSpontaneous' found Pd/ptdata.cpp:307, qualified symbol not found);Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::CompExhTidalVolumeInAPRV:Pd/ptdata.cpp:385;Ptdata::CompNumBreaths:Pd/ptdata.cpp:286;Ptdata::DoAPRV_PHigh:Pd/ptdata.cpp:2146;Ptdata::DoAPRV_PLow:Pd/ptdata.cpp:1966;Ptdata::DoEndExhPSInAPRV:Pd/ptdata.cpp:3781;Ptdata::DoEndInhExtra:Pd/ptdata.cpp:4119;Ptdata::DoEndInhPSInAPRV:Pd/ptdata.cpp:3898;Ptdata::DoExhalationData:Pd/ptdata.cpp:1689;Ptdata::DoInhalationData:Pd/ptdata.cpp:1432

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 3
  - `Test/Pd/PtDataTest/PtDataTest.cpp:148` `PtDataFixture.Sanity_PdSpontaneousMapping` (match tokens: PdSpontaneous)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:678` `PtDataExhalationFixture.DoExhalationData_NonAprvAdvancesIndexAndComputesInhMinuteVolume` (match tokens: DoExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:712` `PtDataExhalationFixture.DoExhalationData_AprvSkipsInhMinuteVolume` (match tokens: DoExhalationData)
- Ambiguous matched test cases (multiple units matched): 4
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3675` `PtDataFixture.BuildInhMsg_ProxyReadyUsesActualValues` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3709` `PtDataFixture.BuildInhMsg_ProxyErrorBlanksVolumes` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3772` `PtDataFixture.BuildExhMsg_ProxyReadyCopiesValues` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3815` `PtDataFixture.BuildExhMsg_ProxyErrorBlanksVolumes` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
- Unassigned test cases in this unit’s referenced test file(s): 92 (see report section C totals)

**Conclusion:** MISMATCH

### DES-MAIN-PD-PE02

**UTS claims (evidence pointers)**

- Unit name: `Ptdata - O2Limits`
- Source files:
  - `Pd/ptdata.cpp` (UTS:blocks[84]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[84]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ DoO2()` (UTS:blocks[84]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 4 (sample: DES-MAIN-PD-PE02-TC01, DES-MAIN-PD-PE02-TC02, DES-MAIN-PD-PE02-TC03, DES-MAIN-PD-PE02-TC04)

**SDD says (evidence pointers)**

- Unit name: `O2Limits`
- Functions:
  - `void Ptdata::DoO2(); void Ptdata::BuildO2Msg();` (SDD:blocks[103]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/ptdata.cpp` exists: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` exists: OK
  - TEST macros found: 179; sample: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Function search hits (token:file:line): Ptdata::BuildO2Msg:Pd/ptdata.cpp:2563;Ptdata::DoO2:Pd/ptdata.cpp:1790

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 4
  - `Test/Pd/PtDataTest/PtDataTest.cpp:773` `PtDataO2Fixture.DoO2_SensorConnectedComputesAndChecksAlarms` (match tokens: DoO2)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:806` `PtDataO2Fixture.DoO2_SensorDisconnectedBlanksAndResetsAlarms` (match tokens: DoO2)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:828` `PtDataO2Fixture.DoO2_HighReadingClampedToTenThousand` (match tokens: DoO2)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:848` `PtDataO2Fixture.DoO2_StandbyModeReportsNotPresent` (match tokens: DoO2)
- Ambiguous matched test cases (multiple units matched): 1
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3840` `PtDataFixture.BuildO2Msg_PostsConcentrationAndVoltage` (candidates: DES-MAIN-PD-PE02, DES-MAIN-PD-PE04)
- Unassigned test cases in this unit’s referenced test file(s): 92 (see report section C totals)

**Conclusion:** AMBIGUOUS

### DES-MAIN-PD-PE03

**UTS claims (evidence pointers)**

- Unit name: `Ptdata - MetricsCalculator`
- Source files:
  - `Pd/pdhist.h` (UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/ptdata.cpp` (UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[86]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ CompCircuitComplianceVolume() ・ CompCompliance() ・ CompExhTidalVolume() ・ CompExhMinuteVolume(); CompInhMinuteVolume() ・ GetAverageVte12(); GetAverageCompliance12() ・ GetMin/MaxVteArray(); GetMin/MaxComplianceArray12() ・ ClearVteBuffer(); ClearExhLeakArray() ・ UpdateMandExhTidalVolume(); CompRapidShallowBreathix() ・ CompPressureSum(); GetLeakVolume(); GetPessureSum() ・ CompCompliance_InAPRV(); CompAverageCompliance(); GetAverageVte() ・ GetMinCompliance(); GetMaxCompliance(); MinVteArray(); MaxVteArray() ・ MinVteArray12(); MaxVteArray12(); GetMin/MaxVteArray12() ・ ResetLeakAndPress(); CompIeRatio(); CompMeanAirwayPressure(); CompRespRate() ・ GetCircuitComplianceFactor(); BlankingData(); ModeInducedAlarmClearing() ・ CompLeakVolume(); CompLeakCompensate()` (UTS:blocks[86]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 88 (sample: DES-MAIN-PD-PE03-TC01, DES-MAIN-PD-PE03-TC02, DES-MAIN-PD-PE03-TC03, DES-MAIN-PD-PE03-TC04, DES-MAIN-PD-PE03-TC05)

**SDD says (evidence pointers)**

- Unit name: `MetricsCalculator`
- Functions:
  - `LONG Ptdata::CompCircuitComplianceVolume(); void Ptdata::CompCompliance(); void Ptdata::CompExhTidalVolume(); void Ptdata::CompExhMinuteVolume(); void Ptdata::CompInhMinuteVolume(); LONG Ptdata::GetAverageVte12(); DOUBLE Ptdata::GetAverageCompliance12(); LONG Ptdata::GetMinVteArray(); LONG Ptdata::GetMaxVteArray(); DOUBLE Ptdata::GetMinComplianceArray12(); DOUBLE Ptdata::GetMaxComplianceArray12(); void Ptdata::ClearVteBuffer(); void Ptdata::ClearExhLeakArray(); void Ptdata::UpdateMandExhTidalVolume(); LONG Ptdata::CompRapidShallowBreathix(); void Ptdata::CompPressureSum(); LONG Ptdata::GetLeakVolume(); LONG Ptdata::GetPessureSum(); DOUBLE Ptdata::CompCompliance_InAPRV(); DOUBLE Ptdata::CompAverageCompliance(); LONG Ptdata::GetAverageVte(); LONG Ptdata::GetMinCompliance(); LONG Ptdata::GetMaxCompliance(); LONG Ptdata::MinVteArray(); LONG Ptdata::MaxVteArray(); LONG Ptdata::MinVteArray12(); LONG Ptdata::MaxVteArray12(); LONG Ptdata::GetMinVteArray12(); LONG Ptdata::GetMaxVteArray12(); void Ptdata::ResetLeakAndPress(); LONG Ptdata::CompIeRatio(); LONG Ptdata::CompMeanAirwayPressure(); LONG Ptdata::CompRespRate(); LONG Ptdata::GetCircuitComplianceFactor(); void Ptdata::BlankingData(); void Ptdata::ModeInducedAlarmClearing(); void Ptdata::CompLeakVolume(); void Ptdata::CompLeakCompensate();` (SDD:blocks[109]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdhist.h` exists: OK
- `Pd/ptdata.cpp` exists: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` exists: OK
  - TEST macros found: 179; sample: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Function search hits (token:file:line): Ptdata::MaxVteArray:MISMATCH(similar-name 'MaxVteArray' found Pd/ptdata.cpp:3068, qualified symbol not found);Ptdata::MaxVteArray12:MISMATCH(similar-name 'MaxVteArray12' found Pd/ptdata.cpp:3113, qualified symbol not found);Ptdata::MinVteArray:MISMATCH(similar-name 'MinVteArray' found Pd/ptdata.cpp:3056, qualified symbol not found);Ptdata::MinVteArray12:MISMATCH(similar-name 'MinVteArray12' found Pd/ptdata.cpp:3112, qualified symbol not found);Ptdata::BlankingData:Pd/ptdata.cpp:2624;Ptdata::ClearExhLeakArray:Pd/ptdata.cpp:3753;Ptdata::ClearVteBuffer:Pd/ptdata.cpp:3338;Ptdata::CompAverageCompliance:Pd/ptdata.cpp:1538;Ptdata::CompCircuitComplianceVolume:Pd/ptdata.cpp:358;Ptdata::CompCompliance:Pd/ptdata.cpp:1461;Ptdata::CompCompliance_InAPRV:Pd/ptdata.cpp:2025;Ptdata::CompExhMinuteVolume:Pd/ptdata.cpp:588;Ptdata::CompExhTidalVolume:Pd/ptdata.cpp:385;Ptdata::CompIeRatio:Pd/ptdata.cpp:816;Ptdata::CompInhMinuteVolume:Pd/ptdata.cpp:670;Ptdata::CompLeakCompensate:Pd/ptdata.cpp:2929;Ptdata::CompLeakVolume:Pd/ptdata.cpp:3965;Ptdata::CompMeanAirwayPressure:Pd/ptdata.cpp:874;Ptdata::CompPressureSum:Pd/ptdata.cpp:4021;Ptdata::CompRapidShallowBreathix:Pd/ptdata.cpp:776;Ptdata::CompRespRate:Pd/ptdata.cpp:943;Ptdata::GetAverageCompliance12:Pd/ptdata.cpp:3220;Ptdata::GetAverageVte:Pd/ptdata.cpp:3046;Ptdata::GetAverageVte12:Pd/ptdata.cpp:3102;Ptdata::GetCircuitComplianceFactor:Pd/ptdata.cpp:2905;Ptdata::GetLeakVolume:Pd/ptdata.cpp:4049;Ptdata::GetMaxCompliance:Pd/ptdata.cpp:1626;Ptdata::GetMaxComplianceArray12:Pd/ptdata.cpp:3686;Ptdata::GetMaxVteArray:Pd/ptdata.cpp:3425;Ptdata::GetMaxVteArray12:Pd/ptdata.cpp:3539

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 54
  - `Test/Pd/PtDataTest/PtDataTest.cpp:390` `PtDataFixture.CompCompliance_PositiveDeltaUpdatesSamples` (match tokens: CompCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:425` `PtDataFixture.CompCompliance_ZeroDeltaInitializesWarmupBuffer` (match tokens: CompCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:458` `PtDataFixture.CompCompliance_Sample12RemainsZeroUntilFillFlagSet` (match tokens: CompCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:501` `PtDataFixture.CompAverageCompliance_SingleSampleUsesAloneValue` (match tokens: CompAverageCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:512` `PtDataFixture.CompAverageCompliance_SubsetDropsOnlyMinimum` (match tokens: CompAverageCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:527` `PtDataFixture.CompAverageCompliance_FullBufferDropsMinAndMax` (match tokens: CompAverageCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:543` `PtDataFixture.GetMinCompliance_ReturnsMinimumAmongPrefix` (match tokens: GetMinCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:552` `PtDataFixture.GetMinCompliance_IgnoresValuesBeyondIndex` (match tokens: GetMinCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:562` `PtDataFixture.GetMinCompliance_ZeroIndexReturnsMaxLong` (match tokens: GetMinCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:572` `PtDataFixture.GetMaxCompliance_ReturnsMaximumAmongPrefix` (match tokens: GetMaxCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:581` `PtDataFixture.GetMaxCompliance_IgnoresValuesBeyondIndex` (match tokens: GetMaxCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:591` `PtDataFixture.GetMaxCompliance_ZeroIndexReturnsZero` (match tokens: GetMaxCompliance)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:1406` `PtDataFixture.MinVteArray_ReturnsLowestSample` (match tokens: MinVteArray)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:1416` `PtDataFixture.MinVteArray_HandlesNegativeValues` (match tokens: MinVteArray)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:1426` `PtDataFixture.MinVteArray_NoSamplesReturnsSentinel` (match tokens: MinVteArray)
- Ambiguous matched test cases (multiple units matched): 0
- Unassigned test cases in this unit’s referenced test file(s): 92 (see report section C totals)

**Conclusion:** MISMATCH

### DES-MAIN-PD-PE04

**UTS claims (evidence pointers)**

- Unit name: `Ptdata - GuiPublisher`
- Source files:
  - `Pd/pdinterface.h` (UTS:blocks[88]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/ptdata.cpp` (UTS:blocks[88]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[88]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ DoPEEP(); DoNCPAP(); DoHFOData() ・ BuildExhMsg(); BuildInhMsg(); BuildO2Msg() ・ BuildHFOMsg(); BuildHFOSIdata(); BuildAPRVPLow() ・ DoAPRVBR() ・ GetExhalationData(); GetInhalationData() ・ CompProxymalLeak() ・ CalculateHFOExh(); GetEndExhFlow(); GetEndExhFlow_Average() ・ ClearHistory(); GetHFOData()` (UTS:blocks[88]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 41 (sample: DES-MAIN-PD-PE04-TC01, DES-MAIN-PD-PE04-TC02, DES-MAIN-PD-PE04-TC03, DES-MAIN-PD-PE04-TC04, DES-MAIN-PD-PE04-TC05)

**SDD says (evidence pointers)**

- Unit name: `GuiPublisher`
- Functions:
  - `void Ptdata::DoPEEP(void); void Ptdata::DoNCPAP(void); void Ptdata::DoHFOData(void); void Ptdata::BuildExhMsg(void); void Ptdata::BuildInhMsg(void); void Ptdata::BuildO2Msg(void); void Ptdata::BuildHFOMsg(void); void Ptdata::BuilHFOSIdata(int); void Ptdata::BuildAPRVPLow(void); void Ptdata::DoAPRVBR(void); void Ptdata::GetExhalationData(void); void Ptdata::GetInhalationData(void); void Ptdata::CompProxymalLeak(void); void Ptdata::CalculateHFOExh(void); void Ptdata::GetEndExhFlow(void); LONG Ptdata::GetEndExhFlow_Average(void); void Ptdata::ClearHistory(void); void Ptdata::GetHFOData(void);` (SDD:blocks[112]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdinterface.h` exists: OK
- `Pd/ptdata.cpp` exists: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` exists: OK
  - TEST macros found: 179; sample: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Function search hits (token:file:line): Ptdata::BuilHFOSIdata:Pd/ptdata.cpp:2819;Ptdata::BuildAPRVPLow:Pd/ptdata.cpp:2059;Ptdata::BuildExhMsg:Pd/ptdata.cpp:2510;Ptdata::BuildHFOMsg:Pd/ptdata.cpp:2470;Ptdata::BuildInhMsg:Pd/ptdata.cpp:2425;Ptdata::BuildO2Msg:Pd/ptdata.cpp:2563;Ptdata::CalculateHFOExh:Pd/ptdata.cpp:2784;Ptdata::ClearHistory:Pd/ptdata.cpp:2236;Ptdata::CompProxymalLeak:Pd/ptdata.cpp:1246;Ptdata::DoAPRVBR:Pd/ptdata.cpp:2113;Ptdata::DoHFOData:Pd/ptdata.cpp:1659;Ptdata::DoNCPAP:Pd/ptdata.cpp:1917;Ptdata::DoPEEP:Pd/ptdata.cpp:1880;Ptdata::GetEndExhFlow:Pd/ptdata.cpp:2849;Ptdata::GetEndExhFlow_Average:Pd/ptdata.cpp:2874;Ptdata::GetExhalationData:Pd/ptdata.cpp:1069;Ptdata::GetHFOData:Pd/ptdata.cpp:1393;Ptdata::GetInhalationData:Pd/ptdata.cpp:1315

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 21
  - `Test/Pd/PtDataTest/PtDataTest.cpp:619` `PtDataHfoDisabledFixture.DoHFOData_NoOpWhenHfoSystemDisabled` (match tokens: DoHFOData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:644` `PtDataHfoDisabledFixture.DoHFOData_RepeatedCallsMaintainState` (match tokens: DoHFOData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:877` `PtDataPeepFixture.DoPEEP_PostsCurrPressureAndChecksAlarms` (match tokens: DoPEEP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:898` `PtDataPeepFixture.DoPEEP_ClampsNegativePressureToZero` (match tokens: DoPEEP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:938` `PtDataNcpapFixture.DoNCPAP_PostsValueAndTriggersAlarms` (match tokens: DoNCPAP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:960` `PtDataNcpapFixture.DoNCPAP_BlanekdLimitsResetExistingAlarms` (match tokens: DoNCPAP)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2636` `PtDataFixture.GetExhalationData_SetsMode_AndHistory_APRV` (match tokens: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2654` `PtDataFixture.GetExhalationData_AprvAddsExtraCompensatedVolume` (match tokens: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2675` `PtDataFixture.GetExhalationData_SetsMode_AndHistory_NonAPRV` (match tokens: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2691` `PtDataFixture.GetExhalationData_NonAprv_TrimAppliesLeakLimit` (match tokens: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2720` `PtDataFixture.GetExhalationData_NonAprv_ClampNegativeLeakToZero` (match tokens: GetExhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2903` `PtDataFixture.GetInhalationData_NonAprv_PopulatesFieldsAndLimits` (match tokens: GetInhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2920` `PtDataFixture.GetInhalationData_Aprv_NonPsBranchWritesHistory` (match tokens: GetInhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:2936` `PtDataFixture.GetInhalationData_Aprv_PsBranchUsesAPRVValues` (match tokens: GetInhalationData)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3994` `PtDataFixture.GetEndExhFlow_WritesValueAndAdvancesIndex` (match tokens: GetEndExhFlow)
- Ambiguous matched test cases (multiple units matched): 5
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3675` `PtDataFixture.BuildInhMsg_ProxyReadyUsesActualValues` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3709` `PtDataFixture.BuildInhMsg_ProxyErrorBlanksVolumes` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3772` `PtDataFixture.BuildExhMsg_ProxyReadyCopiesValues` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3815` `PtDataFixture.BuildExhMsg_ProxyErrorBlanksVolumes` (candidates: DES-MAIN-PD-PE01, DES-MAIN-PD-PE04)
  - `Test/Pd/PtDataTest/PtDataTest.cpp:3840` `PtDataFixture.BuildO2Msg_PostsConcentrationAndVoltage` (candidates: DES-MAIN-PD-PE02, DES-MAIN-PD-PE04)
- Unassigned test cases in this unit’s referenced test file(s): 92 (see report section C totals)

**Conclusion:** AMBIGUOUS

### DES-MAIN-PD-PE05

**UTS claims (evidence pointers)**

- Unit name: `Ptdata - LogPublisher`
- Source files:
  - `Pd/pdinterface.h` (UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File))
  - `Pd/ptdata.cpp` (UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PtDataTest/PtDataTest.cpp` (UTS:blocks[90]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `(No direct logging API in Ptdata - logging handled in PdTask)` (UTS:blocks[90]/Table/Body/Row[3]/Col[1] (対象関数/Function))

**SDD says (evidence pointers)**

- Unit name: `LogPublisher`
- Functions:
  - `void PdTask::TestLogFileWriting();` (SDD:blocks[115]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdinterface.h` exists: OK
- `Pd/ptdata.cpp` exists: OK
- `Test/Pd/PtDataTest/PtDataTest.cpp` exists: OK
  - TEST macros found: 179; sample: PtDataFixture.StopsImmediatelyOnNoPhase_EmptyHistory@136, PtDataFixture.Sanity_PdSpontaneousMapping@148, PtDataFixture.DirectIndicatorSpontaneous_Increments@156, PtDataFixture.PlateauWithSpontaneousBackup_Increments@170, PtDataFixture.CountsSpontaneousAndPlateauBackupCorrectly@184, PtDataFixture.BreaksOnNoPhaseMidway@209, PtDataFixture.ContinuesWhenJustUnderTimeLimit@222, PtDataFixture.StopsWhenTimeLimitReachedOrExceeded@240
- Function search hits (token:file:line): PdTask::TestLogFileWriting:MISMATCH(found Pd/pdtask.cpp:206 not in UTS SourceFile_UTS list)

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 0
- Ambiguous matched test cases (multiple units matched): 0
- Unassigned test cases in this unit’s referenced test file(s): 92 (see report section C totals)

**Conclusion:** MISMATCH

### DES-MAIN-PD-PT01

**UTS claims (evidence pointers)**

- Unit name: `PdTask - Event Loop Unit`
- Source files:
  - `Pd/pdtask.cpp` (UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[74]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ run() ・ TestLogFileWriting()` (UTS:blocks[74]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 4 (sample: DES-MAIN-PD-PT01-TC01, DES-MAIN-PD-PT01-TC02, DES-MAIN-PD-PT01-TC03, DES-MAIN-PD-PT01-TC04)

**SDD says (evidence pointers)**

- Unit name: `EventLoop`
- Functions:
  - `void PdTask::run(); void PdTask::TestLogFileWriting();` (SDD:blocks[86]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdtask.cpp` exists: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` exists: OK
  - TEST macros found: 30; sample: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Function search hits (token:file:line): Ptdata::ClearHistory:MISMATCH(found Pd/ptdata.cpp:2236 not in UTS SourceFile_UTS list);PdTask::TestLogFileWriting:Pd/pdtask.cpp:206;PdTask::run:Pd/pdtask.cpp:42

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 0
- Ambiguous matched test cases (multiple units matched): 15
  - `Test/Pd/PdTask/PdTaskTest.cpp:354` `PdTaskFixture.Run_NonBreathingMode_CallsClearHistory_AndSkipsElseBranch` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
  - `Test/Pd/PdTask/PdTaskTest.cpp:369` `PdTaskFixture.Run_StartInh_CallsDoExhalationData` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:383` `PdTaskFixture.Run_StartExh_CallsDoInhalationData` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:397` `PdTaskFixture.Run_HFODataReady_CallsDoHFOData` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:410` `PdTaskFixture.Run_UpdateAprvBr_CallsDoAPRVBR` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:423` `PdTaskFixture.Run_UpdateNcpap_CallsDoNCPAP` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:436` `PdTaskFixture.Run_AprvPLowStart_CallsDoAPRV_PHigh` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:449` `PdTaskFixture.Run_AprvPHighStart_CallsDoAPRV_PLow` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:462` `PdTaskFixture.Run_O2SettingThenTimedEvent_CallsDoO2` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:477` `PdTaskFixture.Run_TimedEvent_WithNonBreathing_SkipsDoO2` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
- Unassigned test cases in this unit’s referenced test file(s): 9 (see report section C totals)

**Conclusion:** MISMATCH

### DES-MAIN-PD-PT02

**UTS claims (evidence pointers)**

- Unit name: `PdTask - IPC Receive Unit (message gateway)`
- Source files:
  - `Pd/pdtask.cpp` (UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[76]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ S_SetEventFlag()` (UTS:blocks[76]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 6 (sample: DES-MAIN-PD-PT02-TC01, DES-MAIN-PD-PT02-TC02, DES-MAIN-PD-PT02-TC03, DES-MAIN-PD-PT02-TC04, DES-MAIN-PD-PT02-TC05)

**SDD says (evidence pointers)**

- Unit name: `IPCReceive`
- Functions:
  - `void PdTask::initMsgQueue(); int32_t PdTask::S_SetEventFlag(UNSIGNED flags);` (SDD:blocks[89]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdtask.cpp` exists: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` exists: OK
  - TEST macros found: 30; sample: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Function search hits (token:file:line): PdTask::S_SetEventFlag:MISMATCH(found Pd/pdtask.h:115 not in UTS SourceFile_UTS list);PdTask::initMsgQueue:Pd/pdtask.cpp:226

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 6
  - `Test/Pd/PdTask/PdTaskTest.cpp:228` `PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs` (match tokens: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:251` `PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags` (match tokens: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:269` `PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent` (match tokens: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:284` `PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative` (match tokens: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:299` `PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags` (match tokens: S_SetEventFlag)
  - `Test/Pd/PdTask/PdTaskTest.cpp:315` `PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount` (match tokens: S_SetEventFlag)
- Ambiguous matched test cases (multiple units matched): 0
- Unassigned test cases in this unit’s referenced test file(s): 9 (see report section C totals)

**Conclusion:** MISMATCH

### DES-MAIN-PD-PT03

**UTS claims (evidence pointers)**

- Unit name: `PdTask - Dispatcher Unit`
- Source files:
  - `Pd/pdtask.cpp` (UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[78]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ run() - dispatch mapping` (UTS:blocks[78]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 14 (sample: DES-MAIN-PD-PT03-TC01, DES-MAIN-PD-PT03-TC02, DES-MAIN-PD-PT03-TC03, DES-MAIN-PD-PT03-TC04, DES-MAIN-PD-PT03-TC05)

**SDD says (evidence pointers)**

- Unit name: `Dispatcher`
- Functions:
  - `(within PdTask::run())` (SDD:blocks[93]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdtask.cpp` exists: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` exists: OK
  - TEST macros found: 30; sample: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Function search hits (token:file:line): Ptdata::ClearHistory:MISMATCH(found Pd/ptdata.cpp:2236 not in UTS SourceFile_UTS list);PdTask::run:Pd/pdtask.cpp:42

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 0
- Ambiguous matched test cases (multiple units matched): 15
  - `Test/Pd/PdTask/PdTaskTest.cpp:354` `PdTaskFixture.Run_NonBreathingMode_CallsClearHistory_AndSkipsElseBranch` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
  - `Test/Pd/PdTask/PdTaskTest.cpp:369` `PdTaskFixture.Run_StartInh_CallsDoExhalationData` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:383` `PdTaskFixture.Run_StartExh_CallsDoInhalationData` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:397` `PdTaskFixture.Run_HFODataReady_CallsDoHFOData` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:410` `PdTaskFixture.Run_UpdateAprvBr_CallsDoAPRVBR` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:423` `PdTaskFixture.Run_UpdateNcpap_CallsDoNCPAP` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:436` `PdTaskFixture.Run_AprvPLowStart_CallsDoAPRV_PHigh` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:449` `PdTaskFixture.Run_AprvPHighStart_CallsDoAPRV_PLow` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:462` `PdTaskFixture.Run_O2SettingThenTimedEvent_CallsDoO2` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
  - `Test/Pd/PdTask/PdTaskTest.cpp:477` `PdTaskFixture.Run_TimedEvent_WithNonBreathing_SkipsDoO2` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03)
- Unassigned test cases in this unit’s referenced test file(s): 9 (see report section C totals)

**Conclusion:** MISMATCH

### DES-MAIN-PD-PT04

**UTS claims (evidence pointers)**

- Unit name: `PdTask - Mode Status Unit`
- Source files:
  - `Pd/pdtask.cpp` (UTS:blocks[80]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Unit test implementation files:
  - `Test/Pd/PdTask/PdTaskTest.cpp` (UTS:blocks[80]/Table/Body/Row[1]/Col[1] (ファイル名/File))
- Target functions:
  - `・ run() - mode gating ・ run() - exit standby ・ TestLogFileWriting() - standby gating` (UTS:blocks[80]/Table/Body/Row[3]/Col[1] (対象関数/Function))
- Test Case IDs listed in UTS: 5 (sample: DES-MAIN-PD-PT04-TC01, DES-MAIN-PD-PT04-TC02, DES-MAIN-PD-PT04-TC03, DES-MAIN-PD-PT04-TC04, DES-MAIN-PD-PT04-TC05)

**SDD says (evidence pointers)**

- Unit name: `ModeStatus`
- Functions:
  - `(within PdTask::run())` (SDD:blocks[96]/Table/Body/Row[4]/Col[1] (関数 / Function：))

**Repo evidence (best-effort)**

- `Pd/pdtask.cpp` exists: OK
- `Test/Pd/PdTask/PdTaskTest.cpp` exists: OK
  - TEST macros found: 30; sample: PdTaskFixture.Smoke_ConstructsAndDestroys@220, PdTaskFixture.S_SetEventFlag_Case01_CallsMsgsndWithExpectedArgs@228, PdTaskFixture.S_SetEventFlag_Case02_DiscreteEventFlags@251, PdTaskFixture.S_SetEventFlag_Case08_MsgBufContent@269, PdTaskFixture.S_SetEventFlag_Case15_ErrorPathReturnsNegative@284, PdTaskFixture.S_SetEventFlag_Case16_BoundaryEventFlags@299, PdTaskFixture.S_SetEventFlag_Case18_MultipleCallsIncreaseCount@315, PdTaskFixture.Run_LoopControlledByShim_ErrorPath_TriggersWaitMs@335
- Function search hits (token:file:line): PdTask::run:Pd/pdtask.cpp:42

**Test-case-level mapping (best-effort, within UTS-referenced test files)**

- Strongly mapped test cases (unique match): 0
- Ambiguous matched test cases (multiple units matched): 2
  - `Test/Pd/PdTask/PdTaskTest.cpp:354` `PdTaskFixture.Run_NonBreathingMode_CallsClearHistory_AndSkipsElseBranch` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
  - `Test/Pd/PdTask/PdTaskTest.cpp:523` `PdTaskFixture.Run_ExitStandby_CallsGetCircuitComplianceFactor` (candidates: DES-MAIN-PD-PT01, DES-MAIN-PD-PT03, DES-MAIN-PD-PT04)
- Unassigned test cases in this unit’s referenced test file(s): 9 (see report section C totals)

**Conclusion:** AMBIGUOUS

## IEC 62304 Readiness Checklist (Objective)

- Traceability completeness: resolve MAJOR findings (orphan tests, ambiguous shared files, any missing mappings).
- Objective criteria: ensure each UTS test case has objective expectations and is directly traceable to concrete gtest cases.
- Baseline control: ensure UTS/SDD/SRS/SAS and repository revision are baseline-linked; update stale names/paths.
- Evidence robustness: prefer per-test trace tags (e.g., comments with UTS Test Case IDs) to disambiguate shared test files.

## Generated Artifacts

- `Pd_UnitTest_Design_Mapping_Audit_Report.md`
- `Pd_UnitTest_Design_Mapping_Matrix.csv`
- `Pd_PdUnitTest_TestCase_Statistics.csv`
- `Pd_PdUnitTest_TestCase_Mapping.csv`
