---
title: 'Fixing database deadlocks or data integrity issues'
shortTitle: Fixing database deadlocks
intro: '{% data variables.product.prodname_copilot_chat_short %} can help you avoid code that causes slow or blocked database operations, or tables with missing or incorrect data.'
redirect_from:
  - /copilot/example-prompts-for-github-copilot-chat/refactoring-code/fixing-database-deadlocks-or-data-integrity-issues
versions:
  feature: copilot
category:
  - 'Refactoring code'
complexity:
  - Advanced
octicon: rocket
topics:
  - Copilot
---

Complex database operations–particularly those involving transactions–can lead to deadlocks or data inconsistencies that are hard to debug.

{% data variables.product.prodname_copilot_chat_short %} can help by identifying points in a transaction where locking or deadlocks could occur, and can suggest best practices for transaction isolation or deadlock resolution, such as adjusting locking strategies or handling deadlock exceptions gracefully.

> [!NOTE] The responses shown in this article are examples. {% data variables.product.prodname_copilot_chat_short %} responses are non-deterministic, so you may get different responses from the ones shown here.

## Avoiding simultaneous updates on interdependent rows

### Example prompt 1

You can check whether there are any problems with this transaction.

In the editor, select the transaction code, then ask {% data variables.product.prodname_copilot_chat_short %}:

`Is this transaction good?`

### Example response 1

{% data variables.product.prodname_copilot_short %} tells you that "the transaction in your SQL code is generally fine," but lists some things you may want to consider: lock duration, error handling, and concurrency. It mentions that "the transaction holds locks for an extended period, potentially leading to blocking or deadlocks." The response also includes revised code that adds error handling.

In this case, you decide not to add error handling. Right now you want to address the potential for deadlocks.

...

## Avoiding non-index searches

### Example scenario

The following SQL will result in a full table scan if `some_column` is not indexed:

```sql
BEGIN TRANSACTION;
SELECT * FROM my_table WHERE some_column = 'some_value';
-- More actions here, then:
COMMIT TRANSACTION;
```

### Example prompt

Asking {% data variables.product.prodname_copilot_short %} the following question will usually reveal the potential problem.

`How can I optimize this transaction?`

...

## Avoiding long-running transactions

### Example response

...

    INSERT INTO target_table (first_name, last_name, email, dept, role, hire_date)
    SELECT first_name, last_name, email, department, job_title, start_date
    FROM source_table
    WHERE (department = 'Engineering' AND salary > 95000)
       OR (department = 'Engineering' AND years_of_experience > 5)
       OR (department = 'Marketing' AND performance_rating = 'Excellent');
    ORDER BY primary_key_column
    OFFSET @Offset ROWS FETCH NEXT @BatchSize ROWS ONLY;

    SET @RowCount = @@ROWCOUNT;
    SET @Offset = @Offset + @BatchSize;

    COMMIT;
END;

{% data variables.product.prodname_copilot_short %} tells you to replace `primary_key_column` in the suggested code with the name of the actual primary key column of `source_table`.

...

## Avoiding data integrity issues

It's important that the information in your databases remains accurate, consistent, and complete. Poorly designed queries can result in missing or incorrect data.

### Example scenario

...

### Example prompt

...

    RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
END CATCH;

The suggested code ensures that if either the `DELETE` or `INSERT` operation fails, the transaction is rolled back and no changes are made to the database.

...

## Further reading

{% data reusables.copilot.example-prompts.further-reading-items %}
