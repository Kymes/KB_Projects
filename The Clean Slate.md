# The Clean Slate
 
One of the biggest problems with networking is organization.<br>
What I’ve built is a declarative networking CRM designed to streamline professional outreach an help "clean the slate". <br>
Built entirely with native Salesforce tools, the architecture utilizes a clean Master-Detail hierarchy where the Campaign object houses Contact records, which in turn manage individual Interaction records. <br>
This nested structure ensures that all touchpoints, meeting notes, and follow-up activities bubble up directly to the parent campaign for clear visibility without requiring custom code.<br> 
The system provides an out-of-the-box pipeline for tracking active conversations and conversion milestones.<br> 
Users can easily check out the CRM in their own sandbox or Developer Edition org by using the provided unmanaged package ID to test the data model firsthand.
--- 

## **Components:** 

### **Objects and Fields:**  

- Networking Campaign
    - Networking Campaign Name
- Networking Contact
    - First Name
    - Last Name
    - Type
        - Recruiter
        - Peer 
        - Hiring Manager
    - Linkedin URL
    - Title
    - Company
    - Campaign Name
        - Lookup to Networking Campaign object
    - Status
        - Prospect
        - Connection Sent
        - Accepted/Nurturing
        - Pitch Sent
        - Meeting Scheduled
        - Review Completed
        - Closed/Endorsed
        - Stale Lead (+14 days) 
    - Follow Up Date
    - Referral 
        - Checkbox
    - Referred by whom
        - Required if Referral checkbox is True
    - Demo Complete
        - Checkbox
    - Endorsement Received
        - Checkbox
- Interaction
    - Interaction Name
    - Date 
    - Type
        - Picklist
            - Zoom Demo
            - Coffee Chat
            - DM
    - Networking Contact
        - Lookup to Networking Contact Record
 

### **Automation:** 

- Prospect Follow Up Flow
    - calculates the date to follow up as 2 days after the creation date
        - Formula: TODAY() + 2
-  Pitch Sent Follow Up Flow
    - sets a follow up date when record Status field changed to Pitch Sent
        - Formula: TODAY() + 4
- Connection Sent Follow Up Flow
    - calculates date when Status field updated to Connection Sent
        - Formula: TODAY() + 2

### **Package ID** 
- 04tak000000hxJN
- https://login.salesforce.com/?ec=302&startURL=%2Fpackaging%2FinstallPackage.apexp%3Fp0%3D04tak000000hxJN

### **Permission Set:** 

-  Clean Slate CRM User

### **Validation:** 

- none

## **Automation Logic** 


 
### **Data Component(s):** 

- Formula Resources:
    - Prospect Follow Up
        - frm_ProspectDateFollowUp
    - Pitch Sent Follow Up
        - frm_PitchSentFollowUp
    - Connection Sent Follow Up
        - frm_ConnectionSentFollowUp

### **Logic Component(s):** 

- TODAY() + 2
- TODAY() + 4
