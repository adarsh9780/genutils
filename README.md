flowchart TD
    A1[Open Excel sheet (Teams folder)]
    A2[Open C360]
    A3[Sort data in C360 based on:
        a) Job Number<br> 
        b) Assignment Date (Previous Day)<br> 
        c) Status = New]
    A4[Open Jobs from Sorted Results]
    A5[Input Data into Automation App<br>(Python/UI Pathway/Alteryx)]
    
    subgraph Automation Steps
        A6a[Create folders in Shared Drive<br>(for portfolios)]
        A6b[Download reports from File Manager]
        A6c[Put data into Excel Sheet (Teams folder)]
    end

    subgraph Excel Columns (from Step 6c)
        B1[Name of Reviewer (Auto)]
        B2[Job Number (Auto)]
        B3[Account Officer Name (Auto)]
        B4[Line of Business (Auto)]
        B5[Loan Closing Date (Auto)]
        B6[Desired Review/Delivery Date (Auto)]
        B7[Information Details]
        B8[Requester Comments]
        B9[Status of Project = In Progress]
        B10[Date of Status = Today]
        B11[Address]
        B12[City]
        B13[State]
    end

    subgraph Info Details
        C1[Type of Review (Review/Addendum)]
        C2[Type of Loan (New/Internal/External Refi/Purchase)]
        C3[Priority (Rush/High)]
        C4[Property Type (Residential/Retail/Commercial/Mixed-use)]
        C5[Prior Projects Search (Manual)]
        C6[File Manager Search (Manual)]
    end

    A1 --> A2
    A2 --> A3
    A3 --> A4
    A4 --> A5
    A5 --> A6a
    A5 --> A6b
    A5 --> A6c
    A6c --> B1
    A6c --> B2
    A6c --> B3
    A6c --> B4
    A6c --> B5
    A6c --> B6
    A6c --> B7
    A6c --> B8
    A6c --> B9
    A6c --> B10
    A6c --> B11
    A6c --> B12
    A6c --> B13

    B7 --> C1
    B7 --> C2
    B7 --> C3
    B7 --> C4
    B7 --> C5
    B7 --> C6
