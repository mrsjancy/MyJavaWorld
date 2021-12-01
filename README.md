Microservices 
------------------------------------------------------------------

    https://github.com/avamsykiran/Microservices_29Nov21_10Dec21_09001100.git

    Pre-Requisites
    --------------------------------
        Web MVC -       Servlet and JSP
        Spring IoC
        Spring Context
        Spring SpEL
        Spring Web  - REST 
        Spring Data
        Spring Boot
        Spring Test

    Lab Setup
    -----------------------------------
        JDK 1.8
        STS 4.0 or above
        MySQL
        Postman / Isomnia

    Case Study - BudgetTrackingApp
    ----------------------------------------

        1. Each user can register themselves
        2. Each registred user can login and 
            a. record his monthly transactions
            b. modify a recorded transaction
            c. retrive the monthly expenditure analysis / report

        Decomposition By Bussines Capbilitites & Sub-Domain

            User-Management-Service
                UserEntity              userId:long,fullName:string,email:string,password:string,mobile:string
                UserRepo
                UserService
                UserController
            
            Txn-Management-Service
                UserEntity              userId:long,currentBal:double,txns:Set<TransactionEntity>
                TransactionEntity
                UserRepo
                TransactionRepo
                UserService
                TransactionService
                UserController
                TransactionController

            Report-Management-Service
                UserModel
                TxnModel
                MonthlyStatementModel
                UserService
                TxnService
                MomthlyStatementService
                MonthlyStatementController

        API GateWay Pattern

                            AngularApp / AndriodApp /ReactApp..etc
                                            |↑
                                            ↓|
                                API GATEWAY SERVICE / EDGE SERVICE
                                        localhost:9999
                                            |↑
                                            ↓|
                    -------------------------------------------------------------------------
                    ↑↓                          ↑↓                                         ↑↓
            User-Management-Service   <--->  Txn-Management-Service    <---->   Report-Management-Service
                localhost:9100                  localhost:9200                      localhost:9300
             ↑↓          ↑↓                         ↓↑                                  ↓↑
           umsDB         ||                        tmsDB                                ||
                         -----------------------------------------------------------------
            
                
        Log Aggreatation,Performence Metrics and Distributed Tracing and Health Check Pattern 

                             AngularApp / AndriodApp /ReactApp..etc
                                            |↑
                                            ↓|
                            API GATEWAY SERVICE / EDGE SERVICE
                                     localhost:9999
                                            |↑
                                            ↓|
                     -------------------------------------------------------------------------
                     ↑↓                          ↑↓                                         ↑↓
            User-Management-Service   <--->  Txn-Management-Service    <---->   Report-Management-Service
                localhost:9100                  localhost:9200                      localhost:9300
             ↑↓     |    ↑↓                         ↓↑                                  ↓↑    |    
           umsDB    |    ||                        tmsDB                                ||    |    
                    |    -----------------------------------------------------------------    |    
                    ↓                               ↓                                         ↓    
        |------------------[Logs / Performence Metrics / Req Traces / Health Checks ]----------   
        |         
        |         
        ↓                                                                          
    DT Service                                                                     
    localhost:9411                                                                     


    External Configuaration

                             AngularApp / AndriodApp /ReactApp..etc
                                            |↑
                                            ↓|
                            API GATEWAY SERVICE / EDGE SERVICE
                                     localhost:9999
                                            |↑
                                            ↓|
                     -------------------------------------------------------------------------
                     ↑↓                          ↑↓                                         ↑↓
            User-Management-Service   <--->  Txn-Management-Service    <---->   Report-Management-Service
                localhost:9100                  localhost:9200                      localhost:9300
             ↑↓   ↑ |    ↑↓                         ↓↑                                  ↓↑    |    ↑
           umsDB  | |    ||                        tmsDB                                ||    |    |
                  | |    -----------------------------------------------------------------    |    |
                  | ↓                               ↓                                         ↓    |
        |---------|--------[Logs / Performence Metrics / Req Traces / Health Checks ]------------  |  
        |         |                                   ↑                                            |            
        |         ----------------------------------------------------------------------------------
        ↓                                                                                   ↑       
    DT Service                                                                         Config Service
    localhost:9411                                                                     localhost:9797
                                                                                            ↑
                                                                                       Git Repo/SVM Repo
                                                                                        [all config files]

   Discovery Service

                             AngularApp / AndriodApp /ReactApp..etc
                                            |↑
                                            ↓|
                            API GATEWAY SERVICE / EDGE SERVICE   <--------->        Discovery Service
                                     localhost:9999                                   localhost:9000
                                            |↑                                                  ↑↓
                ------------[register self and discover the peer microservice]--------------------
                ↑↓                          ↓|     ↑↓                                           ↑↓
                ||    ------------------------------------------------------------------------- ||
                ||    ↑↓                          ↑↓                                         ↑↓ ||
            User-Management-Service   <--->  Txn-Management-Service    <---->   Report-Management-Service
                localhost:9101                  localhost:9201                      localhost:9301
                localhost:9102                  localhost:9202                  
                localhost:9103                 
             ↑↓   ↑ |    ↑↓                         ↓↑                                  ↓↑    |    ↑
           umsDB  | |    ||                        tmsDB                                ||    |    |
                  | |    -----------------------------------------------------------------    |    |
                  | ↓                               ↓                                         ↓    |
        |---------|--------[Logs / Performence Metrics / Req Traces / Health Checks ]------------  |  
        |         |                                   ↑                                            |            
        |         ----------------------------------------------------------------------------------
        ↓                                                                                   ↑       
    DT Service                                                                         Config Service
    localhost:9411                                                                     localhost:9797
                                                                                            ↑
                                                                                       Git Repo/SVM Repo
                                                                                        [all config files]                                                                                       