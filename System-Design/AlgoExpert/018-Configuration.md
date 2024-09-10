## Configuration

These are settings we use to configure the behavior of a system. They could include special checks like a log level indicator or secret keys like connectionStrings.

We have two types
- Static: will usually include some .env file which be be bundled together with the project. For us to make a change, we would have to redeploy the entire application
- Dynamic: will usually be deployed differently.

While Dynamic is more flexible and can be faster because we do not need to redeploy, it requires a lot more in  terms of access control and security.