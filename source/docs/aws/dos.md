# Denial of service

Usage plans can be used to configure throttling limits and quota limits on individual client API keys. However, if an API key gets leaked, even if the data revealed by the API might not be sensitive, the attacker can try to exhaust the usage plan request quota and cause disruption of the service.

Objective: Exhaust the usage plan request quota and cause a denial of service.
