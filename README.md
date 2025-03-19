# Chocolatey Package Deployment Workflow

This repository uses a GitHub Actions workflow to automate the packaging and deployment of Chocolatey packages to GitHub Packages.

## Workflow Overview

The workflow is triggered whenever changes are made to any `chocolateyInstall.ps1` file in the repository. Specifically, it does the following:

1. **Checks out the repository**: The first step ensures that the repository is available for further actions.
2. **Sets up Chocolatey**: The workflow installs Chocolatey so that it can be used for packaging and pushing.
3. **Detects Changed Packages**: It checks which package folders were modified by comparing the latest commit with the previous commit. It looks for changes in the `chocolateyInstall.ps1` file within the `tools` folder of each package.
4. **Packs and Pushes the Package**: For any modified packages, the workflow:
   - Packs the `.nuspec` file into a `.nupkg` package.
   - Pushes the generated `.nupkg` file to GitHub Packages using the API key stored in GitHub Secrets.

## Workflow Trigger

The workflow is triggered when changes are pushed to the `main` branch, specifically when the `chocolateyInstall.ps1` file in the `tools` directory of any package is modified.

```yaml
on:
  push:
    paths:
      - "**/tools/chocolateyInstall.ps1"
    branches:
      - main
```

This ensures that the workflow only runs when there are updates to the package installation scripts.

How to Modify chocolateyInstall.ps1 to Trigger the Workflow
To trigger the workflow, you only need to modify or add a new chocolateyInstall.ps1 file in one of the package folders. Here's how to do it:

1. Add or Modify the chocolateyInstall.ps1 File
The chocolateyInstall.ps1 script is used by Chocolatey to install your package. You can find this file in the tools directory inside each package folder.

Example of the package folder structure:

```php-template

/<package-name>/
  ├── tools/
      └── chocolateyInstall.ps1
  ├── <other-files>
  └── <nuspec-file>
```

If you modify the chocolateyInstall.ps1 file or add a new one to any package folder, the workflow will automatically detect the change. You don't need to manually trigger anything.

2. Commit the Changes
Once you've made your modifications, commit the changes to your repository.

Example Git commands:

```bash

git add .
git commit -m "Modified chocolateyInstall.ps1 for <package-name>"
git push origin main
```

This will trigger the GitHub Actions workflow as long as the chocolateyInstall.ps1 file was modified or added.

## Environment Variables
The workflow uses a few environment variables, which you should ensure are set correctly in your GitHub repository's settings:

 - `APPTOKEN`: This is the GitHub API token with the necessary permissions to push packages to GitHub Packages. It should be stored as a GitHub Secret.
 - `OWNER`: The username or organization name on GitHub that owns the package repository. It is used for authentication and setting up the Chocolatey source.

## Modifying the Workflow

If you need to modify the workflow to suit your needs, here are the key parts you might want to change:

   1. Change the trigger: If you want to trigger the workflow on different paths or branches, modify the `on.push` section.
   2. Change the Chocolatey source URL: If you're pushing to a different repository or source, you can change the `https://nuget.pkg.github.com/vijakodi/index.json` URL to your desired source.
   3. Change package details: If you're using a different way to generate the `.nupkg` file or need to modify the packaging process, you can edit the `Pack and Push Chocolatey Packages` step.

## Conclusion
This workflow helps automate the process of packaging and pushing Chocolatey packages to GitHub Packages. By simply modifying the `chocolateyInstall.ps1` file in any package folder, you can trigger the workflow to create and deploy the updated package.

For any additional changes or further customization of the workflow, you can modify the `choco_package` job in the `chocolatey_package_deployment.yml` file.

