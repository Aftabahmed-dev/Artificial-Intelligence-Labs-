# Lab 01: Knowledge-Based Systems

**Domain Adaptation:** Employee Performance Evaluation Engine

---

## Exercise 1: Executive Honors Rule (Knowledge Base Rule)

### Task
Add a specialized condition rule to `COMPANY_KB["rules"]` for high-performing employees.

* **Condition:** Performance rating $\ge 3.70$ AND disciplinary strikes $= 0$.
* **Expected Action:** Eligible for annual leadership bonus and promotion.

```python
# Rule Definition Addition for KB
{ 
    "id": "RULE_07",
    "conditions": {
        "performance_rating_min": 3.70,
        "disciplinary_strikes": 0
    },
    "condition_type": "executive_honors",
    "grade": "A+",
    "status": "Awarded Employee Excellence Honors",
    "action": "Eligible for annual leadership bonus and promotion."
}
```

---

## Exercise 2: Tracing Forward Chaining

### Task
Modify `evaluate_employee()` to print each evaluated rule condition as the execution loop runs to trace forward chaining decision steps.

```python
# STEP 2: INFERENCE ENGINE
def evaluate_employee(employee_record, kb):
    print(f"\n--- EVALUATING EMPLOYEE: {employee_record['employee_name']} ---")

    # Check Rule 1: Attendance Compliance
    print("\nChecking Attendance Compliance...")
    if employee_record["attendance"] < 75:
        print("Attendance threshold condition: TRUE")
        attendance_rule = next(r for r in kb["rules"] if r["condition_type"] == "attendance_low")
        print("Rule matched:", attendance_rule["id"])
        return {
            "rule_id": attendance_rule["id"],
            "grade": attendance_rule["grade"],
            "status": attendance_rule["status"],
            "action": attendance_rule["action"]
        }
    else:
        print("Attendance threshold condition: FALSE")

    # Check Rule 07: Executive Honors
    print("\nChecking Executive Honors Criteria...")
    if employee_record.get("performance_rating", 0) >= 3.70 and employee_record.get("disciplinary_strikes", -1) == 0:
        print("Executive Honors condition: TRUE")
        honors_rule = next(r for r in kb["rules"] if r["condition_type"] == "executive_honors")
        print("Rule matched:", honors_rule["id"])
        return {
            "rule_id": honors_rule["id"],
            "grade": honors_rule["grade"],
            "status": honors_rule["status"],
            "action": honors_rule["action"]
        }
    else:
        print("Executive Honors condition: FALSE")

    # Check Performance Score Range Rules
    print("\nChecking Evaluation Score Ranges...")
    score = employee_record["evaluation_score"]
    for rule in kb["rules"]:
        if rule["condition_type"] == "score_range":
            print("Checking rule:", rule["id"])
            if rule["conditions"]["score_min"] <= score <= rule["conditions"]["score_max"]:
                print("Score condition: TRUE")
                print("Rule matched:", rule["id"])
                return {
                    "rule_id": rule["id"],
                    "grade": rule["grade"],
                    "status": rule["status"],
                    "action": rule["action"]
                }
            else:
                print("Score condition: FALSE")

    print("No matching rule found.")
    return {
        "rule_id": "UNKNOWN",
        "grade": "N/A",
        "status": "Review Pending",
        "action": "Consult with HR department regarding evaluation log."
    }
```