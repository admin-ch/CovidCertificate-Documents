# Guide to update the API documentation
The update of the API documentation is needed if endpoints get modified, added or removed. The update is based on the
`api-doc.yaml` that can be looked up from swagger. As the API documentation is for the TEST and PROD environment, the
servers are coded inside the OpenApiConfig class of the CovidCertificate-Api-Gateway-Service.

To update the API we have some steps to do that got mostly automated in the shell script `update_api_doc_template.sh`.

## Prepare the update
Get a clone of the GitHub Project CovidCertificate-Apidoc and ensue that you are able to call the URL:
`cc-api-gateway-service.abn.app.cfap02.atlantica.admin.ch` or ensure that you have a running instance of
the GitHub project CovidCertificate-Api-Gateway-Service.

Create a local copy of `update_api_doc_template.sh` and rename it to `update_api_doc.sh`.
In your local copy change the url attribute of the `define_variables` method.
Also place your individual `git_access_token` into the corresponding variable for pushing the changes.
Make sure you have a clean working directory by executing `git status`. You should be informed that `nothing to commit, working tree clean`.

## Execute the script
### Getting help
- execute `./update_api_doc.sh -h`, to list a simple help

### Step by step - recommended for the first time
- execute `./update_api_doc.sh -u`, to update git files
- execute `./update_api_doc.sh -t`, to take new api doc from ABN
- update the `README.md` where needed
- execute `./update_api_doc.sh -c`, to commit updated files to git
- execute `./update_api_doc.sh -p`, to push changes to remote branch
- open a pull request using the git hub web interface
- let the pull request be reviewed by other members
- merge it at the day of release

### As is if you are familiar with the script and no update of README.md is needed
- execute `./update_api_doc.sh`, to execute all 4 steps above in one row
- open a pull request using the git hub web interface
- let the pull request be reviewed by other members
- merge it at the day of release
