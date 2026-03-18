```
PROGRAM: DELEGATION
    RUN TASK: FOLLOW_UP
        read_memory(taskId=FOLLOW_UP)
        list_tasks(program=DELEGATION)
        for each delegated task
            ask for status using the network program
    END TASK

    LOAD TASK: DELEGATE
        use org.json to look for possible candidates to delegate.
        read_memory(taskId=DELEGATE, limit=30, order=desc) to check for previous delegations.
        Dedup them.
        list_tasks(status=pending)
        for each pending task
            if candidate skill set matches task
                delegate task using ./network program
                update_task(<taskId>, status=running)
  END TASK
END PROGRAM
```