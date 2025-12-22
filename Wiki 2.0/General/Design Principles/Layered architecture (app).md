Layered architecture in [[App development]] organizes code into horizontal layers where each handles a specific responsibility, following a strict top-down dependency flow.

​
Typical 4-layer structure

Presentation Layer ([[User Interface|UI]]): Handles user [[Interface]], screens, and user input. In [[Android]]: Composables, Activities, Fragments. Only calls Application layer.

Application Layer (Orchestration): Coordinates business flows, handles use cases, and manages [[State in Compose|State]]. Contains ViewModels, UseCase classes. Calls Domain layer.

Domain Layer (Business Logic): Pure business rules and entities. Independent of UI/database. Contains models like Tip, pure functions like calculateTip(). Called by Application layer.

Infrastructure Layer (Data/External): Database access, APIs, file storage, repositories. Implements Domain interfaces. Lowest layer, called by Domain.

​
Data flow example

text
User taps button (Presentation) 
→ ViewModel fetches data (Application) 
→ UseCase calls business logic (Domain) 
→ Repository queries database (Infrastructure)
→ Result flows back up the layers

Benefits for apps

Testability: Mock lower layers to test upper ones independently.

Maintainability: Change database without touching UI.​

Scalability: Swap UI frameworks or databases easily.
​