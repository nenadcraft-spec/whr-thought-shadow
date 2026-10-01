## 11. TEXT_INPUT_ADAPTER

Responsible for:
- parsing typed input
- extracting intent
- detecting missing context
- flagging ambiguity
- detecting risky assumptions
- tracking `subject_origin` and `body_origin` separately

### Content provenance
```text
subject_origin: USER_PROVIDED_CONTENT | AI_GENERATED_DRAFT_CONTENT
body_origin: USER_PROVIDED_CONTENT | AI_GENERATED_DRAFT_CONTENT
```

If adaptation/correction occurs, provenance must remain explicit per field:
```text
subject_origin: USER_PROVIDED_CONTENT
body_origin: AI_GENERATED_DRAFT_CONTENT
```

---