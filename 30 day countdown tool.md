# 30 Day Countdown: Lead scoring tool

Every business needs leads to thrive. No business thrives with bad leads. There is a real risk of a lead going cold if visibility drops off. Without a way to keep track, the leads go stale, begin to pile up and before you know it, data-rot sets in leaving an inaccurate, bloated mess of cold, stale records.  

The 30-Day Countdown is built as a lead scoring tool with safeguards to ensure that leads don’t go stale. When a record is created, it is assigned a record score based on completeness. A unique record identifier is also assigned for auditing purposes. 
After 48 hours - if the record isn’t complete - a Chatter message sent to the assigned user for follow-up. At 30 days - if the lead score is still under 100 - the user is notified again and the record is deactivated. 

The record can be reactivated with a completed record score of 100 but to prevent the user from entering “dummy” information, two validation rules are used. The first validation rule guarantees only accurate record information is entered and the second stops reactivation of the record unless the score is at 100 and the information is filled out correctly ensuring clean, actionable data. 

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

- URL: https://login.salesforce.com/packaging/installPackage.apexp?p0=04tak000000QRKj
- Package ID: 04tak000000QRKj

---

### **Post-Install Instructions:**

When the package has been installed, please go to:

Setup > Permission Sets > “guest user: 30 day countdown"

Assign the Guest User Permission Set to yourself
