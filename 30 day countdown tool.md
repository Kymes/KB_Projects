# 30 Day Countdown:

This Salesforce data hygiene tool automates the lifecycle of a record by utilizing a custom Record Score formula to enforce data integrity and completeness. Through a combination of record-triggered flows and validation rules like the Clean Data Gate, the system guides a prospect from creation to either a verified Contact status or automated deactivation after 30 days of inactivity. By integrating scheduled Chatter notifications and a Reactivation Lock, the tool ensures that only high-quality, fully populated data remains active within the environment.

---

## **Components:**

 ### **Object:**
- Entry
 ### **Fields:**
- Record Name
- First Name
- Last Name
- Email
- Record Score
- Type
- Status
- Source
- Contact Number
- Company Name
- Position/Title
### **Automation:**
- ### Flows ###
    - Record Name
    - 48 Hour follow-up
    - 30 Day follow-up
    - Record Score Update
### **Formulas:**
- ### Record Score - formula field ###
    - IF(ISBLANK(TEXT(Source__c)), 0, 25)+  
    IF(ISBLANK(Company_Name__c),0, 25)+  
    IF(ISBLANK(Position_Title__c), 0, 25)+  
    IF(ISBLANK(Contact_Number__c), 0, 25)

### **Validation Rules:** ##
- **Clean Data Gate**
    - OR(  
        CONTAINS(LOWER(Contact_Number__c), "n/a"),  
    CONTAINS(LOWER(Company_Name__c), "n/a"),  
    CONTAINS(LOWER(Position_Title__c), "n/a")  
    )
- **Reactivation Lock**
    - AND(  
        ISPICKVAL(PRIORVALUE(Status__c), "Inactive"),  
        ISPICKVAL(Status__c, "Active"), Record_Score__c < 100
    )

---

## **Automation Logic:**


 ### **Data Component(s):**

- ### Flow Resources 
    - Text Template - 48 hour Reminder
        - **tt_ChatterAlert48hr**
    - Text Template - 30 Day Deactivation
        - **tt30_ChatterAlert**
    - Formula - Record name
        - **frm_RecordName** 

### **Logic Component(s):**

- ### Flow Elements: ###
    - ***48 hour Reminder***
        - ***Chatter Action: Post to Chatter***
            - Label: 48 Hour Reminder
                - message: **tt_ChatterAlert48hr**
                - Target Name or ID: !$Record.Id
    - ***30 Day follow up***
        - ***Chatter Action: Post to Chatter*** 
            - Label 30 Day follow up
                - message: **tt_ChatterAlert**
                - Target Name or ID: !$Record.Id
        - ***Decision Element***
            - Label: Is still incomplete
                - !$Record.Record_Score__c Less Than 100
                - !$Record.Type Equals Prospect
        - ***Update Records Element***
            - Label: update record status
                - Status: Inactive
    - ***Record Score Update***
        - ***Update Records***
            - Label: Record type update
                - Type: Contact
    - ***Record Name***
        - ***Assignment***
            - Label: Record Name
                - !$Record.Name Equals **frm_RecordName**





---
### **Permission Set:**
- guest_user_30_day_countdown

### **Links:**

- URL: https://login.salesforce.com/packaging/installPackage.apexp?p0=04tak000000PpIf
- Package ID: 04tak000000PpIf

---

### **Post-Install Instructions:**

When the package has been installed, please go to:

Setup > Permission Sets > “guest user: 30 day countdown"

Assign the Guest User Permission Set to yourself
