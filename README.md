# Setting Up Azure Key Vault Integration with Kubernetes

This guide will help you set up the integration between Azure Key Vault and Kubernetes using the Secrets Store CSI Driver.

## Prerequisites

- Access to an Azure subscription
- Azure Key Vault set up with necessary secrets
- Kubernetes cluster set up and configured
- Secrets Store CSI Driver installed in your Kubernetes cluster

## Installation Steps

1. Apply the provided SecretProviderClass YAML file to your Kubernetes cluster:

    ```bash
    kubectl apply -f secretProviderClass.yaml
    ```

2. Make sure to replace the placeholders in the YAML file with your specific configurations:
    - `userAssignedIdentityID`: If using a user-assigned managed identity, provide its ID.
    - `keyvaultName`: Specify the name of your Azure Key Vault.
    - `objectName`: Enter the name of the secrets stored in Azure Key Vault.
    - `tenantId`: Provide the Azure Active Directory tenant ID.

3. Verify that the SecretProviderClass resource has been created successfully:

    ```bash
    kubectl get secretproviderclass
    ```

4. Once the SecretProviderClass is created, Kubernetes will automatically sync the secrets from Azure Key Vault to Kubernetes secrets based on the configurations provided in the YAML file.

5. You can verify that the secrets have been synced by checking the Kubernetes secrets:

    ```bash
    kubectl get secrets
    ```

## Usage

Once the integration is set up, you can use the secrets in your Kubernetes pods by referencing the corresponding Kubernetes secret names.

Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    env:
    - name: MY_SECRET
      valueFrom:
        secretKeyRef:
          name: <secretName>
          key: <keyName>
