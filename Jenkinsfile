def SERVICE_NAME="netcoreapi"
def PACKAGE_NAME="${env.BRANCH_NAME}-netcore-api.1.0.${env.BUILD_NUMBER}.zip"


node {
    def remote = [:]
    def app

    if (env.BRANCH_NAME == "dev") {
      stage('Cleaning ENV') {

          bat "IF EXIST Publish RMDIR /S /Q Publish"
          bat "DEL /S /Q /F *.zip"
    }}


    stage('Clone repository') {
        /* repository cloned to our workspace */
        checkout scm
    }



    if (env.BRANCH_NAME == "dev") {
      stage('DEV: Restore Packages') {
          /* This restoring of the packages of the application. */
          bat "dotnet restore"
    }}

    if (env.BRANCH_NAME == "dev") {
      stage('DEV: Clean') {
          /* This clean the solution. */
          bat "dotnet clean"

    }}

    if (env.BRANCH_NAME == "dev") {
      stage('DEV: Build') {
          /* This builds the solution */
          bat "dotnet build --configuration Release -o Publish"

    }}

    if (env.BRANCH_NAME == "dev") {
      stage('DEV: Pack') {
          /* This builds the solution */
          //bat "dotnet pack --no-build -c Release netcore-api.csproj /p:NuspecFile=nupkgs/netcore-api.1.0.${env.BUILD_NUMBER} /p:NuspecBasePath=nupkgs"
          zip zipFile: "${PACKAGE_NAME}", archive: false, dir: 'Publish'
          bat "dir"

    }}

    if (env.BRANCH_NAME == "dev") {
      stage('DEV: Publish') {
          /* This builds the solution **\\nupkgs\\*.nupkg */
          bat "dir"
          //bat "dotnet nuget push *.nupkg -k c4c9eeb0-fc2f-4590-921e-a0b42f3d4cb6 -s https://www.myget.org/feed/Packages/netcoreapi-demo"

    }}


    // stage('Test image') {
    //
    //      bat "echo 'test passed'"
    // }

    /* This builds the solution **\\nupkgs\\*.nupkg */
    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'Deployment.Server',
    usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']])
    {
      remote.name = 'test'
      remote.host = "${env.ServerIP}"
      remote.user = "${USERNAME}"
      remote.password = "${PASSWORD}"
      remote.allowAnyHosts = true
      if (env.BRANCH_NAME == "dev") {
        stage('DEV: Deploy Artifact') {
          //     writeFile file: 'abc.sh', text: 'ls'
          sshPut remote: remote, from: "${PACKAGE_NAME}", into: "deployments/packages/"
          sshCommand remote: remote, command: "ls -al deployments/${env.BRANCH_NAME}/"
          //     sshGet remote: remote, from: 'abc.sh', into: 'bac.sh', override: true
          //     sshScript remote: remote, script: 'abc.sh'
          //     sshRemove remote: remote, path: 'abc.sh'
      }}

    }

}
