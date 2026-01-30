# Add Expense Screen

## Adding a preset expense
A user is on a screen with an empty table that has a visual prompt.

The prompt looks like a plus (+) sign that invites the user to add a preset expense.

Upon pressing the prompt, the user is asked to input the expense category (commute, food, coffee, etc.)
    and the associated cost.

After confirming, the preset button is established on the table, and the visual prompt (+) appears at the bottom of the first preset.

## Implementation
> [!NOTE] Model:
```
data class ExpensePreset(
    val id: Long,
    val amount: Double,
    val category: String,
    val type: String,
    val createdAt: LocalDateTime = LocalDateTime.now()
)
```

Each table row can contain multiple presets.
Each row will be governed by its category:

| Category | Preset1 | Preset2 | Preset3 |
|----------|---------|---------|---------|
| Commute  | P90     | P80     | P70     |
| Food     | P10     | P20     | P30     |



