Microservices 
------------------------------------------------------------------

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
                                            ||
                                            ↓|
                                API GATEWAY SERVICE / EDGE SERVICE
                                        localhost:9999
                                            |↑
                                            ↓|
                    -------------------------------------------------------------------------
                    ↑↓                          ↑↓                                         ↑↓
            User-Management-Service   <--->  Txn-Management-Service    <---->   Report-Management-Service
                localhost:9100                  localhost:9200                      localhost:9300
                         ↑↓                                                             ↓↑
                         -----------------------------------------------------------------                                          
            
                
             