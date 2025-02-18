- This provides us with a method to build scalable cloud-native apps

What is 12 Factor App?
Why 12 Factor App?
What is Portability: Being able to deploy the same app to different environments or infrastructure

# The 12 Factors
Codebase
Dependencies: 
    Exclusively declare and isolate Dependencies. 
    You cannot assume that your dependencies will be on the system where your app will run. 
    Always state the version of dependencies. 
    It is recommended to create an isolated environment for each application. 
    It ensures that the same version of the app is used across environments. What is the alternative for isolation that is not docker?
    Think Docker Containers 
Config:
    The 12 Factor App stores config in Environment Variables. This is best practice and ensures you can securely open source the code and deploy to multiple environments without changing the code
Backing Services:
    Redis, SMTP or Maybe an S3 service, all are backing services and must be treated as attached service. This means that even if i change the redis instance, it should just work
    the redis or any other attached service should not require change in code when we change the instance
Build, Release and Run:
    How do we rollback a recent change especially in a situation where we do not have time to wait for it to build and redeploy. The solution is to have a separation of the build and run phases.
    The flow is this:
        Write Code. To run it, it should be in an .exe format or binary file or docker file.
        Build Code. This is where we convert the text file to an executable. For docker we use the docker file
        Release: After built the executable and the config file together become the release object. Every release has a unique release id. Every tiny change in the code base should trigger a new release
        Run Phase: Where the release object is run in its environment
    By having a different build and run phase, we strictly separate them and we can store all our build artifacts in a separate service such that when we need to roll back we do not need to build code anymore
Processes:
    12 Factor Processes are stateless and share nothing. What is a sticky Session in Load balance and why are they a violation of 12 Factor Application?
    When we talk about process in this case, do we mean a process running in different servers or we mean different instances of containers?
    All information should be stored in an external service - a DB or caching service such that all processes work as one since they have the same Data State.
Port Binding:
    What does "export HTTP as a service mean", and when you say our app is completely self-contained and does not rely on a web server to function, what do you mean
Concurrency:
    Applications should be scaled horizontally and not vertically
Disposability:
    They should be able to start and stop in seconds based on the requirement of the system. For this to happen we should not have complex startup scripts
    The 12 factor app processes should shut on gracefully when they receive a SIGTerm signal . Stopping gracefully means stop receiving additional requests and complete all the requests you received. After a grace period following the SIGTERM if the process is not stopped a SIGKILL signal 
    is sent to force kill the process. Applications should be able to listen to the SIGTERM signal
Dev Prod Portability:
    The 12 factor app is designed for continuous deployment by keeping the gap between development and production small
    The 12 factor app developer resists the urge to use different backing services between development and production
Logs:
    The 12 factor application should not concern itself with routing or storage of its output stream
    Store logs in a centralized location in a structured format.
    This basically means the app should not try to write to a specific file or be tied to a specific logging solution
    So it parses its logs into a format like JSON format and writes it to its standard output and then an agent can take this and send it to a centralized log service like ELK or FluentD 
Admin Processes
    Recommends that administrative task should run as a script and should be kept separate from the main application and they should be rn in an identical set up. For 
    Example, we will have an identical docker container, run  the script for the process there and terminate it. They should be automated, scalable and reproducible


idea: Train ChatGPT using Frank's Voice. Then GIve it Your notes to create tweets for you based on that voice