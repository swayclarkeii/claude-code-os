# Sway's Expense System - Version Log

**Project**: Automated Expense Management System
**Location**: `/Users/swayclarke/coding_stuff/claude-code-os/02-operations/technical-builds/SwaysExpenseSystem`
**Current Version**: v1.2.0 (Gmail Receipt Monitor)
**Last Updated**: December 30, 2025

---

## Quick Reference

| Version | Status | Date | Phase | Efficiency Score |
|---------|--------|------|-------|------------------|
| v1.2.0 | ✅ Active | 2025-12-30 | Core Automation | 4.0/10 |
| v1.1.0 | ✅ Complete | 2025-12-30 | Core Automation | 3.5/10 |
| v1.0.0 | ✅ Complete | 2025-12-30 | Foundation | 3.0/10 |

---

## Versioning Scheme

**Format**: `MAJOR.MINOR.PATCH`

- **MAJOR (v1.x.x)**: Complete phase or major milestone
  - v1.x.x = Foundation (infrastructure setup)
  - v2.x.x = Core Automation (workflows operational)
  - v3.x.x = Full Integration (all features + edge cases)

- **MINOR (vx.1.x)**: Individual workflow completion or feature addition
  - New n8n workflow implemented
  - Major feature added
  - Database schema changes

- **PATCH (vx.x.1)**: Bug fixes, refinements, small improvements
  - Workflow parameter tweaks
  - Vendor mapping additions
  - Configuration updates
  - Documentation corrections

---

## Version History

### v1.2.0 - Gmail Receipt Monitor
**Date**: December 30, 2025
**Status**: ✅ Active
**Phase**: Core Automation
**Efficiency Score**: 4.0/10

#### New Components

**Workflow 2: Gmail Receipt Monitor**
- **n8n Workflow ID**: `2CA0zQTsdHA8bZKF`
- **Blueprint**: [`workflow2_gmail_receipt_monitor_v1.2.0.json`](workflow2_gmail_receipt_monitor_v1.2.0.json)
- **Node Count**: 9 nodes
- **Purpose**: Automated daily search of Gmail for receipt emails from 6 vendors, downloads attachments, stores in Receipt Pool, logs in database

**Workflow Structure**:
1. **Daily Receipt Check** (Schedule Trigger) - Runs every 24 hours
2. **Load Vendor Patterns** (Code) - Defines 6 vendor email search patterns
3. **Search Gmail for Receipts** (Gmail) - Searches last 7 days for emails with attachments
4. **Get Email Details** (Gmail) - Retrieves full email metadata
5. **Extract Attachment Info** (Code) - Parses PDF/image attachments from email parts
6. **Download Attachment** (Gmail) - Downloads attachment binary data
7. **Upload to Receipt Pool** (Google Drive) - Stores in `_Receipt-Pool/` folder
8. **Prepare Receipt Record** (Code) - Generates receipt metadata with ID format `RCPT-{VENDOR}-{timestamp}`
9. **Log Receipt in Database** (Google Sheets) - Writes to Receipts sheet

**Vendors Monitored**:
- OpenAI (from:noreply@openai.com)
- Anthropic (from:billing@anthropic.com)
- AWS (from:aws-billing@amazon.com)
- Google Cloud (from:billing-noreply@google.com)
- GitHub (from:billing@github.com)
- Oura Ring (from:hello@ouraring.com)

**Environment Variables Required**:
- `RECEIPT_POOL_FOLDER_ID` - Google Drive folder ID for receipt storage

**What Changed**:
- Added automated Gmail monitoring (previously manual)
- Receipt downloads now happen daily automatically
- All receipts logged to database with metadata
- Efficiency increased from 3.5/10 → 4.0/10

**Known Limitations**:
- Only searches last 7 days (prevent duplicate downloads)
- Limited to 10 emails per vendor per run
- Requires Gmail API credentials configured
- Does not extract amount from email (Amount field left empty)

---

### v1.1.0 - PDF Statement Parser
**Date**: December 30, 2025
**Status**: ✅ Complete
**Phase**: Core Automation
**Efficiency Score**: 3.5/10

#### New Components

**Workflow 1: PDF Intake & Parsing**
- **n8n Workflow ID**: `BggZuzOVZ7s87psQ`
- **Blueprint**: [`workflow1_pdf_intake_v1.0.json`](workflow1_pdf_intake_v1.0.json)
- **Node Count**: 8 nodes
- **Purpose**: Monitors inbox for bank statement PDFs, parses using OpenAI Vision, extracts transactions, writes to database

**Workflow Structure**:
1. **Watch Bank Statements Folder** (Google Drive Trigger) - Monitors for new PDF uploads
2. **Download PDF** (Google Drive) - Retrieves PDF file
3. **Extract File Metadata** (Code) - Parses filename for bank/month/year
4. **Parse PDF with OpenAI Vision** (HTTP Request) - Sends PDF to GPT-4 Vision API
5. **Parse OpenAI Response** (Code) - Transforms AI response to transaction records
6. **Write Transactions to Database** (Google Sheets) - Appends to Transactions sheet
7. **Log Statement Record** (Google Sheets) - Records in Statements sheet
8. **Move PDF to Archive** (Google Drive) - Organizes processed files

**Statement ID Format**: `STMT-{bank}-{yearmonth}-{timestamp}`
**Transaction ID Format**: `{statementId}-{index:003}`

**Environment Variables Required**:
- `BANK_STATEMENTS_FOLDER_ID` - Watch folder for new statements
- `ARCHIVE_STATEMENTS_FOLDER_ID` - Archive location after processing

**What Changed**:
- Added PDF parsing capability (previously manual entry)
- German bank statement support via AI
- Automatic transaction extraction
- Efficiency increased from 3.0/10 → 3.5/10

**Known Limitations**:
- Requires OpenAI API key configured
- Only supports PDF format
- Filename must follow format: `{BANK}_{YYYY-MM}_Statement.pdf`
- No receipt matching yet (all transactions marked `MatchStatus: unmatched`)

---

### v1.0.0 - Foundation Complete
**Date**: December 30, 2025
**Status**: ✅ Complete
**Phase**: Foundation
**Efficiency Score**: 3.0/10

#### Components Created

**1. Google Drive Folder Structure** (96+ folders)
- **Root Folder**: Expenses-System
  - ID: `1fgJLso3FuZxX6JjUtN_PHsrIc-VCyl15`
  - URL: https://drive.google.com/drive/folders/1fgJLso3FuZxX6JjUtN_PHsrIc-VCyl15

**Structure**:
```
📁 Expenses-System/
├── 📁 _Inbox/
│   ├── 📁 Bank-Statements/
│   ├── 📁 Credit-Card-Statements/
│   └── 📁 Expensify-Exports/
├── 📁 _Receipt-Pool/
├── 📁 _Unmatched/
├── 📁 _Archive/
│   └── 📁 Statements/
│       ├── 📁 ING/
│       ├── 📁 Deutsche-Bank/
│       ├── 📁 Barclay/
│       └── 📁 Miles-More/
├── 📁 Income/
│   └── 📁 2025/
│       └── 📁 Invoices/
├── 📁 _Reports/
│   ├── 📁 2024/
│   └── 📁 2025/
├── 📁 2025/
│   ├── 📁 01-January/
│   │   ├── 📁 ING/
│   │   │   ├── 📁 Statements/
│   │   │   └── 📁 Receipts/
│   │   ├── 📁 Deutsche-Bank/
│   │   │   ├── 📁 Statements/
│   │   │   └── 📁 Receipts/
│   │   ├── 📁 Barclay/
│   │   │   ├── 📁 Statements/
│   │   │   └── 📁 Receipts/
│   │   └── 📁 Miles-More/
│   │       ├── 📁 Statements/
│   │       └── 📁 Receipts/
│   ├── 📁 02-February/ (same structure)
│   └── ... (all 12 months)
└── 📄 Expense-Database.gsheet
```

**Total Folders Created**: 96+
- 7 main folders
- 12 monthly folders
- 48 bank folders (4 banks × 12 months)
- 96 Statements/Receipts subfolders
- Archive folders for 4 banks

**2. Transaction Database Spreadsheet**
- **Name**: Expense-Database
- **ID**: `1l1uA8qA0DCGzGLBhmP2HqTzaajjbkURY2SLeqSuHMXM`
- **URL**: https://docs.google.com/spreadsheets/d/1l1uA8qA0DCGzGLBhmP2HqTzaajjbkURY2SLeqSuHMXM/edit
- **Location**: Expenses-System folder

**Sheets Created** (4 total):

**Sheet 1: Transactions** (16 columns)
```
TransactionID | Date | Bank | Amount | Currency | Description | Vendor | Category |
ReceiptID | StatementID | MatchStatus | MatchConfidence | Notes | Tags | Type | AnnualInvoiceID
```

**Sheet 2: Statements** (8 columns)
```
StatementID | Bank | Month | Year | FileID | FilePath | ProcessedDate | TransactionCount
```

**Sheet 3: Receipts** (10 columns)
```
ReceiptID | Source | Vendor | Date | Amount | Currency | FileID | FilePath | ProcessedDate | Matched
```

**Sheet 4: VendorMappings** (4 columns)
```
BankPattern | NormalizedVendor | Category | GmailSearch
```

**3. System Design Documentation**
- **File**: `/Users/swayclarke/.claude/plans/typed-tickling-sprout.md`
- **Size**: 850+ lines
- **Status**: Complete and approved
- **Sections**: 13 main sections covering all workflows, edge cases, vendor mappings

**4. Project Organization Structure**
- **Location**: `claude-code-os/02-operations/technical-builds/SwaysExpenseSystem/`
- **Folders**:
  - `workflows/` - Workflow documentation and diagrams
  - `N8N_Blueprints/v1_foundation/` - n8n workflow JSON exports
  - `_archive/` - Historical versions and deprecated files

#### What Works
✅ Complete Google Drive folder infrastructure (96+ folders)
✅ Transaction Database with all 4 sheets and proper headers
✅ Comprehensive system design specification approved
✅ Clear vendor mapping patterns defined (11+ vendors)
✅ Edge case handling rules documented
✅ 4 financial institutions supported (ING, Deutsche Bank, Barclay, Miles & More)

#### What Doesn't Work Yet
❌ No automation workflows built (0% automation capability)
❌ VendorMappings sheet is empty (needs 11+ patterns from design doc)
❌ No PDF parsing workflow
❌ No Gmail monitoring workflow
❌ No transaction matching engine
❌ No file organization automation
❌ No notification/reporting system

#### Known Issues
⚠️ n8n workflow "Expense System - Database Setup" (ID: `hf37ifjSLspiQyxF`) exists but webhook registration had issues
- Workaround: Database created manually via Google Sheets MCP instead
- Workflow may be useful for reference but not currently operational

#### Blockers Identified
🔴 **Critical**:
- OpenAI API key needed for PDF parsing (German bank statements)
- Sample German bank statement PDF needed for parsing rule development

🟡 **Important**:
- Gmail API credentials need verification in n8n
- Expensify export format needs to be verified

#### Rollback Instructions
**To completely reset to pre-v1.0.0 state**:
1. Delete Google Drive folder: Expenses-System (ID: `1fgJLso3FuZxX6JjUtN_PHsrIc-VCyl15`)
2. Delete spreadsheet: Expense-Database (ID: `1l1uA8qA0DCGzGLBhmP2HqTzaajjbkURY2SLeqSuHMXM`)
3. Keep design document at `/Users/swayclarke/.claude/plans/typed-tickling-sprout.md` for reference

**Note**: This is the foundation version, so there's no previous state to roll back to.

#### Files & Resources
- Design Document: `/Users/swayclarke/.claude/plans/typed-tickling-sprout.md`
- Version Log: `claude-code-os/02-operations/technical-builds/SwaysExpenseSystem/N8N_Blueprints/v1_foundation/VERSION_LOG.md`
- Google Drive Root: https://drive.google.com/drive/folders/1fgJLso3FuZxX6JjUtN_PHsrIc-VCyl15
- Transaction Database: https://docs.google.com/spreadsheets/d/1l1uA8qA0DCGzGLBhmP2HqTzaajjbkURY2SLeqSuHMXM/edit

#### Next Version Goal
**v1.1.0 - Workflow 1: PDF Intake & Parsing**
- Build n8n workflow to monitor `_Inbox/Bank-Statements/`
- Integrate OpenAI Vision API for German PDF parsing
- Extract transactions and write to Transactions sheet
- Move processed PDFs to Archive
- **Estimated time**: 2-3 hours
- **Efficiency score target**: 4.5/10

---

## Planned Versions

### v1.1.0 - Workflow 1: PDF Intake & Parsing
**Target Date**: TBD
**Estimated Time**: 2-3 hours
**Efficiency Gain**: 3.0/10 → 4.5/10

**Components to Build**:
- n8n workflow JSON
- OpenAI Vision API integration
- German date/amount parsing rules
- Database write operations
- File moving logic

**Success Criteria**:
- Can process 1 sample German bank statement
- Correctly extracts all transactions
- Writes to database with proper IDs
- Moves PDF to Archive folder

### v1.2.0 - Workflow 2: Gmail Receipt Monitor
**Target Date**: TBD
**Estimated Time**: 1.5-2 hours
**Efficiency Gain**: 4.5/10 → 5.5/10

**Components to Build**:
- Gmail API integration
- Vendor pattern matching
- Receipt download logic
- Store in `_Receipt-Pool/`
- Log in Receipts sheet

### v1.3.0 - Workflow 3: Transaction-Receipt Matching
**Target Date**: TBD
**Estimated Time**: 3-4 hours
**Efficiency Gain**: 5.5/10 → 6.5/10

**Components to Build**:
- Matching algorithm (date ±3 days, amount exact/±€0.50)
- Confidence scoring (0.0-1.0)
- Vendor pattern regex matching
- Database updates (ReceiptID, MatchStatus, MatchConfidence)
- Move matched receipts to organized folders

### v1.4.0 - Workflow 4: File Organization
**Target Date**: TBD
**Estimated Time**: 1.5-2 hours
**Efficiency Gain**: 6.5/10 → 7.0/10

**Components to Build**:
- Receipt sorting from `_Receipt-Pool/` to monthly folders
- Unmatched transaction handling (`_Unmatched/`)
- Statement archiving logic
- Folder existence checks

### v2.0.0 - Core Automation Complete
**Target Date**: TBD
**Estimated Time**: 8-10 hours total (all workflows)
**Efficiency Score**: 7.0/10
**Milestone**: MVP Ready for Testing

**What Makes v2.0.0**:
- All 4 core workflows operational
- End-to-end processing tested with real data
- VendorMappings sheet populated (11+ vendors)
- System can process monthly expenses in 25-30 minutes
- Successfully matched 90%+ of recurring vendor transactions

### v2.1.0 - Edge Case Handling
**Target Date**: TBD
**Components**:
- Small expense handling (<€10)
- GEMA income portal reminders (quarterly)
- Annual invoice tracking system
- Alert system for unmatched transactions ≥€10

### v2.2.0 - Expensify Integration
**Target Date**: TBD
**Components**:
- API-based monthly export automation
- Receipt photo matching to transactions
- Cash expense tracking

### v3.0.0 - Full Integration
**Target Date**: TBD
**Efficiency Score**: 9.0/10
**Components**:
- Weekly digest reports (every Monday)
- Monthly summary generation
- All edge cases handled
- Notification system operational
- Complete system documentation

---

## Component Inventory

### Google Drive Resources
| Component | ID | Version | Status |
|-----------|----|---------| ------|
| Root Folder | `1fgJLso3FuZxX6JjUtN_PHsrIc-VCyl15` | v1.0.0 | ✅ Active |
| Expense-Database | `1l1uA8qA0DCGzGLBhmP2HqTzaajjbkURY2SLeqSuHMXM` | v1.0.0 | ✅ Active |

### n8n Workflows
| Workflow | ID | Version | Status |
|----------|----|---------| ------|
| Workflow 1: PDF Intake & Parsing | `BggZuzOVZ7s87psQ` | v1.1.0 | ✅ Built (untested) |
| Workflow 2: Gmail Receipt Monitor | `2CA0zQTsdHA8bZKF` | v1.2.0 | ✅ Built (untested) |
| Workflow 3: Transaction-Receipt Matching | `LUPoW9jO3VfYIbgC` | v1.3.0 | ✅ Built (untested) |
| Workflow 4: File Organization & Sorting | - | Not built | ⏳ Pending |
| Workflow 5: Notifications & Reporting | - | Not built | ⏳ Optional |
| Database Setup (deprecated) | `hf37ifjSLspiQyxF` | v1.0.0 | ⚠️ Not operational |

### Documentation Files
| File | Location | Version | Status |
|------|----------|---------|--------|
| System Design | `/Users/swayclarke/.claude/plans/typed-tickling-sprout.md` | v1.0.0 | ✅ Complete |
| Version Log | `SwaysExpenseSystem/N8N_Blueprints/v1_foundation/VERSION_LOG.md` | v1.0.0 | ✅ Active |

---

## Efficiency Scoring Rubric

**Scale**: 0-10 (where 10 = fully automated, 25-30 min/month)

### Current Score Breakdown (v1.0.0): 3.0/10

| Category | Weight | Score | Points | Notes |
|----------|--------|-------|--------|-------|
| **Foundation** | 20% | 10/10 | 2.0 | ✅ All infrastructure complete |
| **Core Automation** | 50% | 0/10 | 0.0 | ❌ No workflows built yet |
| **Data Setup** | 10% | 0/10 | 0.0 | ❌ VendorMappings empty |
| **Testing & Validation** | 10% | 0/10 | 0.0 | ❌ No test data processed |
| **Integration & Edge Cases** | 10% | 5/10 | 0.5 | ⚠️ Partial (GDrive/Sheets ✅, Gmail pending) |
| **Total** | 100% | - | **3.0** | Foundation only |

### Target Score by Version
- v1.0.0 (Current): 3.0/10 - Foundation only
- v1.1.0: 4.5/10 - Add PDF parsing
- v1.2.0: 5.5/10 - Add Gmail monitoring
- v1.3.0: 6.5/10 - Add transaction matching
- v1.4.0: 7.0/10 - Add file organization
- v2.0.0: 7.0/10 - All core workflows operational (MVP)
- v2.1.0: 8.0/10 - Edge cases handled
- v3.0.0: 9.0/10 - Full integration with reporting

---

## Rollback Procedures

### General Rollback Process
1. **Identify target version** from this version history
2. **Review what changed** between current and target version
3. **Follow specific rollback instructions** for that version
4. **Verify all components** match target version state
5. **Update "Current Version"** at top of this document
6. **Test system** to ensure rollback was successful

### Component-Specific Rollback

#### Google Drive Folder Structure
- Use Google Drive's "Manage versions" for file changes
- Delete folders created in newer version
- Restore folders from trash if deleted in newer version
- **Verify folder IDs** match version manifest above
- Use MCP tool: `mcp__google-drive__listFolder` to verify structure

#### Transaction Database (Google Sheets)
- Navigate to: File > Version history > See version history
- Find version matching target version timestamp
- Click "Restore this version"
- **Export current version** as backup before rollback
- Verify column headers match version schema in this log

#### n8n Workflows
- Each workflow has built-in version history in n8n UI
- Navigate to workflow > ... menu > Versions
- **Export current workflow JSON** before rollback (File > Download)
- Select previous version to restore
- Click "Restore" button
- **Reactivate workflow** after rollback
- Test workflow execution

#### Design Documents
- If using git: `git checkout <commit-hash> -- <file-path>`
- Otherwise: Keep dated backups manually
- Compare versions using diff tools

### Emergency Rollback (Complete System Reset)
If system becomes unstable, reset to v1.0.0:
```bash
1. Delete all n8n workflows (keep JSON backups)
2. Reset Transaction Database to v1.0.0 schema (empty data)
3. Keep Google Drive folder structure intact
4. Clear VendorMappings sheet
5. Update VERSION_LOG.md to mark current version as v1.0.0
```

---

## Archiving Procedures

### When to Archive
Archive workflow versions whenever:
- Making changes to existing workflows (parameter tweaks, node additions, connection changes)
- Upgrading to a new minor or major version
- Testing experimental features
- Before any potentially breaking changes

### Archive Folder Structure
```
N8N_Blueprints/
└── v1_foundation/
    ├── _archive/
    │   ├── workflow1_pdf_intake_v1.0.0_2025-12-30.json
    │   ├── workflow1_pdf_intake_v1.0.1_2025-12-31.json
    │   ├── workflow2_gmail_receipt_monitor_v1.2.0_2025-12-30.json
    │   └── workflow2_gmail_receipt_monitor_v1.2.1_2025-01-01.json
    ├── workflow1_pdf_intake_v1.1.0.json (current)
    ├── workflow2_gmail_receipt_monitor_v1.2.1.json (current)
    └── workflow3_transaction_receipt_matching_v1.3.0.json (current)
```

### Archiving Workflow Versions

**Step 1: Export Current Workflow from n8n**
```bash
# In n8n UI:
1. Open workflow
2. Click ... menu → Download
3. Save as: workflow{N}_{name}_v{version}_{date}.json
```

**Step 2: Move Previous Version to Archive**
```bash
# Before making changes, copy current version to _archive
cp workflow2_gmail_receipt_monitor_v1.2.0.json \
   _archive/workflow2_gmail_receipt_monitor_v1.2.0_2025-12-30.json
```

**Step 3: Update Current Version File**
```bash
# After changes, export new version with updated version number
# Example: v1.2.0 → v1.2.1 (patch) or v1.3.0 (minor)
mv workflow2_gmail_receipt_monitor_v1.2.0.json \
   workflow2_gmail_receipt_monitor_v1.2.1.json
```

### File Naming Convention

**Active Workflows** (in v1_foundation/):
```
workflow{number}_{name}_v{MAJOR.MINOR.PATCH}.json
Example: workflow2_gmail_receipt_monitor_v1.2.1.json
```

**Archived Workflows** (in _archive/):
```
workflow{number}_{name}_v{MAJOR.MINOR.PATCH}_{YYYY-MM-DD}.json
Example: workflow2_gmail_receipt_monitor_v1.2.0_2025-12-30.json
```

### What to Archive

**Always Archive:**
- ✅ Workflow JSON files before modifications
- ✅ Major documentation changes (SYSTEM_DESIGN.md updates)
- ✅ Database schema changes (export Expense-Database as CSV)

**No Need to Archive:**
- ❌ VERSION_LOG.md (this file tracks all changes already)
- ❌ README.md (living document)
- ❌ Folder structures (Google Drive has version history)

### Retrieval from Archive

**To restore an old version:**
```bash
# 1. Find archived version
ls -la _archive/workflow2_gmail_receipt_monitor_v1.2.0*

# 2. Copy back to active folder
cp _archive/workflow2_gmail_receipt_monitor_v1.2.0_2025-12-30.json \
   workflow2_gmail_receipt_monitor_v1.2.0.json

# 3. Import to n8n
# In n8n UI: Workflows → Import from File → Select JSON
# Choose: "Import as new workflow" to test side-by-side
# OR "Replace current workflow" to restore completely
```

### Archive Retention Policy

- **Keep all archives** for current major version (v1.x.x)
- **Keep last 3 minor versions** of previous major versions
- **Never delete v1.0.0 foundation versions** (baseline for rollback)
- Review and clean archives annually

---

## Version Update Checklist

**Before creating a new version**:
- [ ] Export current n8n workflows as JSON backups
- [ ] Export Transaction Database as CSV/Excel backup
- [ ] Document current system state (folders, files, data)
- [ ] Test rollback procedure to previous version

**When creating a new version**:
- [ ] Update "Current Version" and "Last Updated" at top of file
- [ ] Add new version entry with complete details
- [ ] Document all components created/modified
- [ ] List what works and what doesn't
- [ ] Document known issues and blockers
- [ ] Provide clear rollback instructions
- [ ] Update component inventory with new IDs/versions
- [ ] Update efficiency score and breakdown
- [ ] Test new components thoroughly
- [ ] Export n8n workflow JSONs to `N8N_Blueprints/vX_X/` folder

**After creating a new version**:
- [ ] Tag version in git (if using version control)
- [ ] Update related documentation
- [ ] Notify relevant stakeholders (if applicable)
- [ ] Archive previous version files to `_archive/`

---

## Notes

- **Version numbers are permanent** - never reuse or skip numbers
- **Document ALL breaking changes** clearly in version notes
- **Always test rollback procedures** before deploying to production
- **Keep JSON exports** of all n8n workflows for each version
- **This is a living document** - update as system evolves
- **Efficiency scoring** is subjective but should be consistent across versions
- **Rollback time estimates** should be added for complex versions

---

## Questions & Support

For questions about this version log or the expense system:
1. Review the main design document: `/Users/swayclarke/.claude/plans/typed-tickling-sprout.md`
2. Check component inventory for IDs and URLs
3. Review known issues section for current blockers
4. Consult rollback procedures for recovery steps

Last reviewed: December 30, 2025
