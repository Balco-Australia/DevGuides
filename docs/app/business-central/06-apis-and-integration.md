# Business Central: APIs and Integration

BC provides standard REST APIs and supports custom APIs developed with AL. Before planning an integration, define data ownership, synchronization direction, triggers, and failure handling; then check whether standard APIs meet the need.

A simple integration design should answer at least:

- Which system is the authoritative source for each type of data?
- Which environment, company, and API version will be used?
- How will retries avoid duplicate creation or processing?
- Who will detect, fix, and resend failed synchronizations?

References: [REST API overview](https://learn.microsoft.com/dynamics365/business-central/dev-itpro/webservices/api-overview) and [integration overview](https://learn.microsoft.com/dynamics365/business-central/dev-itpro/developer/integration-overview).
