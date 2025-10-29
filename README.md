flowchart TD
    A[Website Visitor] -->|Visits Gift Category| B[AB Tasty]
    B -->|Generates Anonymous ID<br>Tracks interactions| C[AB Tasty Tracking Data]
    A -->|Clicks "Back in Stock" + Enters Email| D[CRM]
    B -->|Attaches "Gift" tag to email payload| D
    D -->|Stores Email + Creates Unique ID| E[CRM Database]
    D -->|Sends Email with Product Recommendations| A2[User Email Inbox]
    A2 -->|Clicks Recommended Product Link| F[Mobile App]
    F -->|User Creates Account| G[App Database]
    G -->|Matches via Email + Unique ID| E
    G -->|Sends User Events + Segments| H[Wandz SDK]
    E -->|Stores Unique ID + Gift Segment| I[CRM Hub]
    I -->|Syncs Unique ID Cookie| J[Website Login]
    J -->|Recognises User + Personalises| B
    I -->|Shares User Data + Segment| K[In-Store Clienteling App]
    K -->|User Identifies with QR/Barcode| L[In-Store Interaction]
    L -->|Retrieves Email + ID + Segment| I

    subgraph GDPR_Compliance
        note1[All data stored in EU]
        note2[Consent managed per GDPR]
        note3[Clauses defined in client contract]
    end
