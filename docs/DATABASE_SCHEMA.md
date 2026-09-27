# Database Schema

This reflects the record structure currently implemented in the MVP, plus the fields anticipated for modules not yet built (marked *planned*). Field names match the working application exactly so this doc stays a reliable reference, not an aspirational one.

## Deal

| Field | Type | Notes |
|---|---|---|
| `id` | string | Unique identifier |
| `owner` | string | Owner name |
| `leaseUnit` | string | Lease or unit name |
| `county` | string | |
| `operator` | string | |
| `apiNumber` | string | API 10 well identifier |
| `nri` | string | Net revenue interest |
| `totalOffer` | number | Dollar amount |
| `stage` | enum | `Prospecting` \| `Mailer Sent` \| `Negotiating` \| `PSA Sent` \| `Due Diligence` \| `Closing` \| `Closed` \| `Dead` |
| `ddFlag` | enum | `None` \| `Red — Title/PSA issue` \| `Yellow — Royalty/title follow-up` \| `Blue — AOH needed` |
| `lastContactDate` | date | |
| `closingDate` | date | Target or actual, depending on stage |
| `notes` | text | |
| `createdAt` / `updatedAt` | datetime | |

## Mailer

| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `owner` | string | |
| `address`, `city`, `state`, `zip` | string | |
| `interestType` | string | e.g. mineral, royalty |
| `decimalInterest` | string | |
| `operator` | string | |
| `leaseName` | string | |
| `county` | string | |
| `totalOffer` | number | |
| `campaign` | string | Links to a Campaign by name |
| `status` | enum | `Not Sent` \| `Sent` \| `Delivered` \| `Responded` \| `Returned (RTS)` \| `Dead` |
| `createdAt` / `updatedAt` | datetime | |

## Campaign

| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `name` | string | |
| `coverage` | string | Counties covered |
| `touchNumber` | number | Which touch in the sequence |
| `recipients` | number | Estimated/actual recipient count |
| `status` | enum | `Planned` \| `Upcoming` \| `Shipped` \| `Delivered` \| `Complete` |
| `targetShipDate`, `targetDropDate`, `actualShipDate` | date | |
| `purchasePrice` | number | Cost of the touch |
| `profit` | number | |
| `notes` | text | |
| `createdAt` / `updatedAt` | datetime | |

## Asset

| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `leaseUnit` | string | |
| `county` | string | |
| `operator` | string | |
| `apiNumber` | string | |
| `unitAcreage` | number | |
| `status` | enum | `Active` \| `Inactive` \| `Plugged` \| `Unknown` |
| `notes` | text | |
| `createdAt` / `updatedAt` | datetime | |

## Document

| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `name` | string | |
| `type` | enum | `PSA` \| `Deed` \| `Title Opinion` \| `Affidavit of Heirship` \| `Invoice` \| `Curative` \| `Division Order` \| `Other` |
| `linkedDeal` | string | Deal/owner reference |
| `date` | date | |
| `status` | enum | `Pending` \| `Sent` \| `Signed / Received` \| `Filed` |
| `notes` | text | Often includes a link/reference to the source file |
| `createdAt` / `updatedAt` | datetime | |

## Planned modules

### Due-Diligence *(planned — structured view, not an independent table)*
Derived from `Deal.ddFlag` plus linked `Document` records. See `modules/Due-Diligence.md`.

### Title-Review *(planned)*
| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `assetId` | string | Links to Asset |
| `chainEntries` | array | Ordered list of `{grantor, grantee, interest, date, instrumentType, sourceDocumentId, confidence}` |
| `curativeItems` | array | `{description, affectedOwner, status, sourceDocumentId}` |

### Curative *(planned)*
| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `dealId` | string | |
| `itemType` | enum | e.g. `Probate Needed`, `AOH Needed`, `Title Gap` |
| `description` | text | |
| `status` | enum | `Open` \| `In Progress` \| `Resolved` |
| `assignedTo` | string | |

### Closing *(planned)*
| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `dealId` | string | |
| `closingDate` | date | |
| `deedRecorded` | boolean | |
| `wireConfirmed` | boolean | |
| `checklist` | array | Closing checklist items and status |

### Payments *(planned)*
| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `dealId` | string | |
| `amount` | number | |
| `method` | enum | `Wire` \| `Check` \| `Other` |
| `status` | enum | `Pending` \| `Sent` \| `Confirmed` |
| `date` | date | |

### Tasks *(planned)*
| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `linkedRecordType` | enum | `Deal` \| `Mailer` \| `Campaign` \| `Asset` \| `Document` |
| `linkedRecordId` | string | |
| `title` | string | |
| `dueDate` | date | |
| `assignedTo` | string | |
| `status` | enum | `Open` \| `Done` |

## Storage notes

- MVP: each module is one key in `window.storage`, holding a JSON array of that module's records — see `docs/SYSTEM_ARCHITECTURE.md` for the storage layer diagram.
- Target: relational tables mirroring the structures above, with foreign keys instead of string references (e.g., `Mailer.campaign` becomes `campaignId` referencing `Campaign.id`).
