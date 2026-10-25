# RevPilot — Modèle de domaine proposé

Ce modèle décrit les entités métier à partager entre modules. Il doit être adapté au schéma et à la stack déjà présents dans le dépôt ; ce n'est pas une instruction de migration aveugle.

## Entités principales

### BusinessUnit
- `id`
- `code` (ex. CSP, CSTLO, SYGED)
- `display_name`
- `aliases` (mapping source, à administrer)
- `included_in_ged_consolidation`
- `active`

### Client
- `id` (identifiant stable)
- `external_ids`
- `legal_name`
- `display_name`
- `sector`
- `client_type`
- `status_current`
- `created_at`, `updated_at`
- `source_refs`

### ClientBusinessUnit
Association entre un client et une BU ; permet qu'un client ait des activités dans plusieurs BUs.
- `client_id`
- `business_unit_id`
- `status`
- `valid_from`, `valid_to`
- `modules`

### ClientContact
- `id`, `client_id`
- `name`, `role_title`, `phone`, `email` (facultatif)
- `is_primary`
- `valid_from`, `valid_to`

### Contract / PurchaseOrder
- `id`, `client_id`, `business_unit_id`
- `contract_number`, `purchase_order_number`
- `start_date`, `end_date`
- `contract_amount`, `currency`
- `billing_frequency`
- `renewal_status`
- `status`
- `source_document_id`

### Invoice
- `id`, `client_id`, `business_unit_id`, `contract_id`
- `invoice_number`, `issue_date`
- `service_start_date`, `service_end_date`
- `amount_ex_tax`, `currency`
- `invoice_type` (invoice, credit_note, replacement, adjustment)
- `payment_status`, `paid_date`, `paid_amount` (nullable if unknown)
- `status` (draft, needs_review, validated, rejected, cancelled/replaced)
- `source_document_id`, `source_import_id`
- `created_by`, `validated_by`, timestamps

### RecognizedRevenueLine
Ligne calculée par facture/ajustement et période.
- `id`, `invoice_id`, `client_id`, `business_unit_id`
- `product_or_module`
- `period_month`
- `recognized_amount`
- `calculation_method` (linear_monthly, daily_proration, approved_manual_adjustment)
- `calculation_version`
- `source_reference`
- `status`
- `created_at`

### ExcelReconciliation
- `id`, `invoice_id`, `period_month`
- `excel_reference_amount`
- `revpilot_calculated_amount`
- `difference`
- `status` (conform, discrepancy, impossible)
- `explanation`, `reviewed_by`, `reviewed_at`

### BudgetLine
- `id`, `business_unit_id`, `period_month`
- `amount`, `currency`, `version`, `source_reference`

### ForecastVersion
- `id`, `name`, `created_at`, `created_by`
- `is_initial_frozen`
- `status` (draft, in_review, approved, superseded)
- `notes`

### ForecastLine
- `id`, `forecast_version_id`, `client_id`, `business_unit_id`
- `contract_id` nullable
- `period_month`, `amount`, `currency`
- `security_type` (secured, unsecured)
- `assumption_type`, `assumption_value`
- `source_reference`

### ForecastScenario
- `id`, `name`, `base_forecast_version_id`
- `created_by`, `created_at`, `status`
- `assumptions` (structured, validated payload)
- Results stored separately from official ForecastLine until approved.

### NepiMovement
- `id`, `client_id`, `business_unit_id`, `product_or_module`
- `period_n`, `revenue_n_minus_1`, `revenue_n`, `signed_variance`
- `category`
- `comment`
- `status` (unclassified, in_review, validated)
- `source_reference`, audit fields

### AttritionEvent
- `id`, `client_id`, `business_unit_id`
- `modules_affected`
- `departure_date`, `contract_termination_date` nullable
- `last_paid_invoice_id` nullable
- `last_paid_invoice_reference` nullable if no invoice relation exists
- `lost_monthly_revenue`, `lost_annualized_revenue`
- `departure_reason`, `reason_comment`
- `status` (incomplete, in_review, validated, corrected)
- `source_reference`, `created_by`, `validated_by`, timestamps

### Alert
- `id`, `type`, `severity`, `status`
- `client_id`, `business_unit_id`, `period_month` nullable
- `amount_exposed` nullable
- `trigger_rule`, `evidence_refs`
- `assigned_to` nullable
- `created_at`, `resolved_at`, `resolution_comment`

### SourceDocument / ImportBatch
- `id`, `file_name`, `file_type`, `checksum`, `storage_reference`
- `imported_at`, `imported_by`, `status`, `error_summary`
- `source_system`, `source_version`
- Original files must be retained according to security and retention rules.

### AuditEvent
- `id`, `entity_type`, `entity_id`, `action`
- `before_value`, `after_value`
- `actor_id`, `occurred_at`, `reason`

## Relationships and constraints

- One client can have multiple BU associations, contracts, invoices, contacts and attrition events over time.
- An invoice belongs to one client and one BU under the canonical model; cross-BU invoices require an explicit allocation model, not implicit duplication.
- A validated invoice may produce multiple recognized-revenue lines by service month.
- Forecast lines are versioned. Initial forecast stays immutable.
- NEPI movements are categorized individually; only validated price-category movements contribute to NEPI.
- An AttritionEvent may affect a client globally or only a BU/module scope; global client status must not be changed if the client remains active in another BU/module.
- Source references and audit events should support traceability from dashboards to source rows and documents.
- Unique constraints and import idempotency must be defined against actual source formats before implementation.
