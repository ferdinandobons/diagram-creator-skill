# Example: OAuth 2.0 Authorization Code Flow

**Input:** "Create a diagram of the OAuth 2.0 authorization code flow"

**Topology:** left-to-right

**Steps:**
1. User — Clicks "Login with Google" — red
2. Client App — Redirects with client_id, scope — red
3. Auth Server — Consent screen — orange
4. Redirect — Auth code to redirect_uri — orange
5. Token Exchange — code + client_secret (server-to-server) — blue
6. Access Token — access_token + refresh_token — blue
7. API Call — Bearer token in header — cyan
8. Resource Server — Validates, returns data — green

**Callout:** "Step 5 happens server-to-server. The client_secret never reaches the browser."

**Legend:** red=Client-side, orange=Authorization, blue=Token exchange, cyan=API access, green=Protected resource
