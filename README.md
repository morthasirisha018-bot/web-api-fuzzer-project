import requests

# Target API URL (example)
url = "https://example.com/api/login"

# Different test inputs
payloads = [
    {"username": "admin", "password": "admin123"},
    {"username": "' OR 1=1 --", "password": "test"},
    {"username": "<script>alert(1)</script>", "password": "1234"},
    {"username": "A"*5000, "password": "longinput"},
    {"username": "!@#$%^&*", "password": "random"}
]

print("Starting API Fuzz Testing...\n")

for payload in payloads:
    try:
        response = requests.post(url, json=payload)

        print("Testing payload:", payload)
        print("Status Code:", response.status_code)

        # Detect possible vulnerabilities
        if response.status_code >= 500:
            print("⚠ Possible server error detected!")

        if "error" in response.text.lower():
            print("⚠ Error message detected in response!")

        print("Response:", response.text[:100])
        print("-" * 50)

    except Exception as e:
        print("Request failed:", e)
