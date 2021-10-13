<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css"/>
## [About Me](/) / Projects  
### Workshop Planning
My first major project was to create a scheduling tool to allow various CNC stations in a workshop to have visibility of upcoming tasks and their priority.
The software was to replace a card based system where different coloured cards represented the priority of the job.
As you can imagine, the cards were often lost or cherry picked, so a better solution was required.
The data on the tasks, and bills of materials were managed by Sage Line 500 backed by an Oracle database.
The solution was a WinForms application written in VB.Net which called stored procedures to obtain, and update this data.
Eventually this software grew to include primitive capacity planning capabilities.  
`VB.NET` `Oracle` `T-SQL` `Entity Framework`

### Customer Portal
This project was to allow customers from around the world to log in to view the status of their orders.  The complexity here was that order data was stored in 3 different locations globally, using 2 different ERP systems.  This was abstracted away by creating an OData API, which mapped the data model for each ERP system to a common model which could be consumed by the customer portal. This was eventually flipped, and the data for the orders was scraped from each location into a cental data warehouse closer to the web host to enable better query performance.  The portal allowed customers to get the current status of their orders, and perform stock enquiries.  Security was key here, given this was a public facing website, extra attention went into making sure the site was not vulnerable to any of the OWASP Top 10.
`C#` `ASP.NET MVC` `OData` `Oracle` `Microsoft Sql Server` `T-SQL` `Entity Framework 4`

### Goods In Trailer CCTV
Joining an existing team here who were working with warehouse automation.  Goods vehicles docking at a warehouse would have their trailer physically lifted into a docking bay, where operatives then opened and unloaded the trailer.  There was a requirement to take a photo of the condition of goods inside the trailer before unloading could commence.  This was initially done using a hand held digital camera, the contents of which then needed to be uploaded manually.  However, the warehouse automation had sensors that could detect when the trailer doors were opened, and by making use of the Web API provided by the CCTV system located opposite each docking bay, it was possible to automatically take a snapshot of the trailer interior whenever the trailer doors were opened.  These images were then stored as blobs in a SQL Server database, and a web dashboard was created to allow the images to be browsed and downloaded.  Access to the dashboard was controlled by Windows Active Directory.
`C#` `ASP.NET Web Api` `AngularJs` `Microsoft Sql Server` `T-SQL`

### Supplier Integration API
The requirement here was to create a proxy system for the management of orders which were to be fulfilled on demand by 3rd party suppliers.
Given that each supplier's API could have a different contract (though all with a common theme), it was possible to create a system that abstracted the 3rd party API's behind a simple interface.  Customer orders entered the system via MQ Messages and were transformed into orders for the relevant supplier API.  The supplier API was called to send the order and any status updates were simularly mapped back to a common model which could be understood internally.  A dashboard was provided where the status of orders could be monitored, with charts showing daily throughput per supplier, and hourly figures during the day.  Access to the system was restricted using Windows Active Directory groups.  
`C#` `ASP.NET Core` `Web Api` `Angular` `Microsoft Sql Server` `Entity Framework Core` `IBM Websphere MQ`

### Stock Checking
A system which directs stock checkers to validate stock levels at specific locations within a warehouse.
Data on which locations need to be validated are provided via IBM MQ Messages and an ETL operation imports the data into a SQL Server database.
A handset device then interrogates this data to direct users to the locations within the warehouse
`C#` `ASP.NET Core` `Web Api` `Angular` `Microsoft Sql Server` `Entity Framework Core` `IBM Websphere MQ`

### State Machine Runner
A generic presentation layer rendering the UI for various state machine based applications.
The Angular front end provides various pre-defined 'states' where a user is prompted for an action and exposes a contract to which a remote API can implement.
User inputs are sent to the configured remote API, where any process specific business logic is executed, and a standard response is returned which instructs the State Machine Runner which state to render next.
State Machines are configured using a dedicated admin screens, and state is preserved using Cosmos DB.
`C#` `ASP.NET Core` `Web Api` `Angular` `Azure Cosmos DB` `SignalR` `Azure App Service`

### Parcel Packing
`C#` `ASP.NET Core` `Web Api` `Angular` `Azure Cosmos DB` `Azure App Service` `Azure Service Bus`
