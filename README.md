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

        UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

## Container diagram
Container diagram that presents part of the sales portal for insurance agents.

```mermaid
    C4Container
    title Container diagram for insurance agents sales portal

    Person(salesAgent,"Sales agent", "Person who works for an insurance agency")

    Container_Boundary(b0,"") {
    Container(spa, "Agent Frontend Single-Page App", "Blazor WASM", "Agents login, browse products,<br/> create offers and sell policies,<br/> review their sales stats,<br/> chat with each other")
    }

    Container_Boundary(b1,"") {
    Container(apiGw, "API Gateway", ".net core, Ocelot", "Gives SPA apps access to microservices")
    }


    Container_Boundary(b2,"") {
    Container(productCatalog, "Product catalog", ".net core, minimal API, MongoClient", "Provides access to list of<br/> insurance products and their details")
    Container(policyService, "Policy Service", ".net core, minimal API, NHibernate", "Manages offers,<br/> and policies")
    Container(pricingService, "Pricing Service", ".net core, minimal API, Marten", "Calculates prices")

    ContainerDb(productCatalogDb, "Product catalog db", "Mongodb" , "Products data")
    ContainerDb(policyServiceDb, "Policies db", "Postgresql 16" , "Offers and policies")
    ContainerDb(pricingServiceDb, "Tariffs db", "Postgresql 16" , "Tariffs")
    }

    Rel(salesAgent,spa,"searches for product, creates offers, sells policies, reviews sales stats, chats with other")
    Rel(spa,apiGw, "Makes API calls", "REST JSON/HTTP")
    Rel(apiGw, productCatalog, "Get products", "REST JSON/HTTP")
    Rel(productCatalog, productCatalogDb, "Reads/writes data", "MongoClient")

    Rel(apiGw, policyService, "Create offers and policies", "REST JSON/HTTP")
    Rel(policyService, policyServiceDb, "Reads/writes data", "NHibernate")

    Rel(policyService, pricingService, "calc price", "REST JSON/HTTP")
    Rel(pricingService, pricingServiceDb, "Reads/writes data", "Marten")

    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

## Component diagram
Component diagram for policy service.

```mermaid
    C4Component
    title Component diagram for Policy Service

    Container_Boundary(policyService, "Policy Service") {
        Component(offerController, "OfferController", "C# Web API Controller", "Exposes REST API for offer management")

        Component(createOffer, "CreateOffer", "C#,MediatR Handler", "creates offer")
        Component(getOffer, "GetOffer", "C#,MediatR Handler", "retrieves offer")

        Component(pricingService, "PricingService", "C#, Refit Http Client", "Http client for pricing service")
        Component(offer, "Offer", "C#, Aggregate Root", "Offer agg. rooot")
        Component(offerRepo, "OfferRepository", "C#, NHibernate", "Saves/loads offers")

        Rel(offerController, createOffer, "Uses")
        Rel(offerController, getOffer, "Uses")
        UpdateRelStyle(offerController, getOffer, $offsetY="-140")

        Rel(createOffer,pricingService,"Calc price")
        Rel(createOffer,offer,"Creates offer")
        Rel(createOffer,offerRepo,"Save")

        Rel(getOffer,offerRepo,"Load offer")   
    }

    Container_Boundary(db, "Database") {
        ContainerDb(policiesDb, "Policies db", "Postgresql 16" , "offers and policies")
    }

    Rel(offerRepo,policiesDb,"Save/load data","Nhibernate")

    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```
```
