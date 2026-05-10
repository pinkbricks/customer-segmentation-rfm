# Cleaning Log

## Original Rows: 525461

### Drop missing CustomerID
Rows Removed: 107927

### Remove cancelled invoices
Rows Removed: 9839

### Remove Quantity <= 0
Rows Removed: 0

### Remove UnitPrice <= 0
Rows Removed: 31

### Drop duplicates
Rows Removed: 6748

### Filter United Kingdom only
Rows Removed: 36683

## Final Rows: 364223

## Final Columns:
- Invoice
- StockCode
- Description
- Quantity
- InvoiceDate
- Price
- CustomerID
- Country
- TotalSum
