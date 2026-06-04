## Enrolment (NHS Instigated)

This diagram shows the high level sequence of events and communication involved in enroling a patient on a DTx
```mermaid
  sequenceDiagram
    actor Patient
    NHS->>DTx: Patient Enrolment (NHS Number, Names, DoB, Email Address, other required fields)
    DTx->>DTx: Validation and Creation of Patient Account
    DTx->>NHS: Acknowledge Enrolment and Account Creation (MI)
    NHS->>Patient: Notification of Enrolment
    DTx-->>Patient: Welcome Email / Onboarding Instruction
```

## DTx App Launch - Pre-installed

This is the sequence for when a patient already has the DTx app installed and they click on a link in the NHS App to launch the DTx App. It essentially uses a native equivalent of OAuth by opening links to the native apps with OAuth parameters, instead of links to the websites with OAuth parameters.  Initial investigation has shown this should be possible.
```mermaid
  sequenceDiagram
  actor Patient
    Patient ->> NHS: Open NHS App / Login
    NHS ->> Patient: Session Tokens and App Content (Universal Links to relevant DTx)
    Patient->>DTx: DTx Universal Link requesting OAuth Start
    DTx->>DTx: OAuth 2 Start Authorisation Request
    Note right of DTx: Example<br/> HTTP Request<br/> GET https://dtx-authorisation-service-api/nhs-app-login <br/><br/>HTTP Response <br/>302 Found<br/> Location: https://nhs-app-universal-link/authorize?<br/>response_type=code&<br/>client_id=<clientId>&<br/>redirect_uri=https://dtx-universal-link/redirect&<br/>scope=email openid profile&<br/>state=<state as per spec>
    DTx->>Patient: NHS App Universal Link with OAuth authorization request
    Patient->>NHS: NHS App Universal Link with OAuth authorization request
    NHS->>Patient: OAuth Authentication Flow
    Note right of Patient: User will already be authenticated in NHS App so this should be largely a no-op
    Patient->>NHS: OAuth Authentication Flow
    NHS->>NHS: Generate DTx Universal Link with OAuth Exchange Code
    NHS->>Patient: DTx Universal Link with OAuth Exchange Code
    Note right of NHS: Example<br/> https://dtx-universal-link/exchange?<br/>code=<exchange-code>&<br/>state=<state as per auth request>
    Patient->>DTx: DTx Universal Link with OAuth Exchange Code
    DTx->>DTx: Exchange OAuth code for Session Tokens, checking state matches authorisation request
    DTx->>Patient: Session Tokens and App Content
```

## DTx App Launch - SSO through Installation

This is the sequence for when a patient does not have the DTx app installed and they click on a link in the NHS App to launch the DTx App. 
It relies on a custom app page, which should theoretically be able to launch an app with a custom parameter, this being one which initiates the OAuth style communication as above.
```mermaid
  sequenceDiagram
  actor Patient
    Patient ->> NHS: Open NHS App / Login
    NHS ->> Patient: Session Tokens and App Content (Universal Links to relevant DTx)
    Patient->>AppStore: DTx Custom Page Link requesting OAuth Start
    AppStore->>Patient: App from Custom Page
    Patient ->> DTx: Launch with OAuth Start Launch Parameter
    Note right of DTx: Example<br/> HTTP Request<br/> GET https://dtx-authorisation-service-api/nhs-app-login <br/><br/>HTTP Response <br/>302 Found<br/> Location: https://nhs-app-universal-link/authorize?<br/>response_type=code&<br/>client_id=<clientId>&<br/>redirect_uri=https://dtx-universal-link/redirect&<br/>scope=email openid profile&<br/>state=<state as per spec>
    DTx->>Patient: NHS App Universal Link with OAuth authorization request
    Patient->>NHS: NHS App Universal Link with OAuth authorization request
    NHS->>Patient: OAuth Authentication Flow
    Note right of Patient: User will already be authenticated in NHS App so this should be largely a no-op
    Patient->>NHS: OAuth Authentication Flow
    NHS->>NHS: Generate DTx Universal Link with OAuth Exchange Code
    NHS->>Patient: DTx Universal Link with OAuth Exchange Code
    Note right of NHS: Example<br/> https://dtx-universal-link/exchange?<br/>code=<exchange-code>&<br/>state=<state as per auth request>
    Patient->>DTx: DTx Universal Link with OAuth Exchange Code
    DTx->>DTx: Exchange OAuth code for Session Tokens, checking state matches authorisation request
    DTx->>Patient: Session Tokens and App Content
```
