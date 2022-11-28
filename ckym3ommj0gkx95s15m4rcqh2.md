# Solving A Real World Business Problem using Web Dev

I decided to write about a real world problem solving experience using web development and programming skills in my day job. This project is one of my favorite because I was able to identify a business problem, propose an idea, and implement a solution all on my own over the course of a few short months. Working as the solo developer allows me great flexibility, but also requires a lot of time invested in a project such as this one. Continue reading to learn about my process and approach. 

This is not a complete representation of the code that it took to solve the problem, any code shown has been modified to redact or remove any potential sensitive information. 

## Background 
In my day job, I work with attorneys at a small legal firm as the sole IT Manager / Developer / System Administrator. The software we use allows me to create custom web apps that interface with our software and database to solve problems. When other attorneys file documentation with our office, our staff reads that documentation and creates a summary narrative of all the content on that documentation and enters that as text content into our software. 

## Problems Identified 
There are several problems with this approach that are causing problems throughout the organization. These documents being filed are crucial to the administration of the case and the summary document is referenced many times over the lifespan of the case which is 3-5 years for most cases. 

**Time**

Our staff spends several hours a week reading the documentation and creating summary narratives. 

 **Consistency**

With multiple staff members reading these documents, they had had their own flavor of creating the narrative. Each staff member had a unique way of writing, formatting, and styling their summary which created a variety of different styles that our two attorneys had to decipher each week. 

**Tracking Changes**

The other attorneys may submit the same form, with subtle or major changes to the document between the last version and this version. Some cases have 6 or more versions of the same document filed over the lifetime of the case. Our staff must compare, line by line, the previous and current document and manually highlight the changes. 

**Human Error**

Since the staff are reading, interpreting and writing their own version of the form, this introduced a lot of human error, resulting in the attorneys just reading the document themselves instead of the narrative. 

## Project Idea
While observing this business process, I came up with the idea of a web form that the staff could fill in the important details that change on these forms, and have the narrative completed automatically. Our primary case management software is a local web based application allowing full HTML, CSS, and JavaScript pages to run that can also connect into the SQL database where the data was stored which made a web based approach perfect. 

## Project Proposal 
I met with the owner and proposed my web form idea on the basis: 

I can create a streamlined approach that all staff will use for each form submitted, that will provide us the following benefits. 

- Faster Summary Narratives 
- Consistent Entries 
- Streamlined Formatting
- More Accurate 
- Automatic change tracking 

The business owner agreed to commission this project immediately. This project will take approximately 3 months and will be my main focus outside of daily job duties and IT priority issues that come up along the way. 

## Project Stakeholder Buy-In
The most important part of any major business project that will affect all staff is to get buy-in from the stakeholders. This ensures the greatest project success and is achieved by making the stakeholders feel that this project is going to benefit them and allow them to shape the final result of this project. 

To accomplish buy-in, I held meetings with each staff member to understand how they  created the narratives, what information on the form is important, the types of data that will be submitted, and expectations for the generated narrative. 

Each staff member had a list of items that were important to them, based on how they were taught to do these narratives. I reviewed all my notes and found consistent items were the data being entered and the types of data. I used this to hold a series of compromise meetings to determine what would work best for the company and establish a baseline rule for the content, the format, style of the notes, and layout of the web form. 

The staff feeling at this point is a little bit nervous because of the pending change to their workflow, but also excited about the idea of saving time and freeing up their time to be better spent elsewhere. 

## Project Steps
To start this project, I created a simple mock-up of the design that will be used for the various types of content. Some sections had check boxes, others had select drop downs, and others had tables of content being filled out. I used a blank copy of the form that attorney's fill out to give me an idea of the base line content that I needed to plan. 

My code base allows for HTML, CSS, JS, jQuery, SQL and Bootstrap by default, and this project doesn't require anything more than these technologies to accomplish this project. 

There are 20 sections to this document that hold various data from the plan number, to the options and tables of data contained within. I sorted my HTML code into 20 separate sections and started coding these sections one at a time. These sections are critical to the design and are a basis of tracking changes later in the project. 

This layout gave me an idea for what type of content that I would be storing in the database. I designed and created a database table for storing all of the content and how it would relate to other tables in the system for consistency with how the other tables are stored. 

```sql
Create Table tblCustomNarrativeData(
    PlanID INT IDENTITY(1,1) PRIMARY KEY, -- Auto incrementing primary key ID number 
    PlanNumber INT, -- Establishes PlanNumber for reference, defaults to 1 or value of amendeded plan doc number to track changes 
    PLanDate DATE,
    CaseID INT, --Foreign Key with tblCases to join on CaseID 
    SECTION INT, -- Section of worksheet variable that is updated throughout script 
    tblRow INT,
    COL1 VARCHAR(MAX), 
    COL2 VARCHAR(MAX), 
    COL3 VARCHAR(MAX), 
    COL4 VARCHAR(MAX), 
    COL5 VARCHAR(MAX), 
    COL6 VARCHAR(MAX), 
    COL7 VARCHAR(MAX),
    COMMENT VARCHAR(MAX),
    AMENDED INT,
    CONSTRAINT FK_CaseID FOREIGN KEY (CaseID) REFERENCES tblCaseData(CaseID)); -- Establishes FK for CaseID 
```

After having the initial layout requirements and database ready it was time to start coding the layout. I started with HTML and setting up all of the content for each section of the form. There are 20 sections and each one holds a different set of content. I used Expanding and Collapsing sections to allow the staff to expand only the sections that required filling out and avoid overloading the screen with too many input fields. I also used jQuery to limit the table rows of each table section to just display 1 row by default, and expand up to 10 rows as needed for each section. 


![The web form showing check boxes and input fields](https://cdn.hashnode.com/res/hashnode/image/upload/v1642629047534/xvqR1avcH.png)

I tested sending data to the database and ensure we had information flowing as far as text box content ending up in the temporary table and permanent storage tables. 

![SQL database showing content in the database](https://cdn.hashnode.com/res/hashnode/image/upload/v1642628968974/Vfl-TgKXQ.png)

### Saving Data (temporary) 
To save the data as the user fills out the form I created an OnChange event handler that would write content to a temporary database based on the ID of the input field. This way, if the user experienced any issues or needed to close and re-open then all content will be saved until submitted. 

### Submitting Data to Database 
At the conclusion of completing all of the appropriate fields, the user then clicks Update which executes a SQL script on the server. This script takes all of the data from the temporary database, checks through every section for content, formats data as needed into Dates, Money, VarChar, and other data types, and pushes into my custom table in the database. 

The narrative text is stored in another table of the database. I wrote a second script that runs at the conclusion of the previous script to gather all of the database records from the data just entered, and then pushes an update to the narrative text table. This data had to be formatted different based on how it will be displayed in the software. The software is able to render pure HTML content for displaying narrative text to this user which allows me to customize how I present the data and create a standard format for all narrative notes. Using HTML combined with SQL, I pushed a full HTML body section to the narrative table that includes tables, checkboxes, and calculations, and other values to be rendered. This ta6ble stores it as a varchar(max) but when ready by the software it's converted into HTML so all of my HTML tags work. 

Example of mixing HTML with SQL Query to generate a table, using inline styling because that's the only option that works when this is rendered in the software. 

```sql
(SELECT 
    '</br><table style="width:auto;margin-bottom:4px;">
        <caption style="text-align:left;font-size:.8rem;font-family:Arial,Helvetica,sans-serif;"><strong> Plan Step</strong></caption>
        <thead>
            <tr>
                <th style="width:auto;text-align:left;padding:2px;border: 1px solid black;background:lightgrey">Start Month</th>
                <th style="width:auto;text-align:left;padding:2px;border: 1px solid black;background:lightgrey">End Month</th>
    <th style="width:auto;text-align:left;padding:2px;border: 1px solid black;background:lightgrey">Amount</th>
            </tr>
        </thead>' + 
    
(SELECT '<tr>' + 
            '<td style="width:auto;text-align:left;vertical-align:top;padding:2px;border: 1px solid black;">' + COL1 + '</td>' + 
            '<td style="width:auto;text-align:left;vertical-align:top;padding:2px;border: 1px solid black;">' + COL2 + '</td>' + 
            '<td style="width:auto;text-align:left;vertical-align:top;padding:2px;border: 1px solid black;">' + COL3 + '</td>' +
        '</tr>' FROM tblCustomNarrativeData  WHERE PlanNumber = @PLANID AND CaseID = {Params.CaseID} AND SECTION = 40 FOR XML PATH (''),TYPE).value('.', 'VARCHAR(MAX)') + '</table>' )
```


## Tracking Changes
This was by far the most difficult part of the entire project, but also the most rewarding to actually solve. The concept here is that when a new version of the form is filed with our office, we need to know what changed from the document, and bring attention to these changes so when the attorney reads the updated forum note, they know exactly what changed in the form without having to read the whole note. My idea for this was to use the HTML `<mark>` tag which adds a highlight, default Yellow, to all fields that changed. 

Using SQL I needed to be able to look at the last form filed by form number, and the new form filed by form number, and compare the two sets of table data and see what changed.  I also needed to know which sections had been changed, or if a section that previously didn't have content, now has content.  To accomplish this, The data had to go through a series to checks to determine what changes happened if this was an amended form. 

```sql
--Add Date to Plan for reference purposes 
Update tblCustomNarrativeData SET PlanDate = GetDate() where CaseID = {Params.CaseID} and PlanDate IS NULL; 

-- Need to determine if previous plan exists in tblCustomNarrativeData for this case. 
-- If no plan has been filed in new table then we don't want to track changes since it will highlight everything. 
Declare @PrevPlanV2Exists INT;
Set @PrevPlanV2Exists = (Select Case when (Select DISTINCT CaseID from tblCustomNarrativeData where CaseID = {Params.CaseID}) = {Params.CaseID} then '1' ELSE  '0' end)

--Change the PrevPlanID to account for the random plan numbers being saved in database
 SET @PREVPLANID = (Select TOP 1 PlanNumber from tblCustomNarrativeData where section = 111 and CaseID = {Params.CaseID} and PlanNumber <> @PlanID order by PlanID desc)

--CLEANUP PREVIOUS PLAN IN tblCustomNarrativeData to remove <mark> and </mark> for previous highlighting before comparing fields 
 Update tblCustomNarrativeData set COL1 = Replace(Replace(COL1, '<MARK>', ''),'</MARK>', '') where PlanID in ( Select PLanID from tblCustomNarrativeData where COL1 like '%<MARK>%' and caseid = {Params.CaseID} and PlanNumber = @PrevPlanID)

--Table Comparison
--Using Inner Join on same table by matching CaseID, Section, and Row, check to see if the data is different
--If the data is different, concatenation is done to add html '<MARK>' and '</MARK>'
Update tblCustomNarrativeData set COL1 = CONCAT('<MARK>', COL1, '</MARK>') where PlanID in(
Select d1.PlanID from tblCustomNarrativeData d1
INNER JOIN tblCustomNarrativeData d2 on d1.CaseID = d2.CaseID and d1.SECTION = d2.SECTION  and d1.tblRow = d2.tblRow
where @PREVPLANID <> 0 AND d2.col1 <> d1.COL1
and d1.section <> 111 and d2.section <> 111
 and d1.CaseID = {Params.CaseID} and d2.CaseID = {Params.CaseID} 
 and d1.PlanNumber = @PlanID and d2.PlanNumber = @PREVPLANID)

 --This function checks for new sections that were not part of previous plan and sets Amended to 1. 
        --Only works if not original and a previous plan exists in table  
        Declare @Amended VARCHAR;
        Set @Amended = (SELECT CASE WHEN dbo.udfGetWorksheetData({Params.CaseID},@WORKSHEETID,'CheckBox',112,0) = CAST(1 AS BIT) then '1' ELSE  '0' end) 
        Select CaseID, PLanNumber, Section into #scrows from tblCustomNarrativeData where CaseID = {Params.CaseID} and PlanNumber = @PlanID group by CaseID, PlanNumber, Section
        Select CaseID, PlanNumber, SEction into #sprows from tblCustomNarrativeData where CaseID = {Params.CaseID} and PLanNumber = @PREVPLANID group by CaseID, PlanNumber, SEction
        --Create 2 temp tables, using an EXCEPT clause, determine new sections and Set Amended to 1 
        Update tblCustomNarrativeData Set Amended = 1 where CaseID = {Params.CaseID} and @Amended = '1' and @PrevPlanV2Exists = '1' AND @PREVPLANID <> 0 AND PlanNumber = @PlanID and Section IN 
        (
            SELECT Section from #scrows
            EXCEPT
            SELECT Section From #sprows 
   );

	Update tblCustomNarrativeData 
    SET Amended = 1
    where PlanID in(
    Select d1.PlanID from tblCustomNarrativeData d1
    INNER JOIN tblCustomNarrativeData d2 on d1.CaseID = d2.CaseID and d1.SECTION = d2.SECTION and d1.tblRow > d2.tblRow
    where d1.CaseID = {Params.CaseID} and d2.CaseID = {Params.CaseID}
	and d1.tblRow NOT IN (Select tblrow from tblCustomNarrativeData where section = d1.SECTION and PlanNumber = @PrevPlanID and tblrow <> d2.tblRow)
    and d1.section <> 111 and d2.section <> 111
    and d1.PlanNumber = @PlanID and d2.PlanNumber = @PrevPlanID
    )

   --For rows with Amended (meaning new sections or rows, add <mark> </mark> to all column fields since they are all new)
 Update tblCustomNarrativeData 
 set COL1 = CONCAT('<MARK>', COL1, '</MARK>'), 
 COL2 = CONCAT('<MARK>', COL2, '</MARK>'), 
 COL3 = CONCAT('<MARK>', COL3, '</MARK>'),
 COL4 = CONCAT('<MARK>', COL4, '</MARK>'),
 COL5 = CONCAT('<MARK>', COL5, '</MARK>'),
 COL6 = CONCAT('<MARK>', COL6, '</MARK>'),
 COL7 = CONCAT('<MARK>', COL7, '</MARK>')
 where PlanID in(Select PlanID from tblCustomNarrativeData where AMENDED = 1 and PlanNumber = @PlanID and CaseID = {Params.CaseID} and Section <> 111)
```

The result of this code is that all modified data from the form is properly tracked and highlighted when it's rendered for display for the users. A full comparison is performed on every character of every box so all the user has to do is fill out the form, update any changed fields, and let the system do the rest of the work for them. 

### Summary Section
On the web form that I created, I also created a Summary Section, using JavaScript and JQuery, to build a live rendered view of what the narrative text would look like when it submitted. This allowed the staff to do a quick check and determine if they missed anything when completing the form before the information was submitted and saved to the database. The yellow highlights here are similar to what will appear in the narrative text section of the software once completed. This section live updates as fields are modified in the web form. 

![The summary section as described above](https://cdn.hashnode.com/res/hashnode/image/upload/v1642629084674/ByKqBlAdu.png)


## The End Result
This project was a success! The proposed project came together great and was rell received by the staff that use it every single day to fill out these forms when our office receives them. 


![A well formatted HTML narrative note using this tool](https://cdn.hashnode.com/res/hashnode/image/upload/v1642629154963/TVZwRlThj.png)


## Project Conclusion
This project went through several revisions and updates from first draft to production, and 5 major updates were derived along the journey, and over 5,000 lines of code were written to achieve the result. 

The business and staff now benefit from the increased efficiency, improved accuracy, and consistent summary narratives. The attorney's are happy that now all notes going forward will be consistent and uniform, along with tracked changes to make amended forms clear on any changes made. 

Tracking of changes is the single most important benefit of this entire project.  Comparing two versions of a form, line by line, is very time consuming and often leads to missed changes. This new method ensures that all changes to the form are caught each and every time, and attention is drawn immediately to all changes to eliminate any guess work. 

Overall, everyone benefited from this project and now over 75,000 lines have been written to the custom table full of content from this web app being used in less than 1 year. The staff have said this cut their daily work on filling out and comparing plans down from 20-30 minutes each document to 5 minutes or less for each document. If we look at this as 4-10 forms per day this is hours worth of saved time! 

The best part about being a developer is being able to create a custom solution to a problem that isn't possible using something off the shelf. There is not a product that existed that we could have purchased that would have worked directly in our software, with our database, and achieved the same result as this custom development project has for the business.


 

I’m on @buymeacoffee. If you like my work, you can buy me a taco and share your thoughts 🎉🌮
<a href="https://www.buymeacoffee.com/ChrisBenjamin" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>