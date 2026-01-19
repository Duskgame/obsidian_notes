[[Jetpack Compose]]
```
data class ReplyUiState(  
    val mailboxes: Map<MailboxType, List<Email>> = emptyMap(),  
    val currentMailbox: MailboxType = MailboxType.Inbox,  
    val currentSelectedEmail: Email = LocalEmailsDataProvider.defaultEmail,  
    val isShowingHomepage: Boolean = true  
) {  
    val currentMailboxEmails: List<Email> by lazy { mailboxes[currentMailbox]!! }  
}
```

Derived property with lazy
```
val currentMailboxEmails: List<Email> by lazy { mailboxes[currentMailbox]!! }
```

- This is a **computed** property, not passed in the constructor.
- `by lazy { ... }` means the block runs the first time `currentMailboxEmails` is accessed, and the result is cached for later accesses.
- Inside, it looks up the list of emails for the currently selected mailbox: `mailboxes[currentMailbox]`.
- The `!!` asserts that the value is non‑null, so it will crash if there is no entry for `currentMailbox` in the `mailboxes` map.