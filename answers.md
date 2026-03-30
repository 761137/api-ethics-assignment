# Task 1 — Classify and Handle PII Fields
full_name       - Direct PII    - Drop 
email           - Direct PII    - Drop/Mask
date_of_birth   - Indirect PII  - Mask (Ex.Keep only the year)
zip_code        - Indirect PII  - Mask (Ex. Keep only the first 3 digits)
job_title       - Indirect PII  - Keep
diagnosis_notes - Sensitive PII - Pseudonymize

full_name and email are Direct PII which conttains user personal information (which helps in identifying the user directly) and hence they need to be dropped or Masked
date_of_birth,zip_code and job_title comes under Indirect_PII which cannot be used to identify when alone, but the person can be identified when all 3 fields are together.
diagnosis_notes field contains sensitive personal and health information, so it should be pseudonymized to remove any details before sharing


# Task 2 — Audit the API Script for Ethical Compliance

 ## Issue 1 : Hardcoded API key:
     The API key is hardcoded in script, which is a security risk  and violates best practices
 ## Issue 2 : No Rate limiting:
     The script makes repeated API calls without any delay, which may violate API rate limits and overload the server
 ## Issue 3 : Storing raw PII data
     The script stores all collected records without cleaning sensitive data , which can lead to privacy violations
     
```python
import requests
import os
import time

API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.getenv("API_KEY")

records = []
for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    if response.status_code!=200:
      break
      
    data = response.json()
    records.extend(data["results"])

    time.sleep(1)

# Store all records permanently in company database
save_to_database(records)

```
