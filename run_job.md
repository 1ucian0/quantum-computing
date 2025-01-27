---

copyright:
  years: 2021, 2022
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}


# Run a job using a Qiskit runtime program
{: #run_job}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

This tutorial walks you through the steps to use a program to run a job on an IBM quantum computer, then review the results. See this topic for a detailed [example](/docs/quantum-computing?topic=quantum-computing-vqe).
{: shortdesc}


You can run a job by using Qiskit Runtime (Python) or by directly calling the Quantum Cloud Runtime API (curl). The text and code samples update dynamically depending on your choice. Choosing an option in one table updates the content in the entire page. To view the appropriate information, choose Python or curl below:

```
Leave this tab selected if you use Python.
```
{: codeblock}
{: python}

```
Leave this tab selected if you use curl.
```
{: pre}
{: curl}

## Before you begin
{: #run-program-byb}

You'll need a circuit to submit to the program. To learn how to create circuits by using Qiskit, see the [Circuit basics tutorial](https://qiskit.org/documentation/tutorials/circuits/01_circuit_basics.html){: external}.


## Choose a program
{: #choose-program}
{: step}


The list of available programs is on the [Programs page](cloud.ibm.com/quantum/programs){: external}.  Choose the program you want to run and note its ID.  You need this information to run the job.

The following programs are publicly available. For more information about these programs, including the required input parameters, see the [Estimator](/docs/quantum-computing?topic=quantum-computing-program-estimator) or [Sampler](/docs/quantum-computing?topic=quantum-computing-program-sampler) topic.

- **estimator**:  
       Returns an observable expectation value generated by given circuits executed on the target backend.
- **sampler**:  
       Sample distributions generated by given circuits executed on the target backend.

## Run the job
{: #run-job-step}
{: step}


You will use the Qiskit Runtime IBMRuntimeService.run() method, which takes the following parameters:
{: python}

- program_id: ID of the program to run.
- inputs: Program input parameters. These input values are passed to the runtime program and are dependent on the parameters defined for the program.
- options: Runtime options. These options control the execution environment. Currently, the only available option is backend_name, which is optional. If you do not specify a backend, the job is sent to the least busy device that you have access to.
- result_decoder: Optional class used to decode the job result.
{: python}

In the following example, we will submit a circuit to the Sampler program.
{: python}

Use the [Run a job API](/apidocs/quantum-computing#create-job){: external}. Optionally, use [Swagger](https://us-east.quantum-computing.cloud.ibm.com/openapi/#/Jobs/create_job){: external}. You must specify the program ID and can optionally supply parameters and the device to run on. Any other input is ignored. Note the job ID that is returned. You need this information to check the status and view results.
{: curl}

```
python
from qiskit.test.reference_circuits import ReferenceCircuits
from qiskit_ibm_runtime import IBMRuntimeService

service = IBMRuntimeService()
program_inputs = {
    'circuits': ReferenceCircuits.bell()
}
options = {'backend_name': 'ibmq_qasm_simulator'}
job = service.run(
      program_id="sampler",
      options=options,
      inputs=program_inputs)
print(f"job ID: {job.job_id}")
result = job.result()
```
{: codeblock}
{: python}

```
curl -g --request POST 'https://us-east.quantum-computing.cloud.ibm.com/jobs' --header 'Content-Type:application/json' --header 'Service-CRN: crn:v1:{cname}:{ctype}:{service-name}:{location}:a/{IBM-account}:{service-instance}::' --header 'Authorization: Bearer <Bearer Token>' --data-raw '{
  "programId": "id",
  "params": [
    "parameter1 ",
    "parameter2"
    ],
}'
```
{: pre}
{: curl}

If you do not specify the device, the job is sent to the least busy device that you have access to.

To ensure fairness, there is a maximum execution time for each Qiskit Runtime job. If a job exceeds this time limit, it is forcibly terminated. The maximum execution time is the smaller of 1) the system limit and 2) the `max_execution_time` defined by the program. The system limit is 3 hours for jobs running on a simulator and 8 hours for jobs running on a physical system.
{: note}

## (Optional) Return the job status
{: step}

Follow up the Qiskit Runtime IBMRuntimeService.run() method by running a RuntimeJob method. The run() method returns a RuntimeJob instance, which represents the asynchronous execution instance of the program.
{: python}

There are several RuntimeJob methods to choose from, including job.status():

Run the [List job details API](/apidocs/quantum-computing#get-job-details-jid){: external}, manually or by using [Swagger](https://us-east.quantum-computing.test.ibm.com/openapi/#/Jobs/get_job_details_jid){: external} to check the job's status.
{: curl}

```
job.status()
```
{: codeblock}
{: python}

```
{
  "id": "c5dge2d3rn7breq27i9g",
  "backend": "ibmq_qasm_simulator",
  "status": "completed",
  "params": {
    "iterations": 3
  },
  "program": {
    "id": "myprogram-abcdef12345"
  },
  "created": "2021-10-04T13:52:09.456851Z",
  "runtime": "qiskit-program:latest"
}
```
{: pre}
{: curl}



## Next steps
{: #next-steps}

- [View the results](/docs/quantum-computing?topic=quantum-computing-results).
- View the [API reference](/apidocs/quantum-computing/quantum-computing){: external}.
- Learn about IBM Quantum:
    - [IBM Quantum Computing](https://www.ibm.com/quantum-computing/){: external}
    - [Qiskit](https://qiskit.org/){: external}