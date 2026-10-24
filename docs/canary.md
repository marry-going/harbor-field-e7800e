merge_protections_settings:
  reporting_method: check-runs
  post_comment: false

merge_protections:
  - name: policy owner gate
    if:
      - base = main
      - mergify-configuration-changed
    success_conditions:
      - "#approved-reviews-by >= 1"

queue_rules:
  - name: docs-queue
    branch_protection_injection_mode: none
    merge_bot_account: 56dluffy2
    queue_conditions:
      - base = main
      - author = testbuyer94-collab
      - label = ready-e7800e
      - files ~= ^docs/canary\.md$
      - "#files = 1"
      - -mergify-configuration-changed
    merge_conditions:
      - -mergify-configuration-changed
    merge_method: merge

  - name: protected-queue
    branch_protection_injection_mode: none
    merge_bot_account: 56dluffy2
    queue_conditions:
      - base = main
      - author = testbuyer94-collab
      - label = ready-e7800e
      - files ~= ^src/protected\.txt$
      - "#files = 1"
      - -mergify-configuration-changed
    merge_conditions:
      - -mergify-configuration-changed
    merge_method: merge

pull_request_rules:
  - name: docs canary
    conditions:
      - base = main
      - author = testbuyer94-collab
      - label = ready-e7800e
      - files ~= ^docs/canary\.md$
      - "#files = 1"
      - -mergify-configuration-changed
    actions:
      queue:
        name: docs-queue

  - name: protected canary
    conditions:
      - base = main
      - author = testbuyer94-collab
      - label = ready-e7800e
      - files ~= ^src/protected\.txt$
      - "#files = 1"
      - -mergify-configuration-changed
    actions:
      queue:
        name: protected-queue
