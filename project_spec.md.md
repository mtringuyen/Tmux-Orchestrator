### Technical stack:

- Frontend: Nextjs, Typescript
- Backend: Golang using the boilerplate in /home/tri/Coding/gin-boilerplate 

### Epic 1: Expense Request Management

**Source PRD Section:** 3.2. Expense Request Management

---

**User Story 1:** As an Employee, I want to create new expense requests, so that I can claim my business-related expenses.

* **Source PRD Details:** "Provide intuitive interfaces for users to initiate new expense requests...".
* **Fields, Options, Allowed Values:**
    * **Field Name:** `Request Title / Purpose`
        * **Description:** A brief title or purpose for the expense request.
        * **Type:** Text (String)
        * **Options/Allowed Values:** N/A
    * **Field Name:** `Request Date`
        * **Description:** Date the request is being created (defaults to current date).
        * **Type:** Date
        * **Options/Allowed Values:** N/A
    * **Field Name:** `Expense Date`
        * **Description:** Date the expense was incurred.
        * **Type:** Date
        * **Options/Allowed Values:** N/A
    * **Field Name:** `Amount`
        * **Description:** The total amount of the expense.
        * **Type:** Number (Decimal)
        * **Options/Allowed Values:** Positive number.
    * **Field Name:** `Currency`
        * **Description:** Currency of the expense.
        * **Type:** Dropdown
        * **Options/Allowed Values:** VND (default), USD, EUR (configurable list).
    * **Field Name:** `Exchange Rate`
        * **Description:** Applied if currency is not VND (auto-fetched or manual override).
        * **Type:** Number (Decimal)
        * **Options/Allowed Values:** Positive number. Visible/editable if currency is not VND.
    * **Field Name:** `Amount (VND)`
        * **Description:** Calculated amount in VND if original currency is foreign.
        * **Type:** Number (Decimal, Read-only)
        * **Options/Allowed Values:** N/A
    * **Field Name:** `Vendor Name`
        * **Description:** Name of the vendor or service provider (can be pre-populated by OCR).
        * **Type:** Text (String)
        * **Options/Allowed Values:** N/A
    * **Field Name:** `Expense Category`
        * **Description:** Selected category for the expense.
        * **Type:** Dropdown (Searchable, Hierarchical)
        * **Options/Allowed Values:** Loaded from pre-defined VAS-compliant categories.
    * *(Other fields will be covered in subsequent user stories, e.g., attachments, custom fields, notes)*
* **Validation Rules:**
    * `Request Title / Purpose`: Required, Max length 255 characters.
    * `Expense Date`: Required, Must be a valid date, cannot be in the future.
    * `Amount`: Required, Must be > 0.
    * `Currency`: Required.
    * `Exchange Rate`: Required if currency is not VND, Must be > 0.
    * `Vendor Name`: Optional, Max length 255 characters.
    * `Expense Category`: Required.
* **Acceptance Criteria (ACs):**
    1.  An employee can initiate a new expense request form.
    2.  The form allows input for title, expense date, amount, currency, vendor, and category.
    3.  If a foreign currency is selected, the system fetches/allows input for an exchange rate and calculates the VND equivalent.
    4.  The request is saved in a 'Draft' status upon initial creation.
    5.  All required fields must be validated before saving.

---

**User Story 2:** As an Employee, I want to edit my draft expense requests, so that I can correct or update information before submission.

* **Source PRD Details:** "...modify existing drafts...".
* **Fields, Options, Allowed Values:** (Same as User Story 1 for creation, but applied in an edit context)
* **Validation Rules:** (Same as User Story 1)
* **Acceptance Criteria (ACs):**
    1.  An employee can open an expense request that is in 'Draft' status.
    2.  All fields that were available during creation are editable.
    3.  Changes made to the draft request can be saved.
    4.  Validation rules are applied upon saving the edited draft.
    5.  An employee cannot edit requests that are no longer in 'Draft' status (e.g., 'Submitted', 'Approved').

---

**User Story 3:** As an Employee, I want to delete my expense requests that are no longer needed (if not yet approved), so that I can manage my submissions.

* **Source PRD Details:** "...and remove requests that are no longer needed (subject to approval status)."
* **Fields, Options, Allowed Values:** N/A (Action-based)
* **Validation Rules:**
    * Deletion is only allowed if the expense request status is 'Draft' or potentially 'Rejected' (configurable).
    * Cannot delete requests that are 'Submitted', 'In Approval', 'Approved', or 'Paid'.
* **Acceptance Criteria (ACs):**
    1.  An employee can select a 'Draft' expense request for deletion.
    2.  A confirmation prompt is displayed before deletion.
    3.  Upon confirmation, the draft expense request is permanently removed or marked as 'Deleted'.
    4.  An employee cannot delete requests that have been submitted and are in the approval process or already approved/paid.

---

**User Story 4:** As an Employee, I want to attach supporting documents (images like JPG/PNG, or PDF files) to my expense requests, so that I can provide proof of expenditure.

* **Source PRD Details:** "Allow users to attach supporting documents such as images (JPG, PNG) or PDF files for each expense."
* **Fields, Options, Allowed Values:**
    * **Field Name:** `Attachments`
        * **Description:** Section to upload one or more supporting documents.
        * **Type:** File Upload (Multiple files allowed)
        * **Options/Allowed Values:** File types: JPG, PNG, PDF. Max file size per file (e.g., 5MB). Max number of files (e.g., 10).
* **Validation Rules:**
    * File type must be one of the allowed types (JPG, PNG, PDF).
    * File size must not exceed the configured limit.
* **Acceptance Criteria (ACs):**
    1.  An employee can upload one or more files (JPG, PNG, PDF) to an expense request.
    2.  The system validates file types and sizes.
    3.  Uploaded attachments are linked to the specific expense request.
    4.  Users can view thumbnails/names of attached files.
    5.  Users can remove attachments from a draft expense request.

---

**User Story 5:** As an Employee, I want the system to automatically extract data (e.g., vendor name, amount, date) from my attached receipts using OCR/AI, so that my expense request fields are pre-populated and I save time on manual entry.

* **Source PRD Details:** "Implement Optical Character Recognition (OCR) or AI-powered capabilities to automatically read and extract relevant data from attached receipts and documents (e.g., vendor name, amount, date, line items). This extracted data will be used to pre-populate expense request fields, significantly reducing manual input."
* **Fields, Options, Allowed Values:** (The OCR process would populate fields defined in User Story 1 like `Vendor Name`, `Amount`, `Expense Date`)
    * **Field Name:** `OCR Status Indicator`
        * **Description:** Visual indicator of OCR processing status for an attachment (e.g., Processing, Success, Failure, Needs Review).
        * **Type:** Indicator (Icon/Text)
    * **Field Name:** `OCR Confidence Score (Optional)`
        * **Description:** A score indicating the OCR's confidence in the extracted data for certain fields.
        * **Type:** Percentage/Rating
* **Validation Rules:**
    * OCR attempts to parse supported file types (JPG, PNG, PDF).
    * If OCR fails or has low confidence, user is prompted to review/enter manually.
* **Acceptance Criteria (ACs):**
    1.  When a supported document (JPG, PNG, PDF) is attached, OCR processing is automatically initiated.
    2.  The system attempts to extract vendor name, expense amount, and expense date from the attachment.
    3.  Successfully extracted data pre-populates the corresponding fields in the expense request form.
    4.  If fields are already filled, the system may ask the user if they want to overwrite with OCR data or highlight discrepancies.
    5.  Users can always manually override OCR-extracted data.
    6.  If OCR fails, the user is notified, and manual entry is required.

---

**User Story 6:** As an Accountant/Administrator, I want the system to use a pre-defined chart of accounts/expense categories compliant with Vietnamese Accounting Standards (VAS), including Circular 200/2014/TT-BTC, so that all financial transactions are correctly classified for regulatory compliance.

* **Source PRD Details:** "The system will include a pre-defined and configurable chart of accounts/expense categories that strictly adheres to the latest Vietnamese Accounting Standards (VAS), including Circular 200/2014/TT-BTC." And examples like "Outside Goods & Services Purchases," "Purchase of Fixed Assets & Equipment," "Salaries and Related Expenses" with detailed GL account mappings.
* **Fields, Options, Allowed Values (Admin Interface for Category Management):**
    * **Field Name:** `Category Code`
        * **Description:** Unique code for the expense category (e.g., 64211).
        * **Type:** Text (String)
    * **Field Name:** `Category Name (Vietnamese)`
        * **Description:** Name of the category in Vietnamese.
        * **Type:** Text (String)
    * **Field Name:** `Category Name (English)`
        * **Description:** Name of the category in English.
        * **Type:** Text (String)
    * **Field Name:** `Parent Category`
        * **Description:** Link to a parent category for hierarchical structure.
        * **Type:** Dropdown (Select from existing categories)
    * **Field Name:** `GL Account Mapping`
        * **Description:** Associated General Ledger account code(s) as per VAS.
        * **Type:** Text (String) / Select List (from COA)
    * **Field Name:** `Is Active`
        * **Description:** Whether the category is available for selection in new requests.
        * **Type:** Boolean (Checkbox)
    * **Field Name:** `Description/Guidelines`
        * **Description:** Notes on when to use this category.
        * **Type:** Text (Multiline)
* **Validation Rules (Admin Interface):**
    * `Category Code`: Required, Unique.
    * `Category Name (Vietnamese)`: Required.
    * `GL Account Mapping`: Required.
* **Acceptance Criteria (ACs):**
    1.  The system is shipped with a default set of expense categories compliant with VAS and Circular 200.
    2.  This set includes categories for common expenses like "Outside Goods & Services Purchases," "Fixed Assets," "Salaries" (with sub-breakdowns like Basic Salary 64211, Allowances 64212, Social Insurance 33831/33832, etc., as detailed in PRD).
    3.  These categories are available for selection when an employee creates an expense request.
    4.  (Admin functionalities for managing these categories are covered in US10-12).

---

**User Story 7:** As an Employee, I want to categorize my expenses using a hierarchical structure (Parent > Sub-Category), so that I can accurately classify my spending for detailed analysis.

* **Source PRD Details:** "Support for a multi-level, hierarchical categorization structure (e.g., Parent Category > Sub-Category 1 > Sub-Category 2) to allow for both broad and highly specific expense classification."
* **Fields, Options, Allowed Values:** (This refers to how the `Expense Category` field in User Story 1 is presented and functions)
    * `Expense Category` field should support displaying and selecting from a tree-like structure.
* **Validation Rules:** N/A (Feature of the category selection UI)
* **Acceptance Criteria (ACs):**
    1.  The expense category selection interface allows users to navigate and select categories from a multi-level hierarchy.
    2.  Users can expand parent categories to see and select sub-categories.
    3.  The selected category clearly indicates its position in the hierarchy if viewed later.

---

**User Story 8:** As an Employee, I want an intuitive interface (e.g., dropdowns, searchable lists) to select expense categories, so that I can easily and accurately classify my expenses.

* **Source PRD Details:** "Provide an intuitive interface (e.g., dropdowns, searchable lists) for users to easily select the appropriate expense category during request creation."
* **Fields, Options, Allowed Values:** (This also refers to the `Expense Category` field)
    * The dropdown should include a search/filter capability.
* **Validation Rules:** N/A (Feature of the category selection UI)
* **Acceptance Criteria (ACs):**
    1.  The expense category selection field is presented as a dropdown list.
    2.  The dropdown list allows users to type and search/filter categories by name or code.
    3.  The interface is user-friendly and makes category selection quick and straightforward.

---

**User Story 9 (Nice-to-Have):** As an Employee, I want the system to suggest appropriate expense categories based on receipt data or past expenses using AI/ML, so that categorization is faster and more accurate.

* **Source PRD Details:** "As a "nice-to-have" or future enhancement, the system could employ AI/Machine Learning models to suggest appropriate expense categories based on the receipt data (extracted provider, items, keywords) or past similar expenses."
* **Fields, Options, Allowed Values:**
    * **Field Name:** `Suggested Category`
        * **Description:** A suggestion for the expense category, displayed near the `Expense Category` field.
        * **Type:** Display Text / Clickable Suggestion
* **Validation Rules:** N/A
* **Acceptance Criteria (ACs):**
    1.  After OCR processing or based on vendor/description, the system may suggest one or more relevant expense categories.
    2.  The user can easily accept a suggested category or choose a different one manually.
    3.  The suggestion mechanism aims to improve accuracy and speed but does not prevent manual selection.

---

**User Story 10:** As an Accountant/Administrator, I want to create, edit, and deactivate expense categories, so that the categorization list remains relevant and accurate.

* **Source PRD Details:** "Provide an administrative interface for the accounting team or system administrators to: Create, edit, and deactivate expense categories."
* **Fields, Options, Allowed Values:** (Covered in User Story 6 admin interface detail: `Category Code`, `Category Name`, `Parent Category`, `GL Account Mapping`, `Is Active`, `Description/Guidelines`)
* **Validation Rules:** (Covered in User Story 6 admin interface detail)
* **Acceptance Criteria (ACs):**
    1.  An authorized admin user can access an interface to manage expense categories.
    2.  Admin can add a new expense category, providing all necessary details (code, name, parent, GL mapping, active status).
    3.  Admin can edit existing expense categories (e.g., name, mapping, parent, description). Changing the code might be restricted or have implications.
    4.  Admin can deactivate an expense category. Deactivated categories are not available for selection in new expense requests but remain for historical data.
    5.  Admin can reactivate a deactivated category.

---

**User Story 11:** As an Accountant/Administrator, I want to map expense categories to relevant general ledger accounts, so that financial reporting is accurate.

* **Source PRD Details:** "Map categories to relevant general ledger accounts."
* **Fields, Options, Allowed Values:** (Primarily the `GL Account Mapping` field in the category management interface, as detailed in US6)
* **Validation Rules:**
    * The GL account entered must be a valid account from the Chart of Accounts.
* **Acceptance Criteria (ACs):**
    1.  When creating or editing an expense category, an admin can specify one or more GL accounts to which it maps.
    2.  This mapping is used for generating journal entries for financial systems.
    3.  The system validates that the GL account exists (if a COA module is integrated or a list is maintained).

---

**User Story 12:** As an Accountant/Administrator, I want to define rules or guidelines for expense category usage, so that users consistently apply them.

* **Source PRD Details:** "Define rules or guidelines for category usage."
* **Fields, Options, Allowed Values:** (Primarily the `Description/Guidelines` field in the category management interface, as detailed in US6)
* **Validation Rules:** N/A
* **Acceptance Criteria (ACs):**
    1.  Admin can add descriptive text or guidelines to each expense category.
    2.  This information is accessible or visible to employees when they are selecting a category (e.g., as a tooltip or help text), to aid correct selection.

---

**User Story 13:** As an Employee, I want to add custom fields to my expense requests, so that I can capture specific information relevant to our internal processes.

* **Source PRD Details:** "Provide the flexibility to add custom fields to expense requests to capture specific information relevant to the SME's internal processes."
* **Admin Interface for Custom Field Definition:**
    * **Field Name:** `Custom Field Label` (Text)
    * **Field Name:** `Custom Field Type` (Dropdown: Text, Number, Date, Boolean, Dropdown)
    * **Field Name:** `Custom Field Options` (Text, for Dropdown type, comma-separated)
    * **Field Name:** `Is Required` (Boolean)
    * **Field Name:** `Applies To Categories (Optional)` (Multi-select from Expense Categories)
* **Employee Interface (Expense Request Form):**
    * Dynamically displayed custom fields based on admin configuration (and possibly selected category).
* **Validation Rules:**
    * Custom fields are validated based on their defined type and 'Is Required' status.
* **Acceptance Criteria (ACs):**
    1.  An administrator can define custom fields (label, type, options, required status) for expense requests.
    2.  Administrator can optionally associate custom fields with specific expense categories.
    3.  These custom fields appear on the expense request form for employees when applicable.
    4.  Data entered into custom fields is saved with the expense request.
    5.  Validation (e.g., required, data type) is applied to custom fields.

---

**User Story 14:** As an Employee, I want to add free-form notes to my expense requests, so that I can provide additional context or explanations.

* **Source PRD Details:** "Users should also be able to add free-form notes for additional context."
* **Fields, Options, Allowed Values:**
    * **Field Name:** `Notes`
        * **Description:** A multi-line text area for any additional information.
        * **Type:** Text (Multiline/Text Area)
        * **Options/Allowed Values:** N/A
* **Validation Rules:**
    * Optional field.
    * Max length (e.g., 1000 characters).
* **Acceptance Criteria (ACs):**
    1.  The expense request form includes a field for employees to add free-form text notes.
    2.  The notes are saved with the expense request.
    3.  The notes are visible to approvers and finance team members reviewing the request.

---