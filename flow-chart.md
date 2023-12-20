Amazing mermaid flow chart

```mermaid
flowchart TD
A(Deploy CloudFormation Template)
A --> B{Was deployment successful?}
B -->|Yes| C{What cross account discovery mode is being used?}
B -->|No| D{Were deployment <u><a href='https://docs.aws.amazon.com/solutions/latest/workload-discovery-on-aws/deployment-process-overview.html'>pre-requisites</a></u> checked?}
D -->|Yes| E[Retrieve errors from CloudFormation console. NB: the error from the main stack is insufficent, retrieve the error from the first nested stack failed.]
D -->|No| F[Follow instructions in <u><a href='https://docs.aws.amazon.com/solutions/latest/workload-discovery-on-aws/deployment-process-overview.html'>pre-requisites</a></u> documentation]
F --> A
C -->|SELF_MANAGED| G{Has the required CloudFormation been deployed in the accounts and regions to be discovered as per the <u><a href='https://docs.aws.amazon.com/solutions/latest/workload-discovery-on-aws/deployment-process-overview.html'>documention</a></u>?}
G --> |No| I[Deploy the global resources template exacty once in each account and the regional template in every region as <u><a href='https://docs.aws.amazon.com/solutions/latest/workload-discovery-on-aws/import-a-region.html'>documented</a></u>.]
G --> |Yes| UiErrors{Are there errors dispalyed in the UI}
UiErrors --> |Yes| GetUiLogs[Retrieve any browser errors using the browser dev tools.]
C -->|AWS_ORGANIZATIONS| UiErrors
UiErrors --> |No| MissingResources{Are there resource types <u><a href='https://docs.aws.amazon.com/solutions/latest/workload-discovery-on-aws/supported-resources-1.html'>supported by WD</a></u> that are missing and that are known to be deployed in the target accounts?}
OutOfMemory --> |No| DbCpuStats{Do either the Neptune or Opensearch DBs have abnormally high CPU spikes? Verify by following <u><a href='https://aws-solutions.github.io/workload-discovery-on-aws/workload-discovery-on-aws/2.0/scaling-the-discovery-process.html#_prerequisites'>the pre-requisite steps</a></u>.}
DbCpuStats ---> |No| GetEcsLogs[Retrieve the logs for the discovery process running in ECS as per <u><a href='https://docs.aws.amazon.com/solutions/latest/workload-discovery-on-aws/deployment-process-overview.html'>the documention</a></u>.]
DbCpuStats ---> |Yes| ScaleDbs[Increase the instance size of the DBs through CloudFormation as per the <u><a href='https://aws-solutions.github.io/workload-discovery-on-aws/workload-discovery-on-aws/2.0/scaling-the-discovery-process.html#_increasing_the_database_instance_sizes'>the documention</a></u>.]
ScaleDbs --> A
OutOfMemory --> |Yes| L[Increase the memory of the discovery task by updating the Memory parameter in CloudFormation as per <u><a href='https://docs.aws.amazon.com/solutions/latest/workload-discovery-on-aws/launch-the-stack.html'>the documentation</a></u>.]
L --> A
MissingResources ----> |No| UiProblem[Describe the problem you are seeing and the steps to reproduce]
MissingResources --> |Yes| OutOfMemory[Are there out of memory errors for the discovery process running in ECS? Verify by following <u><a href='https://aws-solutions.github.io/workload-discovery-on-aws/workload-discovery-on-aws/2.0/scaling-the-discovery-process.html#_prerequisites_2'>the pre-requisite steps</a></u>.]
```
