# AzureFunctionsTest
Reproduce issue with Azure Functions running on Windows vs Mac

Both MacOS High Sierra and Windows 10 are running:
- .Net Core v2.1.403
- Azure Functions CLI 2.1.725

Azure CLI (MacOS) (via Homebrew): 
- azure-cli (2.0.48)
- Python (Darwin) 3.7.0 (default, Jun 29 2018, 20:13:13)
- [Clang 9.1.0 (clang-902.0.39.2)]

Windows:
- azure-cli (2.0.47) (latest that can be downloaded at the moment?)
- Python (Windows) 3.6.5 (v3.6.5:f59c0932b4, Mar 28 2018, 16:07:46) [MSC v.1900 32 bit (Intel)]

Project was created using `func init` and `func new` and choosing Orchestration function. No other changes were made.

In Mac, the function starts as expected (either via VS Code or by running `func host start --build`). Console output:

```
Azure Functions Core Tools (2.1.725 Commit hash: 68f448fe6a60e1cade88c2004bf6491af7e5f1df)
Function Runtime Version: 2.0.12134.0
[10/20/2018 12:18:33] Building host: startup suppressed:False, configuration suppressed: False
[10/20/2018 12:18:33] Reading host configuration file '/Users/richard/Development/Personal2/FunctionsTest/bin/output/host.json'
[10/20/2018 12:18:33] Host configuration file read:
[10/20/2018 12:18:33] {
[10/20/2018 12:18:33]   "version": "2.0"
[10/20/2018 12:18:33] }
[10/20/2018 12:18:38] Host secret 'durabletask_extension' for 'systemkeys' Created.
[10/20/2018 12:18:38] Initializing extension with the following settings: Initializing extension with the following settings:
[10/20/2018 12:18:38] AzureStorageConnectionStringName: , MaxConcurrentActivityFunctions: 80, MaxConcurrentOrchestratorFunctions: 80, PartitionCount: 4, ControlQueueBatchSize: 32, ControlQueueVisibilityTimeout: 00:05:00, WorkItemQueueVisibilityTimeout: 00:05:00, ExtendedSessionsEnabled: False, EventGridTopicEndpoint: , NotificationUrl: https://0.0.0.0:7071/runtime/webhooks/durabletask, LogReplayEvents: False. InstanceId: . Function: . HubName: DurableFunctionsHub. AppName: . SlotName: . ExtensionVersion: 1.6.2. SequenceNumber: 0.
[10/20/2018 12:18:38] Initializing Host.
[10/20/2018 12:18:38] Host initialization: ConsecutiveErrors=0, StartupCount=1
[10/20/2018 12:18:38] Starting JobHost
[10/20/2018 12:18:38] Starting Host (HostId=richardsmacbookpro2-1825371859, InstanceId=ba9f56d1-1c93-49d7-b79b-57926d998ecf, Version=2.0.12134.0, ProcessId=20071, AppDomainId=1, Debug=False, FunctionsExtensionVersion=)
[10/20/2018 12:18:38] Loading functions metadata
[10/20/2018 12:18:38] 3 functions loaded
[10/20/2018 12:18:38] Generating 3 job function(s)
[10/20/2018 12:18:38] Found the following functions:
[10/20/2018 12:18:38] FunctionsTest.TestFunction.HttpStart
[10/20/2018 12:18:38] FunctionsTest.TestFunction.RunOrchestrator
[10/20/2018 12:18:38] FunctionsTest.TestFunction.SayHello
[10/20/2018 12:18:38]
```

In Windows however, the functions aren't loaded:
```
Azure Functions Core Tools (2.1.725 Commit hash: 68f448fe6a60e1cade88c2004bf6491af7e5f1df)
Function Runtime Version: 2.0.12134.0
[2018/10/20 13:07:23] Building host: startup suppressed:False, configuration suppressed: False
[2018/10/20 13:07:23] Reading host configuration file 'Y:\Development\Personal2\FunctionsTest\bin\output\host.json'
[2018/10/20 13:07:23] Host configuration file read:
[2018/10/20 13:07:23] {
[2018/10/20 13:07:23]   "version": "2.0"
[2018/10/20 13:07:23] }
[2018/10/20 13:07:24] Initializing Host.
[2018/10/20 13:07:24] Host initialization: ConsecutiveErrors=0, StartupCount=1
[2018/10/20 13:07:24] Starting JobHost
[2018/10/20 13:07:24] Starting Host (HostId=desktopcuf4r9j-1404840500, InstanceId=815f4d06-97db-44f8-a672-31d39afaed12, Version=2.0.12134.0, ProcessId=9356, AppDomainId=1, Debug=False, FunctionsExtensionVersion=)
[2018/10/20 13:07:24] Loading functions metadata
[2018/10/20 13:07:24] 3 functions loaded
[2018/10/20 13:07:24] Generating 1 job function(s)
[2018/10/20 13:07:24] Error indexing method 'TestFunction.HttpStart'
[2018/10/20 13:07:24] Microsoft.Azure.WebJobs.Host: Error indexing method 'TestFunction.HttpStart'. Microsoft.Azure.WebJobs.Host: Cannot bind parameter 'starter' to type DurableOrchestrationClient. Make sure the parameter Type is supported by the binding. If you're using binding extensions (e.g. Azure Storage, ServiceBus, Timers, etc.) make sure you've called the registration method for the extension(s) in your startup code (e.g. builder.AddAzureStorage(), builder.AddServiceBus(), builder.AddTimers(), etc.).
[2018/10/20 13:07:24] Function 'TestFunction.HttpStart' failed indexing and will be disabled.
[2018/10/20 13:07:24] No job functions found. Try making your job classes and methods public. If you're using binding extensions (e.g. Azure Storage, ServiceBus, Timers, etc.) make sure you've called the registration method for the extension(s) in your startup code (e.g. builder.AddAzureStorage(), builder.AddServiceBus(), builder.AddTimers(), etc.).
[2018/10/20 13:07:24] Host initialized (322ms)
[2018/10/20 13:07:24] Host started (336ms)
[2018/10/20 13:07:24] Job host started
[2018/10/20 13:07:24] The following 2 functions are in error:
[2018/10/20 13:07:24] TestFunction: The binding type(s) 'orchestrationTrigger' are not registered. Please ensure the type is correct and the binding extension is installed.
[2018/10/20 13:07:24] TestFunction_Hello: The binding type(s) 'activityTrigger' are not registered. Please ensure the type is correct and the binding extension is installed.
[2018/10/20 13:07:24]
[2018/10/20 13:07:24]
Hosting environment: Production
```

Checking 