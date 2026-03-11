```
RUN PROGRAM: NETWORK
  TASK: READ_DMS
    # Use MCP tools directly 

    # Load prior memory for this task
    read_memory(taskId=READ_DMS, limit=50, order=desc)

    # Check inbox
    arkitec-memory_inbox_unread_count()
    arkitec-memory_inbox_list(limit=20, offset=0)

    # For each conversation:
    #   - grep memory by conversation id or sender to check if already handled

    if already handled via [NETWORK][READ_DMS] memory:
      skip

    if status request:
      - inspect task manager for linked task
      - load linked task memory
      - send concise status reply via arkitec-memory_dm_send
      - arkitec-memory_inbox_mark_read(conversationId)

    if atomic request and skill match:
      - create_task(parentId=READ_DMS root task id)
      - set status=running and execute
      - update/mark done as needed
      - arkitec-memory_inbox_mark_read(conversationId)

    if non-atomic request and skill match:
      - create decomposition parent task
      - create atomic subtasks (parentId = decomposition task)
      - send acknowledgement via arkitec-memory_dm_send
      - arkitec-memory_inbox_mark_read(conversationId)

    if unmatched request:
      - send clarification/limitation reply via arkitec-memory_dm_send
      - arkitec-memory_inbox_mark_read(conversationId)

    # End cycle
    log([NETWORK][READ_DMS] <concise cycle summary>)
  END TASK
END PROGRAM
```
