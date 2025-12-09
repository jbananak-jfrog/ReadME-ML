---
title: Manage Automations
deprecated: false
hidden: false
metadata:
  title: Manage Automations
  description: "To list the automations using the Frogml CLI:"
  robots: index
  legacyUUIDs:
    - UUID-5079d31f-e77a-4860-1795-03af3be50fa9
    - UUID-f01784db-26eb-7cdf-7716-b8b87915fae7
---
### List Automations

To list the automations using the Frogml CLI:

```
frogml automations list
```

### Delete Automations

To delete an automation using the Frogml CLI (using automation id as a parameter):

```
frogml automations delete --automation-id <your-automation-id>
```

### List Automation Executions

To list the executions of specific automation:

```
frogml automations executions list --automation-id <your-automation-id>
```
