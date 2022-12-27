# .github

This function, in addition to providing the _Lost Worlds Profile_, also defines organization-wide Github Actions for our CI/CD pipeline.

They all leverage a variety of GCP Services.

All are `callable`, meaning they use a `workflow_call`. This simply means they are like _functions_ for CI/CD.

## Workflows

All workflows must define the following `secrets`:

- `PGP_SECRET_SIGNING_PASSPHRASE` - Password to allow secret passing, using the encryption pattern shown [here](https://nitratine.net/blog/post/how-to-pass-secrets-between-runners-in-github-actions/)

### deploy-cloud-function

Deploy a single Cloud Function

#### Inputs

- `directory: string` - Which directory, relative to the calling directory, should be used to find the code.
- `function: string` - The name of the function we want to deploy, which _MUST_ be the same as the `target` (i.e. the route) inside the cloud function
- `env_name: string` - The name of the environment to run the function in in order to get the proper credentials
- `env_string: string (optional)` - An optional string to pass in to make a `.env` file for the cloud function

#### Secrets

- `FUNCTIONS_CREDENTIALS` - Service account key with the [proper permissions](https://github.com/lost-worlds/service-account-builders/blob/main/create-gh-cloud-function-deployer.sh)

### deploy-gae

Deploy GAE Instance

#### Inputs

- `env_name: string` - The name of the environment to run the function in in order to get the proper credentials
- `env_string: string (optional)` - An optional string to pass in to make a `.env` file for the cloud function
- `working_directory: string (optional, default ".")` - Which directory, relative to the calling directory, should be used to find the code

#### Secrets

- `GAE_CREDENTIALS` - Service account key with the [proper permissions](https://github.com/lost-worlds/service-account-builders/blob/main/create-gh-gae-deployer.sh)

### deploy-gce

Deploy GCE Instance

#### Inputs

- `env_name: string` - The name of the environment to run the function in in order to get the proper credentials
- `env_string: string (optional)` - An optional string to pass in to make a `.env` file for the cloud function
- `working_directory: string (optional, default ".")` - Which directory, relative to the calling directory, should be used to find the code
- `GCE_INSTANCE` - The name of the instance to update
- `GCE_INSTANCE_ZONE` - The zone of the instance we are deploying too

#### Secrets

- `GCE_PROJECT` - The project we are targeting with the deployment
- `GCE_CREDENTIALS` - Service account key with the [proper permissions](https://github.com/lost-worlds/service-account-builders/blob/main/create-gh-gce-deployer.sh)
