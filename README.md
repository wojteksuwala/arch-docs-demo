# Documemnting architecture demo repository
Below you can find sample diagrams created with Mermaid.

## System context diagram
System context diagram for a system that manages the sales process of financial products: leasing from cars and agro equipment.

```mermaid
    C4Context
        title System Context Diagram for Financial Products Sales System

        Enterprise_Boundary(b1, "Public internet") {
            Person(carDealer, "Car dealer", "Sells car leasing with cars.")
        }
        Enterprise_Boundary(b0, "Bank") {
            Person(bankSalesPerson, "Bank salesperson", "A bank employee <br/>who sells financial products.")
            Person(riskTeamPerson, "Risk Team Member", "A bank employee <br/>who performs risk analysis")

            System(falconSystem, "Falcon", "System that handles sales process for financial car/agro leasing.")

            System_Ext(g2iSystem, "G2I", "Gateway to gov and commercial services <br/>that aggregate customers financial data.")
            System_Ext(ckkSystem, "CKK", "Bank Main customer repository.")

            Rel(carDealer, falconSystem, "Creates offers<br/> and contract")
            Rel(bankSalesPerson, falconSystem, "Create offers/contracts")
            Rel(riskTeamPerson, falconSystem, "Performs risk analysis")

            Rel(falconSystem, g2iSystem, "Get customer financial data")
            Rel(falconSystem, ckkSystem, "Search for, add or update customer")
        }

        UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
```

## Container diagram
Container diagram that presents part of the sales portal for insurance agents.

```mermaid
    C4Container
    title Container diagram for insurance agents sales portal

    Person(salesAgent,"Sales agent", "Person who works for an insurance agency")

    Container(spa, "Agent Frontend Single-Page App", "Blazor WASM", "Agents login, browse products,<br/> create offers and sell policies,<br/> review their sales stats,<br/> chat with each other")

    Container(apiGw, "API Gateway", ".net core, Ocelot", "Gives SPA apps access to microservices")

    Container(productCatalog, "Product catalog", ".net core, minimal API, MongoClient", "Provides access to list of<br/> insurance products and their details")

    ContainerDb(productCatalogDb, "Product catalog db", "Mongodb" , "Products data")

    Rel(salesAgent,spa,"searches for product, creates offers, sells policies, reviews sales stats, chats with other")
    Rel(spa,apiGw, "Makes API calls", "REST JSON/HTTP")
    Rel(apiGw, productCatalog, "Get products", "REST JSON/HTTP")
    Rel(productCatalog, productCatalogDb, "Reads/writes data", "MongoClient")
```
