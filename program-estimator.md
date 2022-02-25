---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: quantum, Qiskit, runtime, near time compute

subcollection: quantum-computing

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Estimator primitive
{: #program-estimator}
{: toc-content-type="reference"}
{: toc-completion-time="10m"}

Understand the input and output for the Estimator primitive.
{: shortdesc}

## Overview
{: #estimator-overview}

The Estimator primitive returns an observable expectation value generated by given circuits executed on the target backend.  For a full, end-to-end example of how to use this primitive, see [Get started with Estimator](/docs/quantum-computing?topic=quantum-computing-example-estimator).

## Input parameters
{: #estimator-parameters}

- **circuits**:
       - **Description**: A QuantumCircuit or list of QuantumCircuits. The circuits can be parameterized.
        - **Example**:
            ```python
            psi1 = RealAmplitudes(num_qubits=2, reps=2)
            psi2 = RealAmplitudes(num_qubits=2, reps=3)
            circuits=[psi1, psi2]          
            ```
        - **Required**: True
- **observables**: Properties of the system or function being evaluated.  Currently must be opflow `PauliSumOp` (primitive is SparsePauliOp) or quantum_info operator, `BaseOperator`, which is compatible with SparsePauliOp.
        - **Example**:
             ```python
             H1 = PauliSumOp.from_list(
             [
             ("II", -1.052373245772859),
             ("IZ", 0.39793742484318045),
             ("ZI", -0.39793742484318045),
             ("ZZ", -0.01128010425623538),
             ("XX", 0.18093119978423156),
             ]
             )
             H2 = PauliSumOp.from_list([("IZ", 1)])
             H3 = PauliSumOp.from_list([("ZI", 1), ("ZZ", 1)])
             observables=[H1, H2, H3],
             ````
    - **backend**:
        - **Description**: The name of the backend to run the program on.
        - **Example**:
            ```python
            backend=aer_simulator
            ```
        - **Required**: False.  If none is specified, the program will be run on the next available backend that you have access to.
    - **grouping**:
        - **Description**: Specifies `(circuit, observable)` pairs for which to calculate expectation values. They are specified as index pairs.  For example `(0,1)` specifies to calculate the expectation value for the first circuit and the second observable.
        - **Example**:
            ```python
            grouping=[(0, 0), (0, 1)]
            ```
        - **Required**: False
    - **circuit parameters**:
        - **Description**: Optional input for parameterized circuits.
        - **Example**:
            ```python
            θ1 = [0, 1, 1, 2, 2, 6, 3, 4]
            H1_result = e(θ1, grouping=[Group(0, 0)])
            ```
        - **Required**: False
    - **shots**:
        - **Description**: The number of times to run each circuit.
        - **Example**:
            ```python
            shots = 1024
            ```
        - **Required**: False


## Output parameters
{: #estimator-parameters-read}

    - **values**:  
        - **Description**: Quasiprobabilities for each grouping.  The resulting list is in order based on the specified groupings.  For example, if this grouping is specified: `grouping=[(0, 0), (0, 1)]` , then the result contains two results.  The first result is the value calculated for circuit 1 with observable 1, and the second result is the value calculated for circuit 1 with observable 2.
        - **Type**: list
        - **Example**:
            ```python
            EstimatorArrayResult(values=array([-1.27506899, -0.59895964]), variances=array([0.30091147, 0.22917904]))
            ```
    - **variances**:  
        - **Description**: Variances for each grouping.  The resulting list is in order based on the specified groupings.  For example, if this grouping is specified: `grouping=[(0, 0), (0, 1)]` , then the result contains two results.  The first result is the value calculated for circuit 1 with observable 1, and the second result is the value calculated for circuit 1 with observable 2.
        - **Type**: list
        - **Example**:
            ```python
            EstimatorArrayResult(values=array([-1.27506899, -0.59895964]), variances=array([0.30091147, 0.22917904]))
            ```