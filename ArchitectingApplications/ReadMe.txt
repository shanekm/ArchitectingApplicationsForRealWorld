Architecting Applications for the Real World in .NET

Programtic - sensible, just enough
- don't overcomplicate design/architecture
- not 'resume' driven but simply "barely" (ie. bridge that 'barely' stands)

Principles
1. Do simplest thing that could possibly work (KISS)
	- finish sooner
	- easier to communicate
2. Agile Principle
3. Loan Principle
	- eliminat waste
	- decide as late as possible
	- deliver as fast as possible
	- amplify learning
	- build integrity in
4. 80/20 Rule - 80% of benefit from 20% of features
	- 80% of benefit by writing tests for 20% most important features
5. Minimum Viable Product (MVP)

Austronat architecture - going overboard with design, features and development
	- Service architecture vs. No API
	- Unit tests vs. No tests

1. Presentation / UI- Web forms, MVC, WinForms, Silverlight
2. Service / API - Web API, WCF, ServiceStack, POCOs
3. Domain / Business Logic (BLL) - C#, VB.net, F#, IronRuby many more - you're deciding whych languate you're willing to use
4. Data / Data Access (DAL) - Entity frameowrk, nHibernate, Dapper, ADO.net

Layers vs Tiers
- Layers - logical (seperation via projects in .net ie. csproj files (project files))
	- Business layer, Data access, UI, Tests
	- Seperate concerns
	- Aid understanding - thing seperate responsibility
	- Abstract away complexity
	- Support testing - by defining interfaces these layers interact
	- Minimizes dependencies - Business no dependency on UI, but UI on Service layer
	- Enable reuse
	: Downside to Layers:
		- Leak
		- More code
- Tiers - physical seperation (DB server, web server etc)
	- Scalability - new server etc
	- Security 
	- Uptime
	- Reusable
	: Downsides to tiers:
		- Performance cost
		- Increased complexity

Small Application => methods = layers
Escalate as complexity justifies

Business Logic Layer - center of the application
	- handles the logic, manages behaviour, interacts with data access

Business Logic Layer Patterns
1. Transaction Script - Procedural
	- Simple, procdural, One public funciton per UI operation
2. Table Module - DataTable & DataSets - Data Driven - structure of DB dictates structure of clasees
	- Classes represent tables, DataTables, Datasets
3. Active Record - Data Driven
	- Each instance represents a DB row (POCO => customer in db = customer.cs class, new Customer() = row in DB)
	- Class knows how to persists itself
	- Contains business logic
	- Often implemented using (EF, Linq to SQL, Subsonic)
4. Domain Driven Design Model - Business driven approach
	- Design classes/object model without worrying what database looks like
	- Use clases to describe your customers business problem
	- All about objects (business objects and business problems)
	- Ex => Custome and Addrss = Meember class, Purchases => Orders.cs
	- Leverage design patterns
	- Abstracts ugly DB schema
	- Reusable
	- Compliments large teams => seperation


Service Layer - handle all other interactions between UI and Business Layer
	- Ex. => Click "Purchase" - interaction between product placement, email and shippment  business objects handled by Service layer
	- Web API, WCF, POCOs
	- Intermediary, Coordinate domain
	- Sits between Business Layer and UI
	- Really a facade (wraps something complex and makes it simpler)
	- Takes requests from one layer and sends to another
	Metathores:
		- Should be thin 
		- Delegate work to business objects - (makes proper calls / orchastrating things)
		- Security (in service layer)
		- Logging / Notification 
		- Bind classes and Manage transactions
		- Data Validation
	Example:
		Restaurant - Many service layers => waitress => kitchen, cook => other cooks etc
		Boss - Service layer is like a boss, orchastras between other layers, does not perform any complex taskss
		UI -> Service (SubmitPurchase) -> Domain Layer (Calcualte Tax, Shipping, Email etc)
	When to Use Service Layer?
		- Multiple UIs
		- Multiple consumers of business logic
	Where is it called?
		- WebForms: code behind  - calls service layer
		- MVC - controllers - calls service layer
	Implementing Service Layer:
		1. WCF
		2. WebAPI
		3. ServiceStack
		4. POCOs

	Service Layer: Web Service VS. Shared Library
	Web Service Advantages
	- Immediate bug fixes to all clients
	- Upgradable easily (hitting different end point), multiple versions
	- Consumers can NOT get to the code (complete abstraction)
	- Autonomous - can scale service hardware seperately
	- Technology agnostic - endpoint with JSON - JAVA or .net etc

	Shared Library Advantages
	- Native code called in process = higher performance
	- No serialization (JSON to object or XML like in Web Service approach)
	- No internet connection required
	- No risk of centralized service going down and impacting all consumers

	What Should Service Layer Return?
	- Data Transfer Object (DTO) - class with no methods inside - carries data across an applications boudries
	- Compy of domain entities without behaviour
	- Domain entity


Presentation Layer - UI, presentation logic, HTML, XAML etc.
	- WebForms, MVC, WPF, WinForms, Silverlight, JavaScript frameworks, CSS, Jqueryy, Knockout
	Goals for Presentation layer
	- Seperate data from the View (MVC)
	- Support concurrent development - seperate teams can work on UI and Business Layer at the same time
	- Minimize logic - avoid business logic and data to access creep
	What Belongs to Presentation layer
	- User interface
	- View model
	- Basic data validation
	Technologies
	Client => WPF, Modern UI, WinForms
	Web => MVC, WebForms, Silverlight, JavaScripts MVVM


Data Access Layer
	- Talks to Data base, abstracts away SQL, handles transactions
	Problem: SQL data and .NET objecst - How do we move data between DB and .NET oobjects
		- Map SQL Results to .NET data types
		- Active record example => customer => customer.cs, address => address.cs
		- Domain model example => customer and address => member.cs, purchases => order.cs
	Data Mapper (DAL): allows mapping DB objects to .NET objects
	DAL Responsibilities
		- CRUD - create, read, update, delete => ex. User.Save()
		- Transactions (Batch unit of work)
		- Concurrency (Handling concurrent processes sent by .net)
		Relational DB => Object Model: Tables : Class, Column : Properties, Rows : Instance

	Repository Pattern 
	- Hides data access behind Interface
	- Isolates domain objects from the database access code
	- Sits between Domain model and data access: Domain => Repository => Data Access
	Why Hide DB behind Inteface? - IMPORTANT!!! (LAYERING within Data Access layer)
		- Unit testing - allows for unit testings
		- Database independence - no knowledge of Database
		- Abstract complexity of multiple data sources
	ORM behind Repository?
		- When behind repository, simply an implementaiton detail

	Stored Procedures
		- Avoid multiple calls to the DB
		- Help protect from SQL injection
		- Can place query generation in hands of DBA
		- Limit data access
		- Not faster typically
	Choosing SPROC?
		- How much work is it to change a sproc vs code in your application
		- Is your DB under source control? => No? = risk
		- How automated is your deployment process?
		- Risk: Are we placing business logic in sprocs?


Architectural Levels
	Level 1 : Simplest vs. Level 2 : Somewhere in between vs. Level 3 : Every tool in the shed
	More complex => More scalable

	Steps to Add a feature in Level 1:
	1. UI change
	2. DB column change
	3. Regenerate DAL	
	4. Implement BL (in AR class)

	Steps to Add a feature in level 3:
	1. UI change
	2. DB POCO change
	3. Update / create interface
	4. Update DB mappings
	5. Update Add service calls as part of this new feature
	6. Add / Update tests

	Feature Comparison - L1 VS L2 - IMPORTANT
	1. Centralized data access
	2. Mockable Data Access
	3. Central lifetime management
	4. Seperation of Concerns
	5. Domain Driven Design
	6. Unit Testing Friendly
	7. Large team friendly
	8. Service oriented
	9. Honors SOLID Principles
	10. Single page App (SPA) friendly
	11. Simplest thing / YAGNI - You arent gonna need it